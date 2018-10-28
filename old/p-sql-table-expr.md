---
title: PostgreSQL 的 SQL 表表达式
date: 2018-07-16 12:29:22
tags:
- PostgreSQL
- SQL
---

表表达式计算一个表。
该语句包含一个FROM子句，该子句后面可以根据需要选用WHERE、GROUP BY和HAVING子句。
最简单的表表达式只是引用磁盘上的一个表，一个所谓的基本表，但是我们可以用更复杂的表表达式以多种方法修改和组合基本表。

表表达式里可选的WHERE、GROUP BY和HAVING子句指定一系列对源自FROM子句的表的转换操作。所有这些转换最后生成一个虚拟表，它提供行传递给选择列表计算查询的输出行。 

# FROM 子句
从一个用逗号分隔的表引用列表中的一个或更多个其他表中生成一个表。

```sql
FROM 表引用 [, 表引用[, ...]]
```

表引用可以是一个表名字（可能有模式限定）或者是一个生成的表， 例如子查询、一个JOIN结构或者这些东西的复杂组合。如果在FROM子句中引用了多于一个表， 那么它们被交叉连接（即构造它们的行的笛卡尔积，见下文）。FROM列表的结果是一个中间的虚拟表，该表可以进行由WHERE、GROUP BY和HAVING子句指定的转换，并最后生成全局的表表达式结果。 
如果一个表引用是一个简单的表名字并且它是表继承层次中的父表，那么该表引用将产生该表和它的后代表中的行，除非你在该表名字前面放上ONLY关键字。但是，这种引用只会产生出现在该命名表中的列 — 在子表中增加的列都会被忽略。 
除了在表名前写ONLY，你可以在表名后面写上`*`来显式地指定要包括所有的后代表。书写`*`并不是必需的，因为该行为是默认值（除非你修改sql_inheritance配置选项的设置）。不过对于强调附加表也应该被搜索，书写`*`是有用的。 

## 连接表

一个连接表是根据特定的连接类型的规则从两个其它表 （真实表或生成表）中派生的表。目前支持内连接、 外连接和交叉连接。 一个连接表的一般语法是： 
```sql
T1 join_type T2 [ join_condition ]
```
所有类型的连接都可以被链在一起或者嵌套：T1 和T2都可以是连接表。在JOIN 子句周围可以使用圆括号来控制连接顺序。如果不使用圆括号， JOIN子句会从左至右嵌套。 

### 交叉连接

```sql
T1 CROSS JOIN T2
```

对来自于T1和T2的行的每一种可能的组合（即笛卡尔积），连接表将包含这样一行：它由所有T1里面的列后面跟着所有T2里面的列构成。如果两个表分别有 N 和 M 行，连接表将有 N * M 行。 
FROM T1 CROSS JOIN T2等效于 FROM T1 INNER JOIN T2 ON TRUE（见下文）。 它也等效于 FROM T1, T2。 

### 条件连接

条件连接
```sql
T1 { [INNER] | { LEFT | RIGHT | FULL } [OUTER] } JOIN T2 ON boolean_expression
T1 { [INNER] | { LEFT | RIGHT | FULL } [OUTER] } JOIN T2 USING ( join column list )
T1 NATURAL { [INNER] | { LEFT | RIGHT | FULL } [OUTER] } JOIN T2
```

INNER和OUTER对所有连接形式都是可选的。INNER是缺省；LEFT、RIGHT和FULL指示一个外连接。 
连接条件在ON或USING子句中指定， 或者用关键字NATURAL隐含地指定。连接条件决定来自两个源表中的哪些行是"匹配"的，这些我们将在后文详细解释。 
可能的条件连接类型是： 


