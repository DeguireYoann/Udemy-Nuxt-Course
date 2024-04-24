---
title: "Vue introduction"
tags: [Vue3]
date: "2023-08-24"
toc: true
---

# Introduction to Vue 3
## Getting Started with Vue 3

```javascript
const { createApp } = Vue;
const app = createApp({
    data() {
        return {
            message: 'Hello Vue 3!'
        }
    }
})
app.mount('#app');
```