//basic mongodb

//How to use $updateOne AND updateMany operator in MongoDB


//dataset
_id:1, 
'name': "Manasa", 
age: 25, 
grades: [ 
{ grade: "Maths", score: 80 }, 
{ grade: "Science", score: 85 } 
] 
_id: 2, 
'name': "Chandrasekar", 
age: 35, 
grades:  [ 
{ grade: "Maths", score: 95 }, 
{ grade: "Science", score: 88 } 
] 
_id: 3, 
'name': "Deepak", 
age: 30, 
grades: [ 
{ grade: "Maths", score: 85 }, 
{ grade: "Science", score: 90 } 
] 

// create a database
use('BASIC');

//result
switched to db BASIC

// Insert a few data into the score collection.
db.scorce.insertMany([
{'_id': 1,'name':'Deepak','age': 30,'grades':[{'grade':'Maths','score': 85 },{'grade':'Science','score': 90 }]}, 
{'_id': 2,'name':'Chandrasekar','age': 35,'grades':[{'grade':'Maths','score': 95 },{'grade':'Science','score': 88}]},
 {'_id': 3,'name':'Manasa','age': 25,'grades':[{'grade':'Maths','score': 80 },{'grade':'Science','score': 85}]}
]);

//result

{
 "acknowledged": true,
"insertedIds": {
   "0": 1,
   "1": 2,
    "2": 3
  }
}

//find operation

db.score.find()

//result

[
  {
    "_id": 1,
   "name": "Deepak",
   "age": 30,
    "grades": [
      {
        "grade": "Maths",
        "score": 85
      },
      {
       "grade": "Science",
        "score": 90
      }
    ]
  },
  {
    "_id": 2,
    "name": "Chandrasekar",
    "age": 35,
    "grades": [
      {
        "grade": "Maths",
        "score": 95
      },
      {
        "grade": "Science",
        "score": 88
      }
   ]
  },
  {
    "_id": 3,
    "name": "Manasa",
    "age": 25,
    "grades": [
      {
        "grade": "Maths",
        "score": 80
      },
      {
        "grade": "Science",
        "score": 85
      }
    ]
  }
]

//98) Query To Update Age =26 Whoose Name Is  Manasa

db.score.updateOne(
  { name: "Manasa" },
  { $set: { age: 36 } }
)

//result

{
 "acknowledged": true,
  "insertedId": null,
  "matchedCount": 1,
  "modifiedCount": 1,
  "upsertedCount": 0
}

//verifying the result

db.score.find({_id:3})

//output

[
 {
    "_id": 3,
    "name": "Manasa",
    "age": 30,
    "grades": [
      {
        "grade": "Maths",
        "score": 80
      },
      {
        "grade": "Science",
        "score": 85
      }
    ]
  }
]

//99) Query To  Increment Age By 5  Whose Name Is  Chandrasekar 

db.score.updateOne(
  { name: "Chandrasekar" },
  { $inc: { age: 6 } }
)

//result

{
  "acknowledged": true,
  "insertedId": null,
  "matchedCount": 1,
  "modifiedCount": 1,
  "upsertedCount": 0
}

//verifying the result

db.score.find({_id:2})

//output

[
  {
    "_id": 2,
    "name": "Chandrasekar",
    "age": 40,
    "grades": [
      {
        "grade": "Maths",
        "score": 95
      },
      {
        "grade": "Science",
        "score": 88
      }
    ]
  }
]

//100) Demonstrate Example of Upsert in MongoDB

db.score.updateOne(
  { name: "Anjali" },
  { $set: { age: 25, grades: [] } },
  { upsert: true }
)

//output

{
  "acknowledged": true,
  "insertedId": {
    "$oid": "6837117411443bc75ec3564a"
  },
  "matchedCount": 0,
  "modifiedCount": 0,
  "upsertedCount": 1
}

//verifing output

db.score.find({age:22})

//output

[
 {
    "_id": {
      "$oid": "6837117411443bc75ec3564a"
    },
    "name": "Anjali",
    "age": 22,
    "grades": []
  }
]

//101) Query Update Maths Score to 95 Using $[Elem] And Array Filters Of Student Name Deepak 

db.score.updateOne(
  { name: "Deepak" },
  { $set: { "grades.$[elem].score": 99 } },
  { arrayFilters: [ { "elem.grade": "Maths" } ] }
)

//output

{
  "acknowledged": true,
  "insertedId": null,
  "matchedCount": 1,
  "modifiedCount": 1,
  "upsertedCount": 0
}

//verifing output

db.score.find({ name:'Deepak'})

//output

[
  {
    "_id": 1,
    "name": "Deepak",
    "age": 30,
    "grades": [
      {
        "grade": "Maths",
        "score": 100
     },
      {
        "grade": "Science",
        "score": 90
      }
    ]
  }
]