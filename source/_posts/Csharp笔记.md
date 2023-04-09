---
title: Csharp笔记
date: 2023-01-27 18:59:22
tags:
---

# C# 静态成员和实例成员

- 　静态成员（[static](https://so.csdn.net/so/search?q=static&spm=1001.2101.3001.7020) member）：又叫类成员，指的是在成员类型或返回值类型前用static关键字修饰的变量或方法，包括静态数据和静态方法；
- 　实例成员（instance member）：又称非静态成员、对象成员，是没有用static修饰的变量或方法，包括实例数据和实例方法。

### 静态成员的特点：

- 静态成员（包括静态数据和静态方法）必须由类名调用，不能使用对象调用（静态数据可以由实例方法调用）。
- 静态数据属于类的级别，当类加载时，一个静态数据在内存只分配一个存储空间，无论new出多少个实例，它也只是有那一个空间。
- 静态方法只能调用静态数据，不能调用对象。

### 实例成员的特点：

- 实例成员（包括实例数据和实例方法）必须通过对象来调用，不能使用类名调用。
- 类的实例数据属于类的实例级别，每新创建一个对象，都会在内存中为实例成员开辟一块新的存储空间。
- 实例方法可以调用实例数据和静态数据。

# 类型转换

### 隐式类型转换

- 不丢失精度的转换
- 子类向父类的转换
- 装箱

### 显式类型转换

- 有可能丢失精度的转换，即cast（（T）x）T：目标类型
- 拆箱
- 使用convert类
- ToString方法与各数据类型的Parse/TryParse方法



# C#封装

**封装** 被定义为"把一个或多个项目封闭在一个物理的或者逻辑的包中"

 **C#支持的访问修饰符：**

- public：所有对象都可以访问；
- private：对象本身在对象内部可以访问；
- protected：只有该类对象及其子类对象可以访问，这样有助于实现继承
- internal：同一个程序集的对象可以访问；
- protected internal：访问限于当前程序集或派生自包含类的类型。

## 属性封装

- 属性（Property)是类（class）、结构（structure）和接口（interface）的命名（named）成员

```csharp
 private int age;//private:私有的,仅供内部进行访问
 public int Age//public:公有的,任何地方都可以访问
        {
            //获取或读取字段值
            get { return age; }//属性的读取
            set { age = value; }//属性赋值(value为关键字)
        }

```

```csharp
first.Age = 21;
Console.WriteLine("年龄为:{0}",first.Age);
```



**字段和属性有什么区别？**

- 字段：占用内存
- 属性：不占内存
- 属性必须依赖一个字段

## 方法封装

- 第一种无参数的方法（没有返回值的方法）
- 第二种有参数的方法（有返回值的方法）

# C# List<T>用法

## List<T>的用法

- **声明**：

```csharp
List<T> mList = new List<T>(); //T为列表中元素类型

//以一个集合作为参数创建List：
string[] temArr = { "Ha", "Hunter", "Tom", "Lily", "Jay", "Jim", "Kuku", "Locu" };
List<string> testList = new List<string>(temArr);
```

- **添加元素**：List.Add(element)      List.Insert(index,element)

```csharp
List<string> mList = new List<string>();
mList.Add("John");

//在index位置添加一个元素
List<string> mList = new List<string>();
mList.Insert(1, "Hei")
```

- **遍历List元素**：

```csharp
foreach (T element in mList)  //T的类型与mList声明时一样
{
    Console.WriteLine(element);
}
```

- **删除元素**:   
  - List. Remove(element);  
  - List. RemoveAt(int index);   删除下标为index的元素
  - List. RemoveRange(int index, int count); 从下标index开始，删除count个元素

- **判断某个元素是否在该List中：**List. Contains(element)  返回值为：true/false

```csharp
if (mList.Contains("Hunter"))
{
    Console.WriteLine("There is Hunter in the list");
}
else
{
    mList.Add("Hunter");
    Console.WriteLine("Add Hunter successfully.");
}
```

- **给List里面元素排序：**List. Sort ()  默认是元素第一个字母按升序
- **自定义排序**:默认比较规则在CompareTo方法中定义，该方法属于IComparable<T>泛型接口。请看下面代码：

