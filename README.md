# Setting up a Full Stack Project + Curriculum Review

Hey we're super excited to get started with new fellow projects. As it may have been over a month since you've touched TPEO full-stack development, we decided it would be helpful for y'all to go over a brief review. Additionally, product and design will be busy interviewing and incorporating general feedback into the structure/design of the app you're building, so this is a good time to write out a skeleton app.

Note: you are not required to use the given stack for your project, we will give you free rein for stack choice as long as you can complete the project; after all, most stakeholder projects require some form of flexibility in tech stack and no two TPEO apps are the same. In your **technical spec** however, you will be justifying the choice of stack based on project requirements. However, for the purposes of this exercise, please follow the instructions and use the stack provided.

### Deliverables

- Send your project mentor a link to the completed repository on GitHub and a link to the deployed site on Vercel.

### Resources:

- TPEO Engineering Course Slides + Recording: https://txproduct.notion.site/Full-Stack-Curriculum-Recordings-8ee19dc2e6fd49cc9b16ed3cfddb5422
- TPEO Course Repository: https://github.com/tpeo/full-stack-curriculum-2023

## Contents

We will review the following topics:

1. React + Frontend Review: Setting up
2. Backend + Firebase Interaction
3. Swagger Documentation
4. Authentication
5. Git Review
6. Hosting with Vercel

Each section will have some instructions to help you set up, and a mini-assignment to get you coding.

## Part 1: React Project Structure & Starting a React Project

In this exercise, you will learn how to start a new React project and understand the basic project structure.

### Prerequisites

- Node.js and npm (or yarn) installed on your computer
- Basic knowledge of React

### Starting a React Project

1. Open the terminal and navigate to the directory where you want to create your project.

2. Run the following command to create a new React project using Create React App:

   ```
   npx create-react-app frontend
   ```

3. Once the command has finished running, navigate into the project directory:
   ```
   cd frontend
   ```
4. Run the following command to start the development server:
   ```
   npm run start
   ```
5. Open a browser and navigate to `http://localhost:3000` to see the default React application running.

### Understanding the Project Structure

The project structure created by Create React App is organized as follows:

```
frontend/
├── node_modules/
├── public/
│   ├── index.html
│   ├── manifest.json
│   └── ...
├── src/
│   ├── index.js
│   ├── App.js
│   ├── ...
├── package.json
└── ...

```

`node_modules/`: This directory contains all the npm packages that your project depends on.

`public/`: This directory contains static assets such as the index.html file and the manifest.json file.

`src/`: This directory contains all the source code for your React application. The index.js file is the entry point for your application, and the App.js file is the root component of your application.

`package.json`: This file contains the project's dependencies and scripts.

You can add new components, pages, and styles to the `src/` directory and use them in your application.

### Assignment

Currently, the page is blank. We would like you to create some kind of page containing information that you think your app will need. For example, if you were making an app for west campus apartments, create a form for apartment information. On the same page, we would like you to list the objects in the database and connect it with the backend in a later section.

## Part 2: Backend + Firebase Integration

We will be using express and firebase for this section of the tutorial so please make sure you have a firebase account.

In this exercise, you will learn how to set up a Node/Express application with basic CRUD endpoints, connect it to a Firebase Firestore database, and securely load credentials from environment variables.

### Prerequisites

1. Node.js and npm (or yarn) installed on your computer
2. A Firebase account and a Firebase Firestore project set up

### Setting up the Express server

1. Open the terminal and navigate to the directory `(/backend)` where you want to create your project.

2. Run the following command within that directory to create a new Node project:
   ```
   npm init
   ```
3. Install the Express.js framework by running the following command:
   ```
   npm install express
   ```
4. Create a new file called `server.js` and add the following code to set up the boilerplate Express server:

   ```js
   const express = require("express");
   const app = express();

   app.listen(4000, () => {
     console.log("Server running on port 4000");
   });
   ```

5. Run the following command to start the Express server:
   ```
   node server.js
   ```
6. Open a browser and navigate to `http://localhost:4000` to see the Express server running.

### Connecting to Firebase Firestore

1. Install the Firebase Firestore library by running the following command:
   ```
   npm install firebase-admin
   ```
2. Create a new file called `firebase.js` and add the following code to initialize the Firebase Firestore library:

   ```js
   const admin = require("firebase-admin");

   admin.initializeApp({
     credential: admin.credential.cert(
       require("./path/to/serviceAccountKey.json")
     ),
     databaseURL: "https://your-project-id.firebaseio.com",
   });

   const db = admin.firestore();
   ```

3. Replace `./path/to/serviceAccountKey.json` with the path to your Firebase service account key JSON file and `your-project-id` with your Firebase project ID.

4. Import the `firebase.js` file in your `server.js` file and use the `db` object to perform CRUD operations on your Firebase Firestore database:
   ```js
   const firebase = require("./firebase");
   const db = firebase.firestore;
   ```

### Using Environment Variables

Recall that we need environment variables to hide sensitive information in our repository such as the JWT salt, database configurations,etc.

