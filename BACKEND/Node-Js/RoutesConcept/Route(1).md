const express = require("express);
const app = express();

//Basic route

//Home
app.get("/", (req, res) => {
    res.send("Helle Bala");
});

//About
app.get("/about", (req,res) => {
    res.send("This the about Page");
});

//Contact
app.get("/contact", (req, res) => {
  res.send("Contact Bala at: bala@example.com 📧");
});

**Next step: Dynamic routes (real-world style)**
app.get("/hello/:name", (req, res) => {
  const name = req.params.name; // grab the part after /hello/
  res.send(`Hello ${name}, welcome to Express! 🙌`);
});

//Start the server
app.listen(3000, () => {
 console.log("Server running on http://localhost:3000");
})



1. Can you explain back to me (in your own simple words) the difference between a fixed route and a dynamic route in Express?
         So now you know three building blocks:
         / → fixed route (always same response)
         /about, /contact → multiple fixed routes
         /hello/:name → dynamic route (changes based on input)