```csharp
class Person ：IComparable<Person>
{
    //按年龄比较
    public int CompareTo(Person p)
    {
        return this.Age - p.Age;
    }
}
//定义好默认比较规则后，就可以通过不带参数的Sort方法对集合进行排序
```

**实际使用中，经常需要对集合按照多种不同规则进行排序，这就需要定义其他比较规则，可以在Compare方法中定义，该方法属于IComparer<T>泛型接口**

```csharp
class NameComparer : IComparer<Person>
{
    //存放排序器实例
    public static NameComparer Default = new NameComparer();
    //按姓名比较
    public int Compare(Person p1, Person p2)
    {
        return System.Collections.Comparer.Default.Compare(p1.Name, p2.Name);
    }
}


//按照姓名对集合进行排序 
persons.Sort(NameComparer.Default); 
```

- **给List里面元素顺序反转：**List. Reverse ()
- **List清空：**List. Clear () 
- **获得List中元素数目：**List. Count ()  返回int值

## List的方法和属性

　　Capacity 用于获取或设置List可容纳元素的数量。当数量超过容量时，这个值会自动增长。您可以设置这个值以减少容量，也可以调用trin()方法来减少容量以适合实际的元素数目。

　　Count 属性，用于获取数组中当前元素数量

　　Item( ) 通过指定索引获取或设置元素。对于List类来说，它是一个索引器。

　　Add( ) 在List中添加一个对象的公有方法

　　AddRange( ) 公有方法，在List尾部添加实现了ICollection接口的多个元素

　　BinarySearch( ) 重载的公有方法，用于在排序的List内使用二分查找来定位指定元素.

　　Clear( ) 在List内移除所有元素

　　Contains( ) 测试一个元素是否在List内

　　CopyTo( ) 重载的公有方法，把一个List拷贝到一维数组内

　　Exists( ) 测试一个元素是否在List内

　　Find( ) 查找并返回List内的出现的第一个匹配元素

　　FindAll( ) 查找并返回List内的所有匹配元素

　　GetEnumerator( ) 重载的公有方法，返回一个用于迭代List的枚举器

　　Getrange( ) 拷贝指定范围的元素到新的List内

　　IndexOf( ) 重载的公有方法，查找并返回每一个匹配元素的索引

　　Insert( ) 在List内插入一个元素

　　InsertRange( ) 在List内插入一组元素

　　LastIndexOf( ) 重载的公有方法，，查找并返回最后一个匹配元素的索引

　　Remove( ) 移除与指定元素匹配的第一个元素

　　RemoveAt( ) 移除指定索引的元素

　　RemoveRange( ) 移除指定范围的元素

　　Reverse( ) 反转List内元素的顺序

　　Sort( ) 对List内的元素进行排序

　　ToArray( ) 把List内的元素拷贝到一个新的数组内

　　trimToSize( ) 将容量设置为List中元素的实际数目



# C# Dictionary(字典）

## Dictionary的描述

1、从一组键（Key）到一组值（Value）的映射，每一个添加项都是由一个值及其相关连的键组成

2、任何键都必须是唯一的

3、键不能为空引用null（VB中的Nothing），若值为引用类型，则可以为空值

4、Key和Value可以是任何类型（string，int，custom class 等）

## Dictionary的语法

```csharp
//1、创建及初始化
Dictionary<int,string> myDic = new Dictionary<int,string>();//第一个参数是索引的类型，第二个参数是内容的类型

//2、添加元素
myDic.Add(1,"C#");
myDic.Add(2,"C++");
myDic.Add(3,"Java");
myDic.Add(4,"JavaScript");

//3、通过Key(键)查找元素
if(myDictionary.ContainsKey(1)){
    Console.WriteLine("Key:{0},Value:{1}","1", myDic[1]);
 }

//4、通过KeyValuePair遍历元素
foreach(KeyValuePair<int,string>kvp in myDic)
{
        Console.WriteLine("Key = {0}, Value = {1}",kvp.Key, kvp.Value);
}

//5、仅遍历键 Keys 属性
Dictionary<int,string>.KeyCollection keyCol=myDic.Keys;
foreach(intkeyinkeyCol)
{
        Console.WriteLine("Key = {0}", key);
}

//6、仅遍历值 Valus属性
Dictionary<int,string>.ValueCollection valueCol=myDic.Values;
foreach(stringvalueinvalueCol)
{
        Console.WriteLine("Value = {0}", value);
}

//7、通过Remove方法移除指定的键值
myDic.Remove(1);
if(myDic.ContainsKey(1))
...{
　　Console.WriteLine("Key:{0},Value:{1}","1", myDic[1]);
}
else{
    Console.WriteLine("不存在 Key : 1"); 
 }
```

