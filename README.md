# OpenAI API Quickstart - Node.js example app

## Introduction

You can find the tutorial for this example app [here](https://beta.openai.com/docs/quickstart/build-your-application).

This is an example pet name generator app used in the OpenAI API [quickstart tutorial](https://beta.openai.com/docs/quickstart). It uses the [Next.js](https://nextjs.org/) framework with [React](https://reactjs.org/). Check out the tutorial or follow the instructions below to get set up.

## Generating an API key

Go to the user on the upper right corner and click on the API Keys tab. 
![](docs/images/menu-1.png)

Choose View API Keys.

![](docs/images/generate-api-key.png)

 Click on the "Create new secret Key" button and copy the key.

## Setup

1. If you don’t have Node.js installed, [install it from here](https://nodejs.org/en/)

2. Clone this repository

3. Navigate into the project directory

   ```bash
   $ cd openai-quickstart-node
   ```

4. Install the requirements

   ```bash
   $ npm install
   ```

   Which installs the dependencies listed in `package.json`:

   ```json
   ✗ jq -c '.dependencies | keys' package.json
   ["next","openai","react","react-dom"]
   ```

5. Make a copy of the example environment variables file

   ```bash
   $ cp .env.example .env
   ```

6. Add your [API key](https://beta.openai.com/account/api-keys) to the newly created `.env` file

7. Run the app

   ```bash
   $ npm run dev
   ```

   The console shows:

   ``` 
   ➜  openai-quickstart-node git:(master) ✗ npm run dev

   > openai-quickstart-node@0.1.0 dev
   > next dev

   ready - started server on 0.0.0.0:3000, url: http://localhost:3000
   info  - Loaded env from /Users/casianorodriguezleon/campus-virtual/2223/learning/openai-learning/openai-quickstart-node/.env
   wait  - compiling...
   event - compiled client and server successfully in 1174 ms (113 modules)
   ```

8. You should now be able to access the app at [http://localhost:3000](http://localhost:3000)! 

   ![](docs/images/local-app-runnning.png)


For the full context behind this example app, check out the [tutorial](https://beta.openai.com/docs/quickstart).

## pages/api/generate.js

```js
import { Configuration, OpenAIApi } from "openai";

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);

export default async function (req, res) {
  const completion = await openai.createCompletion({
    model: "text-davinci-002",
    prompt: generatePrompt(req.body.animal),
    temperature: 0.6,
  });
  res.status(200).json({ result: completion.data.choices[0].text });
}

function generatePrompt(animal) {
  const capitalizedAnimal =
    animal[0].toUpperCase() + animal.slice(1).toLowerCase();
  return `Suggest three names for an animal that is a superhero.

Animal: Cat
Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
Animal: Dog
Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
Animal: ${capitalizedAnimal}
Names:`;
}
```