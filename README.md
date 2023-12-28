# OpenRouter OAuth PKCE Integration with Next.js

### Overview

This integration is designed to enable developers to quickly integrate [OpenRouter's OAuth PKCE](https://openrouter.ai/docs#oauth) authentication and access a wide range of AI models provided by OpenRouter. It's built using [Next.js](https://nextjs.org/), a popular React framework, and provides a streamlined way to authenticate users and retrieve responses from OpenRouter's AI APIs. 

> [!NOTE]
> Here we are storing API Keys returned to us in local storage just for testing and educational purposes. For production use cases it is recommended to integrate into a seperate backend to
> store these keys and not have them exposed!

## Features

- **OAuth PKCE Authentication**: Securely authenticate users and obtain access tokens using OpenRouter's OAuth PKCE flow.
- **Model Access**: Access tons of AI models provided by of OpenRouter to generate AI-driven responses.
- **User-friendly UI**: A simple interface for users to log in and interact with OpenRouter AI models.
- **Customizable Responses**: Customize the AI responses based on user input and selected models.

## Prerequisites

Before you begin, ensure you have the following:
- [Node.js](https://nodejs.org/en) and [npm](https://www.npmjs.com/package/npm) installed on your machine.
- An understanding of [Next.js](https://nextjs.org/) and [Tailwind CSS](https://tailwindcss.com/).
- An account with [OpenRouter](https://openrouter.ai/).

## Getting Started

### Clone the Repository

Start by cloning this repository to your local machine.

```bash
git clone <repository-url>
```

### Install Dependencies

Navigate to the cloned directory and install the necessary packages.

```bash
cd <project-directory>
npm install
```

### Running the Application

Start the development server to run your application.

```bash
npm run dev
```

## Usage
### OAuth Authentication (oAUTH code)
This function handles the POST request to exchange the authorization code for an access token:

```javascript
// pages/api/oauth.js
const { NextResponse } = require("next/server");

export async function POST(req) {
    // ...code to handle OAuth token exchange...

    const response = await fetch("https://openrouter.ai/api/v1/auth/keys", {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            code: code,
            // code_verifier: jsonBody.code_verifier // Assuming this is also part of the request
          })
        });

    // ...code to handle response...

}
```

How it Works:
- The user is redirected to the OpenRouter login page.
- Once authenticated, they are redirected back to your app with an authorization code.
- The code is then exchanged for an access token using the /api/v1/auth/keys endpoint.

### AI Model Completion (completion code)
This function handles the POST request to the AI model completions endpoint:

```javascript
// pages/api/completions.js
const { NextResponse } = require("next/server");

export async function POST(req) {
    // ...code to handle completion requests...

    const response = await fetch("https://openrouter.ai/api/v1/chat/completions", {
              method: 'POST',
              headers: {
                "Authorization": `Bearer ${OPENROUTER_API_KEY}`,
                // "HTTP-Referer": `${YOUR_SITE_URL}`, // Optional, for including your app on openrouter.ai rankings.
                // "X-Title": `${YOUR_SITE_NAME}`, // Optional. Shows in rankings on openrouter.ai.
                "Content-Type": "application/json"
              },
              body: JSON.stringify({
                "model": MODEL, // Optional (user controls the default),
                "messages": [
                  {"role": "system", "content": "You are a helpful assistant that only responds in gen-z slang like no cap and that's fire. You will always try to respond in Gen-Z slang when you give a response."},
                  {"role": "user", "content": USER_MESSAGE},
                ]
              })
            });

    // ...code to handle response...
  
}
```
How it Works:
- The function reads the user's message and selected model from the request. You can select any model available by OpenRouter so that you can explore tons of models more rapidly!
- It then makes a POST request to OpenRouter's /api/v1/chat/completions with the necessary parameters.
- The response from OpenRouter's AI is returned to the user.

> [!TIP]
> The system setting for the model response is set to respond in fun Gen-Z slang. You can update it to however you'd like it to respond to you in, it's set like that for now because who doesn't
> like lit responses? No cap! 🔥

## Frontend Interaction
The main application code provides:

- A button to log in using OpenRouter
- An interface for the user to select an AI model.
- A text input for the user to send their message.
- A button to initiate the AI response retrieval.
- A display area for the AI's response.
  
How it Works:

- Log in to your OpenRouter account and get access to all their models.
- Users choose a model and enter their text.
- Upon clicking "Get Response", the app sends the input to the completions API.
- The AI's response is fetched and displayed to the user.

##  What You Can Do
- Customize AI Personality: Modify the system message to change the AI's response style.
- Extend Model Selection: Add more models to the dropdown for varied interactions. Check out how to do this using [OpenRouter's Runner](https://github.com/OpenRouterTeam/openrouter-runner)
- Improve UI/UX: Enhance the user interface and experience based on your needs.
- Integrate More Features: Add additional functionalities like saving user sessions, history, or more advanced AI interactions.

## Support & Contribution
For support, issues, or contributions, please:

- Open an issue on the repository's issue tracker.
- Follow the contribution guidelines outlined in the CONTRIBUTING.md file. (coming soon)

## License
This project is licensed under the [MIT](https://opensource.org/license/mit/) License. Feel free to use, modify, and distribute the code as you see fit.
