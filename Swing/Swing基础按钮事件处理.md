
   ## Swing按钮事件处理
      当我们向窗体中添加了按钮之后，我们点击按钮时，希望点击按钮，能触发事件，那么事件怎么样绑定到按钮上呢？
      这就要使用监听器了
      
   ## 监听器Listener
   
   步骤：</br>
   1. 创建监听器对象listener
   2. 将监听器对象交给按钮
   3. 当按钮被点击时，Swing框架会调用监听器对象里的方法，进行处理
   
   语法：</br>
      1. 创建一个类实现**ActionListener**接口</br>
      2. 在该类中必须覆写**actionPerformed()方法**</br>
      3. **addActionListener()方法**添加监听器
     
![监听器](https://github.com/LanYuan123/JAVA/blob/master/Swing/img/%E7%9B%91%E5%90%AC%E5%99%A8%20.png)

  ## 匿名内部类对监听器使用的优化
  
  我们在使用监听器的时候，都会创建一个类来实现ActionListener接口，我们在开发的时候会使用很多个按钮，要是用很多个只用一次的内部类，就会写很多，为了解决这个问题，我们可以使用匿名内部类来优化
  
  ![匿名内部类](https://github.com/LanYuan123/JAVA/blob/master/Swing/img/%E5%8C%BF%E5%90%8D%E5%86%85%E9%83%A8%E7%B1%BB.png)
  
  ## 再优化：使用lambda表达式
  
  我们在使用匿名内部类的时候，虽然已经进行了一次简化，但是使用匿名内部类时，还是会有大量相同的代码，为了更进一步的优化代码，我们可以使用lambda表达式
  ![lambda表达式](https://github.com/LanYuan123/JAVA/blob/master/Swing/img/lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F.png)
