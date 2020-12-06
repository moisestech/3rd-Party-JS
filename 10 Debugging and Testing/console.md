# JS Console

## Blog

- [Mastering JS Console.log list a Pro](https://medium.com/javascript-in-plain-english/mastering-js-console-log-like-a-pro-1c634e6393f9) by Harsh Makadia, Javascript in Plain English

#### **`console.log() | info() | debug() | warn() | error()`**

- These will directly print the raw string with appropriate color based on the type of event that is provided to them.

- `console.dir()`
- `console.table()`
- `console.group()`
- `console.groupEnd()`
- `console.memory()`
- `console.clear()`
- `console.timeEnd()`
- `console.trace()`
- `console.assert()`
- `console.time()`

### **Use Placeholders**

- There are different placeholders that can be used as listed below

  - **%o** — which takes an object,
  - **%s** — which takes a string, and
  - **%d** — which is for a decimal or integer

  ```js
  console.log(
    "Hey %s, here is your details %o in form of an object",
    "John",
    userDetails
  );
  ```

### **Add CSS to console messages**

- Add CSS to console messages

  ```js
  console.log("%cColor", "color: blue; font-size: x-large");
  ```

- Want to color only a specific word from the log message?
  ```js
  console.log("You just %c Won! a lottry", "background: #222; color: #bada55");
  ```
