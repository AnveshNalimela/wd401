## Add an error tracking system in your React project
we are going to use Sentry for error tracking in the react application(SportBuzz)
Sentry provides In addition to capturing errors, you can monitor interactions between multiple services or applications by enabling tracing.
You can also get to the root of an error or performance issue faster, by watching a video-like reproduction of a user session with session replay.

We need to create sentry account and latest version of javascript SDK should be avaliable in your system
### Installing the Sentry through CMD
```bash
npm install @sentry/react --save
```

we need to intialize the sentry at the entry point of application here it is main.tsx
the code for intializatio of sentry

```tsx
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: "https://209db89451e09b61480f0fc813ba1855@o4507836701933568.ingest.us.sentry.io/4507836705144832",
  integrations: [
    Sentry.browserTracingIntegration(),
    Sentry.replayIntegration(),
  ],
  // Tracing
  tracesSampleRate: 1.0, //  Capture 100% of the transactions
  // Set 'tracePropagationTargets' to control for which URLs distributed tracing should be enabled
  tracePropagationTargets: ["localhost", /^https:\/\/yourserver\.io\/api/],
  // Session Replay
  replaysSessionSampleRate: 0.1, // This sets the sample rate at 10%. You may want to change it to 100% while in development and then sample at a lower rate in production.
  replaysOnErrorSampleRate: 1.0, // If you're not already sampling the entire session, change the sample rate to 100% when sampling sessions where errors occur.
});

and After intialzation of sentry we need to update the vite configuartion to sentry by adding the sentry plugin

```json
sentryVitePlugin({
    org: "cbit-u7",
    project: "javascript-react"
  }),
```


We will upload the source maps to sentry using the command
```bash
npx @sentry/wizard@latest -i sourcemaps
```

and you can build the application
```bash
npm run build
```

![image](https://github.com/user-attachments/assets/66b9479e-b776-4858-be74-873f8f9cea6d)

and Run the application in preview mode
![image](https://github.com/user-attachments/assets/ed3f3335-4cc3-4beb-bbd9-85d37501c39a)

and now our application is configured with error tracking system.
### Explain how did you use a debugger to track a bug in your application?
When your code execution reaches debugger line, it will pause, and you can examine the current state in your browser's developer tools
![image](https://github.com/user-attachments/assets/8f2c0ea3-03ab-4d85-b571-a2f1a73637ba)


![image](https://github.com/user-attachments/assets/8e0dd6a6-f2f0-45c8-936a-62f2731a4210)


First, recognize the issue or bug in your application. This could be a feature not working as expected, a crash, an unexpected output, or even a visual issue.
For a React application, you can use browser developer tools like Chrome DevTools, or an integrated debugger in your IDE (e.g., Visual Studio Code).
Open the file where you suspect the bug might be occurring.
Set breakpoints at key lines of code where you want to pause the execution(debugger line)
open the DevTools (F12 or Ctrl+Shift+I), go to the "Sources" tab, and refresh your application. The code will pause at your breakpoints.
Once the code execution pauses at a breakpoint, inspect the variables to see their values at that point in the execution.
Examine the call stack to understand the sequence of function calls that led to this point.
and Now fix the bug.

+
