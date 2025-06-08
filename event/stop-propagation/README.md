# stopPropagation() method

## 🔹 stopPropagation() — stops bubbling/capturing

#### ✅ What it does:

- Prevents the event from bubbling up (or capturing down) the DOM tree.
- BUT: it does not stop other event listeners on the same element.

### 📘 Example:

```html
<div id="parent">
  <button id="child">Click</button>
</div>
```

```javascript
<script>
  document.getElementById("parent").addEventListener("click", () => {
    console.log("Parent clicked!");
  });

  document.getElementById("child").addEventListener("click", (event) => {
    event.stopPropagation(); // prevents bubbling
    console.log("Child clicked!");
  });
</script>
```

##### 🧠 Click the button:

Output:

```
Child clicked!
```

Without stopPropagation(), it would also log:

```
Parent clicked!
```

> So stopPropagation() stops the event from reaching the parent, but other handlers on the button still run.

## 🔸 stopImmediatePropagation() — stops everything immediately

#### ✅ What it does:

- Prevents the event from bubbling/capturing (like stopPropagation()).

- ALSO stops other listeners on the same element from running.

### 📘 Example:

```html
<button id="myButton">Click</button>
```

```javascript
<script>
  const btn = document.getElementById("myButton");

  btn.addEventListener("click", () => {
    console.log("Handler 1");
  });

  btn.addEventListener("click", (e) => {
    e.stopImmediatePropagation(); // stops ALL further listeners on this element
    console.log("Handler 2");
  });

  btn.addEventListener("click", () => {
    console.log("Handler 3");
  });
</script>
```

##### 🧠 Click the button:

> Output:

```
Handler 1
Handler 2
```

##### ⚠️ Even though there are 3 listeners, only Handler 2 runs.

###### If we used stopPropagation() instead:

```javascript
btn.addEventListener("click", (e) => {
  e.stopPropagation();
  console.log("Handler 2");
});
```

> Output:

```javascript
Handler 1
Handler 2
Handler 3
```

> Because stopPropagation() only affects bubbling, not sibling handlers.

## ⚠️ preventDefault() — stops browser's default behavior

> This is completely different from propagation. It stops what the browser would normally do.

### 📘 Example:

```html
<a href="https://example.com" id="link">Go to Example</a>
```

```javascript
<script>
  document.getElementById("link").addEventListener("click", (e) => {
    e.preventDefault(); // stops the browser from navigating
    console.log("Link clicked, but not navigating");
  });
</script>

```

##### 🧠 Click the link:

> - Normally, you'd go to "example.com".

> - With preventDefault(), that navigation doesn't happen.

## ✅ When to Use stopImmediatePropagation()

> Use this when:

- You want to ensure only ONE handler runs on a given element.

Y- ou have multiple handlers and want to cancel others after a condition.

### 📘 Real use case:

```javascript
<button id="submitBtn">Submit</button>

<script>
  const btn = document.getElementById("submitBtn");

  btn.addEventListener("click", (e) => {
    if (formIsInvalid()) {
      e.stopImmediatePropagation();
      alert("Form is invalid. Stop all actions.");
    }
  });

  btn.addEventListener("click", () => {
    console.log("Sending form to server...");
  });

  function formIsInvalid() {
    return true; // Simulate validation failure
  }
</script>

```

> 🧠 What happens?

> Only the validation alert shows. The second listener does not run, because we used stopImmediatePropagation().

## 🧩 Summary Table

> | Method                       | Prevent Bubbling | Stop Same-Element Handlers | Prevent Default Action |
> | ---------------------------- | ---------------- | -------------------------- | ---------------------- |
> | `stopPropagation()`          | ✅ Yes           | ❌ No                      | ❌ No                  |
> | `stopImmediatePropagation()` | ✅ Yes           | ✅ Yes                     | ❌ No                  |
> | `preventDefault()`           | ❌ No            | ❌ No                      | ✅ Yes                 |
