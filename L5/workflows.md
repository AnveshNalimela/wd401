## Creating a GitHub Actions Workflow
GitHub Actions workflows allow you to automate tasks like building, testing, and deploying your code.
Workflows are defined in YAML files, and they are stored in the .github/workflows directory of your repository
### Create the Workflow File
Create docker.yml workflow file in .github/workflows folder
![image](https://github.com/user-attachments/assets/f12a04ce-5f60-4b2c-84f9-144865d10f1f)
### Define the Workflow
In the YAML file, we can define different jobs and steps that will run when certain events happen, like a push to the main branch.

```docker.yml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  linting:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Run linter
        run: npm run lint

      - name: Report errors to Slack/Discord
        if: failure()
        run: |
          curl -X POST -H 'Content-type: application/json' --data '{"content":"Linting failed in CI pipeline for commit ${{ github.sha }}. Check the logs for details."}' ${{ secrets.WEBHOOK_URL }}

  testing:
    name: Test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Use Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "20"

      - name: Clean install dependencies
        run: |
          rm -rf node_modules
          rm -f package-lock.json
          npm install

      - name: Run Vitest(unit) tests
        run: npm run vitest:test

      - name: Run Cypress(integration) tests
        uses: cypress-io/github-action@v5
        with:
          build: npm run build
          start: npm run dev
          wait-on: "http://localhost:5175"

      - name: Report errors to Slack/Discord
        if: failure()
        run: |
          curl -X POST -H 'Content-type: application/json' --data '{"content":"Testing failed in CI pipeline for commit ${{ github.sha }}. Check the logs for details."}' ${{ secrets.WEBHOOK_URL }}

  build-docker-image:
    name: Build Docker Image
    runs-on: ubuntu-latest
    needs: [linting, testing]

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/sportbuzz:${{ github.sha }} .

      - name: Push Docker image to Docker Hub
        run: docker push ${{ secrets.DOCKER_USERNAME }}/sportbuzz:${{ github.sha }}

      - name: Tag the Docker image with 'latest'
        run: docker tag ${{ secrets.DOCKER_USERNAME }}/sportbuzz:${{ github.sha }} ${{ secrets.DOCKER_USERNAME }}/sportbuzz:latest

      - name: Push 'latest' Docker image to Docker Hub
        run: docker push ${{ secrets.DOCKER_USERNAME }}/sportbuzz:latest

      - name: Report errors to Slack/Discord
        if: failure()
        run: |
          curl -X POST -H 'Content-type: application/json' --data '{"content":"Docker image build failed in CI pipeline for commit ${{ github.sha }}. Check the logs for details."}' ${{ secrets.WEBHOOK_URL }}

  deploy:
    name: Deploy Application
    runs-on: ubuntu-latest
    needs: build-docker-image
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Pull Docker image
        run: docker pull ${{ secrets.DOCKER_USERNAME }}/sportbuzz:latest

      - name: Run Docker container
        run: |
          docker stop sportbuzz || true
          docker rm sportbuzz || true
          docker run -d -p 5000:5000 --name sportbuzz ${{ secrets.DOCKER_USERNAME }}/sportbuzz:latest

      - name: Wait for application to start
        run: sleep 10

      - name: Verify application is running
        run: curl --fail http://localhost:5000 || exit 1

      - name: Report errors to Slack/Discord
        if: failure()
        run: |
          curl -X POST -H 'Content-type: application/json' --data '{"content":"Docker image deploy failed in CI pipeline for commit ${{ github.sha }}. Check the logs for details."}' ${{ secrets.WEBHOOK_URL }}
```

### Explanation of the CI/CD Pipeline 

#### Pipeline Stages:

#### Code Validation (Linting):

Linting checks your code for errors and style issues.
If the linting fails, an error is reported to a Slack or Discord channel.
![image](https://github.com/user-attachments/assets/b753443d-b5dc-40b5-b3fe-03c58fbf0fe0)

#### Testing:

##### Unit Tests: 
Using Vitest to ensure individual units of code function correctly.
![image](https://github.com/user-attachments/assets/a61f41f5-57f5-4703-a259-75e7a336f9d4)

#### Integration Tests: 
Using Cypress to validate the integration of different parts of your application.
![image](https://github.com/user-attachments/assets/fe4e46f9-3487-4fe6-ab30-5f866585f4aa)

![image](https://github.com/user-attachments/assets/3f7c4efd-4214-49d2-bf9f-14ed25afd2f1)



#### Building Docker Image:

The Docker image is built from the latest code.
If successful, the image is tagged and pushed to Docker Hub.
![image](https://github.com/user-attachments/assets/2a9df46e-bf05-4f69-8503-5e612aa944f4)

#### Deployment:


The application is deployed by pulling the latest Docker image and running it on a server.
The pipeline verifies that the application is running properly.

#### Error Reporting:
If any stage fails, detailed error messages are sent to a Slack or Discord channel.
![image](https://github.com/user-attachments/assets/024d6dff-6de9-44e0-ad67-22d34775d91f)


![image](https://github.com/user-attachments/assets/9bd667a3-b935-4f2a-ba7d-19093e395dc9)
