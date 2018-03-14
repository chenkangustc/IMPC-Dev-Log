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
## 20180308
妇女节~~  
purpose:完成一整套泵的计算程序，从输入到输出，输入模式可供选择；看下其他人论文中有关流量的描述；晚上看反应堆安全分析一书；  
        查看上交论文中密度不变的来源；  
想法：不考虑密度的变化，给出Q(t),然后将Q直接当做堆芯程序的边界条件给出，是否可行，需做哪些假设，结果如何？  
     relap5的使用时绕不开了。。  
     系统和部件的耦合问题，如何耦合？
     反应堆中冷却剂密度变化大小？水、铅铋；  
     流量计算的时候是否可以认为第一阶段的时候密度不变化；
     密度堆流量的影响有多少？  
两种模式的输入卡：一种密度不变，直接输入泵的额定H、Q、w、yita，回路sum(L/A)；先写好密度不变的输入卡；  
                一种密度变化，输入泵的额定Q、w、yita,回路sum(L/A),fric、L、De、K、A回路性能；  
先把自己的小程序实现，不考虑耦合的问题；  
自己的程序目前核心的程序是解决好输入模块，即输入卡的设计；两种可选择的输入模式；  
完成driving_plain_read的解读和应用；  
研读输入卡，先把输入卡搞定，完成整个的流量计算程序；  
  ## 20180309
  pupose:完成输入卡的编写；让高育翠师姐看一下daisy中点堆模型的运行机制，是否与采用同一套的网格；
  编译通过新的console
  研究daisy，read卡和scan卡
  找FILES的定义以及CASENAME的含义。file_tp::FILES
  open_file函数中file_unit的含义
  open(newunit=file_unit, file=file_name, status='old', action='read', iostat=io_error)
  files%casename记录的是I/O单元号
  ## 20180311
  purpose:白天完成输入卡的编写；晚上修改简历，学习ML；  
  写好keyword；  
  耦合输入卡和流量计算核心；  
  密度是否变化对输入卡中PIPE、pump、IHX均有影响；  
  先把密度不变的输入卡写好耦合好；  
  再考虑密度不变的情况；  
  程序满足当前的需求即可，一步一步往前坐，不必设计太超前；  
  下载UE，方便输入卡的填写；  
  学习fortran中指针，通过针指向不同的设备，对设备模块进行编号，确定其顺序及管，这在后期的热工计算中也用得上；  
  学习fortran指针，写出指向不同设备的指针，在读取的过程中根据设备的num号来赋值；
  指针也不能指向不同的type;  
  先不管后续热工的计算，毕竟还没有完全搞清楚；  
  先把手头的水力学计算功能实现；  
  第一步是实现固定的reactor-pipe-IHX-pipe-pump-pipe  
  第二步实现pipe可以自动调节；  
  实现第一步读入功能；  
  将输入卡耦合到流量计算程序中去；  
  初始化变量的输入方法；  
  set变量输入完毕，接下来alloc和init变量，包括：热物性和温度、速度、密度这些。
# 20180312
  purpose:完成流量程序；梳理流量与能量方程耦合关系；中午写好简历；  
  写alloc和init函数；  
  在各个设备类中设置初始条件p/v/t/rho  
  在密度不变的情况，根据输入的数据计算流量，并进行后处理；  
  耦合输出；  
  按照上交的论文调整输入卡，验证输出的数值；  
  写好输出函数；  
  后处理：输出数值；  
  后处理：free数据空间，关闭IO文件；  
  先把流量计算的事搞定了在考虑温度计算的事；  
  先稳态后瞬态；  
  稳态的流量计算：直接输入参数  
  稳态的温度、密度、压力计算：核心在于温度计算；  
  关键问题：堆芯流量分配（耦合问题）和换热器的温度场计算；    
  第一步先写好换热器、PIPE、pump的热工计算模块;第二步：与堆芯程序耦合  
  写换热器的热工计算模块（已知流量、入口温度，求解能量方程得到稳/瞬态温度分布）  
  查看pipe和IHX的热工模型，写两个模型的热工计算函数；  
  参考巴西文献、华电文献、西交文献、上交文献；  
  ## 20180313
  整理巴西、华电、西交的换热器、管道稳态热工算法，找出合理的进行实现；
  上午读完巴西、华电、西交的管道、换热器模型，给出一个自己的解决方案；（厚积而薄发）
  下午实现管道和换热的热工程序；
  晚上写好简历，courear课程；
  对换热器数学模型进行积分离散；
  先检查计算过程，保证公式无误，再列出矩阵调用求解器进行求解；
  学习使用lapack；
  制作好投飞常准的简历；20:45-21:30