补充：在类后面有有尖括号的是**泛型类**，需跟其他类组成一个完整的类

# C# 索引器

**索引器（Indexer）** 允许一个对象可以像数组一样使用下标的方式来访问。

当您为类定义一个索引器时，该类的行为就会像一个 **虚拟数组（virtual array）** 一样。您可以使用数组访问运算符 **[ ]** 来访问该类的的成员。

**一维索引器的语法如下：**

```csharp
element-type this[int index]
{
   // get 访问器
   get
   {
      // 返回 index 指定的值
   }

   // set 访问器
   set
   {
      // 设置 index 指定的值
   }
}
```

**实例演示：**

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Csharp_indexer
{
    class Program
    {
        static void Main(string[] args)
        {
            Student stu1 = new Student();
            stu1["Math"] = 100;
            var mathScore = stu1[ "Math"];
            Console.WriteLine(mathScore);
            Console.ReadKey();
        }
    }
    class Student
    {
        private Dictionary<string, int> scoreDictionary = new Dictionary<string, int>();
        public int? this[string name]
        {
            get 
            {
                if (this.scoreDictionary.ContainsKey(name))
                {
                    return this.scoreDictionary[name];
                }  
                else
                {
                    return null;
                }
            }
            set 
            {
                if (value.HasValue==false)
                {
                    throw new Exception("Value connot null!");
                }
                if (this.scoreDictionary.ContainsKey(name))
                {
                    this.scoreDictionary[name] = value.Value;
                }
                else
                {
                    this.scoreDictionary.Add(name, value.Value);
                }
            }
        }
    }

}

```

###  索引器与数组的区别：

- **索引器的索引值（Index）类型不限定为整数：**

​    用来访问数组的索引值（Index）一定为整数，而索引器的索引值类型可以定义为其他类型。

- **索引器允许重载**

​    一个类不限定为只能定义一个索引器，只要索引器的函数签名不同，就可以定义多个索引器，可以重载它的功能。

- **索引器不是一个变量**

​    索引器没有直接定义数据存储的地方，而数组有。索引器具有Get和Set访问器。

### 索引器与属性的区别：

- **索引器以函数签名方式 this 来标识，而属性采用名称来标识，名称可以任意**
- **索引器可以重载，而属性不能重载。**
- **索引器不能用static 来进行声明，而属性可以。索引器永远属于实例成员，因此不能声明为static。**

# C#类型输出

### C#百分数形式输出

==**百分比的格式控制符为"P"或"p"将普通数值输出为百分比形式。**==

```csharp
var a = Console.ReadLine();
var b = Console.ReadLine();
double percent = double.Parse(b) / double.Parse(a);
string q = $"{percent:p3}";
//string q2 = percent.ToString("P0"); 
//P后边跟数字，代表精度。
Console.WriteLine(q);
```

### C#保留小数点后指定位数

```csharp
double d=1.2356;

string str=d.ToString("0.00");  //小数点后有几个0即保留几位小数。
string str=d.ToString("#0.00");  
string str=d.ToString("f2");  //fn 保留n位，四舍五入，"F","f" 不区分大小写
string str=String.Format("{0:F}", d);  
string str=String.Format("{0:N2}", d);
double do=Math.Round(d, 2);  
decimal de = decimal.Round(decimal.Parse(d.ToString()), 2);
```

# C# 继承

注：这个已有的类被称为的**基类**，这个新的类被称为**派生类**

**派生类继承了基类的成员变量和成员方法**

```csharp
using System;
namespace InheritanceApplication
{
   class Shape//基类
   {
      public void setWidth(int w)
      {
         width = w;
      }
      public void setHeight(int h)
      {
         height = h;
      }
       //protected可以从派生类直接访问基类的受保护成员
      protected int width;
      protected int height;
   }

