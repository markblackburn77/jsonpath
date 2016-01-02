### Introduction ###

JSONPath allows query access to JSON structures. JsonStore is a helper class which extends JSONPath in order to perform read, write and delete operations on JSON structures. You can download `jsonstore.php` from the [repository](http://jsonpath.googlecode.com/svn/trunk/etc/).

### Usage ###

```
class JsonStore {
     asObj($jsonstr)
     toString($obj)
   & get(&$obj, $expr)
     set(&$obj, $expr, $value)
     add(&$obj, $parentexpr, $value, $name="")
     remove(&$obj, $expr)
}

```

## `JsonStore::asObj($jsonstr)` ##

**parameters:**

> `$jsonstr (string)`

> String representing the JSON structure.

**return value:**
> `(array)`

> JSON structure as PHP array.

## `JsonStore::toString($obj)` ##

**parameters:**

> `$obj (array)`

> JSON structure as PHP array.

**return value:**
> `(string)`

> String representing the JSON structure.

## `JsonStore::get($obj, $expr)` ##

**parameters:**

> `$obj (reference to array)`

> JSON structure as PHP array.

> `$expr (string)`

> JSONPath expression.

**return value:**
> `(any)`

> reference to selected portion of JSON structure. In case of multiple matches only the first match is returned.

## `JsonStore::set($obj, $expr, $value)` ##

**parameters:**

> `$obj (reference to array)`

> JSON structure as PHP array.

> `$expr (string)`

> JSONPath expression referencing the member of the JSON structure to modify.

> `$value (basic type | array)`

> New value of the selected portion of the JSON structure. In case of multiple matches only the first match is used.

**return value:**

> None

## `JsonStore::add($obj, $parentexpr, $value [, $name])` ##

**parameters:**

> `$obj (reference to array)`

> JSON structure as PHP array.

> `$parentexpr (string)`

> JSONPath expression referencing the parent member of the JSON structure. In case of multiple matches only the first match is used.

> `$value (basic type | array)`

> New value to add to the parent member of the JSON structure.

> `$name (string) (optional)`

> Name of the new value to add to the parent member of the JSON structure. If missing an indexed array is assumed and the new value is appended to that array.

**return value:**

> None

## `JsonStore::remove($obj, $expr)` ##

**parameters:**

> `$obj (reference to array)`

> JSON structure as PHP array.

> `$expr (string)`

> JSONPath expression referencing the member of the JSON structure to remove. In case of multiple matches only the first match is used.

**return value:**

> `true|false`

> Boolean value indicating the success of the operation.

### Example ###
```
<?php
require_once('jsonstore.php');

$json = '
{ "store": {
    "book": [ 
      { "category": "reference",
        "author": "Nigel Rees",
        "title": "Sayings of the Century",
        "price": 8.95
      },
      { "category": "fiction",
        "author": "Evelyn Waugh",
        "title": "Sword of Honour",
        "price": 12.99
      },
      { "category": "fiction",
        "author": "Herman Melville",
        "title": "Moby Dick",
        "isbn": "0-553-21311-3",
        "price": 8.99
      },
      { "category": "fiction",
        "author": "J. R. R. Tolkien",
        "title": "The Lord of the Rings",
        "isbn": "0-395-19395-8",
        "price": 22.99
      }
    ],
    "bicycle": {
      "color": "red",
      "price": 19.95
    }
  }
}
';

$o = JsonStore::asObj($json);

JsonStore::set($o, "$..book[1].author", "Arthur Evelyn St. John Waugh");
$x =& JsonStore::get($o, "$..book[?(@['price'] == 12.99)]");
$x['category'] = "novel";
JsonStore::add($o, "$..book[1]", "id-007", "id");
JsonStore::set($o, "$..book[?(@['price'] == 8.99)].category", "drama");
JsonStore::remove($o, "$..book[2].isbn");
JsonStore::add($o, "$.store.book", JsonStore::asObj('{"author": "D. Adams", "title": "Hitchhiker\'s Guide through the Galaxy"}'));
JsonStore::add($o, "$..book[4]", 8.91, "price");
JsonStore::add($o, "$..book[-1:]", true, "sellout");

$json = JsonStore::toString($o);
print($json);
?>
```

results in the new JSON structure

```
{
  "store":{
    "book":[
      {
        "category":"reference",
        "author":"Nigel Rees",
        "title":"Sayings of the Century",
        "price":8.95
      },
      {
        "category":"novel",
        "author":"Arthur Evelyn St. John Waugh",
        "title":"Sword of Honour",
        "price":12.99,
        "id":"id-007"
      },
      {
        "category":"drama",
        "author":"Herman Melville",
        "title":"Moby Dick",
        "price":8.99
      },
      {
        "category":"fiction",
        "author":"J. R. R. Tolkien",
        "title":"The Lord of the Rings",
        "isbn":"0-395-19395-8",
        "price":22.99
      },
      {
        "author":"D. Adams",
        "title":"Hitchhiker's Guide through the Galaxy",
        "price":8.91,
        "sellout":true
      }
    ],
    "bicycle":{
      "color":"red",
      "price":19.95
    }
  }
}
```



