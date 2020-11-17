---
date: 2013-11-25
title: Keyboard Setup Assistant
---

All of a sudden my _external keyboard_ got the wrong keyboard layout, and keys I heavily use got remapped because of this.

My first guess was that I could change this by opening up ´System Preferences -> Keyboard´ and find a button saying **"Change Keyboard Layout"**, which should be there, **but if its not** here is a work around:

1. Go to `/Library/Preferences`
2. Delete `com.apple.keyboardtype.plist`
3. Restart your computer
4. Voilá, the Keyboard Setup Assistant should show up.

> _(I later found another dude having the same problem; <https://discussions.apple.com/thread/2229413>)_
