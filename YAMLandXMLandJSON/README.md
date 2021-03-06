## WAL    
In computer science, write-ahead logging (WAL) is a family of techniques for providing atomicity and durability (two of the ACID properties) in database systems. The changes are first recorded in the log, which must be written to stable storage, before the changes are written to the database.     
In a system using WAL, all modifications are written to a log before they are applied. Usually both redo and undo information is stored in the log.     

## YAML

![YAML](YAML.JPG)      

YAML (a recursive acronym for "YAML Ain't Markup Language") is a human-readable data-serialization language.     
It is commonly used for configuration files and in applications where data is being stored or transmitted. YAML targets many of the same communications applications as Extensible Markup Language (XML) but has a minimal syntax which intentionally differs from SGML.      

YAML 语言（发音 /ˈjæməl/ ）的设计目标，就是方便人类读写。它实质上是一种通用的数据串行化格式。

它的基本语法规则如下:

大小写敏感
使用缩进表示层级关系
缩进时不允许使用Tab键，只允许使用空格。
缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
#表示注释，从这个字符一直到行尾，都会被解析器忽略。

YAML 支持的数据结构有三种:

对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
纯量（scalars）：单个的、不可再分的值

## XML
XML is “eXtensible Markup Language” whereas YML is not a markup language.   
XML uses a tag to define the structure just like HTML.    
Extensible Markup Language is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. The World Wide Web Consortium's XML 1.0 Specification of 1998 and several other related specifications—all of them free open standards—define XML.     

<img src="https://github.com/zhou-1/State-Of-Art-Researches/blob/master/YAMLandXMLandJSON/XML.png" width="30%">


## JSON   
JSON stands for “JavaScript Object Notation“.    
JavaScript Object Notation is an open-standard file format or data interchange format that uses human-readable text to transmit data objects consisting of attribute–value pairs and array data types.     

<img src="https://github.com/zhou-1/State-Of-Art-Researches/blob/master/YAMLandXMLandJSON/JSON.png" wofth="20%">




