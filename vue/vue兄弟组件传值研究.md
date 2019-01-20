vue中兄弟组件传值总结
=====
近期在书写一个vue项目时，遇到了一个兄弟组件之间传值的问题。场景如下

主体界面

![index组件](https://raw.githubusercontent.com/lstao/file/master/vue1.png)

需求界面

![需求界面](https://raw.githubusercontent.com/lstao/file/master/vue2.png)
在查阅了父子组件相关的资料之后，自身想到了一种解决方案：首先子组件通过$emit()调用父组件index中的监听方法 ，然后父组件的监听方法再通过我们指定给中间主体内容ref，直接调用search的方法，达到传值的目的，具体代码如下<br>


#
父组件内容，重点就是先通过普通的子组件调用父组件的方式，在父组件中的子标签（这里是my-header组件）中监听该自定义事件（searchfromHeader）并添加一个响应该事件的处理方法（search）

父组件内容

![index组件](https://raw.githubusercontent.com/lstao/file/master/vue3.png)

子组件header中为调用父方法的自定义方法如下

![header组件](https://raw.githubusercontent.com/lstao/file/master/vue4.png)


#
中间组件，要指定其ref，之后在父组件指定的处理方法(search)中可以直接通过$refs的方式，直接使用该子组件的方法

父组件index中响应方法

![index组件](https://raw.githubusercontent.com/lstao/file/master/vue5.png)

中间组件search中的相应方法
![search组件](https://raw.githubusercontent.com/lstao/file/master/vue6.png)





#
最终在中间组件search，也就是我们需要获取值的组件中调用test方法，即拿到了我们需要的值
结果如下

中间组件search中的相应方法

![结果](https://raw.githubusercontent.com/lstao/file/master/vue7.png)


总结
=====
这里只提供了一种思路，专业方向上不是前端，只是在边用边学，可能会有一些理解上的偏差，但是大概就是就是上述，实现起来还是很简单
