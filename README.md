# IMPC-Dev-Log
## 20180304
1. 系统程序顶层设计
   1. 程序流程图
   2. 程序module/type/subroutine
## 20180305
1. 底层type实现：pump/pipe/IHX
## 20180306
1. 底层type理清value和subroutine,实现；
   1. 转到NOTEPAD++
   2. 调用需要的模块：constant模块，property模块
   3. 调用水力学计算模块：De  
   思考：管道轴向分段有意义吗？管道不做热工计算，各处温度相同，密度相同，流通面积相同，沿程阻力系数相同。没意义。
   将pipe中的N去掉，alloc和free函数也备注掉，并计算de
   写system类，写pre函数，set和init
   写pre函数和cal函数，大的函数不写在类里面，单独写driving
## 20180307
purpose:完成流量计算的初步调试；  
1. 写pre函数和计算主函数、流量计算模块函数，程序整体计算；    
预处理：读输入卡，设置几何参数和初始参数，需要学习输入卡的编程；  
画出回路示意图，在示意图上标号程序中选用的控件；上午写完输入卡；完成预处理程序；  
搞定输入卡！！！输入卡的函数，下午写计算主程序；
新做简单输入，写好主程序，重要的事情先做  
看懂、修改daisy的输入卡程序；  
先写主函数，并写测试模块，用测试数据验证；  
设置设备指针指向不同的设备；  
geom,mesh那一级可以去掉，没有实际用处，仅供自己理解用；  
简化：去掉loop类，如果用loop，则pre，cal函数都写在type中，调用时在主函数中实例化，调用函数即可，无需global变量；  
为了与daisy中的程序风格一致，不设置loop类，通过global中定义的形式实现共享loop中的数据；  
两种共享数据的类型：通过module共享数据VS通过type共享数据；  
改变的原因，用loop调用的时候太啰嗦。用type的好处是可以设置一些私有变量，然而现在所有的变量都是public的，在用得到loop的地方，use  
global也一样可以实现功能，因此没有必要使用loop。  
去除loop；  
密度是否要用平均密度？比如换热器轴向分为10段，看成10个单独部件，局部损失系数也平均化，相当于在求局部阻力的时候，K有平均化求解；  
在各个部件中hydraulic中增加beta值；  
输入泵的额定参数，输出流量，对于各个部件只需要知道sum(L/A)  
beta不变的前提是rho不变，如果rho变则beta会变be    
**rho不变时可以用泵额定工况来代替beta，当rho变化时，需要用K、fric来计算beta**
