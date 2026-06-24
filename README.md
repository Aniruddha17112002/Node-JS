# Node-JS


# 1st node js node modules
Following Course
What is a Module?
-----------------

👉 **A module is just a file.**

In Node.js, **every JavaScript file is treated as a module**.

A module is used to:

*   Organize code properly
    
*   Avoid messy and lengthy files
    
*   Reuse code in multiple places
    

This is similar to **components in React**, where each component is written in a separate file.

Why do we use modules?
----------------------

If we write all functions in one file, the code becomes:

*   Hard to read
    
*   Hard to maintain
    
*   Hard to reuse
    

So we separate related logic into different files (modules).

Example: Creating functions
---------------------------

```javascript
   function sum(...nums) {    return nums.reduce((curr, accu) => {      return curr + accu;    });  }  
   function product(...nums) {    return nums.reduce((curr, accu) => {      return curr * accu;    });  }   
   ```

Keeping all these in one file looks ugly.So we place them **systematically** inside modules.

Exporting a single value from a module
--------------------------------------

### sum.js

```javascript
   function sum(...nums) {    return nums.reduce((curr, accu) => curr + accu);  } 
    module.exports = sum;   
```

### product.js

```javascript
   function product(...nums) {    return nums.reduce((curr, accu) => curr * accu);  } 
    module.exports = product;   
   ```

Importing a module using require()
----------------------------------

```javascript
   const sum = require("./sum");  const product = require("./product");   
   ```

Understanding require() clearly
-------------------------------

*   require() is a **function**
    
*   It accepts a **string path**
    
*   It finds and executes the file
    
*   It returns **module.exports**
    

Example:

```javascript
   module.exports = "hello";   
   ```

```javascript
   const data = require("./file");  console.log(data); // "hello"   
   ```

👉 Whatever you assign to module.exports is what require() returns.

Exporting multiple values from one file
---------------------------------------

### math.js

```javascript
   function sum(...nums) {    return nums.reduce((a, b) => a + b);  }  
   function product(...nums) {    return nums.reduce((a, b) => a * b);  }
     module.exports = { sum, product };   
   ```

### Importing using destructuring

```javascript
   const { sum, product } = require("./math");   
```

Why can’t we export multiple values directly?
---------------------------------------------

Because:

*   require() is a function
    
*   A function can return **only one value**
    

So we **wrap multiple values** inside:

*   an object {}
    
*   or an array \[\]
    

Example:

```javascript
   module.exports = [sum, product];   
   ```

Important: module.exports default value
---------------------------------------

Initially:

```javascript
   console.log(module.exports);   
```

Output:

```javascript
   {}   
   ```

Node.js gives every module an **empty object by default**.

Two ways to export multiple values
----------------------------------

### Method 1: Replace the object

  ```javascript
   module.exports = { sum, product };   
   ```

### Method 2: Add properties (preferred for clarity)

```javascript
   module.exports.sum = sum;  module.exports.product = product;
```   

👉 Since module.exports is already an object, we just attach values to it.

Final Summary (One-Line)
------------------------

> A module is a file that exports values using module.exports, and those values are imported using require(), which always returns whatever is assigned to module.exports.                                                                                         


# 2nd Module.exports VS Exports

1️⃣ Core Rule (Most Important)
------------------------------

In **Node.js**, **only module.exports is actually returned by require()**.

exports is **just a reference (shortcut)** to module.exports.

```javascript
   console.log(module.exports === exports); // true   
```

At the beginning, **both point to the same object**.

2️⃣ Understanding with Your address Example
-------------------------------------------

```javascript
   const user = {      name: "Aman Mishra",      age: 88,      address: {          city: "Mumbai",          state: "Maharashtra"      },      hobbies: ["Sleeping", "Coding", "Eating"]  }; 
    let address = user.address;   
   ```

### Why is this true?

```javascript
   console.log(user.address === address); // true   
   ```

Because:

*   address does **not create a copy**
    
*   It stores a **reference to the same object in memory**
    

📌 **Both variables point to the same address object**

3️⃣ Modifying the Object (Reference Behavior)
---------------------------------------------

```javascript
   address.pincode = 40111;  address.country = "India";   
   ```

Now:

```javascript
`   console.log(user.address);   
```

✔ user.address **also changes**

Why?

> Because both address and user.address point to the **same object**

4️⃣ Reassigning the Variable (Reference Breaks)
-----------------------------------------------

```javascript
   address = {      pincode: 5680,      country: "USA"  };  
   console.log(address);  
   console.log(user.address);   
   ```

### Output Conceptually:

*   address → new object
    
*   user.address → old object (unchanged)
    

💡 **Reassignment creates a NEW object**💡 **Original reference is NOT affected**

5️⃣ Same Concept Applies to exports
-----------------------------------

Internally, Node.js does something like this:

```javascript
  let exports = module.exports;   
```

So initially:

```javascript   module.exports === exports // true   
```


6️⃣ This Works ✅ (Mutating the Object)
--------------------------------------

```javascript
   const product = { name: "Laptop", price: 50000 };  let send = module.exports;  send.product = product;   
```

Why does this work?

*   You are **modifying the same object**
    
*   module.exports still points to it
    

7️⃣ Where the Problem Comes ❌ (Reassignment)
--------------------------------------------

❌ **THIS IS WRONG**

```javascript
   exports = { sum, products };   
   ```

### Why this breaks?

Because:

*   You are **reassigning exports**
    
*   module.exports is still pointing to {}
    

📌 Now:

```javascript
   exports !== module.exports   
   ```

📌 require() only reads **module.exports**📌 So it gets an **empty object {}**

8️⃣ Correct Ways to Export
--------------------------

### ✅ Best & Safest (Recommended)

```javascript
   module.exports = { sum, products };   
   ```

### ✅ Also Safe (Property-wise)

```javascript
  exports.sum = sum;  exports.products = products;   
```


### ❌ Never Do This

```javascript
   exports = { sum, products }; // ❌ breaks reference   
   ```

9️⃣ One-Line Summary (Exam / Interview Ready)
---------------------------------------------

> exports is just a reference to module.exports.Mutating works, reassignment breaks.require() only returns module.exports.  



