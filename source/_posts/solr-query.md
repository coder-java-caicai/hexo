---
title: solr-query
date: 2016-04-28 17:22:43
tags: solr
---
一. Query参数 
1. CoreQueryParam查询的参数 
1) q: 查询字符串，必须的。 
2) q.op: 覆盖schema.xml的defaultOperator（有空格时用"AND"还是用"OR"操作逻辑），一般默认指定。 
3) df: 默认的查询字段，一般默认指定。 
4) qt: query type，指定查询使用的Query Handler，默认为“standard”。 
5) wt: writer type。指定查询输出结构格式，默认为“xml”。在solrconfig.xml中定义了查询输出格式：xml、json、python、ruby、php、phps、custom。 
6) echoHandler：是否在查询结果中显示使用的Query Handler名称。 
7) echoParams：是否显示查询参数。none：不显示；explicit：只显示查询参数；all：所有，包括在solrconfig.xml定义的Query Handler参数。 
8) indent - 返回的结果是否缩进，默认关闭，用 indent=true|on 开启，一般调试json,php,phps,ruby输出才有必要用这个参数。 
9) version - 查询语法的版本，建议不使用它，由服务器指定默认值。 
 
2. CommonQueryParameters 
1) sort：排序，格式：sort=<field name>+<desc|asc>[,<field name>+<desc|asc>]„ 。
示例：（inStock desc, price asc）表示先 “inStock” 降序, 再 “price” 升序，默认是相关性降序。。 
2) start：用于分页定义结果起始记录数，默认为0。 
3) rows：用于分页定义结果每页返回记录数，默认为10。 
4) fq：filter query。使用Filter Query可以充分利用Filter Query Cache，提高检索性能。作用：在q查询符合结果中同时是fq查询符合的，
例如：q=mm&fq=date_time:[20081001 TO 20091031]，找关键字mm，并且date_time是20081001到20091031之间的。 
5) fl：field list。指定返回结果字段。以空格“ ”或逗号“,”分隔。 
6) debugQuery：设置返回结果是否显示Debug信息。 
7) explainOther：设置当debugQuery=true时，显示其他的查询说明。 
8) defType：设置查询解析器名称。 
9) timeAllowed：设置查询超时时间。 
10) omitHeader：设置是否忽略查询结果返回头信息，默认为“false”。 
 
二. 查询语法 
1. 匹配所有文档：*:* 
2. 强制、阻止和可选查询： 
1) Mandatory：查询结果中必须包括的(for example, only entry name containing the word make) Solr/Lucene Statement：+make, +make +up ,+make +up +kiss 
2) prohibited：(for example, all documents except those with word believe) Solr/Lucene Statement：+make +up -kiss 3) optional：Solr/Lucene Statement：+make +up kiss 
 
3. 布尔操作：AND、OR和NOT布尔操作（必须大写）与Mandatory、optional和prohibited相似。 
1) make AND up ＝ +make +up :AND左右两边的操作都是mandatory 
2) make || up ＝ make OR up＝make up :OR左右两边的操作都是optional 
3) +make +up NOT kiss ＝ +make +up –kiss 
4) make AND up OR french AND Kiss不可以达到期望的结果，因为AND两边的操作都是mandatory的。  
4. 子表达式查询（子查询）：可以使用“()”构造子查询。 
For ex：(make AND up) OR (french AND Kiss) 
 
5. 子表达式查询中阻止查询的限制： 
For ex:make (-up):只能取得make的查询结果；要使用make (-up *:*)查询make或者不包括up的结果。  
6. 多字段fields查询：通过字段名加上分号的方式（fieldName:query）来进行查询 
For ex：entryNm:make AND entryId:3cdc86e8e0fb4da8ab17caed42f6760c  
7. 通配符查询（wildCard Query）： 
1) 通配符？和*：“*”表示匹配任意字符；“？”表示匹配出现的位置。 
For ex：ma?*（ma后面的一个位置匹配），ma??*(ma后面两个位置都匹配) 
2) 查询字符必须要小写：+Ma +be**可以搜索到结果；+Ma +Be**没有搜索结果 
3) 查询速度较慢，尤其是通配符在首位：主要原因一是需要迭代查询字段中的每个term，判断是否匹配；二是匹配上的term被加到内部的查询，当terms数量达到1024的时候，查询会失败。 
4) Solr中默认通配符不能出现在首位（可以修改QueryParser，设置 setAllowLeadingWildcard为true） 
5) set setAllowLeadingWildcard to true. 
8. 模糊查询、相似查询：不是精确的查询，通过对查询的字段进行重新插入、删除和转换来取得得分较高的查询解决（由Levenstein Distance Algorithm算法支持）。 
1) 一般模糊查询：for ex：make-believ~ 
2) 门槛模糊查询：对模糊查询可以设置查询门槛，门槛是0~1之间的数值，门槛
越高表面相似度越高。For ex：make-believ~0.5、make-believ~0.8、make-believ~0.9  
9. 范围查询（Range Query）：Lucene支持对数字、日期甚至文本的范围查询。结束的范围可以使用“*”通配符。 
For ex： 
1) 日期范围（ISO-8601 时间GMT）：sa_type:2 AND a_begin_date:[1990-01-01T00:00:00.000Z TO 1999-12-31T24:59:99.999Z] 
2) 数字：salary:[2000 TO *] 3) 文本：entryNm:[a TO a] 
 
