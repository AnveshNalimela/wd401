## Configuration of Environment Variables in a Dockerized Node.js Application
When containerizing a Node.js application, it’s important to manage environment variables effectively, 
especially when dealing with sensitive information like API keys and database credentials. 

#### Defining Environment Variables
Environment variables allow you to pass configuration settings to your application without hardcoding them in your codebase.
This flexibility is crucial for managing different environments like development, staging, and production.
##### Using a .env File
The .env file stores environment variables in a key-value format. It is typically used during development and can be loaded into the application using a library like process.env
Whereas we developing using vite templtate we use "import.meta.env" inorder to access the environment varibales

#### Secure Management of Environment Variables
 To keep  Sensitive Data Secure
 we use .env file for local devlopment and Add the .env file to .gitignore to prevent it from being committed to version control.
 ![image](https://github.com/user-attachments/assets/38ddfd3e-95fc-462d-9693-6d543e78d6fd)
In case of Deployment of application on any platform provides add environment varibles option to secure the sensitive information like API keys and user credintials

![image](https://github.com/user-attachments/assets/a55d3888-6bce-44f8-8acd-f5c09f1e6dab)

#### secure management of Environment variables in CI/CD pipeline
GitHub Actions Secrets: Store sensitive information like API keys and credentials in GitHub Secrets. Access these secrets in your GitHub Actions workflows
Here the user creditianls of docker hub are stored in the action secerts to secure them from unauthorized access
![image](https://github.com/user-attachments/assets/b7607961-2597-4f26-a1f2-801d306e1fee)

## Integration of error reporting mechanisms:
we need to create an channel on Discord in order reports of deployment over there
### Set Up a Webhook in Discord

Open Discord and go to the server where you want to receive notifications.
go to Server Settings -> Integrations -> Webhooks.
Create a new Webhook and select the channel.
Copy the Webhook URL.
![image](https://github.com/user-attachments/assets/f0b2e694-3537-4ac1-bfc3-f8c5800ea7e1)

###  Store the Webhook URL in GitHub Secrets
Go to your GitHub repository.
Navigate to Settings -> Secrets and variables -> Actions -> New repository secret.
Add a new secret with a name like WEBHOOK_URL and paste the Webhook URL you copied.
![image](https://github.com/user-attachments/assets/937b8a68-2ccc-42af-a61a-2ea2c860398a)

Integration of these with our workflow
```yml
name: Report errors to Slack/Discord
        if: failure()
        run: |
          curl -X POST -H 'Content-type: application/json' --data '{"content":"Docker image deploy failed in CI pipeline for commit ${{ github.sha }}. Check the logs for details."}' ${{ secrets.WEBHOOK_URL }}

```
the purpose of this step is to report errors to Slack or Discord.GitHub Actions to only execute this step if the previous steps in the job have failed. 
The failure() function is a built-in GitHub Actions conditional that returns true if any previous step has failed.
By using this, you ensure that notifications are only sent out when something goes wrong, avoiding unnecessary alerts for successful builds.
curl -X POST: Specifies that this is a POST request. In this context,we sending data to a URL (the Webhook URL for Discord).

-H 'Content-type: application/json': Sets the Content-Type header to application/json. This tells the receiving server ( Discord) that the data being sent is in JSON format.

--data: Specifies the data to send in the POST request. In this case, it's a JSON object containing a message.
The content field holds the message that will appear in the  Discord channel