   // 派生类
   class Rectangle: Shape
   {
      public int getArea()
      {
         return (width * height);
      }
   }
   
   class RectangleTester
   {
      static void Main(string[] args)
      {
         Rectangle Rect = new Rectangle();
         Rect.setWidth(5);
         Rect.setHeight(7);
         // 打印对象的面积
         Console.WriteLine("总面积： {0}",  Rect.getArea());
         Console.ReadKey();
      }
   }
}
```

### base 关键字

`base` 关键字用于从派生类中访问基类的成员。 如果要执行以下操作时使用它：

- 调用基类上已被其他方法重写的方法。
- 指定创建派生类实例时应调用的基类构造函数。

仅允许基类访问在构造函数、实例方法和实例属性访问器中进行。

在静态方法中使用 `base` 关键字将产生错误。

所访问的基类是类声明中指定的基类。 例如，如果指定 `class ClassB : ClassA`，则从 ClassB 访问 ClassA 的成员，而不考虑 ClassA 的基类。

# C# 多态性

==多态：只有多个类同时继承了同一个类。==

在 C#语言中体现多态有三种方式：虚方法，抽象类， 接口。

### 抽象方法和虚方法

基类将方法声明为 [`virtual`](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/virtual) 时，派生类可以使用其自己的实现[`override`](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/override)该方法。 

如果基类将成员声明为 [`abstract`](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/abstract)，则必须在直接继承自该类的任何非抽象类中重写该方法。 如果派生类本身是抽象的，则它会继承抽象成员而不会实现它们。 抽象和虚拟成员是多形性（面向对象的编程的第二个主要特征）的基础。

### 抽象类

C# 允许您使用关键字 **abstract** 创建抽象类，用于提供接口的部分类的实现。当一个派生类继承自该抽象类时，实现即完成。**抽象类**包含抽象方法，抽象方法可被派生类实现。派生类具有更专业的功能。

请注意，下面是有关抽象类的一些规则：

- 您不能创建一个抽象类的实例。
- 您不能在一个抽象类外部声明一个抽象方法。
- 通过在类定义前面放置关键字 **sealed**，可以将类声明为**密封类**。当一个类被声明为 **sealed** 时，它不能被继承。抽象类不能被声明为 sealed。

# C#事件

1. 角色：**使对象或类具备通知能力的成员**
2. 使用：**用于对象或类间的动作协调与信息传递（消息推送）**

### 事件模型的五个组成部分

1. 事件的拥有者（event source，对象）
1. 事件成员（event，成员）
1. 事件的响应者（event subscribe，对象）
1. 事件处理器（event handler，成员）--本质是一个回调方法
1. 事件订阅---把事件处理器与事件关联在一起

```c#
using System;
using System.Timers;

namespace Cs_teat
{
    public delegate double Calc(double x, double y);
    class Program
    {
        static void Main(string[] args)
        {
            var timer = new Timer();
            timer.Interval = 1000;
            var boy = new Boy();
            timer.Elapsed += boy.Action;
            timer.Start();
            Console.ReadKey();
        }
    }
    class Boy
    {
        internal void Action(object sender, ElapsedEventArgs e)
        {
            Console.WriteLine("Jump!!!");
        }
    }
}


using System;
using System.Windows.Forms;

namespace Cs_teat
{
    public delegate double Calc(double x, double y);
    class Program
    {
        static void Main(string[] args)
        {
            MyForm form = new MyForm();
            form.Click+=form.Action;
            form.ShowDialog();
        }
    }
    class MyForm : Form
    {
        internal void Action(object sender, EventArgs e)
        {
            this.Text = DateTime.Now.ToString();
        }
    }
}

