# Express + TypeScript Boilerplate in vercel ✏️

This repository contains a boilerplate for setting up an Express project with TypeScript, including a structured folder setup with controllers and models. Follow the steps below to get started.


[Visit-Link](https://express-ts-vercel-starter.vercel.app/)

## Table of Contents

1. [Initialize Node Project](#initialize-node-project)
2. [Install And Initalialize Typescript](#install-typescript)
3. [Install Packages](#install-packages)
4. [Folder Structure](#project-folder-structure)
5. [Update tsconfig.json](#update-tsconfigjson)
6. [Write Express Server Code](#write-express-server-code)
7. [Update Scripts in `package.json`](#update-scripts-in-packagejson)
8. [Create Vercel Account](#create-vercel-account)
9. [Add Vercel Configuration](#add-vercel-configuration)
10. [Update pakage.json](#update-packagejson)
11. [Add Project on vercel](#add-project)
12. [Run command before push the code ](#update-packagejson)

---

## 1. Initialize Node Project

```bash
npm init -y
```

## 2. Install And Initalialize Typescript

```bash
npm install --save-dev typescript
npx tsc --init
```

## 3. Install Packages

```bash
npm install dotenv  rimraf express @types/express typescript ts-node nodemon dotenv --save-dev
```

## 4. Folder Structure

```
│
├── /src
│   ├── /controllers
│   │   └── user-controller.ts
│   ├── /models
│   │   └── user.models.ts
│   ├── /routes
│   │   └── user.routes.ts
│   ├── index.ts
│   └── app.ts
│
├── /dist
│
├── /node_modules
│
├── .env
├── .gitignore
├── package.json
├── tsconfig.json
└── vercel.json
```

## 5. Update tsconfig.json

```bash
{
  "compilerOptions": {
      "module": "commonjs",
      "esModuleInterop": true,
      "allowSyntheticDefaultImports": true,
      "target": "es6",
      "noImplicitAny": true,
      "moduleResolution": "node",
      "sourceMap": true,
      "outDir": "dist",
      "baseUrl": ".",
      "paths": {
          "*": ["node_modules/*", "src/types/*"]
      }
  },
  "include": ["./src/**/*"]
  }
```

## 6. Write Express Server Code

- models/user.model.ts

  ```bash
  export interface User {
      id: number;
      name: string;
      email: string;
  }
  ```

- controllers/user.controller.ts

  ```bash
  import { Request, Response } from "express";
  import { User } from "../models/user.model";

  const mockUser: User = {
    id: 1,
    name: "Gourav Mahobe",
    email: "gourav.mahobe@example.com",
  };

  const getUser = (_req: Request, res: Response) => {
    res.json(mockUser);
  };

  export { getUser };
  ```

- routes/user.route.ts

  ```bash
  import express from "express";
  import { getUser } from "../controllers/  user-controller";

  const router = express.Router();

  router.get("/user", getUser);

  export default router;
  ```

- index.ts

  ```bash
  import app from "./app";

  const port = process.env.PORT || 8080;

   app.listen(port, () => {
     console.log(`Server is listening on ${port}`);
   });
  ```

- app.ts

  ```bash
  import express from "express";
  import userRoutes from "./routes/user.routes";

  const app = express();

  //Health check endpoint at root
  app.get("/", (req, res) => {
    res.status(200).send("Server is live");
  });

  app.use(express.json());
  app.use("/api", userRoutes);

  export default app;

  ```

## 7 Update Scripts in `package.json`

- Explaination :

  1. 'nodemon' is a utility that monitors for any changes in your source and automatically restarts your server. Perfect for development.

  2. '-r' option in nodemon (or node itself) stands for require and 'dotenv/config' is a configuration module that loads environment variables from a .env file into process.env.

  3. '--exec' option in [ nodemon ] specifies an executable command that should be run to start your application. ts-node is a TypeScript execution engine that allows you to run TypeScript files directly without the need to pre-compile them to JavaScript. It compiles TypeScript on-the-fly and runs the resulting JavaScript code

  ```bash

   "scripts": {
    "dev": "nodemon -r dotenv/config --exec ts-node src/index.ts",

    }
  ```

## 8. Create Vercel Project

- Explanation:

  Creating a Vercel project allows you to deploy your application quickly and efficiently. Vercel provides a seamless deployment experience, especially for projects using frameworks like Next.js.

## 9. Add Vercel Configuration

- create an vercel.json in root and paste the given code

```bash
{
  "version": 2,
  "builds": [
    {
      "src": "dist/index.js",
      "use": "@vercel/node",
      "config": { "includeFiles": ["dist/**"] }
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "dist/index.js"
    }
  ]
}

```

- # Explanation

  - version
    : Specifies the version of the Vercel configuration. "version": 2 indicates that you're using version 2 of the configuration schema, which is the latest.

- builds: An array of build settings that define how Vercel should build your application.

  - src: Specifies the source file that Vercel should use to start building your application. In this case, it's "dist/index.js", which means your build output is in the dist directory, and the entry point is index.js.

  - use: Specifies the build engine or runtime to use. "@vercel/node" indicates that you're using Vercel's Node.js runtime.

  - config: An optional object that provides additional configuration for the build process.

    - includeFiles: Specifies additional files or directories to include in the build. "dist/\*\*" means that all files and subdirectories within the dist directory should be included in the deployment.

- routes: An array of routing rules that define how incoming requests should be handled.

  - src: Specifies the source path pattern for incoming requests. "/(.\*)" is a regex pattern that matches all paths.
  - dest: Specifies the destination file to handle the matched requests. "dist/index.js" means that all requests should be handled by the index.js file in the dist directory.

- Summary
  - This configuration file tells Vercel to use version 2 of the configuration schema, build your application using the dist/index.js file with the Node.js runtime, include all files in the dist directory, and route all incoming requests to dist/index.js. This setup is typically used for deploying a Node.js application with a custom build output directory.

## 10. Update pakage.json

- update script on the pakage.json

```bash
"scripts": {
    "dev": "nodemon -r dotenv/config --exec ts-node src/index.ts",
    "build": "rimraf dist && tsc",
    "ts.check": "tsc --project tsconfig.json",
    "add-build": "git add dist"
  },

```

- npm run build: Clean the previous build and compile your TypeScript code into JavaScript.

- npm run ts.check: Type-check your TypeScript code to ensure there are no type errors.

- npm run add-build: Stage the build output for committing to your Git repository.


## 11. Add project on your vercel account
## 12. Push code on git 