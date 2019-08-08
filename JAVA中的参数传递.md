# JAVA中的参数传递
:question:&nbsp;&nbsp;&nbsp;在JAVA的学习中，我们或许经常性的会遇到把对象作为一个参数传递给方法，那么自然就会产生一个问题，这到底是值传递，还是引用传递呢？</br>
或许很多人都会觉得是引用传递，但其实我们都错了！

:heavy_check_mark:答案是：**对象作为参数传递是值传递，并且在JAVA中，:heavy_exclamation_mark::heavy_exclamation_mark::heavy_exclamation_mark:只有按值传递，没有按引用传递！**

-----
我们在回答为什么之前，先来看看**形参**是什么，**实参**是什么，什么是**值传递**，什么又是**引用传递**

**形参：是在定义函数名和函数体的时候使用的参数,目的是用来接收调用该函数时传入的参数。**

**实参：在调用有参函数时，主调函数和被调函数之间有数据传递关系。在主调函数中调用一个函数时，函数名后面括号中的参数称为“实际参数”。**

**值传递（pass by value）：在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。**

**引用传递（pass by reference）：在调用函数时将实际参数的地址直接传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。**

------

在弄清楚了这些概念之后，我们现在通过代码实践的方式，来看看JAVA中到底是值传递还是引用传递:question::question::question:

首先我们来看这样一段代码：</br>
:one:
```
public class Test {
    public static void main(String[] args) {
        Test test = new Test();
        int i = 1;
        test.change(i);
        System.out.println(i);
    }

    public void change(int i){
        i = 2;
        System.out.println(i);
    }
}
```
运行结果：
```
2
1
```

从这个运行结果来看，change方法内改变i的值并没有影响到mian方法中的i，由此来看应该是值传递，那我们在来看一段实例代码</br>

:arrow_down:</br>

:two:
```
public class Test {
    public static void main(String[] args) {
        Test test = new Test();
        User user = new User();
        user.setName("first");
        test.change(user);
        System.out.println("main方法内的name：" + user.name);
    }

        public void change(User user){
        user.setName("second");
        System.out.println("change方法内的name："+user.name);
    }
}
```

这是运行结果：
```
change方法内的name：second
main方法内的name：second
```
从这样的结果来看，在change方法体内对user对象进行的操作已经影响到了main方法体内的user对象

:three:
