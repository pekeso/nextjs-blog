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







