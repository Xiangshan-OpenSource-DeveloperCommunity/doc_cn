![image.png](https://cdn.nlark.com/yuque/0/2024/png/942091/1711949457078-8928ffc3-be11-49b4-8a3b-0a263c0edd72.png#averageHue=%23182a3e&clientId=u07b9deb8-ad28-4&from=paste&height=304&id=u46500340&originHeight=608&originWidth=1080&originalType=binary&ratio=2&rotation=0&showTitle=false&size=688738&status=done&style=none&taskId=ubab2a2c2-4cfc-438f-80dd-bf4522d4182&title=&width=540)<br />原创 贾志杰、安旭 北京开源芯片研究院 _2024-01-04 12:00_ _北京_
<a name="akpKm"></a>
## 本次资源包升级内容

1. 升级了Docker，注意下载链接已更新
2. 增加了香山仿真演示
3. 增加了本次使用的香山工具AM、NEMU、difftest的详细解说
<a name="HvkK0"></a>
## 本次整合包适合以下人群使用
-零基础入门，没有芯片设计经验的人<br />-没有Linux操作系统<br />-不想在本地搭建运行环境<br />-想要一步到位，快速上手的心急派
<a name="Rc9Hn"></a>
## 特别说明

- 整合包基于高性能开源处理器香山制作，是香山团队倾情打造手把手教学。
- 这个整合包发布经过“一生一芯”同学们的实战检测，如果出现问题，应该是你自己的问题。但欢迎加入QQ群793255484，与小伙伴们一起探讨香山使用经验。
- 资源包在通过Docker形式提供了运行香山必须的Linux操作系统、香山开发环境、香山和仿真相关工具的代码，会大大降低自己部署环境和工具的难度，理论上比自己部署要方便。如果想自己配置，可以参看[本地使用香山](https://www.gitlink.org.cn/OSchip/HowtoUseXiangShan/issues/2)的文档自行安装。

相关链接：https://www.gitlink.org.cn/OSchip/HowtoUseXiangShan/issues/2
<a name="pC8F4"></a>
## 电脑配置需求（笔者电脑配置参考）

- 操作系统：Windows 11 
- CPU：不做强制性要求，但是性能越高越好
- 内存：推荐64G+，内存不够编译会炸掉
- 整合包推荐放在固态硬盘中
<a name="QJzkS"></a>
# I 通过Docker使用香山第一部分：准备工作

1. XS-Docker提供了啥、如何下载以及Docker使用学习
   1. docker提供：Linux操作系统Ubuntu 20.04、香山开发环境(软件包和编译工具链)、香山和仿真相关工具的代码
   2. 下载XS-Docker链接：

链接：[https://pan.baidu.com/s/1gOjHyyqU6DBcbqE8caEEaw?pwd=bv9t](https://pan.baidu.com/s/1gOjHyyqU6DBcbqE8caEEaw?pwd=bv9t) <br />提取码：bv9t 

   3. Docker安装使用帮助：[https://magic3007.github.io/docs/docs/Cheatsheet/docker/](https://magic3007.github.io/docs/docs/Cheatsheet/docker/) （Docker的使用说明非常多，此处是同学自己的学习记录，欢迎大家交流分享）
2. 运行XS-Docker的命令

注：以下命令中：#符号表示之后文字为注释，使用时不必复制；<>符号表示变量，需要替换为用户看到的实际值，符号本身不要保留
```bash
docker load --input <下载好的docker镜像路径>
docker images    # 执行后可以看到镜像信息xs-ubuntu:v2.2，大小为6.25GB
docker run -itd --net host xs-ubuntu:v2.2 /bin/bash
docker ps    # 执行后可以看到上一步运行的docker容器，记下<CONTAINER ID>
docker exec -it <CONTAINER ID> /bin/bash    # 使用前一步记录的<CONTAINER ID>

```
最后一步如果成功，命令行界面会变为：
```bash
root@ubuntu:/# 
```
这样就成功进入docker镜像提供的ubuntu界面。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/942091/1703579057700-4b7b12f2-36cc-4571-bdbd-f84c1fd20634.png#averageHue=%231a1a1a&clientId=ub85e56b1-df44-4&from=paste&height=442&id=u24df8449&originHeight=663&originWidth=1574&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=106697&status=done&style=none&taskId=u23ebe6c4-ca2d-40d5-92ed-b3862579c55&title=&width=1049.3333333333333)

<a name="VlGWG"></a>
# II 通过Docker使用香山第二部分：香山仿真演示
我们在 docker 镜像里提供了预先编译好的香山仿真模拟程序，可以用它来运行 coremark、linux_hello 等程序。<br />此处我们暂不解释各项参数的含义，只先熟悉一下仿真运行程序的整个过程。<br />从第一部分的最后，进入 ubuntu 界面开始：
```bash
# 进入香山目录
cd /root/XiangShan/

# 设置环境变量
export NOOP_HOME=$(pwd)
export NEMU_HOME=$(pwd)

# 仿真运行coremark
./ready-to-run/emu_master -i ready-to-run/coremark-2-iteration.bin --diff ready-to-run/riscv64-nemu-interpreter-so 2> perf.log
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/942091/1703579281786-1becd6f5-14b9-4abc-90b4-3ffdd9b48203.png#averageHue=%23131313&clientId=ub85e56b1-df44-4&from=paste&height=781&id=u88716f46&originHeight=1172&originWidth=2237&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=160418&status=done&style=none&taskId=uc0e64e0d-6418-4290-b356-23aa747f4a2&title=&width=1491.3333333333333)<br />运行成功可以看到类似下图的输出：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/942091/1703579459643-2a0f8dac-cac1-4cfd-aa22-39d665a77eed.png#averageHue=%23131313&clientId=ub85e56b1-df44-4&from=paste&height=440&id=ubed30f74&originHeight=660&originWidth=1444&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=83295&status=done&style=none&taskId=u9376b6a2-b923-4ef4-b37d-be1baeefd2e&title=&width=962.6666666666666)<br />如果电脑性能较差，可能在中间卡住较长时间。本人电脑（i5-1240P）小于5分钟完成。<br />#给大家展示下机主 CPU 狂烧的样子，嘎嘎O(∩_∩)O<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/942091/1703579701186-74c3e0e8-f02c-4a1e-b489-83b4a06c8a3e.png#averageHue=%23fafaf9&clientId=uf1a9986c-15ee-4&from=paste&height=274&id=Mkhue&originHeight=872&originWidth=1144&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=74368&status=done&style=none&taskId=uc6f6296a-e7ca-424b-889d-3b0a69b9adb&title=&width=359.66668701171875)![image.png](https://cdn.nlark.com/yuque/0/2023/png/942091/1703579809859-9ab2fdbe-7503-45e5-804f-b83346a1f94b.png#averageHue=%23e0b97d&clientId=uf1a9986c-15ee-4&from=paste&height=280&id=zgK40&originHeight=873&originWidth=1142&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=72858&status=done&style=none&taskId=u0b516bb7-d2ad-459b-b243-f21c6bbb8b7&title=&width=366.66668701171875)<br />如果电脑性能较好，可以尝试运行更复杂的 linux_hello 程序。
```bash
# 仿真运行linux_hello
./ready-to-run/emu_master -i ready-to-run/linux.bin --diff ready-to-run/riscv64-nemu-interpreter-so 2> perf.log
```
运行成功可以看到如下输出：<br />![屏幕截图 2023-12-26 171928-1111.png](https://cdn.nlark.com/yuque/0/2023/png/942091/1703584651676-f08e4112-a2e2-48a1-b3ad-924b2c6698a7.png#averageHue=%23151515&clientId=ub46157a9-0c1c-4&from=drop&id=u33d37d42&originHeight=1323&originWidth=2239&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=122485&status=done&style=none&taskId=ubfd08d60-ab82-4e57-8800-5f47738ab70&title=)<br />![屏幕截图 2023-12-26 172031---2.png](https://cdn.nlark.com/yuque/0/2023/png/942091/1703584656876-0d973871-97b2-4102-9df9-bb4a865f1c0e.png#averageHue=%23181818&clientId=ub46157a9-0c1c-4&from=drop&id=uc8828ab0&originHeight=1327&originWidth=2232&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=233661&status=done&style=none&taskId=ud63a8da8-6d33-41a5-8149-2822d55ccfb&title=)

<a name="E756b"></a>
# III 通过Docker使用香山第三部分：本次使用的香山工具解说
<a name="fvgb3"></a>
## AM
仿真时用到的 .bin 文件，是测试程序镜像，将其输入给 EMU 就可以模拟香山核运行该程序的过程。<br />获取 .bin 的方式由多种，最常用是通过 AM 来生成。<br />AM 是一个裸机运行时环境，我们可以使用 AM 来编译在香山裸机上运行的程序。<br />在 AM 的 apps 目录下提供了一些预置的程序源代码，比如 coremark、hello、microbench 等。下面以 coremark 为例，演示利用AM编译生成测试程序镜像的过程。

```bash
# 进入 AM 目录
cd /root/nexus-am/

# 设置环境变量
export AM_HOME=$(pwd)

# 进入 coremark 目录
cd $AM_HOME/apps/coremark/

# 编译 coremark
make ARCH=riscv64-xs
```
按 riscv64-xs 架构进行编译<br />![](https://cdn.nlark.com/yuque/0/2023/png/942091/1703585450349-74e3fdfc-0e15-4a66-9c23-cb53713473e3.png#averageHue=%23242424&clientId=ub46157a9-0c1c-4&from=paste&id=L62nO&originHeight=304&originWidth=1896&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u0a64e867-8382-4955-996a-1085a745ffb&title=)<br />编译完成后，可以看到新生成的文件夹 build<br />其中 riscv64-xs 目录为编译中间文件，其他三个是编译产物。<br />txt 是程序的反汇编文件<br />bin 是仿真所需的程序镜像<br />![](https://cdn.nlark.com/yuque/0/2023/png/942091/1703585451594-a89777c7-2b6e-49ef-b872-5fff10c25e91.png#averageHue=%231f1f1f&clientId=ub46157a9-0c1c-4&from=paste&id=uc6f4164e&originHeight=227&originWidth=982&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=uc5370987-ea70-4475-ba95-dc9c016f0d8&title=)
<a name="l3JBO"></a>
## NEMU
NEMU 是一个解释型的指令集模拟器，在仿真过程中它同样执行程序，为香山提供一个作为对照的正确结果。

```bash
# 进入 NEMU 目录
cd /root/NEMU/

# 设置环境变量
export NEMU_HOME=$(pwd)

# 按“给香山对照”进行设置
make riscv64-xs-ref_defconfig

# 编译 NEMU
make -j
```
第一步：make riscv64-xs-ref_defconfig<br />![](https://cdn.nlark.com/yuque/0/2023/png/942091/1703585816553-dc444115-c247-4dc4-9b71-2c59659ca6c0.png#averageHue=%23232222&clientId=ub46157a9-0c1c-4&from=paste&id=u31df14e1&originHeight=556&originWidth=1424&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u1cdaf28d-4705-44f4-83f3-defaec3fd50&title=)<br />第二步：make -j<br />![](https://cdn.nlark.com/yuque/0/2023/png/942091/1703585816958-beae5681-33b6-4365-8650-1e6e1b339671.png#averageHue=%232f2f2f&clientId=ub46157a9-0c1c-4&from=paste&id=u737ea0a6&originHeight=226&originWidth=1058&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u299c928c-1303-4690-903a-1d37e8ba135&title=)<br />完成这两步，才是完成“给香山对照的 nemu”的编译<br />编译生成的结果，在 build 目录的如下位置<br />![](https://cdn.nlark.com/yuque/0/2023/png/942091/1703585816913-cdb2621a-9e32-4a13-8865-8f97c90fe3e9.png#averageHue=%232b2b2b&clientId=ub46157a9-0c1c-4&from=paste&id=u0cbfe479&originHeight=194&originWidth=1886&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u4a0d6bed-18b1-43fc-bbfb-72ddfc4d223&title=)

<a name="FrsPE"></a>
## difftest(差分测试)
difftest 是一个协同仿真框架，它在仿真运行时负责将香山核的输出与 NEMU 的输出进行对比，判断香山是否按照指令集定义的那样正确运行。<br />difftest 是验证香山功能正确性的重要工具，也对我们定位 bug 和解决 bug 提供了极大帮助。<br />difftest 与香山代码有较高的耦合，目前是作为一个子模块放在香山目录下，在编译仿真程序 emu 时将自动使用。<br />在仿真运行（执行 emu 时），通过 --diff 参数指定 nemu 来开启对比功能，如果不需要对比可以使用 --no-diff 关闭该功能，这时将只进行香山核的仿真。

<a name="nbLyz"></a>
## 编译香山核的仿真程序 EMU
有了前面部分的基础，这里我们尝试自行编译前面仿真演示中使用的香山仿真程序EMU。<br />这一过程中使用 mill 将香山的 chisel 语言代码编译为 Verilog 语言代码，然后通过 verilator 将 Verilog 代码编译为 c++ 语言的仿真模拟程序。<br />这些步骤已经在香山的 Makefile 里预先配置好了，我们可以通过如下命令直接编译
```bash
# 进入香山目录
cd /root/XiangShan/

# 设置环境变量
export NOOP_HOME=$(pwd)
export NEMU_HOME=$(pwd)

# 编译emu
make emu -j32 CONFIG=MinimalConfig EMU_THREADS=16 EMU_TRACE=1
```
解释参数<br />emu，表示编译预设为 emu，目标是编译仿真程序 EMU。香山还有生成可综合 Verilog 的预设，具体可见Makefile。<br />-j32 ，是 make 编译命令的参数，表示编译时使用 32 个线程。<br />后面大写的参数是对EMU的设置，其中：<br />CONFIG 指定香山配置，MinimalConfig 是简化版香山配置，为双发射，其余内部参数也相应缩减；DefaultConfig 是完整版香山配置，为6发射，需要的编译时间较长，并且对机器性能有较高要求。不指定 CONFIG 时默认为 DefaultConfig 配置。<br />EMU_THREADS 指定之后运行 EMU 时可用到的线程<br />EMU_TRACE=1 指定之后运行 EMU 时候可以打印波形

编译 MinimalConfig 至少需要 32GB 内存，编译 DefaultConfig 则推荐 64GB 内存以上。<br />这一步时间会比较久，需要耐心等待，生成结束后，可以在 ./build/ 目录下看到一个名为 emu 的仿真程序。<br />(如果你已经做到这里了，可以在 gitlink 的 howtouseissue 提交截图获得奖励～)<br />提交链接：https://www.gitlink.org.cn/OSchip/HowtoUseXiangShan/issues/1

利用 emu 仿真程序，可以让香山核运行指定的测试程序，进而验证功能或者测试性能。<br />emu 的基本用法是：
```bash
./build/emu -i ready-to-run/coremark-2-iteration.bin --diff ready-to-run/riscv64-nemu-interpreter-so 2> perf.log
```
其中：<br />-i 指定测试程序镜像<br />--diff 指定用于给 difftest 提供对照的 nemu 动态链接库<br />2> 是 shell 操作，把 std error 的数据（性能计数器的数据）打印到文件，之后可以通过该文件查看性能计数器的结果。这样避免性能计数器结果和emu执行结果混在一起难以观察。

emu 还可以打印出波形<br />可以使用 --dump-wave 参数打开波形，并使用 -b 和 -e 参数设置生成波形的开始和结束周期，例如想要生成 10000 ~ 11000 周期的波形，可以使用如下命令：
```bash
./build/emu -i MY_WORKLOAD.bin --dump-wave -b 10000 -e 11000
```
其中 -b 和 -e 的默认值为 0，注意仅当 -e 参数大于 -b 时才会真正记录波形；波形文件将会生成在 ./build/ 目录下，波形格式默认为 vcd。

emu 的更多功能可以使用 help 查看
```bash
./build/emu -h
```

欢迎大家来 [https://www.gitlink.org.cn/OSchip/HowtoUseXiangShan/about](https://www.gitlink.org.cn/OSchip/HowtoUseXiangShan/about) 查看更多香山使用文档。<br />同时，香山社区志愿者招募中，将个人简历发送至 anxu@bosc.ac.cn, 标题"香山开源生态建设志愿者报名"。