1. Install the `dotenv` library by running the following command:
   ```
   npm install dotenv
   ```
2. Create a new file called `.env` in the root of your project, and add your Firebase credentials as environment variables:
   ```
   FIREBASE_PROJECT_ID=your-project-id
   FIREBASE_CLIENT_EMAIL=your-client-email
   FIREBASE_PRIVATE_KEY=your-private-key
   ```
3. In your firebase.js file, replace the hardcoded values for the projectId and private_key with the process.env.FIREBASE_PROJECT_ID and process.env.FIREBASE_PRIVATE_KEY.

4. In your `server.js` file, add the following line at the top of the file, to load the environment variables:
   ```js
   require("dotenv").config();
   ```
5. Use the environment variables in your `firebase.js` file to initialize the Firebase Firestore library, instead of hardcoding the credentials and project ID:

   ```js
   const admin = require("firebase-admin");

   admin.initializeApp({
     credential: admin.credential.cert({
       projectId: process.env.FIREBASE_PROJECT_ID,
       clientEmail: process.env.FIREBASE_CLIENT_EMAIL,
       privateKey: process.env.FIREBASE_PRIVATE_KEY,
     }),
     databaseURL: `https://${process.env.FIREBASE_PROJECT_ID}.firebaseio.com`,
   });

   const db = admin.firestore();
   ```

### Assignment: Creating basic CRUD endpoints

You might not know what endpoints or routes you need yet, and that's fine. We can start by creating two simple CRUD endpoints (creating and reading), to test functionality with the frontend and database. An example of a data json could be:

```js
{
    apartment_name: [str],
    address: [str]
}
```

## Part 3: Swagger Documentation

Swagger is an essential tool for documenting your API. It helps in creating clear, interactive documentation that your team and users can reference easily. It is an optional step - as in you could complete the new fellow project without Swagger configured - but it makes documenting and testing API endpoints much, much easier.

#### Step 1: Install Required Packages

First, you need to install `swagger-jsdoc` and `swagger-ui-express`. These packages are essential for creating and serving your Swagger documentation. Run the following command in your terminal:

```bash
npm install swagger-jsdoc swagger-ui-express
```

#### Step 2: Setting Up Swagger in Your Express.js File

After installing the necessary packages, you need to set up Swagger in your Express.js application. Here’s how you can do it:

1. **Require the Packages**: At the top of your server.js file, add the following lines to require the installed packages:

   ```javascript
   const swaggerJsDoc = require("swagger-jsdoc");
   const swaggerUi = require("swagger-ui-express");
   ```

2. **Configure Swagger Options**: Define the Swagger options. This configuration includes basic information about your API like the title, version, and description. You also specify the path to your API documentation (typically your main server file). Add the following code snippet to your file:

   ```javascript
   // Swagger configuration
   const swaggerOptions = {
     definition: {
       openapi: "3.0.0",
       info: {
         title: "TPEO Practice API",
         version: "1.0.0",
         description: "A simple To-Do API. Built by TPEO",
       },
     },
     apis: ["./server.js"], // Path to the API docs
   };
   ```

3. **Initialize and Use Swagger Middleware**: To serve the Swagger UI, initialize Swagger docs using the options defined above and use the Swagger middleware in your Express app. Add the following lines:

   ```javascript
   const swaggerDocs = swaggerJsDoc(swaggerOptions);
   app.use(
     "/api-docs",
     swaggerUi.serve,
     swaggerUi.setup(swaggerDocs, {
       customCssUrl:
         "https://cdnjs.cloudflare.com/ajax/libs/swagger-ui/4.1.0/swagger-ui.min.css",
     })
   );
   ```

#### Step 3: Adding Swagger Documentation for Endpoints

Finally, you need to document your API endpoints using Swagger. This is done using special comment syntax above your route definitions.

Here's an example for a `GET` endpoint at `/tasks`:

```javascript
/**
 * @swagger
 * /tasks:
 *   get:
 *     summary: Retrieve a list of tasks
 *     description: Returns a simple greeting as a placeholder for task list.
 *     responses:
 *       200:
 *         description: A list of tasks (currently a simple greeting).
 *         content:
 *           text/plain:
 *             schema:
 *               type: string
 *               example: Hello
 *       500:
 *         description: Server Error
 */
