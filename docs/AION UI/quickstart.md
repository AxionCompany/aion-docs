---
sidebar_position: 2
---

# Quickstart

If you want to get started quickly, you can use the built-in OpenAI provider to render AI-ON UI in any HTML file fairly simply:

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


Just change `your-open-ai-key-here` for your actual API key in OpenAI, and that's it! You have successfully integrated the AI-ON UI library into your project. You can now customize the widget's appearance, handle user interactions, and implement your own logic for sending and receiving messages.
