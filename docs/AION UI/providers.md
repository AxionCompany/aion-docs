---
sidebar_position: 6
---

# Providers
In the AI-ON UI library, providers are an important concept. Providers allow you to integrate different chatbot platforms and services into the chatbot widget. With providers, you can easily switch between different chatbot backends without changing your UI code.

Let's learn more about them:

## Methods
Providers are defined as functions that take a configuration object as input and return an async function that defines method. There are methods available in the provider:
- **onSendMessage**: this method is called when the user sends a message to the chatbot. It takes an object with the message and conversation as input and returns a response. Is also takes a stream callback as a second argument, which can be used to stream responses to the UI.
- **onCreateConversatio**n: this method is called when the user creates a new conversation. It takes an object with the conversation as input and returns a response.
- **onDeleteConversation**: this method is called when the user deletes a conversation. It takes an object with the conversation as input and returns a response.
- **onInit**: this method is called when the widget is initialized. It takes a setters object as input and returns a response.

## Example Providers

### OpenAI Provider
The OpenAI Provider is a built-in provider that enables integration with the OpenAI GPT-3 API, via the `onSendMessage` Method. With the OpenAI Provider, you can leverage the power of the GPT-3 model to generate chatbot responses. To use the OpenAI Provider, you need to provide your OpenAI API key, set the maximum length of the response, specify the model to use (such as 'gpt-3.5-turbo'), and enable streaming if desired.
To use a provider, you need to include it in the Providers object when initializing the widget. For example, if you want to use the OpenAiProvider, you can include it like this:

```js
import renderer, { Providers } from '@ai-on/ui';

const initialProps = {
    ...Providers['OpenAiProvider']({
        apiKey: 'your-open-ai-key-here',
        maxLength: 12000,
        model: 'gpt-3.5-turbo',
        stream: true
    }),
    // other props
};

renderer(initialProps);
```

In the above example, we pass the necessary configurations for the OpenAiProvider as an object to the provider's function in the Providers object. Make sure to replace `your-open-ai-key-here` with your actual OpenAI API key.

By including a provider, the widget will handle the communication with the chatbot service for you, including sending and receiving messages.

- p.s.: please, note that it is not recommended to use the OpenAI Provider as is in a production setting, as your API keys would be visible through the browser. Instead, you should use the OpenAI Provider as a starting point for creating your own custom provider that handles the API calls on the server-side.


## Creating Custom Providers
In addition to the built-in providers, you can also create your own custom providers. This allows you to integrate any chatbot platform or service with the AI-ON UI library. To create a custom provider, you need to define a function that takes a configuration object as input and returns an async function that handles sending messages and receiving responses.
```js
const customProvider = (config) =>{
    // You can access the configuration object here or inside any of the props.
    const onSendMessage = (config) => async ({ message, conversation }, stream) => {
        // Implement your logic when user sends a message here
    }
    const onCreateConversation = (config) =>({ conversation }) => {
        // Implement your logic for when the user creates a new conversation
    }
    const onDeleteConversation = (config) =>({ conversation }) => {
        // Implement your logic for when the user deletes a conversation
    }
    const onInit = (config) => (setters) => {
        // Implement your logic for when the widget is initialized
    }
    return {
        onSendMessage(config),
        onCreateConversation(config),
        onDeleteConversation(config),
        onInit(config)
    }
}
```

Let's dive into some practical examples to see how this works.


### onSendMessage

#### Communication with an API
For simulating an API call, we're going to be using an Echo API, that is, an API that simply returns the same body that was sent to it in a POST request. This is useful for testing purposes, as it allows us to simulate the behavior of a chatbot without having to implement a full chatbot backend.

----

##### Creating a simple server for testing purposes
Here's a simple simple implementation of the Echo API in Deno:
```js
Deno.serve((req: Request) => new Response(req.body));
```

You can test this API by issuing the following command in your terminal:

```bash
curl -X POST https://echo-provider.deno.dev -d '{"message":"Hello, World!"}'
```

This will return the following response:
```json
{"message":"Hello, World!"}
```

 - *You can also [fork it in Deno Deploy playground](https://dash.deno.com/playground/echo-provider)*;

----

Now, Create a file called `customOnSendMessageProvider.js` and add the following code to it:
```js
const customOnSendMessageProvider = (config) => async ({ message, conversation }, stream) => {

    const response = await fetch('https://echo-provider.deno.dev', {
        method: 'POST',
        body: JSON.stringify({
        message,
        conversation
        })
    }).then(res => res.json())
  
    return response.message
  
}

export default customOnSendMessageProvider;
```

Then you can use it like:
```js
import renderer, { Providers } from '@ai-on/ui';
import customOnSendMessageProvider from './customOnSendMessageProvider';

const initialProps = {
    onSendMessage:customOnSendMessageProvider(),
    //... other props
}

renderer(initialProps);
```

Note that, in this case, we're not using the `stream` callback from the `onSendMessage` prop, so we're not passing a stream of responses to the UI. Instead, we're simply returning the response from the API call. That means that the UI will show the entire response at once (and once it is completed loading), instead of showing it as it is generated.

Instead of creating a Provider, you could simply add the following code to you Initial Props:

```js
const initialProps = {
    onSendMessage: async ({ message, conversation }, stream) => {
        const response = await fetch('https://echo-provider.deno.dev', {
            method: 'POST',
            body: JSON.stringify({
            message,
            conversation
            })
        }).then(res => res.json())
      
        return response.message
    },
    //... other props
}
```

#### Using the `stream` callback
The Lorem Ipsum Provider is another built-in provider that is provided as an example implementation. It generates random lorem ipsum text as responses. 

While it is not a practical provider for real-world use, it serves as a starting point for understanding how to create your own custom providers. With it, we can simulate a stream of responses by using the `stream` callback.

Here's an example implementation of the Lorem Ipsum `onSendMessage` Provider:
```js
const loremIpsumProvider = (config) => async ({ message, conversation }, stream) => {
    const response = '## Title. \n - lorem ipsum dolor sit amet consectetur adipiscing elit sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. \n - lorem ipsum dolor sit amet consectetur adipiscing elit sed do eiusmod tempor incididunt ut labore et dolore magna aliqua';

    // simulate stream response
    let i = 0;
    const delta = 1;
    // stream response
    if (config.stream) {
        const interval = setInterval(() => {
            let part = response.slice(i, i + delta)
            i++
            stream(part)
            if (i >= response.length) {
                clearInterval(interval)
            }
        }, 20);
    } else {
        // return initial response
        return response
    }
    return
}
```

You can use the `stream` callback to pass a stream of responses to the UI. 
 
Note that in the funciton above, we're simulating a stream of responses by using the `setInterval` function for sending an incremental part of the `response` every 20 milliseconds. This is just for demonstration purposes of the `stream` callback.

This is useful for sending a feedback to the user as soon as possible, as some models might take some time to return the complete response. 

For example, if you are using the OpenAI Provider to generate responses, you can use the `stream` callback to pass the responses as they are generated. This ensures that the UI remains responsive and does not freeze while waiting for the response to be generated.

You can create your own custom `onSendMessage` providers by following a similar pattern. Simply define a function that takes a configuration object as input and returns an async function that handles sending messages and receiving responses. This allows you to integrate your own chatbot platforms or services with the AI-ON UI library.

### onCreateConversation

Comming soon...
### onDeleteConversation

Comming soon...

### onInit

Comming soon...







