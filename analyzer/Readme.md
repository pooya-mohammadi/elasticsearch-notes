# Persian Analyzer
### rebuilt_persian is the name of the analyzer the default analyzer which will be chosen for upcoming fields is the english default analyzer. 
### Change the rebuild_presian to default and every field will be based on that and not english default one!

```
DELETE persian_example
PUT persian_example
{
  "settings": {
    "analysis": {
      "char_filter": {
        "zero_width_spaces": {
          "type": "mapping",
          "mappings": [
            """\u200C=>\u0020"""
          ]
        }
      },
      "filter": {
        "persian_stop": {
          "type": "stop",
          "stopwords": "_persian_"
        }
      },
      "analyzer": {
        "rebuilt_persian": {
          "tokenizer": "standard",
          "char_filter": [
            "zero_width_spaces"
          ],
          "filter": [
            "lowercase",
            "decimal_digit",
            "arabic_normalization",
            "persian_normalization",
            "persian_stop"
          ]
        }
      }
    }
  }
}
```

### GET Analyzer's output on a random text
```GET persian_example/_analyze
{
  "field": "text",
  "text" : "من و علی با هم در کتابخانه آشنا شدیم"
}
```
### PUT mapping with the created analyzer
```PUT persian_example/_mapping
{
  "properties": {
    "text": {
      "type": "text",
      "analyzer": "rebuilt_persian"
    }
  }
}
```
### PUT a document to check new analyzer's effects
```PUT persian_example/_doc/1
{
  "text": "من و حسین با یکدیگر در مدرسه هستیم."
}
```
### GET the saved doc, as you can see it's not changed. The analyzer is performed at search time
```GET persian_example/_doc/1```

### Search for the word حسین
### As you can see it finds the only doc we saved earlier
```GET persian_example/_search
{
  "query": {
    "match": {
      "text": "حسین"
    }
  }
}
```
### Now let's search for a persian stop word which happened to be in our doc, "در"
```GET persian_example/_search
{
  "query": {
    "match": {
      "text": "در"
    }
    
  }
}
```
### As you can see output is empty although the searched text is present in the document but it's removed by persian stop words at indexing time!

### Search analyzer can be modified as well:
```PUT persian_example/_mapping
{
  "properties": {
    "text": {
      "type": "text",
      "analyzer": "rebuilt_persian",
      "search_analyzer": "rebuilt_persian"
    }
  }
}
```
### Still the search will return no documents
```GET persian_example/_search
{
  "query": {
    "match": {
      "text": "در"
    }
    
  }
}```