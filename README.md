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