```

### 自定义事件的声明

- 事件声明：
  - 完整声明
  - 简略声明（字段式声明，field-like）
- 有了委托字段/属性，为什么还需要事件：为了使程序逻辑更加完善
- <span style="color:red">事件的本质</span>是委托字段的一个包装器
  - 这个包装器对委托字段的访问起<span style="color:red">限制作用</span>，相当于一个“蒙板”
  - 封装的一个重要功能就是隐藏
  - 事件对外界隐藏了委托实例的大部分功能，<span style="color:red">仅暴露添加/移除事件处理器的功能</span>
- 用于声明事件的委托类型的命名约定
  - 一般命名为 XXXEventHandler （除非是一个非常通用的事件约束）
  - XXXXEventHandler委托的参数一般有两个（由Win32 API演化而来）
  - 触发XXX事件的方法一般命名为OnXXX，即“因何发生”
    - 访问级别为protected，不能为public
- 事件的命名约定
  - 带有时态的动词或者动词短语

```csharp
using System;

public delegate void MyEventHandler(object sender, EventArgs e);

class MyClass
{
    public event MyEventHandler MyEvent;

    public void DoSomething()
    {
        Console.WriteLine("Do something...");
        MyEvent?.Invoke(this, EventArgs.Empty);
    }
}

class Program
{
    static void Main()
    {
        MyClass obj = new MyClass();
        obj.MyEvent += MyEventHandlerMethod;

        obj.DoSomething();

        Console.ReadLine();
    }

    static void MyEventHandlerMethod(object sender, EventArgs e)
    {
        Console.WriteLine("Event handled.");
    }
}

```

​	在这个例子中，创建了一个自定义事件 `MyEvent`，并在 `DoSomething` 方法中触发事件。在 `Main` 方法中，创建了一个 `MyClass` 实例并注册事件处理程序。在调用 `DoSomething` 方法时，事件处理程序将被调用。

# C#委托

功能：一个函数或一组函数的封装器

声明委托：使用`delegate`关键字，与类平级

```csharp
public delegate int MyDelegate (string s);
```

### 委托的多播(Multicasting of a Delegate)

委托对象可使用<span style="color:red"> "+" 运算符进行合并，"-" 运算符可用于从合并的委托中移除组件委托</span>，只有相同类型的委托可被合并。

```csharp
using System;

namespace Cs_teat
{
    class Program
    {
        static void Main(string[] args)
        {
            double a = 2342.31;
            double b = 223.1;
            var calcutating = new Calculate();
            Cal operation = new Cal(calcutating.Add);
            double c = operation(a, b);
            Console.WriteLine(c);
            operation += calcutating.Sub;
            double d = operation(c, b);
            Console.WriteLine(d);
        }
    }
    delegate double Cal(double x, double y);
    class Calculate
    {
        public double Add(double x,double y)
        {
            double result;
            return result = x + y;
        }
        public double Sub(double x,double y)
        {
            double result;
            return result = x - y;
        }

    }
}

```

==Action委托：==用于参数列表为空，返回值为空

==Action<>委托：==用于参数列表不为空，返回值为空，例如：`Action<string> action=new Action<string>(SayHello);`

==Func<>委托：==用于参数列表不为空，返回值不为空，例如：`Func<int,int，double> func=new Func<int,int，double>`返回值为double类型，参数都为int类型

# C#抽象类与开闭原则

​	1. 抽象类是一种特殊的类，<span style="color:red">它不能被实例化，只能被用作其他类的基类</span>，提供一个或多个抽象方法或抽象属性。

​	2. 抽象类通过 `abstract` 关键字来定义，不能直接实例化。

​	3. 派生类必须实现抽象类中的所有抽象成员，否则编译器会报错。

- 具体类-->抽象类-->接口：越来越抽象，内部实现的东西越来越少
- 抽象类是<span style="color:red">未完全实现逻辑的类</span>（可以有字段和非public成员，他们代表了”具体逻辑“）
- 抽象类为复用而生：专门作为基类来使用，也具有解耦功能

**==开闭原则：==**封装确定的，开放不确定的，推迟到合适的子类中去实现
