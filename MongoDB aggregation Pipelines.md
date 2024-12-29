
2024-11-27 19:46

Tags: [[MongoDB]] [[MongoDB Aggregation]]


# MongoDB aggregation Pipelines

```js
[
  {
    $match: {
      isActive: true,
    },
  },
  {
    $count: "activeUsers",
  },
]
```



// Average age of all users
```js
[
  {
    $group: {
      _id: null,
      averageAge: {
        $avg: "$age"
      }
    }
  }
]
```


// top 5 most common favorite fruites
```js
[
  {
    $group: {
      _id: "$favoriteFruit",
      count: {
        $sum: 1	// add 1 for each entry
	// $count: {} // we can also do this
      }
    }
  },
  {
    $sort: {
      count: -1
    }
  },
  {
    $limit: 2
  }
]
```

accumulator simply means that it doesn't calculate the value fresh, it just accumulates it and counts on top of previous value



// total number of males and females
```js
[
  {
    $group: {
      _id: "$gender",
      count: {
        $sum: 1
      }
    }
  }
]
```




// which country has the highest number of registered users
```js
[
  {
    $group: {
      _id: "$company.location.country",
      count: {
        $sum: 1
      }
    }
  },
  {
    $sort: {
      count: -1
    }
  }
]
```


// average number of tags per user
$unwind
Deconstructs an array field from the input documents to output a document for each element. Each output document is the input document with the value of the array field replaced by the element.

```js
[
  {
    $unwind: "$tags"
  },
  {
    $group: {
      _id: "$_id",
      numOfTags: {
        $sum: 1
      }
    }
  },
  {
    $group: {
      _id: null,
      avgNumberOfTags: {
        $avg: "$numOfTags"
      }
    }
  }
]
```

// Another way
```js
[
  {
    $addFields: {
      numberOfTags: {
        $size: {$ifNull: ["$tags", []]}
      }
    }
  },
  {
    $group: {
      _id: null,
      avgNumberOfTags: {
        $avg: "$numberOfTags"
      }
    }
  }
]
```




// how many users have 'enim' as one of thier tags
```js
[
  {
    $match: {
      tags: "enim",
    }
  },
  {
    $count: "userWithEnimTag"
  }
]
```



// what are the names and age of users who are inactive and have 'velit' as a tag
```js
[
  {
    $match: {
      isActive: false,
      tags: 'velit'
    }
  },
  {
    $project: {
      name: 1,
      age: 1
    }
  }
]
```


// how many users have a phone number starting with '+1 (940)
```js
[
  {
    $match: {
      "company.phone": /^\+1 \(940\)/
    }
  }
]
```


// who has registered most recently
```js
[
  {
    $sort: {
      registered: -1
    }
  },
  {
    $limit: 4
  },
  {
    $project: {
      name: 1,
      registered: 1,
      favoriteFruit: 1
    }
  }
]
```



// categorize users by their favorite fruite
```js
[
  {
    $group: {
      _id: "$favoriteFruit",
      users: {$push: "$name"}
    }
  }
]
```

// categorizing and sorting arrays result
```js
[
  {
    $group: {
      _id: "$favoriteFruit", 
      users: { $push: "$name" }
    }
  },
  {
    $project: {
      users: {
        $sortArray: {
          input: "$users",
          sortBy: 1
        }
      }
    }
  }
]
```


// how many users have 'ad' as the second tag in their list of tags
```js
[
  {
    $match: {
      "tags.1": "ad"
    }
  },
  {
    $count: "secondTagAd"
  }
]
```


// find users who have both 'enim' and 'id' as their tags
```js
[
  {
    $match: {
      tags: {
        $all: ["enim", "id"]
      }
    }
  }
]
```

// list all companies located in the USA with their corresponding user count
```js
[
  {
    $match: {
      "company.location.country": "USA"
    }
  },
  {
    $group: {
      _id: "$company.title",
      userCount: {
        $sum: 1
      }
    }
  }
]
```




```js
[
  {
    $lookup: {
      from: "authors",
      localField: "author_id",
      foreignField: "_id",
      as: "author_details"
    }
  },
  {
    $project: {
      author_details: {
        // $first: "$author_details"
        $arrayElemAt: ["$author_details", 0]
      }
    }
  }
]
```


## Other
 [`$map`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/map/)
 [`$filter`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/filter/)
 [`$sum`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sum/)
 [`$indexOfArray`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/indexOfArray/#-indexofarray--aggregation-)
 [`$cond`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/cond/)
 [`$expr`](https://www.mongodb.com/docs/manual/reference/operator/query/expr/)
 `$size` `$multiply` `$divide` `$year` `$month`
# References
[MongoDB aggregation Pipelines](https://www.youtube.com/playlist?list=PLRAV69dS1uWQ6CZCehxKy0rjkqhQ2Z88t)