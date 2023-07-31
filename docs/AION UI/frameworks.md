---
sidebar_position: 7
---

# Using with Next.js

If  you are using **Next.js** (or any other server-side render framework), you will need to use `dynamic imports` to import the library, because this library uses the `window` object, which are not available on the server-side.

Here's an example implementation of a Wrapper Component for `@ai-on/ui` for Next.js:

```jsx
import React from 'react'

export default function Snippet({ children, ...props }) {

    React.useEffect(() => {
        import('@ai-on/ui').then(module => {
            const renderer = module.default;

            renderer({
                onSendMessage:() =>{},
                onCreateConversation: () =>{},
                onDeleteConversation: () =>{},
                onInit: () =>{},
                placeholder: "Type a message...",
                regenerateLimitText: "",
                botTypingCaption: "Typing...",
                allowConversations: typing.allowConversations || true,
                hideHeader: props.hideHeader || false,
                conversations: props.conversations || [],
                bots: props.bots || [],
                ...props
            })
        })
        return () => {
            const aionuiEl = document.getElementById('aion-ui');
            aionuiEl && (aionuiEl.innerHTML = "");
        }
    }, [])

    return (
        <div
            id="aion-ui"
            style={{ width: (props.width || "100%"), height: (props.height || '100%') }}
        >
        </div>
    )
}
```
