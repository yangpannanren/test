
<a name="beginToc"></a>

## 目录
[检测理论](#检测理论)
 
&emsp;[背景](#背景)
 
&emsp;[二元假设检测](#二元假设检测)
 
&emsp;[复合假设](#复合假设)
 
&emsp;[恒定虚警率检测器](#恒定虚警率检测器)
 
[连续波信号检测](#连续波信号检测)
 
&emsp;[背景](#背景)
 
&emsp;[公式](#公式)
 
&emsp;[求解](#求解)
 
&emsp;[性能分析](#性能分析)
 
[扩频信号检测](#扩频信号检测)
 
&emsp;[背景](#背景)
 
&emsp;[公式](#公式)
 
&emsp;&emsp;[DSSS编码](#dsss编码)
 
&emsp;&emsp;[扩频雷达信号](#扩频雷达信号)
 
&emsp;[解法](#解法)
 
&emsp;&emsp;[能量检测器](#能量检测器)
 
<a name="endToc"></a>

# 电子战辐射源检测与定位\-威胁辐射源检测
# 检测理论
## 背景
## 二元假设检测

接收机工作特性（receiver operating characteristics，ROC）曲线。

## 复合假设
## 恒定虚警率检测器

若待检测的信号已知，但信号幅度未知，并且噪声功率也是未知的，则无法通过建立一个确保给定 $P_{\textrm{FA}}$ 的阈值测试。解决方法是使用恒定虚警率（constant false alarm rate，CFAR）检测器。该检测器既可以缩放输入信号以适应噪声功率的变化，也可以根据输入噪声信号的功率调整检测阈值。


CFAR检测器可以通过最大不变检验统计量的阈值检验来实现，该检测器可在噪声功率条件未知条件下检测出已知信号：

 $$ T(\mathbf{z})=\frac{{\mathbf{s}}^H {\mathbf{P}}_s \mathbf{z}}{\sqrt{{\mathbf{s}}^H \mathbf{s}}\sqrt{{\mathbf{z}}^H {\left(\mathbf{I}-{\mathbf{P}}_s \right)}\mathbf{z}}} $$ 

其中， ${\mathbf{P}}_s$ 为已知信号 $\mathbf{s}$ （ ${\mathbf{P}}_s =\frac{\mathbf{s}{\mathbf{s}}^H }{{\mathbf{s}}^H \mathbf{s}}$ ）的投影矩阵。分子项 ${\mathbf{s}}^H {\mathbf{P}}_s \mathbf{z}$ 用于计算与预期信号 $\mathbf{s}$ 相关的接收能量，而分母为 $T(\mathbf{z})$ 大小的缩放因子。第一项 $\sqrt{{\mathbf{s}}^H \mathbf{s}}$ 控制与 $\mathbf{s}$ 相乘的尺度，而 $\sqrt{{\mathbf{z}}^H {\left(\mathbf{I}-{\mathbf{P}}_s \right)}\mathbf{z}}$ 计算噪声功率，以使噪声能量的变化不会影响测试统计量 $T(\mathbf{z})$ 。


测试统计量 $T(\mathbf{z})$ 是按照学生t分布（student\-t distribution）进行分布的，该分布可以用于计算一些预期 $P_{\textrm{FA}}$ 的阈值η。但该分布无法表示为闭式解形式，因此必须使用数值计算方法。


CA\-CFAR（Cell Averaging\-Constant False Alarm Rate）使用滑动窗口来计算相邻单元内的平均功率强度，并与被测单元的功率强度比较。可以在预期噪声稳定的任何维度（如时间或频率）中完成这一点。CA\-CFAR的主要参数包括窗口大小、计算噪声功率估计值的平均单元数、保护带大小、被测单元附近被忽略的单元数。保护带可以防止被测单元中的信号泄露污染相邻单元或影响噪声功率估计值。


CA\-CFAR估计：

 $$ {\overline{\sigma} }_n^2 =\frac{1}{K(M-1)}\sum_{i=1}^M \textrm{ }{\overline{\mathbf{z}} }_i^H {\overline{\mathbf{z}} }_i $$ 

 $$ T(\mathbf{z})=\frac{1}{\sqrt{2NE_s }}\sum_{i=1}^K  \Re {\left{{\mathbf{s}}^H {\mathbf{z}}_i \right}} $$ 

 $$ \begin{array}{rcl} \eta =\sqrt{2{\overline{\sigma} }_n^2 }\textrm{erfinv}{\left(1-2P_{\textrm{FA}} \right)} \end{array} $$ 

其中， ${\overline{\mathbf{z}} }_i ,i=1,\ldots,M$ 为当前被测单元的M个侧信道（即当前检验中未使用的数据样本）样本矢量；K为样本矢量 ${\bar{\mathbf{z}} }_i$ 的长度。输出 ${\bar{\sigma} }_n^2$ 为噪声功率 $\sigma_n^2$ 的估计值。为确保无偏估计，将噪声功率估计值除以M\-1而不是M，以确保其是无偏估计。

# 连续波信号检测

本章讨论具有有限带宽连续波信号的检测。随后的章节将重点讨论扩频信号检测以及具有未知载频（通过扫描或搜索正在使用的信道）的信号检测。

## 背景

连续波信号的定义特征是时域无限长（与脉冲信号相反，他始终处于开启状态）和频域有限长（频率成分仅限于某个频带）。

## 公式

二元假设表示为：

 $$ \begin{array}{lll} {\mathcal{H}}_0 :\textrm{    }\mathbf{z}=\mathbf{n}\textrm{    }\sim \mathcal{C}\mathcal{N}{\left(0,\sigma_n^2 {\mathbf{I}}_M \right)}\newline {\mathcal{H}}_1 :\textrm{    }\mathbf{z}=\mathbf{s}+\mathbf{n}\textrm{    }\sim \mathcal{C}\mathcal{N}{\left(\mathbf{s},\sigma_n^2 {\mathbf{I}}_M \right)} \end{array} $$ 

其中M为样本数 $M=\lfloor TF_s \rfloor$ ，T为观测间隔，Fs为采样率。

## 求解

若信号 $\mathbf{s}$ 未知，假设噪声功率已知或估计结果是准确的，则检验统计量为：

 $$ T(\mathbf{z})=\frac{2}{\sigma_n^2 }{\mathbf{z}}^H \mathbf{z} $$ 

此时：

 $$ T_{{\mathcal{H}}_0 } (\mathbf{z})\sim \chi_{2M}^2 $$ 

 $$ T_{{\mathcal{H}}_1 } (\mathbf{z})\sim \chi_{2M}^2 {\left(\frac{2\textrm{MS}}{N}\right)} $$ 

其中， $\chi_{2M}^2$ 为具有2M自由度的卡方分布，S为从带通滤波器发出的信号功率。


则：

 $$ P_{FA} =P_{{\mathcal{H}}_0 } {\left{T(\mathbf{z})\ge \eta \right}}=1-F_{\chi^2 } {\left(\eta ;2M\right)} $$ 

 $$ P_{\mathrm{D}} =1-F_{\chi^2 } {\left(\eta ;2M,2M\xi \right)} $$ 

其中， $\xi =\frac{S}{N}$ 。

## 性能分析

讨论两个示例场景：（非常强大的）调频无线电传输检测和机载防撞雷达检测。


发射和接收功率相关的单项链路方程： $S=\frac{P_t G_t G_r }{L_{\textrm{prop}} L_{\textrm{atm}} L_t L_r }$ 。其中， $P_t$ 为发射功率； $G_t$ 为发射天线增益； $G_r$ 为接收天线增益； $L_{\textrm{prop}}$ 为传播损耗（如自由空间或双径）； $L_{\textrm{atm}}$ 为大气损耗； $L_t$ 包括待检测发射机中的所有系统损耗； $L_r$ 包括检测器硬件的所有损耗以及处理损耗。


自由空间路径损耗，指（通过各向同性天线）接收到的功率与（接收机方向上）辐射源功率之比： $L_{\mathrm{f}\mathrm{s}\mathrm{p}\mathrm{l}} ={{\left(\frac{4\pi R}{\lambda }\right)}}^2$ 。收发机之间没有障碍物，且视线附近没有主要反射物。


双射线路径损耗，存在主要反射物（如地球表面）的情况下，主要反射会与直接路径信号重叠，从而造成相消干扰。 $L_{2-\mathrm{r}\mathrm{a}\mathrm{y}} =\frac{R^4 }{h_t^2 h_r^2 }$ 。其中， $h_t$ 和 $h_r$ 分别为发射机和接收机的离地高度。


对于任何地面链路，都有一个范围，超过这个范围，自由空间模型就不再适用，而应该使用双射线模型。这被称为菲涅尔带（Fresnel zone）： $\mathrm{F}\mathrm{Z}=\frac{4\pi h_t h_r }{\lambda }$ 。


大气吸收，由于大气中气体的电磁偶极子效应引起的。损耗方程： $L_{\textrm{atm}} \lbrack dB\rbrack =\alpha \frac{R}{1e3}$ 。其中， $\alpha$ \[dB\]为每千米传播距离的特定衰减率。当频率低于1GHz时，大气气体的吸收通常被忽略。


调频广播塔检测：最容易检测的CW传输信号是电视和无线电广播信号，它们以非常高的功率从塔顶发射并传输，以最大程度地减小地面对传播的影响。


对于发射源通常采用有效辐射功率（effective radiated power，ERP）代替 $P_t G_t /L_t$ 。


连续波雷达检测：二进制编码雷达，将二进制信号编码调制到载波信号的相位上。

# 扩频信号检测

电磁频谱（Electromagnetic Spectrum，EMS）自从被用于通信或感知以来，同样也被用来通过敌方辐射源来检测敌方的存在。因此，这一类受保护的信号被广泛采用。这些信号具有不同的保护级别，大致分为三类：低截获概率 （low probability of detection，LPD）、低拦截概率（low probability of interception，LPI）和低利用概率（low probability of exploitation，LPE）。根据定义，LPD信号也是LPI和LPE信号。同样，LPI信号也是 LPE信号。


LPD是指难以检测的信号。获取LP信号的最常见方法是在大带宽上传播信号能量，如直接序列扩频技术（Direct Sequence Spread Spectrum，DSSS）通信信号。在大带宽上稀释能量，接收机成功检测到信号的概率更小。


LPI是指防止敌方截获某段传输信号。敌方可能会检测到正在发生的信号传输，但无法截获完整信号。LPI的主要优势是，由于敌方只能捕获少部分的信号，因此很难难估计LPI信号参数。实现LPI的一种常见方法是跳频。


最后一类受保护的信号是LPE，即防止敌方对信号特征（如传输的信息）进行利用。实现LPE的主要方法是通过加密。


本章讨论扩频信号（DSSS是其中一种类型）以及一些非常适合此类难以检测信号的专用检测器。

## 背景

DSSS是指EMS中用于信号传输的部分要远多于所要求的部分。通过将发射信号分布在超过其原始带宽10~100倍的带宽上，传输的能量会被显著稀释，通常达到传输信号的功率谱密度（power spectral density，PSD）低于背景噪声水平的程度，从而使传统方法（能量检测器）的检测更具挑战性。


跳频扩频（Frequency\-hopping spread spectrum，FHSS）是一种在更宽带宽上传播信号的技术，但它依赖于中心频率的快速变化，并且在任何给定瞬间都是窄带信号。


DSSS在军事和民用环境中的使用越来越多。除了军事优势外，还可以减少干扰，并让多个用户占用单个信道。


雷达信号通常表现出与DSSS通信信号相似的特性，最明显的是相移键控（phase shift keying，PSK），噪声雷达波形表现出与DSSS信号相同的扩频特性，因为它们占用的瞬时带宽很宽。对于雷达信号，大带宽通常是有利的（因为分辨率与带宽成反比），但它可能会使跨多个分辨率单元检测大型目标的处理复杂化。


无论是哪种情况，数字扩频信号的在结构上都定义为由一系列重复的“码片”组成。这些码片的变化方式取决于所选的调制方案，例如二进制或多相PSK或正交幅度调制。每个码片传输的时间称为码片持续时间： $T_{\textrm{chip}}$ 。信号的带宽是码片速率的倒数（除非借助加窗功能使频谱平滑）： $B_s =\frac{1}{T_{\mathrm{c}\mathrm{h}\mathrm{i}\mathrm{p}} }$ 。

## 公式

与前几章中使用相同的二元假设检验：

 $$ \begin{array}{l} {\mathcal{H}}_0 :~~\mathbf{z}=\mathbf{n}~~\sim \textrm{CN}{\left(0,\sigma_n^2 {\mathbf{I}}_M \right)}\newline {\mathcal{H}}_1 :~~\mathbf{z}=\mathbf{s}+\mathbf{n}~~\sim \mathcal{C}\mathcal{N}{\left(\mathbf{s},\sigma_n^2 {\mathbf{I}}_M \right)} \end{array} $$ 

### DSSS编码

DSSS信号生成过程中，会将一串信息符号 $d_i$ 乘以工作速率更高的重复码 $c\left(t\right)$ 来实现的。长重复码（代码序列长于单个符号）与短重复码（代码序列至少每个符号重复一次）都可被协作接收机使用来扩频或调制，接着解扩或解调并恢复窄带宽传输。解调过程可以提升SNR，因为期望信号会响应解调滤波器，而噪声和干扰信号则不会，这被称为DSSS的处理增益，公式为： $G_p =\frac{R_s }{R_d }$ 。其中， $R_s$ 是扩频序列的码片速率（单位：b/s）， $R_d$ 是基础传输的数据速率（单位：b/s）。同理，如果信号在扩频前后具有相同的调制，则增益可以等效地表示为带宽之比： $G_p =\frac{B_s }{B_d }$ 。


实际中，短码更容易被敌方解码，特别是当码持续时间 $T_c$ 小于码片持续时间 $T_{\textrm{symbol}}$ （意味着每个符号都包含整个编码序列的副本）时。因此长码更安全，但同时也增加了复杂性和延迟，因为接收器的同步需要将接收到的信号与已知码相关联，以确定适当的延迟。

### 扩频雷达信号

LPD信号经常用于雷达，尽管在这种情况下它们通常被称为LPI。最常见的一类LPI雷达信号是PSK调制信号，其中，每 $T_{\textrm{chip}}$ 秒调整一次基础载波信号的相位。PSK雷达信号： $s(t)=Ae^{j(2\pi f_c t+\phi (t))}$ 。对于某些时间序列的相位偏移 $\phi (t)$ ，相位码可以是二进制（0和 $\pi$ ）或多相。Barker和Costas码因具有旁瓣结构是流行的选择。由于某些约束或期望的结果，波形设计越来越多地用于选择最优码。


雷达信号的特点因任务而异，但大多数雷达信号被设计为具有图钉形的模糊函数，即在距离向和多普勒向上主目标响应附近具有较低的旁瓣。

## 解法

由于假设发射机是非协作的，因此检测器不知道码片速率或信号的任何其他参数。本节考虑3种类型的检测器：一种是未知信号结构的检测器，一种是利用扩频信号特性（称为循环平稳性）的检测器，另一种是利用多个接收机来检测相干信号的检测器。

### 能量检测器

第一个也是最直接的解法是求解二元假设检验，而不使用 $s\left(t\right)$ 结构的其他信息。此方法与上章中介绍的能量检测器具有相同的解法，即能量探测器：

 $$ T(\mathbf{z})=\frac{2}{\sigma_n^2 }{\mathbf{z}}^H \mathbf{z} $$ 

 $P_{\textrm{FA}} 、\eta$ 和 $P_D$ 的方程都保持不变。在这种情况下，唯一的区别是信号为扩频信号，其PSD和相应的检测性能降低。

