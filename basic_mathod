# go 语法
go atlas
go atlas simflags="-V 5.0.8.R"
go devedit "-3d"


# set 语法
set temp=1000
set gaspress=1
diffuse time=30 temp=$temp press=$gaspress # 此例中变量为temp 和 gaspress 值分别为 1000 和 1 。这样在后续仿真语句中声明 temp 和 gaspress 时， 设置的值 将 自动 赋予 这些变量

# 变量为经某种运算后的结果
extract name="oxide thickness" thickness oxide
set etch_thickness=($"oxide thickness"*1000)+0.05
etch oxide dry thickness=$etch_thickness

# 安装设置文件"show.set"来进行显示
tonyplot structure.str -set show.set


# tonyplot 语法    
# tonyplot 可打开的文件类型有结构文件(*.str)、日志文件(*.log)、RSM文件(*.rsm)、统计文件(*.sta)、
用户数据文件(*.dat)和压缩文件(*.gz)等等
# tonyplot 有三种显示方式 "Add Plot"、"overlay Plot"、"Replace Plot"  器件仿真得到的特性通常用Overlay Plot的方式

# 重叠显示overlay 将得到的mos0_1.log mos0_2.log mos0_3.log按照设置文件show.set的设置
# 以重叠overlay的方式显示在一个窗口中
tonyplot -overlay mos0_1.log mos0_2.log mos0_3.log -set show.set

# 显示当前的结构  tonyplot 相当于 commands>Plot Current Structure


# Extract 语法

# 提取栅氧化层厚度
extract name="gateox" thickness oxide mat.occno=1 x.val=0.49

# 提取结深
extract name="nxj" xj silicon mat.occno=1 x.val=0.1 junc.occn=1

# 提取结深的另一种方法
extract name="Junction Depth" x.val from curve(depth,\
(impurity="Gallium" material="Silicon" mat.occno=1)\
-(impurity="Phosphorus"material="Silicon" mat.occno=1)) where y.val=0.0

# 提取表面浓度
extract name="chan surf conc" surf.conc impurity="Net Doping"\
    material="silicon" mat.occno=1 x.val=0.45

# 提取 x=0.1um 处的硼浓度分布
extract name="bcurve" curve(depth, boron silicon mat.occno=1 x,val=0.1 )\
    outfile="extract.dat"

# 提取激活了的砷的总浓度
extract name="Active_Arsenic" 1.0e-04 * (area from curve(depth,\
    impurity="Active Arsenic" material="Silicon" mat.occno=1))


# 提取方块电阻
extract name="n++ sheet rho" sheet.res material="Silicon"\
    mat.occno=1 x.val=0.05 region.occno=1

# 提取结电容
extract start material="Silicon" mat.occno=1 bias=0.0  bias.step=0.25 \
    bias.stop=5.0 region,occno=1
extract done name="cjcurve" curve(bias, 1djunc.cap material="Silicon"\\
    mat.occno=1 region.occno=1 junc.occno=2) outfile="cj.dat"

# 提取电导
extract start material="Polysilicon" mat.occno=1 bias=0.0 bias.step=0.2 \
    bias.stop=2 x.val=0.45
extract done name="cond v bias" curve(bias, 1dn.conduct \
material="Silicon" mat.occno=1 region.occno=1)

# 提取结的击穿电压
extract start material="Silicon" mat.occno=1 bias=0.0 bias.step=0.25 \
    bias.stop=30.0 region.occno=1
extract done name="jbv" x.val from curve(bias, n.ion material="Silicon" \
    mat.occno=1 region.occno=1) where y.val=1.0

# 提取Vt
extract name="nldvt" ldvt ntype vb=0.0 qss=1e10 x.val=0.49

# 提取Vt的另外的方法
extract start poly mat.occno=1 bias=0.0 bias.step=0.20 bias=3.0
extract done name="vt_from_cond_curve" xintercept(maxslope(curve(bias, \
ldn,conduct  silicon mat,occno=1 region.occno=1)))


# 提取器件仿真的特性
"""
器件仿真的方法是对器件
施加电流、电压、磁场或是光照等，对器件的端电流电压和器
件内部的电学量进行仿真计算（ solve ）。仿真时 solve 得到的器件信息（含电学信息）可以
保存在结构文件（ （*.str ）或日志文件 （*.log ）中。
"""
# 提取器件仿真的特性前的导入提取的来源文件
extract init inf=“device_test.log"

# 从结构文件中提取solve文件后的特性
extract name="test" 2d.max.conc impurity="E Field" material="Silicon" x.val=0.5

# I~V 特性提取
extract name="iv" curve(v."emmitter1", i."base2")

# 瞬态特性的提取
extract name="It curve" curve(time, i."anode")

# 提取漏电压随温度变化的特性
extract name="VdT" curve(v."drain",temperature)

# 提取漏电流随频率变化的特性
extract name="Idf" curve(i."drain",temperature)

# 提取电容
extract name="cv" curve(c."electrode1""electrode2",v."electrode3")

# 提取电导
extract name="gv" curve(g."electrode1""electrode2",v."electrode3")

# 提取其他的电学参数曲线
extract name="IdT"curve(elet."parameter",v."drain")


# 轴操作
# 提取栅压除以50作为横坐标，漏电流乘以10为纵轴的曲线
extract name="big iv" curve(v."gate"/50,10*i."drain")

# 提取集电极电流作为横轴，集电极电流和基极电流的商(即电流放大倍数)作为纵轴的曲线
extract name="combine" curve(i."collector",i."collector"/i."base")

# 提取栅电压对漏电流的微分曲线并保存在文件中。
extract name="dydx" deriv(v."gate",i."drain") outfile="dydx.dat"

# 提取X(栅压)在0.5到2.5范围内的漏电流的最大值并保存
extract name="limit" max(curve(v."gate",i."drain",x.min=0.5 x.max=2.5))\
    outf="limit.dat"

# 提取栅电压对漏点流的二阶微分曲线并保存\
extract name="dydx2" deriv(v."gate",i."drain",2) outfile="dydx2.dat"
                                        
# 提取放大倍数的最大值
extract name="Vt" xintercept(max(curve(i."collector",i."collector"/i."base"))

# 提取转移特性曲线中斜率最大处的X(栅压)值作为阀值电压
extract name="vt" xintercept(maxslope(curve(abs(v."gate",abs(i."drain")))))

# 提取X(基极电压)为2.3时的Y(集电极电流)值
extract name="Ic[Vb=2.3]"y.val from curve(abs(v."base"),abs(i."collector")) \
    where x.val=2.3

