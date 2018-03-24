# IMPC-Dev-Log
## 20180324
* 继续调试耦合的loop,目前已经发现的问题：
  * Tfout有问题；
  * 输出与后处理；
  * 热工求解AX=B的优化；
  * 稳态系统耦合的收敛因子
  * 吃饭之前上交一个与daisy完整耦合的例子；
## 20180323
* daisy输入卡中遇到的问题
 * th_configure对应的是材料层数还是组件个数？反射层组件不参与计算
* 点堆的接口：pkmodel_run();driver_transient_pk?
  * 找西交要点堆模型的输入卡
* 调试运行3D-loop，问题在于不能debug->在服务器上debug
  * 源代码通过复制粘贴；工程文件通过cmake;
  * debug查看是否由于is_pass不为T导致；
  * 稳态计算输出参数，交服务器；
  * 通过输出计算过程的方式了解计算进程
  * 稳态NK2TH无限迭代，查看迭代收敛条件
  * 本机进行4核调试计算，服务器安装调试环境；
* 程序的一些优化
  * loop水力学传递daisy中的数据
  * 调研Nusselt计算公式


## 待办事项 
* 调研选定换热器中water和LBE的对流换热公式； 
* 验证换热器的计算；  
* 换热器程序代码优化；  
* 需要有二回路才能模拟全厂断电；
* 先把IMPC-v1.0重新编译起来，看懂cmkae语句；
* 手动建立工程并尝试编译；
* 写一个perform_TH_loop:将perform_TH_IMP中的driving_imp_steady改为driving_imp_loop
* driving_imp_steady在driving_loop中调用（需要做一定修改）
* 查看IHX和PIPE中Q的来源：从输入卡输入，因此一回路需要输入同样的流量；
* 写好瞬态，用点堆进行调试热工反馈模块；在输入卡中加入选项，选择需要的热工反馈模型；
* 测试一下present函数
* 查看点堆的运行与热工反馈流程；core类在水力学计算中有用；先保留core类；
* 耦合前处理； 
## 软件可以进行的改进
* 计算流量时考虑密度变化；
  冷却剂放在一个类内？
---
## 20180322-imp_热工模块修改
* 尝试IMPC-loop的运行
   * 在loop模块写print保证运行到
   * 写loop的后处理输出，输出堆芯进出口温度，换热器进出口温度
   * 从inputcard中拆分出I/O模块
   * 查找点堆的运行方式
   * 合并re_input.txt和loopinput.txt
   * 输出运行日志；输入以及比较清晰；输出和I/O两块需要打通一下；
   * 改了is_imp,is_loop，set assembly的地方就出错？如果选用is_inner如何
   * 合并输入卡：input.case,re_input.txt,loopinput.case
* 修改IMPC中的imp热工模块；
* core的几何输入由daisy网格读取；
## 20180321耦合代码编写
* 初步完成代码耦合；
* 在Imp_drving_THcore中写输出温度的反馈，耦合边界条件；
* 在imp_loop_interface.f90中写对物理的温度反馈；
* 提交计算；取消core的声明和分配变量等等；
## 20180319
  写瞬态计算函数和测试模块；先实现后优化，过早的优化是灾难性的。
  验证换热器的计算；
  输入卡增加time分类，包含ttotal，Nt;
  输出瞬态计算的计算结果t,Tpout,Tsout
  ## 20180319 
  下午编写pipe热工计算模块：首先整理IHX中的函数以及依赖关系；  
  写pipe的测试模块；
  耦合一个回路进行计算，堆芯暂时用加热源的pipe代替；
  先给出一个延时效应的word，再给出系统耦合软件；
  写一个模拟的堆芯类，在pipe的基础加热源。  
  ## 20180320
  耦合daisy和系统模块；
  所有的代码放到一个地方，有git来统一管理，便于寻找版本；  
  在Imp_loop_interface中添加热工反馈；
  daisy中选用点堆模型；
 ## 20180318
  purpose:完成IHX的调试；编写pipe；linux/Ml;  
  温度出现负值:
  等效模型的半径；
  矩阵求解模块的问题？测试矩阵求解模块；
  矩阵求解模块没有问题，info==0,也说明数学求解没有遇到问题；
  那就是系数和源项矩阵的表示问题？
  稳态的系数矩阵和源项矩阵的表达方式；
  检查系数矩阵的推导和表达；
  矩阵的表达没有问题，推导问题？
  矩阵的对应关系没有问题，查看推导问题，正负号；  
  查看公式推导是否有问题；
  精度问题？应该要到左边去
  写瞬态
  打印出A,B
