## Vue.js Notes
### Project CLI Setup
```bash
# Install Vue globally if needed
npm install -g vue-cli

# Create a new project with webpack template
vue init webpack *my-app-name*
cd *my-app-name*
mkdir test

# Install dependencies and run dev server
npm install
npm run dev
```

### Deploying to Firebase ([Source](https://medium.com/@ShayneOSullivan/deploy-a-vue-js-app-with-firebase-hosting-3fc420cf3998))
In terminal:
```bash
# Install Firebase tools
npm install -g firebase-tools
npm run build
# 👆 this will create a 'dist' folder
firebase init
# choose hosting option
```
Configure `firebase.json` file:
```json
{
  "hosting": {
    "public": "./dist"
  }
}
```
In terminal:
```
firebase deploy
```
