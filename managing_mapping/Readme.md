# ElasticSearch

## Notes:
1. An `index` is like a `database` and likewise a `mapping` is like a `table`

## Codes:
```commandline
# Go to `kibana/management/dev_tools

# create an index:
PUT test

# put a document in the index. First document put creates the mapping of table
PUT test/_doc/1
{
  "name": "Pooya",
  "age": 35
}

# Get an specific id
GET test/_doc/1

# get the mapping
GET test/_mapping


# Adding a new document with incompatible types which throws a `400 - bad request` error!
PUT test/_doc/2
{
  "name": 1,
  "age": "old"
}

# delete the index:
DELETE test
```

## Creating a custom mapping:
```commandline
# Create test index
PUT test

# Create a test mapping:
PUT test/_mapping
{
  "properties": {
    "id": {
      "type": "keyword"
    },
    "date": {
      "type": "date"
    },
    "customer_id": {
      "type": "keyword"
    },
    "sent": {
      "type": "boolean"
    },
    "name": {
      "type": "keyword"
    },
    "quantity": {
      "type": "integer"
    },
    "price": {
      "type": "double"
    },
    "vat": {
      "type": "double",
      "index": "false"
    }
  }
}

# Get the mapping:
GET test/_mapping
```

## Creating array fields
```commandline
# Create an index
PUT test

# Create a mapping
# Setting store to true one can have array form inputs
PUT test/_mapping
{
  "properties" : {
    "name" : {"type" : "keyword"},
    "tag" : {"type" : "keyword", "store" : "true"}
  }
}

# Get mapping
Get test/_mapping

# Store a document
Put test/_doc/1
{"name": "document1", "tag": "awesome"}

# Store an array form doc
PUT test/_doc/2
{"name": "document2", "tag": ["cool", "awesome", "amazing"] }

# Get the array form doc
GET test/_doc/2
```

## Creating Objects
```commandline
# Creating an index
PUT test

# Create a mapping for object form inputs
# Objects can have inner embeddings
PUT test/_mapping
{
  "properties": {
    "id": {
      "type": "keyword"
    },
    "date": {
      "type": "date"
    },
    "customer_id": {
      "type": "keyword",
      "store": "yes"
    },
    "sent": {
      "type": "boolean"
    },
    "item": {
      "type": "object",
      "properties": {
        "name": {
          "type": "text"
        },
        "quantity": {
          "type": "integer"
        },
        "price": {
          "type": "double"
        },
        "vat": {
          "type": "double"
        }
      }
    }
  }
}
```

## Creating dynamic types:
```commandline
# Remove index test
DELETE test

# Creating an index
PUT test

# Creating a dynamic mapping
PUT test/_mapping
{
  "dynamic_date_formats": [
    "yyyy-MM-dd",
    "dd-MM-yyyy"
  ],
  "date_detection": true,
  "numeric_detection": true,
  "dynamic_templates": [
    {
      "template1": {
        "match": "*",
        "match_mapping_type": "long",
        "mapping": {
          "type": "{dynamic_type}",
          "store": true
        }
      }
    }
  ]
}

# Getting the mapping
GET test/_mapping

# Adding a doc
PUT test/_doc/1
{
  "dynamic_date_formats": "2022-02-12"
}

# Getting the mapping
GET test/_mapping
# the datetype is fixed

# Get doc
GET test/_doc/1

# Adding another doc
PUT test/_doc/2
{
  "dynamic_date_formats": "12-02-2022"
}
# This will raise an error, because the input is the mapping is fixed by adding the former document!
```

## Creating nested documents:
Nested objects are not searchable with standard queries, only with nested ones. Theyare not shown in standard query results.
```commandline
# Remove index test
DELETE test

# Creating an index
PUT test

# Creating a netsted mapping
PUT test/_mapping
{
  "properties": {
    "id": {
      "type": "keyword"
    },
    "date": {
      "type": "date"
    },
    "customer_id": {
      "type": "keyword"
    },
    "sent": {
      "type": "boolean"
    },
    "item": {
      "type": "nested",
      "properties": {
        "name": {
          "type": "keyword"
        },
        "quantity": {
          "type": "long"
        },
        "price": {
          "type": "double"
        },
        "vat": {
          "type": "double"
        }
      }
    }
  }
}

# Getting the mapping
GET test/_mapping

# Adding a doc
PUT test/_doc/1
{
  "customer_id": 1,
  "date" : "2022-02-11",
  "id" : 1,
  "item": [
    {"name": "pooya", "price" : 10},
    {"name": "mohammadi", "price" : 10}
    ]
}

# Get doc
GET test/_doc/1

```

# Creating GeoPoints
```commandline
# Creating a geo index
PUT geo_index

# Creating a geopoint mapping
PUT geo_index/_mapping
{
  "properties": {
    "id": {
      "type": "keyword"
    },
    "timestamp": {
      "type": "date"
    },
    "text": {
      "type": "text"
    },
    "location": {
      "type": "geo_point"
    }
  }
}


# Getting the mapping
GET geo_index/_mapping


# Adding a doc
PUT geo_index/_doc/1
{
    "timestamp": "2022-02-12",
    "text": "Geo-point as an object",
    "location": {
        "lat": 41.12,
        "lon": -71.34
    }
}

# get a doc
GET geo_index/_doc/1
```

# Creating GeoShape
```commandline
Having a simple example is bit harder than I thought it would be!
I implemented information from https://wsdot.wa.gov/mapsdata/geodatacatalog/default.htm#admin using a node code!
Node code is in here: https://github.com/ereshzealous/node_playground/tree/master/elastic-geo-spatial
```