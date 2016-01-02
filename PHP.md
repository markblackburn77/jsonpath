### Usage ###

Include `jsonpath.php` in your HTML page and use the simple API consisting of one single function.

`jsonPath(obj, expr [, args])`

**parameters:**

`obj (array):`
> Array representing the JSON structure.
`expr (string):`
> JSONPath expression string.
`args (object|undefined):`
> Object controlling path evaluation and output. Currently only one member is supported.
`args['resultType'] ("VALUE"|"PATH"):`
> causes the result to be either matching values (default) or normalized path expressions.

**return value:**

`(array|false):`


### Example ###
This example uses the `Services_JSON` class from Michal Migurski's JSON parser (http://mike.teczno.com/json.html).
```
<html>
<head>
<title> JSONPath - Example (php)</title>
</head>
<body>
<pre>

<?php
require_once('json.php');
require_once('jsonpath.php');
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

$parser = new Services_JSON(SERVICES_JSON_LOOSE_TYPE);
$o = $parser->decode($json);

$out = "";
$out .= $parser->encode(jsonPath($o, "$.store.book[*].author")) . "\n";
$out .= $parser->encode(jsonPath($o, "$..author")) . "\n";
$out .= $parser->encode(jsonPath($o, "$.store.*")) . "\n";
$out .= $parser->encode(jsonPath($o, "$.store..price")) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[(count(@)-1)]")) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[-1:]")) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[0,1]")) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[:2]")) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[?(@['isbn'])]")) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[?(@['price']<10)]")) . "\n";
$out .= $parser->encode(jsonPath($o, "$..*")) . "\n";

print($out);
?>

</pre>
</body>
</html>
```
### Example Output (Values) ###
```
["Nigel Rees","Evelyn Waugh","Herman Melville","J. R. R. Tolkien"]
["Nigel Rees","Evelyn Waugh","Herman Melville","J. R. R. Tolkien"]
[[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}],{"color":"red","price":19.95}]
[8.95,12.99,8.99,22.99,19.95]
[{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}]
[{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}]
[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99}]
[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99}]
[{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}]
[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99}]
[{"book":[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}],"bicycle":{"color":"red","price":19.95}},[{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99}],{"color":"red","price":19.95},{"category":"reference","author":"Nigel Rees","title":"Sayings of the Century","price":8.95},{"category":"fiction","author":"Evelyn Waugh","title":"Sword of Honour","price":12.99},{"category":"fiction","author":"Herman Melville","title":"Moby Dick","isbn":"0-553-21311-3","price":8.99},{"category":"fiction","author":"J. R. R. Tolkien","title":"The Lord of the Rings","isbn":"0-395-19395-8","price":22.99},"reference","Nigel Rees","Sayings of the Century",8.95,"fiction","Evelyn Waugh","Sword of Honour",12.99,"fiction","Herman Melville","Moby Dick","0-553-21311-3",8.99,"fiction","J. R. R. Tolkien","The Lord of the Rings","0-395-19395-8",22.99,"red",19.95]
```

### Example Output (Normalized Pathes) ###

replacing the following in the above code ...
```
$out .= $parser->encode(jsonPath($o, "$.store.book[*].author", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$..author", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$.store.*", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$.store..price", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[(count(@)-1)]", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[-1:]", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[0,1]", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[:2]", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[?(@['isbn'])]", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$..book[?(@['price']<10)]", array("resultType" => "PATH"))) . "\n";
$out .= $parser->encode(jsonPath($o, "$..*", array("resultType" => "PATH"))) . "\n";
```

results in the output.
```
["$['store']['book'][0]['author']","$['store']['book'][1]['author']","$['store']['book'][2]['author']","$['store']['book'][3]['author']"]
["$['store']['book'][0]['author']","$['store']['book'][1]['author']","$['store']['book'][2]['author']","$['store']['book'][3]['author']"]
["$['store']['book']","$['store']['bicycle']"]
["$['store']['book'][0]['price']","$['store']['book'][1]['price']","$['store']['book'][2]['price']","$['store']['book'][3]['price']","$['store']['bicycle']['price']"]
["$['store']['book'][3]"]
["$['store']['book'][3]"]
["$['store']['book'][0]","$['store']['book'][1]"]
["$['store']['book'][0]","$['store']['book'][1]"]
["$['store']['book'][2]","$['store']['book'][3]"]
["$['store']['book'][0]","$['store']['book'][2]"]
["$['store']","$['store']['book']","$['store']['bicycle']","$['store']['book'][0]","$['store']['book'][1]","$['store']['book'][2]","$['store']['book'][3]","$['store']['book'][0]['category']","$['store']['book'][0]['author']","$['store']['book'][0]['title']","$['store']['book'][0]['price']","$['store']['book'][1]['category']","$['store']['book'][1]['author']","$['store']['book'][1]['title']","$['store']['book'][1]['price']","$['store']['book'][2]['category']","$['store']['book'][2]['author']","$['store']['book'][2]['title']","$['store']['book'][2]['isbn']","$['store']['book'][2]['price']","$['store']['book'][3]['category']","$['store']['book'][3]['author']","$['store']['book'][3]['title']","$['store']['book'][3]['isbn']","$['store']['book'][3]['price']","$['store']['bicycle']['color']","$['store']['bicycle']['price']"]
```