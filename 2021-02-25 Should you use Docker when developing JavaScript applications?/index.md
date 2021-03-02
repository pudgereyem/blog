---
title: Should you use Docker when developing JavaScript applications?
date: 2021-02-25
---

![Illustration](drawing.png)

**Short answer; I don't think so.**
When developing a JavaScript application you most likely use a bunch of libraries (through `npm` or `yarn`) that will sit in the root of the project under `/node_modules`. Now, if you use Docker it'll live inside the container and won't be accessible to you, your filesystem or code editor. This causes pain, e.g:

- When installing new libraries you have to connect to the Docker container
- You have to copy `node_modules` locally to make your code editor happy
- You can't use tooling such as [`yarn link`](https://classic.yarnpkg.com/en/docs/cli/link/) or [`yalc`](https://github.com/wclr/yalc) to use local packages

Docker is great for many things, and I'm a big fan.  
However, for **active development of JavaScript applications**, not so much.

### 🙋‍♂️ But, but..

> "Docker makes it much simpler for other people who are not necessarily JavaScript developers to spin up the project."

Yep, agreed. For them we can have a simple script that starts a Docker container and running the project. But not for active development.

> "Ehrm, you can solve this with Docker!"

Ok, then you are just simply much smarter than me. I've tried countless times and I'm never completely happy. Share your solution!