## 20180317
  上午完成IHX热工计算函数的编写调试编译；
  下午IHX测试与调试；
  晚上，指定ML学习计划和熟悉linux;
  争取组会之前能够完成IHX和pipe的调试；
  写apz=0情况下的计算
  应为要循环，所以把求解部分放到单独的求解器中：cal_thermal_kerel(this)  
  选择合理的参数
  乘以换热面积；
  vis为0.0
## 20180316
  上午推导换热器的离散公式（9:15-10:15)；  
  整理A矩阵并编程求解；
  晚上看看能否用mathematics进行符号推导；  
  iHX thermal 函数编写；  
  先根据代码草稿把所需要的输入变量都在IHX类中写好，再继续写代码  
  16：48-17：10改好IHXtype并调试；  
  输入卡增加物性填写模块：solid  
  写一个计算对流传热系数的函数；  
  分析稳态计算所需要的参数，先写稳态计算函数；  
  计算模型中采用的流量应该是单管的流量；
## 20180315
  搞清楚单管模型中各种热工参数的意义；编程实现运算；  
  z方向的调整；
  重新推导各项公式；
## 20180314
  purpose:完成IHX的热工模块；制定coursear学习计划（一鼓作气）；制定linux迁移计划（先部署好开发环境）；
  上午部署lapack模块，晚上制定两个计划；
  self_DGESV:
  fortran中外部静态库的使用；lapack的测试使用；学习些CMAKE
  interface的用法
  外部函数写在module里面可以不用写显式接口；有利于公共数据的分配；
  用lapack求解线性方程组;搞清楚线性方程组求解中的PLU；
  测试线性方程组的求解；
  直接写IHX的cal_thermal;
  写伪代码；实现IHX thermal并调试；
  推导换热系数公式：先给出稳态解
## 20180313  
  整理巴西、华电、西交的换热器、管道稳态热工算法，找出合理的进行实现；
  上午读完巴西、华电、西交的管道、换热器模型，给出一个自己的解决方案；（厚积而薄发）
  下午实现管道和换热的热工程序；
  晚上写好简历，courear课程；
  对换热器数学模型进行积分离散；
  先检查计算过程，保证公式无误，再列出矩阵调用求解器进行求解；
  学习使用lapack；
  制作好投飞常准的简历；20:45-21:30
## 20180312
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
## 20180309
  pupose:完成输入卡的编写；让高育翠师姐看一下daisy中点堆模型的运行机制，是否与采用同一套的网格；
  编译通过新的console
  研究daisy，read卡和scan卡
  找FILES的定义以及CASENAME的含义。file_tp::FILES
  open_file函数中file_unit的含义
  open(newunit=file_unit, file=file_name, status='old', action='read', iostat=io_error)
  files%casename记录的是I/O单元号
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
## 20180306
1. 底层type理清value和subroutine,实现；
   1. 转到NOTEPAD++
   2. 调用需要的模块：constant模块，property模块
   3. 调用水力学计算模块：De  
   思考：管道轴向分段有意义吗？管道不做热工计算，各处温度相同，密度相同，流通面积相同，沿程阻力系数相同。没意义。
   将pipe中的N去掉，alloc和free函数也备注掉，并计算de
   写system类，写pre函数，set和init
   写pre函数和cal函数，大的函数不写在类里面，单独写driving
## 20180305
1. 底层type实现：pump/pipe/IHX
## 20180304
1. 系统程序顶层设计
   1. 程序流程图
   2. 程序module/type/subroutine  
 











  

  