INNER JOIN
对于 T1 的每一行 R1，生成的连接表都有一行对应 T2 中的每一个满足和 R1 的连接条件的行。 
LEFT OUTER JOIN 
首先，执行一次内连接。然后，为 T1 中每一个无法在连接条件上匹配 T2 里任何一行的行返回一个连接行，该连接行中 T2 的列用空值补齐。因此，生成的连接表里为来自 T1 的每一行都至少包含一行。 
RIGHT OUTER JOIN 
首先，执行一次内连接。然后，为 T2 中每一个无法在连接条件上匹配 T1 里任何一行的行返回一个连接行，该连接行中 T1 的列用空值补齐。因此，生成的连接表里为来自 T2 的每一行都至少包含一行。 
FULL OUTER JOIN
首先，执行一次内连接。然后，为 T1 中每一个无法在连接条件上匹配 T2 里任何一行的行返回一个连接行，该连接行中 T2 的列用空值补齐。同样，为 T2 中每一个无法在连接条件上匹配 T1 里任何一行的行返回一个连接行，该连接行中 T1 的列用空值补齐。 

ON子句是最常见的连接条件的形式：它接收一个和WHERE子句里用的一样的布尔值表达式。 如果两个分别来自T1和T2的行在ON表达式上运算的结果为真，那么它们就算是匹配的行。 
USING是个缩写符号，它允许你利用特殊的情况： 连接的两端都具有相同的连接列名。它接受共享列名的一个逗号分隔 列表，并且为其中每一个共享列构造一个包含等值比较的连接条件。 例如用USING (a, b)连接T1 和T2会产生连接条件 ON T1.a = T2.a AND T1.b = T2.b。 
更进一步，JOIN USING的输出会废除冗余列：不需要把匹配 上的列都打印出来，因为它们必须具有相等的值。不过JOIN ON会先产生来自T1的所有列，后面跟上所有来自 T2的列；而JOIN USING会先为列出的每 一个列对产生一个输出列，然后先跟上来自T1的剩余列， 最后跟上来自T2的剩余列。 
最后，NATURAL是USING的缩写形式：它形成一个USING列表， 该列表由那些在两个表里都出现了的列名组成。和USING一样，这些列只在输出表里出现一次。如果不存在公共列，NATURAL的行为将和CROSS JOIN一样。

## 表和列别名

给表或复杂的表引用指定一个临时的名字，用于剩下的查询中引用那些表。

```sql
FROM table_reference AS alias
```

或者

```sql
FROM table_reference alias
```

AS关键字是可选的。别名可以是任意标识符

别名成为当前查询的表引用的新名称，我们不能够用该表最初的名字引用它了！

用圆括号解决歧义。

## 子查询

子查询指定一个派生表。
必须被包围在圆括号里，必须被赋予一个表别名。

```sql
FROM (SELECT * FROM table1) AS alias_name
```

一个子查询也可以是一个VALUES列表：
```sql
FROM (VALUES ('anne', 'smith'), ('bob', 'jones'), ('joe', 'blow'))
     AS names(first, last)
```
再次的，这里要求一个表别名。为VALUES列表中的列分配别名是可选的，但是选择这样做是一个好习惯。

## 表函数

表函数是那些生成一个行集合的函数，这个集合可以是由基本数据类型（标量类型）组成， 也可以是由复合数据类型（表行）组成。它们的用法类似一个表、视图或者在查询的FROM子句里的子查询。表函数返回的列可以像一个表列、视图或者子查询那样被包含在SELECT、JOIN或WHERE子句里。 

也可以使用ROWS FROM语法将平行列返回的结果组合成表函数； 这种情况下结果行的数量是最大一个函数结果的数量，较小的结果会用空值来填充。 
```sql
function_call [WITH ORDINALITY] [[AS] table_alias [(column_alias [, ... ])]]
ROWS FROM( function_call [, ... ] ) [WITH ORDINALITY] [[AS] table_alias [(column_alias [, ... ])]]
```

