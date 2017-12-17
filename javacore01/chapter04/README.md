### 类设计技巧

1.一定要保证数据的私有

这是最重要的；绝对不要破坏封装性。有时候，需要编写一个访问器方法或更改器方法，但是最好还是保持实例域的私有性。很多惨痛的经验告诉我们，数据的表示形式很可能会改变，但它们的使用方式却不会经常发生变化。当数据保持私有时，它们的表示形式的变化不会对类的使用者产生影响，即使出现bug也易于检测。

2.一定要对数据初始化

Java不对局部变量进行初始化，但是会对对象的实例域进行初始化。最好不要依赖于系统的默认值，而是应该显式地初始化所有的数据，具体的初始化方式可以是提供默认值，也可以是在所有构造器中设置默认值

3.不要在类中使用过多的基本类型

就是说，用其他的类代替多个相关的基本类型的使用。这样会使类更加易于理解且易于修改。例如，用一个称为Address的新的类替换一个Customer类中以下的实例域：
	
	private String street;
	private String city;
	private String state;
	private int zip;
	
这样，可以很容易处理地址的变化，例如，需要增加对国际地址的支持。

4.不是所有的域都需要独立的域访问器和域更改器

或许，需要获得或设置雇员的薪金。而一旦构造了雇员对象，就应该禁止更改雇佣日期，并且在对象中，常常包含一些不希望别人获得或设置的实例域，例如，在Address类中，存放州缩写的数组。

5.将职责过多的类进行分解

这样说似乎有点含糊不清，究竟多少算是“过多”？每个人的看法不同。但是，如果明显的可以将一个复杂的类分解为两个简单的类，就应该将其分解（但另一方面，也不要走极端。设计十个类，每个类只有一个方法，显然有些矫枉过正了）。
下面试一个反面的设计示例。

	public class CardDeck {
		private int[] value;
		private int[] suit;
		
		public CardDeck() {...}
		public void shuffle() {...}
		public int getTopValue() {...}
		public int getTopSuit() {...}
		public void draw() {...}
	}
	
实际上，这个类实现了两个独立的概念：一副牌（含有shuffle方法和draw方法）和一张牌（含有查看面值和花色的方法）。另外，引入一个表示单张牌的Card类。现在有两个类，每个类完成自己的职责：

	public class CardDeck {
		private Card[] cards;
		
		public CardDeck() {...}
		public void shuffle() {...}
		public Card getTop() {...}
		public void draw() {...}
	}
	
	public calss Card {
		private int value;
		private int suit;
		
		public Card(int aValue, int aSuit) {...}
		public int getValue() {...}
		public int getDuit() {...}
	}
	
6.类名和方法名要能够体现它们的职责

与变量应该有一个能够反映其含义的名字一样，类也应该如此（在标准类库中，也存在着一些含义不明的例子，如：Date类实际上是一个用于描述时间的类）。  
命名类名的良好习惯是采用一个名词（Order）、前面有形容词修饰的名词（RushOrder）或动名词（有“-ing”后缀）修饰名词（例如，BillingAddress）。对于方法来说，习惯是访问器方法用小写的get开头（getSalary），更改器方法用小写的set开头（setSalary）。
