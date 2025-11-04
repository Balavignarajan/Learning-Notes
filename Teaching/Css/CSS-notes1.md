## Learn css
- CSS is the language we use to style an HTML document.
- CSS describes how HTML elements should be displayed.

## What is CSS?
CSS is the language we use to style a Web page :
 - CSS stands for Cascading Style Sheets
 - CSS describes how HTML elements are to be displayed on screen, paper, or in other media
 - CSS saves a lot of work. It can control the layout of multiple web pages all at once
 - External stylesheets are stored in CSS files.

## CSS Syntax
Example :
p {
  color: red;
  font-size: 12px;
}
Example Explained
   - p is a selector in CSS (it points to the HTML element you want to style: <p>).
   - color is a property, and red is the property value
    
## All CSS Simple Selectors - (optional) 
| **Selector**         | **Example**  | **Example Description**                             |
| -------------------- | ------------ | --------------------------------------------------- |
| `#id`                | `#firstname` | Selects the element with `id="firstname"`           |
| `.class`             | `.intro`     | Selects all elements with `class="intro"`           |
| `*`                  | `*`          | Selects all elements                                |
| `element`            | `p`          | Selects all `<p>` elements                          |
| `element,element,..` | `div, p`     | Selects all `<div>` elements and all `<p>` elements |

## How to Add CSS
 There are three ways of inserting a style sheet:   
    - Internal CSS
    - External CSS
    - Inline CSS

1. ## Inline CSS
    - 🎯 CSS written directly inside the HTML element.
    - 🧠 Think of it like putting makeup on just one person (one tag).

   💡 Example:
      <h1 style="color: blue; text-align: center;">Hello Bala!</h1>
      <p style="color: green; font-size: 20px;">This is inline CSS.</p>
   
   📍 Here:
       - style attribute = tells browser you’re styling this element.  
       - Inside it, you write CSS properties → property: value;

       ✅ Good for: quick testing
       ❌ Not good for: big projects (hard to maintain) 

2. ## Internal CSS

    - 🎯 CSS written inside the <style> tag in your HTML’s <head> section.
    - 🧠 Think of it like giving rules for all students in one classroom (page).
  
💡 Example:

   <!DOCTYPE html>
<html>
<head>
  <style>
    body {
      background-color: lightyellow;
    }
    h1 {
      color: darkred;
      text-align: center;
    }
    p {
      color: purple;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <h1>Welcome Bala!</h1>
  <p>This is internal CSS styling the whole page.</p>
</body>
</html>

✅ Good for: styling a single HTML file
❌ Not good for: multiple pages (you’d repeat the same CSS)        

3. ## External css     
 
 - 🎯 CSS written in a separate .css file and linked to HTML.
 - 🧠 Think of it like one school rulebook used by all classrooms (pages).

  *Step 1: Create a CSS file*

   File name: style.css

   body {
  background-color: lightblue;
}
h1 {
  color: navy;
  text-align: center;
}
p {
  color: darkgreen;
  font-size: 18px;
}
 
 *Step 2: Link it in your HTML file*

 File name: index.html
 <!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Hello Bala!</h1>
  <p>This page uses external CSS.</p>
</body>
</html>

<link rel="stylesheet" href="style.css">
 - <link> – connects external files to HTML.
 - rel="stylesheet" – tells the browser it’s a CSS file.
 - href="style.css" – specifies the path to the CSS file.

- 📍 The <link> tag connects your CSS file with your HTML page.
✅ Best for:
        - Real-world projects 🌍
        - Reusing same CSS on multiple pages
        - Easier editing and clean structure

**🔎 Summary Table**
| Type     | Example                                  | Best For       | File Needed |
| -------- | ---------------------------------------- | -------------- | ----------- |
| Inline   | `<h1 style="color:red;">`                | One tag        | No          |
| Internal | `<style>h1{color:red;}</style>`          | One HTML page  | No          |
| External | `link rel="stylesheet" href="style.css"` | Large websites | Yes ✅       |