如果指定了WITH ORDINALITY子句，一个额外的 bigint类型的列将会被增加到函数的结果列中。这个列对 函数结果集的行进行编号，编号从 1 开始（这是对 SQL 标准语法 UNNEST ... WITH ORDINALITY的一般化）。默认情 况下，序数列被称为ordinality，但也可以通过使用一个 AS子句给它分配一个不同的列名。 
调用特殊的表函数UNNEST可以使用任意数量的数组参数， 它会返回对应的列数，就好像在每一个参数上单独调用 UNNEST并且使用 ROWS FROM结构把它们组合起来。 
```sql
UNNEST( array_expression [, ... ] ) [WITH ORDINALITY] [[AS] table_alias [(column_alias [, ... ])]]
```
如果没有指定table_alias，该函数名将被用作 表名。在ROWS FROM()结构的情况中，会使用第一个函数名。 
如果没有提供列的别名，那么对于一个返回基数据类型的函数，列名也与该函数 名相同。对于一个返回组合类型的函数，结果列会从该类型的属性得到名称。 

有时侯，定义一个能够根据它们被调用方式返回不同列集合的表函数是很有用的。为了支持这些，表函数可以被声明为返回伪类型record。 如果在查询里使用这样的函数，那么我们必须在查询中指定所预期的行结构，这样系统才知道如何分析和规划该查询。这种语法是这样的：
```sql
function_call [AS] alias (column_definition [, ... ])
function_call AS [alias] (column_definition [, ... ])
ROWS FROM( ... function_call AS (column_definition [, ... ]) [, ... ] )
```
在没有使用ROWS FROM()语法时， column_definition列表会取代无法附着在 FROM项上的列别名列表，列定义中的名称就起到列别名的作用。 在使用ROWS FROM()语法时， 可以为每一个成员函数单独附着一个 column_definition列表；或者在只有一个成员 函数并且没有WITH ORDINALITY子句的情况下，可以在 ROWS FROM()后面写一个 column_definition列表来取代一个列别名列表。 

## LATERAL 子查询

可以在出现于FROM中的子查询前放置关键词LATERAL。这允许它们引用前面的FROM项提供的列（如果没有LATERAL，每一个子查询将被独立计算，并且因此不能被其他FROM项交叉引用）。 
出现在FROM中的表函数的前面也可以被放上关键词LATERAL，但对于函数该关键词是可选的，在任何情况下函数的参数都可以包含对前面的FROM项提供的列的引用。 
一个LATERAL项可以出现在FROM列表顶层，或者出现在一个JOIN树中。在后一种情况下，如果它出现在JOIN的右部，那么它也可以引用 在JOIN左部的任何项。 
如果一个FROM项包含LATERAL交叉引用，计算过程如下：对于提供交叉引用列的FROM项的每一行，或者多个提供这些列的多个FROM项的行集合，LATERAL项将被使用该行或者行集中的列值进行计算。得到的结果行将和它们被计算出来的行进行正常的连接。对于来自这些列的源表的每一行或行集，该过程将重复。 
```sql
SELECT * FROM foo, LATERAL (SELECT * FROM bar WHERE bar.id = foo.bar_id) ss;
```
这不是非常有用，因为它和一种更简单的形式得到的结果完全一样： 
```sql
SELECT * FROM foo, bar WHERE bar.id = foo.bar_id;
```
在必须要使用交叉引用列来计算那些即将要被连接的行时，LATERAL是最有用的。一种常用的应用是为一个返回集合的函数提供一个参数值。例如，假设vertices(polygon)返回一个多边形的顶点集合，我们可以这样标识存储在一个表中的多边形中靠近的顶点： 
```sql
SELECT p1.id, p2.id, v1, v2
FROM polygons p1, polygons p2,
     LATERAL vertices(p1.poly) v1,
     LATERAL vertices(p2.poly) v2
WHERE (v1 <-> v2) < 10 AND p1.id != p2.id;
```
这个查询也可以被写成： 
```sql
SELECT p1.id, p2.id, v1, v2
FROM polygons p1 CROSS JOIN LATERAL vertices(p1.poly) v1,
     polygons p2 CROSS JOIN LATERAL vertices(p2.poly) v2
WHERE (v1 <-> v2) < 10 AND p1.id != p2.id;
```
或者写成其他几种等价的公式（正如以上提到的，LATERAL关键词在这个例子中并不是必不可少的，但是我们在这里使用它是为了使表述更清晰）。 
有时候也会很特别地把LEFT JOIN放在一个LATERAL子查询的前面，这样即使LATERAL子查询对源行不产生行，源行也会出现在结果中。例如，如果get_product_names()返回一个制造商制造的产品的名字，但是某些制造商在我们的表中目前没有制造产品，我们可以找出哪些制造商是这样： 
```sql
SELECT m.name
FROM manufacturers m LEFT JOIN LATERAL get_product_names(m.id) pname ON true
WHERE pname IS NULL;
```

