# MongoDB
```
Name : Vikhram S
Reg No : 212222060295
```
## Aim

To perform complete CRUD (Create, Read, Update, Delete) operations and advanced MongoDB queries on a Products collection, including conditional filtering, array-based searches, logical operators, regular expressions, sorting, skipping, and aggregation functions to manage and analyze product data effectively.

## Algorithm
1. CREATE Operation

Start MongoDB and select the database.

Create a collection named products.

Insert multiple product documents using insertMany().

Insert an additional product using insertOne().

2. READ Operation

Display all documents using find().pretty().

Filter documents based on:

Price conditions ($lt, $gt)

Category not equal ($ne)

Logical conditions using $and and $or

Search documents by tags using array operators:

$in (match any tag)

$all (must contain all tags)

$size (exact array length)

Use REGEX to filter names starting with a specific letter.

Sort documents using sort().

Skip documents using skip().

3. UPDATE Operation

Update fields using $set (e.g., update price).

Increment numeric values using $inc (e.g., increase stock).

Modify array fields:

$push to add a tag

$pull to remove a tag

4. DELETE Operation

Remove a specific product using deleteOne().

5. AGGREGATION Operations

Use $group to calculate:

Total stock across all products


```MongoDB
db.products.insertMany([
  {
    _id: 1,
    name: "Laptop",
    brand: "Dell",
    price: 55000,
    category: "Electronics",
    stock: 30,
    tags: ["computer", "technology"]
  },
  {
    _id: 2,
    name: "Smartphone",
    brand: "Samsung",
    price: 30000,
    category: "Electronics",
    stock: 50,
    tags: ["mobile", "android"]
  },
  {
    _id: 3,
    name: "Headphones",
    brand: "Sony",
    price: 2500,
    category: "Accessories",
    stock: 100,
    tags: ["audio", "music"]
  },
  {
    _id: 4,
    name: "Smartwatch",
    brand: "Apple",
    price: 45000,
    category: "Electronics",
    stock: 20,
    tags: ["wearable", "ios"]
  },
  {
    _id: 5,
    name: "Keyboard",
    brand: "Logitech",
    price: 1200,
    category: "Accessories",
    stock: 80,
    tags: ["computer", "typing"]
  }
])

mydb> db.products.insertOne({
...   _id: 6,
...   name: "Tablet",
...   brand: "Lenovo",
...   price: 20000,
...   category: "Electronics",
...   stock: 40,
...   tags: ["mobile", "touch"]
... })
{ acknowledged: true, insertedId: 6 }

mydb> db.products.find().pretty()

mydb> db.products.find({ price: { $lt: 5000 } })
[
  {
    _id: 3,
    name: 'Headphones',
    brand: 'Sony',
    price: 2500,
    category: 'Accessories',
    stock: 100,
    tags: [ 'audio', 'music' ]
  },
  {
    _id: 5,
    name: 'Keyboard',
    brand: 'Logitech',
    price: 1200,
    category: 'Accessories',
    stock: 80,
    tags: [ 'computer', 'typing' ]
  }
]

mydb> db.products.find({ price: { $gt: 10000 } })
[
  {
    _id: 1,
    name: 'Laptop',
    brand: 'Dell',
    price: 55000,
    category: 'Electronics',
    stock: 30,
    tags: [ 'computer', 'technology' ]
  },
  {
    _id: 2,
    name: 'Smartphone',
    brand: 'Samsung',
    price: 30000,
    category: 'Electronics',
    stock: 50,
    tags: [ 'mobile', 'android' ]
  },
  {
    _id: 4,
    name: 'Smartwatch',
    brand: 'Apple',
    price: 45000,
    category: 'Electronics',
    stock: 20,
    tags: [ 'wearable', 'ios' ]
  },
  {
    _id: 6,
    name: 'Tablet',
    brand: 'Lenovo',
    price: 20000,
    category: 'Electronics',
    stock: 40,
    tags: [ 'mobile', 'touch' ]
  }
]

mydb> db.products.find({ category: { $ne: "Electronics" } })
[
  {
    _id: 3,
    name: 'Headphones',
    brand: 'Sony',
    price: 2500,
    category: 'Accessories',
    stock: 100,
    tags: [ 'audio', 'music' ]
  },
  {
    _id: 5,
    name: 'Keyboard',
    brand: 'Logitech',
    price: 1200,
    category: 'Accessories',
    stock: 80,
    tags: [ 'computer', 'typing' ]
  }
]

mydb> db.products.find({
...   $and: [
...     { category: "Electronics" },
...     { price: { $lt: 50000 } }
...   ]
... })
[
  {
    _id: 2,
    name: 'Smartphone',
    brand: 'Samsung',
    price: 30000,
    category: 'Electronics',
    stock: 50,
    tags: [ 'mobile', 'android' ]
  },
  {
    _id: 4,
    name: 'Smartwatch',
    brand: 'Apple',
    price: 45000,
    category: 'Electronics',
    stock: 20,
    tags: [ 'wearable', 'ios' ]
  },
  {
    _id: 6,
    name: 'Tablet',
    brand: 'Lenovo',
    price: 20000,
    category: 'Electronics',
    stock: 40,
    tags: [ 'mobile', 'touch' ]
  }
]
Accessories OR price > 40000

db.products.find({
  $or: [
    { category: "Accessories" },
    { price: { $gt: 40000 } }
  ]
})


db.products.find({
  tags: { $in: ["computer", "mobile"] }
})

$all
Tags must contain BOTH "audio" AND "music"

db.products.find({ tags: { $all: ["audio", "music"] } })

$size
Tags array size = 2
db.products.find({ tags: { $size: 2 } })

$set – Update price
db.products.updateOne(
  { name: "Laptop" },
  { $set: { price: 52000 } }
)

$inc – Increase stock

mydb> db.products.updateOne(
... { name : "Keyboard"},
... {
... $inc: {stock : 10}} )

$push – Add tag
mydb> db.products.updateOne (
... { name: "Smartwatch"},
... {$push : {tags : "premium"}})

$pull – Remove tag
mydb> db.products.updateOne(
... { name:"Headphones"},
... {$pull:{tags:"music"}}
... )

Delete
mydb> db.products.deleteOne({name:"Keyboard"})

mydb> db.products.find().sort({price:-1})

Skip 2 products
db.products.find().skip(2)
REGEX
Names starting with "S"
mydb> db.products.find({name: /^S/ })

AGGREGATION
Total stock
db.products.aggregate([
  { $group: { _id: null, totalStock: { $sum: "$stock" } } }
])




Count of products per category
db.products.aggregate([
  { $group: { _id: "$category", count: { $sum: 1 } } }

```
])


Count of products in each category

Display the results using aggregate().
