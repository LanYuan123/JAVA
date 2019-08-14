# Swing基础学习
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
