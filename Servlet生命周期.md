# Servlet的生命周期

Servlet在被第一次调用时加载进入内存，在服务服务器关闭时销毁，所以如果没有进行其他配置，Servlet的生命周期为：第一次被加载进内存--->服务器关闭<br/>
但是在Web.xml中可以对Servlet进行配置，通过load-on-startup进行配置，如果配置了load-on-startup，那么Servlet的生命周期为：服务器启动---->服务器关闭，
而在load-on-startup中的数字则代表了加载顺序