# WHERE 子句

语法
```sql
WHERE search_condition
```

这里的search_condition是任意返回一个boolean类型值的值表达式。

在完成对FROM子句的处理之后，生成的虚拟表的每一行都会对根据搜索条件进行检查。 如果该条件的结果是真，那么该行被保留在输出表中；否则（也就是说，如果结果是假或空）就把它抛弃。搜索条件通常至少要引用一些在FROM子句里生成的列；虽然这不是必须的，但如果不引用这些列，那么WHERE子句就没什么用了。 

这里是一些WHERE子句的例子： 
```sql
SELECT ... FROM fdt WHERE c1 > 5

SELECT ... FROM fdt WHERE c1 IN (1, 2, 3)

SELECT ... FROM fdt WHERE c1 IN (SELECT c1 FROM t2)

SELECT ... FROM fdt WHERE c1 IN (SELECT c3 FROM t2 WHERE c2 = fdt.c1 + 10)

SELECT ... FROM fdt WHERE c1 BETWEEN (SELECT c3 FROM t2 WHERE c2 = fdt.c1 + 10) AND 100

SELECT ... FROM fdt WHERE EXISTS (SELECT c1 FROM t2 WHERE c2 > fdt.c1)
```
在上面的例子里，fdt是从FROM子句中派生的表。 那些不符合WHERE子句的搜索条件的行会被从fdt中删除。请注意我们把标量子查询当做一个值表达式来用。 和任何其它查询一样，子查询里可以使用复杂的表表达式。同时还请注意fdt在子查询中也被引用。只有在c1也是作为子查询输入表的生成表的列时，才必须把c1限定成fdt.c1。但限定列名字可以增加语句的清晰度，即使有时候不是必须的。这个例子展示了一个外层查询的列名范围如何扩展到它的内层查询。 

# GROUP BY 和 HAVING 子句

在通过了WHERE过滤器之后，生成的输入表可以使用GROUP BY子句进行分组，然后用HAVING子句删除一些分组行。 

```sql
SELECT select_list
    FROM ...
    [WHERE ...]
    GROUP BY grouping_column_reference [, grouping_column_reference]...
```

GROUP BY 子句被用来把表中在所列出的列上具有相同值的行分组在一起。 这些列的列出顺序并没有什么关系。其效果是把每组具有相同值的行组合为一个组行，它代表该组里的所有行。

 这样就可以删除输出里的重复和/或计算应用于这些组的聚集。例如：
 ```sql
=> SELECT * FROM test1;
 x | y
---+---
 a | 3
 c | 2
 b | 5
 a | 1
(4 rows)


=> SELECT x FROM test1 GROUP BY x;
 x
---
 a
 b
 c
(3 rows)
```
在第二个查询里，我们不能写成SELECT * FROM test1 GROUP BY x， 因为列y里没有哪个值可以和每个组相关联起来。被分组的列可以在选择列表中引用是因为它们在每个组都有单一的值。 
通常，如果一个表被分了组，那么没有在GROUP BY中列出的列都不能被引用，除非在聚集表达式中被引用。 一个用聚集表达式的例子是： 
```sql
=> SELECT x, sum(y) FROM test1 GROUP BY x;
 x | sum
---+-----
 a |   4
 b |   5
 c |   2
(3 rows)
```
这里的sum是一个聚集函数，它在整个组上计算出一个单一值。

如果一个表已经用GROUP BY子句分了组，然后你又只对其中的某些组感兴趣， 那么就可以用HAVING子句，它很象WHERE子句，用于从结果中删除一些组。其语法是： 
```sql
SELECT select_list FROM ... [WHERE ...] GROUP BY ... HAVING boolean_expression
```
