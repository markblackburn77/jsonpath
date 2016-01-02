| **XPath** | **JSONPath** | **Description** |
|:----------|:-------------|:----------------|
| /         | $            |the root object/element |
| .         | @            |the current object/element |
| /         | . or [.md](.md) |child operator   |
| ..        | n/a          |parent operator  |
| //        | ..           |recursive descent. JSONPath borrows this syntax from E4X. |
| `*`       | `*`          |wildcard. All objects/elements regardless their names. |
| @         | n/a          |attribute access. JSON structures don't have attributes. |
| [.md](.md) | [.md](.md)   |subscript operator. XPath uses it to iterate over element collections and for [predicates](http://www.w3.org/TR/xpath#predicates). In Javascript and JSON it is the native array operator. |
| |         | [,]          |Union operator in XPath results in a combination of node sets. JSONPath allows alternate names or array indices as a set. |
| n/a       | [start:end:step] |array slice operator borrowed from ES4. |
| [.md](.md) | ?()          |applies a filter (script) expression. |
| n/a       | ()           |script expression, using the underlying script engine. |
| ()        | n/a          |grouping in Xpath |