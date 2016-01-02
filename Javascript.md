### Usage ###

Include `jsonpath.js` in your HTML page and use the simple API consisting of one single function.

`jsonPath(obj, expr [, args])`

**parameters:**

`obj (object|array):`
> Object representing the JSON structure.
`expr (string):`
> JSONPath expression string.
`args (object|undefined):`
> Object controlling path evaluation and output. Currently only one member is supported.
`args.resultType ("VALUE"|"PATH"):`
> causes the result to be either matching values (default) or normalized path expressions.

**return value:**

`(array|false):`


### Example ###
This example uses the `toJSONString` method from http://www.json.org/json.js .
```
<html>
<head>
<title> JSONPath - Example (js)</title>
<script type="text/javascript" src="json.js"></script>
<script type="text/javascript" src="jsonpath.js"></script>
</head>
<body>
<pre>
<script type="text/javascript">
   var json = 
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
		},
           out = "";
       out += jsonPath(json, "$.store.book[*].author").toJSONString() + "\n>";
       out += jsonPath(json, "$..author").toJSONString() + "\n";
       out += jsonPath(json, "$.store.*").toJSONString() + "\n";
       out += jsonPath(json, "$.store..price").toJSONString() + "\n";
       out += jsonPath(json, "$..book[(@.length-1)]").toJSONString() + "\n";
       out += jsonPath(json, "$..book[-1:]").toJSONString() + "\n";
       out += jsonPath(json, "$..book[0,1]").toJSONString() + "\n";
       out += jsonPath(json, "$..book[:2]").toJSONString() + "\n";
       out += jsonPath(json, "$..book[?(@.isbn)]").toJSONString() + "\n";
       out += jsonPath(json, "$..book[?(@.price<10)]").toJSONString() + "\n";
       out += jsonPath(json, "$..*").toJSONString() + "\n";
       document.write(out);
  </script>
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
### Example Output (normalized pathes) ###
replacing the following in the above code ...
```
out += jsonPath(json, "$.store.book[*].author", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$..author", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$.store.*", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$.store..price", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$..book[(@.length-1)]", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$..book[-1:]", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$..book[0,1]", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$..book[:2]", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$..book[?(@.isbn)]", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$..book[?(@.price<10)]", {resultType:"PATH"}).toJSONString() + "\n";
out += jsonPath(json, "$..*", {resultType:"PATH"}).toJSONString() + "\n";
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