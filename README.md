This is a starter template for [Learn Next.js](https://nextjs.org/learn).

# Steps to deploy NextJS app to Firebase Hosting

1. Create a new project in Firebase console
2. Install Firebase CLI
3. Login to Firebase CLI
4. Initialize Firebase project
5. Build NextJS app
6. Deploy NextJS app to Firebase Hosting

## 1. Create a new project in Firebase console

Go to [Firebase console](https://console.firebase.google.com/) and create a new project.

## 2. Install Firebase CLI

Install Firebase CLI globally using npm:

```bash
npm install -g firebase-tools
```

## 3. Login to Firebase CLI

Login to Firebase CLI using the following command:

```bash
firebase login
```

## 4. Initialize Firebase project

Initialize Firebase project using the following command:

```bash
firebase init hosting
```
For the public directory, enter `out` as the public directory.

Type `y` to configure as a single-page app.

## 5. Build NextJS app

Edit the `next.config.js` file and add the following code:

```js
output: 'export',
images: {
    unoptimized: true,
}
```

Build NextJS app using the following command:

```bash
npm run build
```

## 6. Deploy NextJS app to Firebase Hosting

To enable navigation between pages, edit the `firebase.json` file and add the following code:

```json
cleanUrls: true,
```

Deploy NextJS app to Firebase Hosting using the following command:

```bash
firebase deploy --only hosting
```


# Firebase Test, Preview, and Deploy

## Test

From the root of the local project directory, run the following command:

```bash
firebase emulators:start
```

Open the following URL in a browser:

```bash
http://localhost:5000
```

In order to update the local URL with the latest changes, refresh the browser.

## Deploy to live & preview channels via GitHub pull requests

What this GitHub Action can do:

- Creates a new preview channel (and its associated preview URL) for every pull request
- Adds a comment to the pull request with the preview URL so that each reviewer can view and test the PR's changes in a preview version of the app
- Update the preview URL with changes from each commit by automatically deploying to the associated preview channel. The URL doesn't change with each new commit.
- Deploys the current state of your GitHub repo to the live channel when the pull request is merged

### Setup

If Firebase hosting has already been configured, then set up the GitHub Action part of hosting.

1. Create a new GitHub Actions workflow file in your repo at `.github/workflows/firebase-hosting-pull-request-preview.yml`
2. Copy the following code into the new file:

```yml  
name: "Firebase Hosting Pull Request Preview"
on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]

jobs:
    hosting:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - uses: firebase/github-actions/hosting@v0
            with:
            repoToken: "${{ secrets.GITHUB_TOKEN }}"
            firebaseServiceAccount: "${{ secrets.FIREBASE_SERVICE_ACCOUNT }}"
            channelId: "pull_request_${{ github.event.number }}"
            projectId: "YOUR_FIREBASE_PROJECT_ID"
            previewChannelId: "pull_request_${{ github.event.number }}"
            previewChannelExpireTime: "7d"  
```
3. Create a new secret in your repo named `FIREBASE_SERVICE_ACCOUNT` and paste the contents of your Firebase service account credentials JSON file as the value. You can find this file in your Firebase project settings under the "Service Accounts" tab. 
4. Commit and push the changes to your repo.

### Usage

1. Create a new pull request in your repo.

2. Wait for the GitHub Action to finish running. You can view the progress by clicking on the "Actions" tab in your repo.

3. Once the GitHub Action has finished running, you'll see a new comment on your pull request with the preview URL. Click on the URL to view the preview version of your app.

4. As you make changes to your pull request, the preview URL will automatically update with the latest changes. The URL doesn't change with each new commit.

5. Once you're ready to merge your pull request, click on the "Merge pull request" button. The GitHub Action will automatically deploy the current state of your GitHub repo to the live channel.

## Deploy to live & preview channels via GitHub Actions workflow

What this GitHub Action can do:

- Creates a new preview channel (and its associated preview URL) for every push to the `main` branch

- Adds a comment to the pull request with the preview URL so that each reviewer can view and test the PR's changes in a preview version of the app

- Update the preview URL with changes from each commit by automatically deploying to the associated preview channel. The URL doesn't change with each new commit.

- Deploys the current state of your GitHub repo to the live channel when the pull request is merged

## Staging and Production 

For the staging and production environments, we will use the following Firebase Hosting URLs:

- Staging: https://staging-<project-id>.web.app

- Production: https://<project-id>.web.app

- Use the following command to add the production environment:

```bash
firebase use --add project-prod
``` 

For the production environment, a couple of additional steps are required:

- Upgrade the project to the Blaze plan

- Add a custom domain (optional)

- Set a bugdet alert 

- Turn on Cloud Logging

- Restrict access to the production environment: 
    - Review project members
    - Restrict API keys

- Automate deployment and testing

To make the application know which environment it is running in, we can environment specific initialization files for the local, staging, and production environments:

- `.env.local.js`

- `.env.staging.project-id.js`

- `.env.production.project-id.js`

To integrate these with the deploy process, we add a predeploy hook to the `firebase.json` file:

```json
"predeploy": [
    "npm ci", "npm run build"
]
```

A useful feature of the predeploy hook is that it provides a $GCLOUD_PROJECT environment variable for the current project to all scripts that it runs. This allows us to use the same predeploy hook for local, staging and production environments.

We then add an `env` script to the `package.json` file:

```json
"env": "cp env.${GCLOUD_PROJECT:-local}.js public/env.js"
```

to copy environment specific initialization files based on the $GCLOUD_PROJECT environment variable and fall back to local environment initialization file if no environment specific initialization file exists.

Then we run the new `env` script before the build process that happens every time we run the app.



