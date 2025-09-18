*Step 1: What’s a query string?* 
  Look at this URL: 
     http://localhost:3000/search?product=laptop&price=50000
       
    - ? → starts the query string.
    - product=laptop → key = product, value = laptop.
    - price=50000 → key = price, value = 50000.
       It’s like attaching sticky notes to the request saying:
       “Hey server, I’m looking for a laptop with price 50000.”

*Step 2: Let’s code it*

app.get("/search", (req, res) => {
  const product = req.query.product;
  const price = req.query.price;
  res.send(`You searched for ${product}, price up to ${price}`);
});


Perfect again, Bala ✅ That shows your server is correctly reading query strings and customizing the response.
         -So now, you’ve got three ways users can send info to your backend:
         -Fixed routes → always the same response.
         -Dynamic routes (params) → part of the URL path (like /hello/Bala).
         -Query strings → after the ?, key=value pairs (like ?product=phone&price=20000).       