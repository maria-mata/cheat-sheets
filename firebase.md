## Firebase Deploy (Front-end)
* In Firebase, create a new project
* In terminal run `firebase init`
  * Choose hosting
  * Choose the project you just created
* Add this code to the `firebase.json` file:
  ```js
  {
    "hosting": {
      "public": "./"
    }
  }
  ```
* Commit, push, then run `firebase deploy`
