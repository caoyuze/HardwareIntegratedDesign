# 更新日志
### v0.1 使用宏定义
1、在实验4的基础上加入defines.vh头文件，引入宏定义，修改alu.v、aludec.v、maindec.v等模块的代码以搭配宏定义。<br>

### v0.2 实现8条逻辑运算指令
1、添加zeroext模块以搭配I型逻辑运算指令，仿照signext的连线连接zeroext。<br>
2、微改datapath，将相应使用到的mux2改为mux3（增加了零扩展立即数）。<br>
3、将alusrc更改为2位，以满足3个srcb来源。<br>
4、通过了给定的逻辑运算指令测试。<br>

### v0.3 实现6条移位指令
1、修改部分alu、aludec、maindec等模块的代码，以实现移位指令。<br>
2、通过了给定的移位指令测试。<br>

### v0.4 实现4条数据移动指令
1、增加hiloreg寄存器。<br>
2、更改部分datapath连线，添加各个阶段的hiloreg控制信号与输入输出，并传至触发器。<br>
3、修改部分alu、aludec、maindec等模块的代码，以用来判断具体的数据移动指令。<br>
4、通过了给定的数据移动指令测试。<br>

### v0.5 实现14条算数运算指令
1、使用以给定的divider除法模块。<br>
2、增加divdec模块，以进行与除法相关的信号量的赋值。<br>
3、修改部分datapath连线，同时增加进行除法时需要的暂停信号至F、D、E阶段。<br>
4、通过了给定的运算指令测试。<br>

### v0.6 BUG修复
1、修复了除法指令在运算过程中会将hilo寄存器的hi和lo均置为0的小bug（虽然不影响最终结果）。<br>

### v0.7 实现8条分支指令
1、修改部分datapath连线，增加分支指令需要用到的相关信号量，增加器件以实现分支指令。<br>
2、增加eqcmp模块中的对分支指令的是否跳转的信号判断。<br>
3、通过了给定的分支指令测试。<br>

### v0.8 实现4条跳转指令
1、修改部分datapath连线，增加跳转指令需要用到的相关信号量，增加多路选择器和触发器等器件以实现跳转指令。<br>
2、修改maindec模块中controls信号量的写法，更加方便修改和找信号量。<br>
3、通过了给定的跳转指令测试。<br>

### v0.9 代码风格改动
1、修改代码的对齐方式，更加美观，同时也更加方便查找。<br>

### v1.0 实现8条访存指令
1、增加memsel模块，用来判断读写字、半字和字节时需要读写的位，并将相应的接口连接起来，理论上相当于在data memory和datapath之间加了一个memsel的中介器件。<br>
2、将datapath中的部分信号推到相应指令需要用到的最晚阶段，方便后期大量指令一起测试时的仿真查看信号。<br>
3、通过了给定的访存指令测试。<br>
4、通过了给定的访存指令与分支跳转指令的混合测试，说明冒险模块没问题。<br>

### v1.1 对接龙芯SRAM接口的CPU、分块大量指令测试
1、完成与龙芯的SRAM接口形式CPU的代码对接。<br>
2、根据生成的golded trace进行指令测试，查看通过情况。<br>
3、在测试时发现D阶段数据前推漏写一种情况，之后更正冒险模块，并增加4路选择器，以用于D阶段的数据前推。<br>
4、更正数据前推后，3个部分的大测试最终均已通过。<br>
4、优化部分代码逻辑，使代码更清晰，同时删掉不必要的之前用于方便调试bug而向后推的几个阶段的分支跳转相关信号量。<br>

### v1.1.1 跑总测试、BUG修复
1、由于之前分块测试时只更换了Coe文件，然后未重置inst rom的IP核，导致生成的golden trace文件数据还是第1个小测试的数据，导致实际上跑的3个测试都是第1个小测试，于是更换总的Coe文件后重置IP核，并跑大的89个测试点的总测试。<br>
2、根据得到的错误提示，找相应的指令与相关信号，最终修复了若干bug，最终通过了位加自陷指令与特权指令时应该通过的64个测试点。<br>

### v1.2 实现5条自陷与特权指令、跑总测试
1、增加5条指令的相关控制信号与通路中使用到的量，以用来判断相关指令和进行相关信号的传输。<br>
2、加入给定的cp0模块，并进行相关的连线，同时在冒险模块中增加mfc0与mtc0指令需要的数据前推判断。<br>
3、增加exceptiondec模块，用于触发例外指令时的异常判断与pc重置至例外处理地址（bfc00380），同时放弃pcreg使用flopenrc模块，增加pc模块，用于触发例外时跳转为例外处理入口地址。<br>
4、在datapath模块中增加exception[7:0]的各个阶段的量，每1位用于存放不同的异常，在相关异常出现的阶段将exception用于存放相应异常的位置为1，最终在M阶段传入exceptiondec模块来判断产生了什么异常。<br>
5、当出现异常时需要将各个阶段的推数据的触发器清空，因此增加相关清空的信号量，并在hazard模块中增加相应的清空信号判断。<br>
6、完成了这5条特殊指令后，进行了小的指令测试并通过了基本的测试。<br>
7、连接上龙芯的SRAM CPU接口，进行总的测试，最终通过了应该通过的76个测试点。<br>
8、综合代码后，进行主频测试，最终测试结果为63.1MHz。<br>

### v1.3 现场加指令、使用新Coe重新跑总测试
1、现场加了1条max指令，并通过了测试。<br>
2、跑新的总测试，第1次发现还是停在了第77个测试点（软件中断测试），后来通过调试发现是datapath中未声明causeout变量，导致exceptiondec模块中的异常类型判断错误，解决这个bug后就通过了全部的89个测试点。<br>
3、将regfile的clk触发边沿从以前的下边沿改为上边沿，直接将主频提升到了89.736MHz。<br>