app.get("/tasks", (req, res) => {
  // Your route logic goes here.
});
```

#### Step 4: Viewing Your Swagger Documentation

After setting up Swagger and documenting your endpoints, you can view your interactive API documentation by navigating to `[http://localhost:PORT/api-docs](http://localhost:PORT/api-docs)` in your web browser (replace `PORT` with the port number your Express app is running on).

## Part 4: Authentication

One of the most common integrations in building a web application is determining the authentication flow as it involves both considerable work on both the frontend and backend. After all, we would like to protect our API from invalid and malicous requests. We will highlight one possible method of authentication which you can use to build out your web app; assumptions are that we are using a email/password authentication model.

### 1. Token authentication with Firebase

Firebase provides us with an authentication service we can embed and in this schema, you will need to have firebase sdks on both frontend and backend side. Additionally, you will have to enable email/password authentication on your firebase project (via the firebase console).

1. Within the frontend section, call
   ```
   npm install firebase
   ```
2. On the backend section, you can add the following protected route middleware:

   ```js
   const express = require("express");
   const admin = require("firebase-admin");
   const app = express();

   admin.initializeApp({
     credential: admin.credential.cert(require("./cred.json")),
     databaseURL: "https://your-project-id.firebaseio.com",
   });

   const auth = (req, res, next) => {
     try {
       const tokenId = req.get("Authorization").split("Bearer ")[1];
       return admin
         .auth()
         .verifyIdToken(tokenId)
         .then((decoded) => {
           req.token = decoded;
           next();
         })
         .catch((err) => res.status(401).send(err));
     } catch (e) {
       res.status(400).send("Errors");
     }
   };
   ```

3. On the frontend, you can sign up and login to your account with the following code

   ```js
   import { initializeApp } from "firebase/app";
   import {
   getAuth,
   createUserWithEmailAndPassword,
   signInWithEmailAndPassword,
   } from "firebase/auth";

   // Replace the following with your app's Firebase project configuration
   const firebaseConfig = {...};

   const app = initializeApp(firebaseConfig);
   const auth = getAuth(app);

   export const createUser = (email, password) => {
   createUserWithEmailAndPassword(auth, email, password)
       .then((userCredential) => {
       // Signed in : get token
       const user = userCredential.user;
       console.log(user);
       })
       .catch((error) => {
       const errorCode = error.code;
       const errorMessage = error.message;
       console.log(errorMessage);
       });
   };

   export const loginUser = (email, password) => {
   signInWithEmailAndPassword(auth, email, password)
       .then((userCredential) => {
       const user = userCredential.user;
       console.log(user.accessToken);
       })
       .catch((error) => {
       const errorCode = error.code;
       const errorMessage = error.message;
       });
   };
   ```

4. You can now save the save the userAccessToken to localstorage and send it along with subsequent requests.

### Assignment: Add a Login to your Application

Implement a login/signup system incorporating both backend and frontend sections. You will protect your apartment API with appropriate middleware and handle React app authentication context. On the frontend, add a login page and route it via React Router. Then, navigate users to the page from the React section once credentials are verified and user is logged in.

## Part 5: Git Review

### Commits

Make small, focused commits with clear messages.

```
# Stage changes
git add .

# Commit changes
git commit -m "Added login page"
```

### Pulling Changes

Fetch changes from the remote repository to stay updated and pull frequently to incorporate changes from the main branch into your feature branch.

```
# Fetch changes from the remote repository
git fetch origin
# Pull changes from the main branch into feature branch
git pull origin main
```

### Pushing Changes

Push changes so your partner can view and pull your changes

```
# Push changes to the remote feature branch
git push origin feature/jane-new-feature
```

### Branching

#### Main Branch

- The `main` branch is often the default branch for production-ready code.

#### Feature Branches

- Create feature branches for developing new features or fixing bugs. `git checkout -b feature/new-feature`
- Create pull requests once a feature is complete for your partner/mentors to review before pushing to the main branch.

### Merge Conflicts

Merge conflicts will likely come up when working on a team with multiple engineers. This typically happens when two branches have changes in the same part of a file.

#### Handling Merge Conflicts

- Fetch and Pull: Before making changes, always fetch or pull the latest changes from the remote repository.

  ```
    # Fetch changes from the remote repository
    git fetch origin

    # Pull changes from a branch
    git pull origin branch-name

  ```

- Resolve Conflicts Locally: When a conflict arises during a pull or merge, Git will mark the conflicted files
  ```
  # Example of a conflicted file in the working directory
    <<<<<<< HEAD
    // Your changes
    =======
    // Changes from the remote branch
    >>>>>>> branch-name
  ```
- Manually Resolve Conflicts:
  - Open the conflicted file in your code editor.
  - Manually edit the file to keep the desired changes, removing conflict markers.
- Add and Commit Changes: After resolving conflicts, stage the modified files and commit the changes.

  ```
  # Stage the resolved files
    git add conflicted-file.txt

    # Commit the changes
    git commit -m "Resolve merge conflict in conflicted-file.txt"
  ```

- Continue the Merge or Pull: After resolving conflicts, continue with the merge or pull operation

  ```
  # Continue the merge (if it was interrupted)
    git merge --continue

    # Or, push the changes to the remote repository
    git push origin branch-name
  ```

## Part 6: Vercel Deployment

As the last part of the assignment, host your application's frontend and backend using Vercel (or another hosting platform of your choice).
You can follow the demo here: https://github.com/VincentDo8439/hosting_demo_react_express

- Make sure it is integrated with GitHub so that each push to the main branch is reflected on your hosted site.
- Also, ensure your env variables are loaded into Vercel as well to avoid errors (especially the firebase creds on the backend)!

Submit the hosted links to your mentors when you are complete :)

That's it for the new fellow project start-up guide! Good luck with the rest of your project, we're super excited to see the final products!!
