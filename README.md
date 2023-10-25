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