10. 日期匹配：YEAR, MONTH, DAY, DATE (synonymous with DAY) HOUR, MINUTE, SECOND, MILLISECOND, and MILLI (synonymous with MILLISECOND)可以被标志成日期。 
For ex： 
1) r_event_date:[* TO NOW-2YEAR]：2年前的现在这个时间 
2) r_event_date:[* TO NOW/DAY-2YEAR]：2年前前一天的这个时间 
 
三. 函数查询（Function Query） 
函数查询 可以利用 numeric域的值 或者 与域相关的的某个特定的值的函数，来对文档进行评分。 
1. 使用函数查询的方法 
这里主要有三种方法可以使用函数查询，这三种s方法都是通过solr http接口的。 
1) 使用FunctionQParserPlugin。ie: q={!func}log(foo) 
2) 使用“_val_”内嵌方法内嵌在正常的solr查询表达式中。即，将函数查询写在 q这个参数中，这时候，我们使用“_val_”将函数与其他的查询加以区别。 ie：entryNm:make && _val_:ord(entryNm)  
3) 使用dismax中的bf参数使用明确为函数查询的参数，比如说dismax中的bf（boost function）这个参数。  注意：bf这个参数是可以接受多个函数查询的，它们之间用空格隔开，它们还可以带上权重。所以，当我们使用bf这个参数的时候，我们必须保证单个函数中是没有空格出现的，不然程序有可能会以为是两个函数。 
For ex： 
 q=dismax&bf="ord(popularity)^0.5 recip(rord(price),1,1000,1000)^0.3   2. 函数的格式（Function Query Syntax) 目前，function query 并不支持 a+b 这样的形式，我们得把它写成一个方法形式，这就是 sum(a,b). 
 
3. 使用函数查询注意事项 
1) 用于函数查询的field必须是被索引的； 
2) 字段不可以是多值的（multi-value） 
 
4.  可以利用的函数 （available function）
1) constant：支持有小数点的常量； 例如：1.5 ；SolrQuerySyntax:_val_:1.5  
2) fieldvalue：这个函数将会返回numeric field的值，这个域必须是indexd的，非multiValued的。格式很简单，就是该域的名字。如果这个域中没有这样的值，那么将会返回0。 
3) ord：对于一个域，它所有的值都将会按照字典顺序排列，这个函数返回你要查询的那个特定的值在这个顺序中的排名。这个域，必须是非multiValued的，当没有值存在的时候，将返回0。例如：某个特定的域只能去三个值，“apple”、“banana”、“pear”，那么ord（“apple”）=1，ord（“banana”）=2，ord（“pear”）=3.需要注意的是，ord（）这个函数，依赖于值在索引中的位置，所以当有文档被删除、或者添加的时候，ord（）的值就会发生变化。当你使用MultiSearcher的时候，这个值也就是不定的了。 
4) rord：这个函数将会返回与ord相对应的倒排序的排名。 格式: rord(myIndexedField)。 
5) sum：这个函数的意思就显而易见啦，它就是表示“和”啦。格式：sum(x,1) 、sum(x,y)、 sum(sqrt(x),log(y),z,0.5) 
6) product：product(x,y,...)将会返回多个函数的乘积。格式：product(x,2)、product(x,y) 
7) div：div(x,y)表示x除以y的值，格式：div（1,x）、div(sum(x,100),max(y,1)) 
8) pow：pow表示幂值。pow(x,y) =x^y。例如：pow(x,0.5) 表示开方pow(x,log(y)) 
9) abs：abs(x)将返回表达式的绝对值。格式：abs(-5)、 abs(x) 
10)  log：log(x)将会返回基数为10，x的对数。格式： log(x)、 log(sum(x,100)) 
11)  Sqrt：sqrt(x) 返回 一个数的平方根。格式：sqrt（2）、sqrt(sum(x,100)) 
12)  Map：如果 x>=min,且x<=max,那么map(x,min,max,target)=target.如果 x不在[min,max]这个区间内，那么map(x,min,max,target)=x.  格式：map(x,0,0,1) 
13) Scale：scale(x,minTarget,maxTarget) 这个函数将会把x的值限制在[minTarget,maxTarget]范围内。 14) query ：query(subquery,default)将会返回给定subquery的分数，如果subquery与文档不匹配，那么将会返回默认值。任何的查询类型都是受支持的。可以通过引用的方式，也可以直接指定查询串。 
例子：q=product(popularity, query({!dismax v='solr rocks'}) 将会返回popularity和通过dismax 查询得到的分数的乘积。 
q=product(popularity, query($qq)&qq={!dismax}solr rocks 跟上一个例子的效果是一样的。不过这里使用的是引用的方式 
q=product(popularity, query($qq,0.1)&qq={!dismax}solr rocks 在前一个例子的基础上又加了一个默认值。 
15)  linear： inear(x,m,c)表示 m*x+c ,其中m和c都是常量，x是一个变量也可以是一个函数。例如： linear(x,2,4)=2*x+4. 
16) Recip：recip(x,m,a,b)=a/(m*x+b)其中，m、a、b是常量，x是变量或者一个函数。当a=b，并且x>=0的时候，这个函数的最大值是1，值的大小随着x的增大而减小。例如：recip(rord(creationDate),1,1000,1000) 
17) Max： max(x,c)将会返回一个函数和一个常量之间的最大值。 例如：max(myfield,0)