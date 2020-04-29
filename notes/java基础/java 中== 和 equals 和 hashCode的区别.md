## Java 中 == 和 equals 和 hashCode 的区别

### 1. “==”

“==” 在基本数据类型中比较的是值相等。  
	 在类中比较的是内存地址，即是否是同一个对象。

### 2. hashCode 

hashCode 也是 Object 类的一个方法，返回一个离散的 int 型整数，在集合类操作中使用，为了提高查询速度，（HashMap、HashSet 等比较是否为同一个）  

如果两个对象 equals 相等，Java 运行时环境会认为他们的 hashCode 一定相等。  

如果两个对象 equals 不想等，它们的 hashCode 有可能相等。  

### 3. equals

如果两个对象 hashCode 相等，它们不一定 equals 相等。  
如果两个对象 hashCode 不相等，它们一定 equals 不相等。
 
