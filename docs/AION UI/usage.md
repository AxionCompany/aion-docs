---
sidebar_position: 5
---

# Usage

To use the AI-ON UI library in your project, follow these steps:

## Step 1: Install the Package

You can install the AI-ON UI library using either npm, yarn or CDN. Please, refer to the [Installation](/AION%20UI/instalation) section for more details.

## Step 2: Import the Module
After installing the package, import the necessary modules in your script file:
```jsx
import renderer, { Providers } from '@ai-on/ui';
```

## Step 3: Define Initial Props
Next, define the initial props for the AI-ON UI widget. These props include callback functions for sending messages, creating conversations, and deleting conversations. You can also specify the placeholder text, regenerate limit text, and bot typing caption. Finally, include the list of available bots and conversations.

Here's an example of the initial props:
```js
const initialProps = {
    onSendMessage: async ({ message, conversation }, stream) => {
        // Implement your logic when user sends a message here
    },
    onCreateConversation: ({ conversation }) => {
        // Implement your logic for when user creates a new conversation here
    },
    onDeleteConversation: ({ conversation }) => {
        // Implement your logic for when user deletes a conversation here
    },
    onInit:(setters) => {
        // Implement your logic for when the widget is initialized here
    },
    placeholder: "Type your message here...",
    regenerateLimitText: "It looks like I were unable to answer you message. Please try again with a different question.",
    botTypingCaption: "Typing...",
    allowConversations: true,
    bots: [
        {
            name: "Bot 1",
            description: "Bot 1 description",
            id: 1,
            introMessage: "Hello, my name is bot 1. How can I help you today?",
            suggestions: [
                "What's franch for 'bread'?",
                'How are you doing? (answer as joey from friends)',
                'What is the meaning of life?'
            ]
        },
        {
            name: "Bot 2",
            description: "Bot 2 description",
            id: 2,
            introMessage: "Hello, my name is bot 2. How can I help you today?",
            suggestions: [
                "What's english for 'pão'?",
                'How are you doing? (answer as chewbaca from star wars)',
                'Why does the universe exists?'
            ]
        }
    ],
    conversations: [
        {
            title: "Thread 1",
            description: "Description from thread 1",
            id: 1,
            messages: [
                {
                    id: 1,
                    text: "Hello, my name is bot 1. How can I help you today?",
                    createdAt: new Date("2023-06-20"),
                    author: "chatbot"
                }
            ],
            bot: {
                name: "Bot 1",
                description: "Bot 1 description",
                id: 1,
                introMessage: "Hello, my name is bot 1. How can I help you today?",
                suggestions: [
                    "What's franch for 'bread'?",
                    'How are you doing? (answer as joey from friends)',
                    'What is the meaning of life?'
                ]
            },
            updatedAt: new Date("2023-06-20")
        },
        {
            title: "Thread 2",
            description: "Description from thread 2",
            id: 2,
            messages: [
                {
                    id: 1,
                    text: "Hello, my name is bot 2. How can I help you today?",
                    createdAt: new Date("2023-06-20"),
                    author: "chatbot"
                }
            ],
            bot: {
                name: "Bot 2",
                description: "Bot 2 description",
                id: 2,
                introMessage: "Hello, my name is bot 2. How can I help you today?",
                suggestions: [
                    "What's english for 'pão'?",
                    'How are you doing? (answer as chewbaca from star wars)',
                    'Why does the universe exists?'
                ]
            },
            updatedAt: new Date("2023-06-20")
        }
    ]
};
```
In this example, there are two available bots and two conversations. You can customize them according to your needs.

## Step 4: Render the Widget
Finally, render the AI-ON UI widget in your HTML file:

```html
<body>
    <!-- ... Rest of your HTML code here ... -->

    <!-- This is where the widget will be rendered. Make sure to set the id as 'aion-ui' -->
    <div id="aion-ui" width="100%" height="100%"></div>
</body>
```
Make sure to include the script tag to import the AI-ON UI module before the closing body tag.


## Minimal Implementation
If you want to get started quickly, you can use a provider. Basically, it implements the methods for sending and receiving messages, creating and deleting conversations, and initializing the widget, given by the `onSendMessage`, `onCreateConversation`, `onDeleteConversation`, and `onInit` props, respectively. 

You can use the built-in providers or implement these props directly in the props mentioned before. Alternativelly, you can build your own custom providers, and if you are interested in contributing to this lib, propose a PR with your custom provider in the github repo.

```html
<html>
    <script type="module">

    import renderer, { Providers } from 'https://cdn.jsdelivr.net/npm/@ai-on/ui';
        
        if (!renderer) {
            console.error("renderer not found");
        }

        const initialProps = {
            ...Providers['OpenAiProvider']({
                apiKey: 'your-open-ai-key-here',
                maxLength: 12000,
                model: 'gpt-3.5-turbo',
                stream: true
            }),
            allowConversations: true, // Allow users to create and delete new threads
            bots: [{
                name: 'ChatGpt',
                description: 'ChatGpt',
                instructions: 'Your are a helpful assistant', // Instructions to be passed as "system prompt",
                suggestions: ['What is the meaning of life?', 'What is the best programming language?', 'What is the best programming language?'],
                introMessage: 'Hello, my name is ChatGpt. How can I help you today?'
            }],
            conversations: []
        }

        renderer(initialProps)

    </script>
    <body>
    <div id="aion-ui" width="100%" height="100%" ></div>
    </body>
</html>
```


That's it! You have successfully integrated the AI-ON UI library into your project. You can now customize the widget's appearance, handle user interactions, and implement your own logic for sending and receiving messages.
