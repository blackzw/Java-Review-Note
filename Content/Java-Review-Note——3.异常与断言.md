# Java-Review-Note——3.异常与断言

标签： JavaStudy


----------

## 异常

**什么是异常**：
> 
程序运行过程中出现的不正常现象，比如：除法运算，除以0，这个就是算术异常；
**记住一点**：**异常是运行时的程序出错**，**编译时检查的只是语法错误**，不运行何来的异常？

**为什么会出现异常**：
> 
程序运行过程中由于各种因素导致程序运行发生错误，比如：用户误操作，代码本身错误，系统环境
因素等；在处理异常时需要对异常进行判断，针对不同的异常类型采取不同的处理措施。

**异常的分类**：

![][1]

**常见异常信息总结表**：

|异常类型|含义|
|:-|:-|
|**ArithmeticException**	|算术异常|
|**ArrayIndexOutOfBoundsException**	|数组下标越界异常|
|**ArrayStoreException**	|数组元素类型不兼容eg:为整型数组赋浮点型的数|
|**ClassCastException**	|类型强制转换异常(精度丢失)|
|**ClassNotFoundException**	|找不到对应类异常|
|**CloneNotSupportedException**	|调用clone()方法,但没有实现Cloneable接口(ps:数组默认实现该接口)|
|**EOFException**	|文件已结束异常|
|**FileNotFoundException**	|找不到对应文件异常|
|**IllegalAccessException**	|否问某个类被拒绝时的异常(eg:在别的类中访问private修饰的类)|
|**IndexOutOfBoundsException**	|下标索引越界异常(eg:数组int a[5],访问a[5]);就会触发该异常,因为下标从0开始|
|**InstantiationException**	|使用newInstance()方法创建抽象类或借口实例抛出的异常|
|**IOException**	|输入输出异常|
|**NullPointerException**	|空指针异常,这个是比较常见的,eg:对象没有实例化直接调用属性或者方法|
|**NumberFormatException**	|数字格式转换异常,eg:"1+2"这个是字符串,转化为数字就报错了|
|**NoSuchFieldException**	|字段未找到异常|
|**NoSuchMethodException**	|方法为找到异常|
|**NoSuchElementException**	|元素未找到异常(eg:调用StringTokenizer的nextToken()方法可以出现)|
|**SecurityException**	|安全异常问题,可能需要修改权限|
|**SQLException**	|数据库操作异常|
|**StringIndexOutOfBoundsException**|	字符串索引越界异常|
|**MalformedURLException**|	URL配置异常(可能是URL协议,格式,或者路径错误;jar包问题,去掉gnujaxp.jar包引用即可)|
|**UnknowHostException**|	域名解析出错异常|
|**IllegArgumentException**|	向方法传递了一个不合法或者不合理的参数|
|**IllegalStateException**|	违法的状态异常,调用了某个不处于合法调用状态的方法(eg:调用已经销毁的方法)|

**检验异常与非检验异常**：
> 
- **Error类**：表示一个程序错误，指的是底层的，低级的，不可以恢复的严重错误；
此时的程序一定会退出，因为已经失去了程序运行所需的物理环境，我们无法进行处理。
- **Exception类**：分为检验异常和非检验异常
> 
**RuntimeException及其子类都是非检验异常**，其他的异常均为检验异常,需要进行捕获
> 
- **检验异常**：程序代码中需要进行捕获的异常,(try-catch或者throws + try - catch)
- **非检验异常**:因为没有进行必要的检查，由于疏忽或错误引起的异常，一定属于虚拟机内部异常
(eg:空指针异常)

**异常的处理**：

**1.异常的捕获**

**try-catch块**：catch块必须与try块一起使用，不能单独使用；例子：数组越界异常

```
class Test {
	public static void main (String[] args) {
		int arr[] = {1,2,3};
        try { System.out.println(arr[4]+""); 
        } catch(ArrayIndexOutOfBoundsException e)  { System.out.println("数组越界了！"); }  
        System.out.println("呵呵");  
	}
}
```
**运行结果**：数组越界了 呵呵
如果没加try-catch的话，数组越界的时候程序会直接终止，不会打印出呵呵。
而加了try-catch，如果发生了异常，那么就会执行catch里面的方法。

**finally块**：同样需与try块一起使用，无论什么情况，finally块中的内容一定会执行，
我们一般在finally块中执行系统资源清理和释放的工作，比如文件，数据库关闭等。
**有一点要注意**：如果在catch块中调用了System.exit(0)；方法，那么finally部分
的代码不会被执行,因为程序已经终止了。

**多重catch块**：使用多个catch块，依次捕获不同的异常；
其实只有一个catch块被执行，要把子类异常的catch需放在父类异常的catch前面！

**2.异常声明**
> 
**throws回避异常**：当一个方法可能会引发某种异常时，但是我们又不想在该方法中
对该异常进行处理；可以用throws将异常抛出，当我们调用该方法时再对异常进行捕获。

**3.异常抛出**
> 
**throw显式引发异常**：一般用于程序调试或者抛出用户自定义异常。

**自定义异常**：
> 
系统提供的异常类不一定能捕获所有的错误异常，在这种情况下，我们 可以根据
自己的需要，自定义异常类型，需注意以下三点：
> 
- 1.需继承Exception类
- 2.需要使用throw抛出自定义异常
- 3.需要使用try-catch对异常进行捕获

**示例代码**：

```
class MyException extend Excetpion {
    public MyException() { super("自定义异常：出书不符合规定"); }
}
public class ExceptionTest {
    public static void main(String[] args) {
        int a = 2;
        int b = 1;
        //如果满足条件就throw异常，同时用try-catch进行捕获
        if(b == 0 || b == 1) {
            try {
                throw new MyException();
            } catch(MyException e) {System.out.println(e.getMessage();)}
        }
    }
}
```


----------

## 断言

**什么是断言：**
> 
jdk 1.4后引入的Assert(断言)，允许在代码中代码中添加一些合法的语句以用于
调试程序；在assert制定的布尔表达式条件不成立时会抛出一个AssertionError
对象，直接终止程序的运行。

**使用断言的好处：**
> 
一种错误处理机制，在程序开发与测试阶段使用；
可以理解为代替if-else或try-catch，这些东西对程序性能是有一定影响的，
完成调试后你还要一个个把他删除掉，如果用断言，则是最低代价。
另外：①断言失败是致命,不可恢复的，②断言检查仅仅用于程序开发测试阶段

**断言的使用：**：
> 
**assert 条件** 或 **assert 条件:错误信息**
第二种方式，如果前面的条件不成立的话，那么后面的字符串作为AssertionError错误输出信息

**断言的开启：**默认情况下断言是关闭的,要通过**enableassertions**或**-ea**来启用断言功能;

**断言的关闭**：使用**disableassertions**或**-da**来关闭断言功能

 代码示例：
 
```Java
public class AssertDemo {  
    static void test(int num)  
    {  
        //如果num <= 0的话,抛出AssertError,显示错误信息  
        assert(num >=0):"传入参数需要大于0";  
        System.out.println("参数为正数！");  
    }  
    public static void main(String[] args) {  
        test(-7);  
    }  
}  
```

运行截图：

![][2]



----------


  [1]: http://static.zybuluo.com/coder-pig/33yf5zb4rczhapiux4o3vljv/7.png
  [2]: http://static.zybuluo.com/coder-pig/llpdibhx14hl77sis9one781/image_1atd1ebah1q40e4f19r91d5d7j49.png