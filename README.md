# Pragmatic tips for getting the most out of GitHub Copilot

This repository contains a demo that you can follow to learn about how to get the most out of GitHub Copilot. This demo is for experienced devs who are looking to understand how Copilot is beneficial to their current flow.

This workshop is intentionally empty. You will build up the project by following the instructions here to create a simple Todo app with a lot of help from GitHub Copilot. This project uses Node and TypeScript.

 ## "Simple" is magic

 1. Open the `index.ts` file and ask copilot to create an express server with the following comment

    ```typescript
    // create an express server
    ```

    Accept GitHub Copilot's suggestions line by line. Notice that you start to get code that includes `bodyparser` and `cors`. These are packages we may or may not need, but it's premature to add them. GitHub Copilot isn't wrong here, but this kind of "cart before the horse" code is unhelpful. We can get GitHub Copilot to give us just what we need by using the "simple" keyword.
    
1. Delete all the code in the `index.ts` file

1. Add the following comment to the top

    ```typescript
    // create a simple express server
    ```

    This time you get a very simple Express server complete with the proper `require`. However when using TypeScript we usually use the es6 `import` statement because it's a more modern and cleaner syntax. TypeScript can transpile this to CommonJS at build time. 

1. Put the cursor on the `require` statement and press Cmd/Ctrl + . and let VS Code refactor this to an `import`. It's important to leverage VS Code's refactoring for Copilot's code as well as your own.

Now we need to run this application. We need to know a thing or two about TypeScript here - specifically that we need a `tsconfig.json` file. Use the terminal to create one.

1. Use the terminal to initialize a new `tsconfig.json` file.

    ```bash
    tsc --init
    ```

Let's create a script to run this application

1. Open the `package.json` file and add a `start` command. Let Copilot complete the command.

    ```json
    // watch the files for changes and restart the application with index.ts
    ```

1. Make sure the script that it generates is called "dev". If not, change it to "dev".

    This generates a script that uses `nodemon` to watch the files for changes and `ts-node` to start the application. However, it's not necessary to specify `ts-node` when using `nodemon`.

1. Remove everything so that the line reads `nodemon index.ts`. 

1. Run the application in the terminal with `npm run dev`.

Now we'll return a static file instead of just text.

1. Create a folder called "public" and add a HTML file to it called "index.html".

1. Add the following code to the HTML file

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Todo</title>
    </head>

    <body>
        <h1>It's a Todo App!</h1>
    </body>

    </html>
    ```

We need to tell Express to return the static HTML file instead of just returning text.

1. In the `index.ts` file, replace the `app.get('/'...)` code block with the following comment.

    ```typescript
    // return a static HTML file
    ```

    Copilot gets most of this correct, but gets the path to the `index.html` incorrect. This is because Copilot doesn't know about your project structure - so it's just guessing here. Every time we give it a little more information about the project, it remembers this and will get it right in the future. 

1. Replace the original comment and any code that Copilot generated with the following comment.

    ```typescript
    // return the public/index.html file
    ```

    Here we are being specific about what we want and teaching Copilot a bit about the structure of the project. Test the changes out in the browser to confirm that you can see the new HTML page with the h1.

## Pause - Copilot Anticipates

You can't (and shouldn't) rely on Copilot to do things that the editor is always quite good at - like making simple classes. However, GitHub Copilot can help you by anticipating your next move. A good rule of thumb is to pause when you have some boilerplate code like a constructor to write and see what Copilot gives you. 

1. Create a new folder called "models". 

1. Create a file inside called "Todo.ts". 

1. At the top, use the VS Code completion to create a class called `Todo`.

    ```typescript
    class Todo {

    }

    ```
    Notice that When you put your cursor in the class body, Copilot might suggest some properties to you. They might even be close. Don't take any of those.
    
1. Add the following properties...

    ```typescript
    id: string;
    title: string;
    isCompleted: boolean;
    ```

    Notice that as you add the id, if you pause a bit, Copilot will try and complete it and guesses that the type is "number". In this case, it is not. It is a string. When you add the `title` and `isCompleted` properties, it gets the types of those correct. This is a case where Copilot is more useful then intellisense because it's smart enough to infer types.

1. After the `isCompleted` property, press ENTER twice.

    Copilot will suggest a constructor to you. It is anticipating your next move. The constructor is kind of correct, but we need the "id" parameter to be optional. But perhaps we don't remember how to do that in TypeScript. 

1. Add the following comment...

    ```typescript
    // the id is optional
    ```

    Copilot suggests a different constructor this time putting the `id` last - as is required for optional parameters in JavaScript and using the `?` denotation. 

1. Accept the constructor that Copilot generatates. If it suggests and new "uuid()" for the id parameter, delete that so it's just setting the id property...

    ```typescript
    this.id = id;
    ```

    We get an error underlining this because we've said that `id` is optional as a parameter, but the `id` property is not optional.

1. Delete the `id` property line.

    Copilot may recommend the same `id` property. If it does, don't accept but hit enter again. It may suggest a comment that says "the id is optional". It's as if it's testing here to see if that's what you want to do. 

1. Accept the "// id is optional comment". Notice that it now creates the property id as optional.

## Setup the database

Now that we've got the app structure and model, it's time to start building up a simple API. To do that, we'll need a database and a connection. This lab uses a Cosmos DB free tier database called "TodoList" with a collection called "Todos". We'll write code that creates those items automatically if they do not exist. All you will need is a CosmosDB connection string. You can create a new free database here: . We'll use the Azure Tools Extension for VS Code to get the connection string.

// log in to Azure and get the database connection string blah blah blah

## Be specific

Sometimes you know what you need to do in a project at an abstract level, but you aren't sure about the implementation details. For instnce, in the index router we need to route all requests to /api to an "api" route, and any other request should return the "public/index.html" page so we have a proper static site here with an API.

1. Create a new folder called "routes".

1. Create a file in that folder called "api.ts".

1. Add the following comment to the top of the "api.ts" file.

    ```typescript
    a simple express router
    ```

    Take the suggestions from Copilot by pressing ENTER until you get to the `export` statement at the bottom. 

1. In the `./index.ts` file, add a comment above the route that returns the HTML file.
  
    ```typescript
    // any request that goes to the /api route will be handled by the apiRouter
    ```

    This should generate a route that looks something like this...

    ```typescript
    app.use('/api', apiRouter);
    ```

    Test the API router in your browser by 