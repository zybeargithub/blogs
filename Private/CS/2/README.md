# 信息的表示和处理

现代计算机存储和处理的信息以二值信号表示。

## 信息存储

大多数计算机使用 8 位的块，或者字节（byte），作为最小的可寻址的内存单位，而不是访问内存中单独的位。机器级程序将内存视为一个非常大的字节数组，称为虚拟内存（virtual memory）。内存的每个字节都由一个唯一的数字来标识，称为它的地址（address），所有可能地址的集合就称为虚拟地址空间（virtual address space）。虚拟地址空间只是一个展现给机器级程序的概念性映像，实际的实现是将动态随机访问存储器（DRAM）、闪存、磁盘存储器、特殊硬件和操作系统结合起来，为程序提供一个看起来统一的字节数组。

例如，C 语言中一个指针的值就是某一个存储块的第一个字节的虚拟地址。

### 十六进制表示法

在 C 语言中，以 0x 或者 0X 开头的数字常量被认为是十六进制的值。

#### 二进制转十六进制

当值是 2 的负整数 n 次幂时，也就是 x = 2^n，我们可以很容易将 x 写成十六进制，只要记住 x 的二进制表示就是 1 后面跟 n 个 0。十六进制 0 代表 4 个二进制 0。所以，当 n 表示为 i + 4j 的形式，其中 0 <= i <= 3，我们可以把 x 写成开头的十六进制数为 1 (i = 0)， 2 (i = 1), 4 (i = 2) 或者 8 (i = 3)，后面跟随着 j 个十六进制的 0。比如 2048 = 2^11，我们有 n = 11 = 3 + 4 \* 2，从而得到十六进制表示 0x800。

#### 十进制转十六进制

十进制与十六进制表示之间的转换需要使用乘法或者除法来处理一般情况。将一个十进制数字 x 转换为十六进制，可以反复地使用 16 除 x，得到一个商 q 和余数 r，也就是 x = q \* 16 + r。然后，我们用十六进制数字表示的 r 作为最低位数字，并且通过对 q 反复进行这个过程得到剩下的数字。

反过来，将一个十六进制数字转换为十进制数字，我们可以用相应的 16 的幂乘以每个十六进制数字。

示例：

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/45.png)

### 字数据大小

每台计算机都有一个字长（word size），指明指针数据的标称大小（nominal size）。因为虚拟地址是以这样的一个字来编码的，所以字长决定的最重要的系统参数就是虚拟地址空间的最大大小。

大多数计算机使用 8 位的块，或者字节（byte），作为最小的可寻址的内存单位，而不是访问内存中单独的位。32 位字长限制虚拟地址空间为 4GB（`2^10(1024) * 2^10(1024) * 2^10(1024) * 2^2(4) * 1B(8 byte)`），扩展到 16 位字长使得虚拟地址空间为 16 EB。

大多数 64 位机器也可以运行为 32 位机器编译的程序，这是一种向后兼容。我们将程序称为 “32 位程序” 或 “64 位程序” 时，区别在于该程序是如何编译的（如下图），而不是其运行的机器类型。

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/47.png)

程序员应该力图使他们的程序在不同机器和编译器上可移植。可移植性的一个方面就是使程序对不同数据类型的确切大小不敏感。

### 寻址和字节顺序

对于跨越多字节的程序对象，我们必须建立两个规则：这个对象的地址是什么，以及在内存中如何排列这些字节。在几乎所有的机器上，多字节对象都被存储为连续的字节序列，对象的地址为所使用字节中最小的地址。

排列表示一个对象的字节有两个通用的规则，某些机器选择在内存中按照从最低有效字节到最高有效字节的顺序存储对象，而另一些机器则按照从最高有效字节到最低有效字节的顺序存储。前一种规则，最低有效字节在前面的方式，称为小端法（little endian）。后一种规则，最高有效字节在前面的方式，称为大端法（big endian）。（如下图）（小端法被多数机器采用，大端法符合人类阅读习惯）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/49.png)

许多比较新的微处理器是“双端法（bi-endian）“，也就是说可以把它们配置成大端或者小端的机器运行。

网络应用程序的代码编写必须遵守已建立的关于字节顺序的规则，以确保发送方机器将它的内部表示转换成网络标准，而接收方机器则将网络标准转换为它的内部表示。

下面是不同机器上的字节表示（如下图）。

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/90.png)

### 表示字符串

在使用 ASCII 码作为字符码的任何系统上都将得到相同的结果，与字节顺序和字节大小规则无关。因而，文本数据比二进制数据具有更强的平台独立性。

### 表示代码

从机器的角度来看，程序仅仅只是字节序列。不同的机器类型使用不同的且不兼容的指令和编码方式。即使是完全一样的进程，运行在不同的操作系统上也会有不同的编码规则，因此二进制代码是不兼容的。二进制代码很少能在不同机器和操作系统组合之间移植。

### 布尔代数

二进制是计算机编码、存储和操作信息的核心，将逻辑值 TRUE（真）和 FALSE（假）编码为二进制值 1 和 0，能够设计出一种代数，以研究逻辑推理的基本原则。

布尔代数的运算：

- ~：~ 等于逻辑运算 NOT，当 P 等于 0 时，~P 等于 1，反之亦然；
- &：& 等于逻辑运算 AND，只有当 p = 1 和 q = 1 时，p & q 才等于 1；
- |：| 等于逻辑运算 OR，当 p = 1 或者 q = 1 时，p | q 等于 1；
- ^：^ 等于逻辑运算 `异或`，当 p = 1 且 q = 0，或者 p = 0 且 q = 1 时，p ^ q 等于 1；

### C 语言的位级运算

C 语言的一个很有用的特性就是它支持按位布尔运算。实际上，我们在布尔运算中使用的那些符号就是 C 语言所使用的：| 就是 OR（或），& 就是 AND（与），~ 就是 NOT（取反），^ 就是 EXCLUSIVE-OR（异或）。

确定一个位级表达式的结果最好的办法，就是将十六进制的参数扩展成二进制表示并执行二进制运算，然后再转换回十六进制。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/91.png)

### C 语言中的逻辑运算

C 语言还提供了一组逻辑运算符 ||、&& 和 !，分别对应命题逻辑中的 OR、AND 和 NOT 运算。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/92.png)

### C 语言中的移位运算

C 语言还提供了一组移位运算，向左或向右移位模式：

- x << k：左移位运算，x 向左移动 k 位，丢弃最高的 k 位，并在右端补 k 个 0；
- x >> k：算术右移位运算，x 向右移动 k 位，丢弃最低的 k 位，并在左端补 k 个最高有效位的值；
- x >>> k：逻辑右移位运算，x 向右移动 k 位，丢弃最低的 k 位，并在左端补 k 个 0；

案例（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/93.png)

## 整数表示

我们描述用位来编码整数的两种不同的方式：一种只能表示非负数，而另一种能够表示负数、零和正数。

下面列出了我们引入的数学术语，用于精确定义和描述计算机如何编码和操作整数。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/95.png)

### 整型数据类型

C 语言标准规定了每种数据类型必须能够表示的最小的取值范围。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/96.png)

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/97.png)

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/98.png)

> C 和 C++ 都支持有符号（默认）和无符号数。Java 只支持有符号数。

### 无符号数的编码

假设有一个整数数据类型有 w 位。我们可以将位向量写成 x^→，表示整个向量，或者写成 [x^w-1, x^w-2, ..., x^0]，表示向量中的每一位。把 x^→ 看做一个二进制表示的数，就获得了 x^→ 的无符号表示。在这个编码中，每个位 x^i 都取值为 0 或 1，后一种取值意味着数值 2^i 应为数字值的一部分。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/100.png)

### 补码编码

有时候，我们希望表示负数值。最常见的有符号数的计算机表示方式就是补码（two's component）形式。在这个定义中，数字的最高有效位解释为负权（negative weight）。我们用函数 B2Tw（Binary to Two's component 的缩写，长度为 w）来表示（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/101.png)

最高有效位 x^w-1 也称为符号位，它的权重为 -2^w-1，是无符号表示中权重的负数。符号数被设置为 1 时，表示值为负，而当设置为 0 时，值为非负。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/104.png)

w 位补码所能表示的最小值是 -2^w-1，所能表示的最大值是 2^w-1 - 1。以长度 4 为例，所能表示的最小值为 -8，最大值为 7。

|TMin| = |TMax| + 1 ：因为 0 是非负数，也就意味着能表示的整数比负数少一个。

UMax_w = 2 \* TMax_x + 1：最大的无符号数值刚好比补码的最大值的两倍大一点。

C 语言标准并没有要求使用补码形式来表示有符号整数，但是几乎所有的机器都是这么做的。而 Java 标准是非常明确的，它要求采用补码表示。

### 有符号数和无符号数之间的转换

C 语言允许在不同的数字数据类型之间做强制类型转换。例如，假如变量 x 声明为 int，u 声明为 unsigned。表达式 `(unsigned)x` 会将 x 的值转换为无符号数值，而 `int(x)` 会将 x 的值转换为有符号整数。

强制类型转换时，结果保持位值不变，只是改变了解释这些位的方式。—— 数值可能会变，但是位模式不变。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/105.png)

#### 公式

U2B_w：将数值映射为无符号数形式的位（二进制）表示；

T2B_w：将数值映射为补码形式的位（二进制）表示；

T2U_w(x) = B2U_w(T2B_w(x))：输入 TMin_w ~ TMax_w 的数值，将其转换为 0 ~ UMax_w；

U2T_w(x) = B2T_w(U2B_w(x))：输入 0 ~ UMax_w 的数值，将其转换为 TMin_w ~ TMax_w；

1 + UMax_w = 2^w

函数 U2T 描述的是从无符号数到补码的转换，而 T2U 描述的是从补码到无符号数的转换。

补码转换为无符号数（如下图）

当一个有符号数映射为它相应的无符号数时，负数就被转换成了大的正数，而非负数会保持不变。

### C 语言中的有符号数与无符号数

C 语言中没有规定有符号数采用哪种表示，但几乎所有的机器都采用补码。大多数的数字都默认是有符号的，要创建一个无符号常量，必须加上后缀字符 “U” 或者 “u”。

当执行一个运算时，如果它的一个运算数是有符号的而另一个运算数是无符号的，那么 C 语言会隐式地将有符号参数强制类型转换成无符号数，并假设这两个数都是非负的，然后来执行这个计算。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/112.png)

### 扩展一个数字的位展示

一个常见的运算是在不同字长的整数之间转换，同时又保持数值不变。

要将一个无符号数转换为一个更大的数据类型，我们只需要在表示的开头添加 0，这种运算被称为零扩展（zero extension），表示原理（如下图）：

要将一个补码数字转换为一个更大的数据类型，可以执行一个符号扩展（sign extension），在表示中添加最高有效位的值，表示为如下原理（如下图）：

补码数值的符号扩展论证（如下图）

### 关于有符号数和无符号数的建议

有符号数到无符号数的转换可能会导致错误和漏洞，避免这类错误的一种方式就是不使用无符号数。（例如 Java 只支持有符号数，并且要求以补码运算来实现）

## 整数运算

### 无符号加法

整数运算的“字长膨胀”表示，要想完整地表示算术运算的结果，我们不能对字长做任何限制。

原理：无符号数加法（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/1.png)

模数加法形成了一种数学结构，称为阿贝尔群，也就是说，它是可交换的和可结合的。它有一个单位元 0，并且每个元素有一个加法逆元。让我们考虑 w 位的无符号数的集合，执行加法运算 +\_w^u。对于每个值 x，必然有某个值 -x_w^u 满足 -x_w^u + x_w^u = 0。

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/2.png)

### 补码加法

原理：补码加法（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/3.png)

补码加法案例（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/4.png)

### 补码的非

原理：补码的非

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/5.png)

对于任意整数，计算表达式 -x 和 ~x + 1 得到的结果完全一样（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/6.png)

### 无符号乘法

原理：无符号数乘法（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/7.png)

原理：补码乘法（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/8.png)

对于无符号和补码乘法来说，乘法运算的位级表示都是一样的。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/9.png)

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/10.png)

### 乘以常数

以往，在大多数机器上，整数乘法指令相当慢，需要 10 个或更多的时钟周期，然而其他整数运算（例如加法、减法和位级运算和移位）只需要 1 个时钟周期。

#### 乘以 2 的幂

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/11.png)

#### 与 2 的幂相乘的无符号乘法

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/12.png)

#### 与 2 的幂相乘的补码算法

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/13.png)

无论是无符号运算还是补码运算，乘以 2 的幂都可能会导致溢出。

由于整数乘法比移位和加法的代码要大得多，许多 C 语言编译器试图以移位、加法和减法的组合来消除许多整数乘以常数的情况。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/14.png)

选择使用移位、加法和减法的组合，还是使用一条乘法指令，取决于这些指令的相对速度，而这些是与机器高度相关的。大多数编译器只在需要少量移位、加法和减法就足够的情况下才使用这种优化。

### 除以 2 的幂

在大多数机器上，整数除法要比整数乘法更慢 —— 需要 30 个或更多的时钟周期。

#### 除以 2 的幂的无符号除法

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/15.png)

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/16.png)

#### 除以 2 的幂的补码算法，向下舍入

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/17.png)

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/18.png)

#### 除以 2 的幂的补码算法，向上舍入

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/19.png)

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/20.png)

同乘法不同，我们不能用除以 2 的幂的除法来表示除以任意常数 K 的除法。

### 关于整数运算的最后思考

计算机执行的 `“整数”` 运算其实是一种模运算形式。表示数字的有限字长限制了可能的值的取值范围，结果运算可能溢出。

## 浮点数

浮点表示对形如 V = x \* 2^y 的有理数进行编码。

### 二进制小数

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/21.png)

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/22.png)

### IEEE 浮点数表示

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/23.png)

给定位表示，根据 exp 的值，被编码的值可以分成三种不同的情况（最后一种情况有两个变种）（下图说明了单精度的情况）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/24.png)

- 情况 1：规格化的值

这是最普遍的情况。当 exp 的位模式既不全为 0（数值 0），也不全为 1（单精度数值为 255，双精度数值为 2047 时），都属于这类情况。在这种情况中，阶码字段被解释为以偏置（biased）形式表示的有符号整数。阶码的值是 E = e - Bias，其中 e 是无符号数，其位表示为 e_k-1···e_1e_0，而 Bias 是一个等于 2^k-1 -1（单精度是 127，双精度是 1023）的偏置值。由此产生的指数范围，对于单精度是 -126~+127，而对于双精度是 -1022~+1023。

小数字段 frac 被解释为描述小数值 f，其中 0 <= f < 1;

E = e - 2^k-1 + 1；

M = 1 + f；

- 情况 2：非规格化的值

当阶码域为全 0 时，所表示的数是非规格化的值。在这种情况下，阶码值是 E = 1 - Bias。非规格化的值有两个用途，首先是提供了一种表示数值为 0 的方法，另外一个功能是表示那些非常接近于 0.0 的数。

E = -2^k-1 + 2；

M = f；

- 情况 3：特殊值

当指阶码全为 1 时为特殊值。

当小数域全为 0 时，得到的值表示无穷，当 s = 0 时是正无穷，当 s = 1 时是负无穷。

当小数域为非零时，结果值被称为 "NaN"。

### 数字示例

#### 8 位浮点格式的非负值示例

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/25.png)

IEEE 格式如此设计是为了浮点数能够使用整数排序函数来进行排序。

下面是一些重要的单精度和双精度浮点数的表示和数字值。（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/26.png)

### 舍入

四种舍入方式（如下图）

![image](http://shadows-mall.oss-cn-shenzhen.aliyuncs.com/images/assets/cs/27.png)

使用向偶数舍入可以在大多数现实情况中避免这种统计偏差。在 50% 的时间里，它将向上舍入，而在 50% 的时间里，它将向下舍入。

### 浮点运算

IEEE 制定了一个规则：把浮点值 x 和 y 看成实数，而某个运算 X 定义在实数上，计算将产生 Round(xXy)，这是对实际运算的精确结果进行舍入后的结果。虽然 x 和 y 都是实数，由于溢出，该运算可能得到无穷值。

实数上的加法也形成了阿贝尔群，作为阿贝尔群，大多数值在浮点加法下都有逆元，也就是说 x + -x = 0。无穷和 NaN 是例外情况，因为对于任何 x，都有 NaN + x = NaN。

浮点加法满足了单调性属性：如果 a >= b，那么对于任何 a、b 以及 x 的值，除了 NaN，都有 x + a >= x + b。无符号和补码加法不具有这个实数（和整数）加法的属性。

### C 语言浮点数

所有的 C 语言版本提供了两种不同的浮点数类型：float 和 double。在支持 IEEE 浮点格式的机器上，这种数据类型就对应于单精度和双精度浮点。这类机器使用向偶数舍入的舍入方式。

当在 int、float 和 double 格式之间进行强制类型转换时，程序改变数值和位模式的原则如下：

- 从 int 转换成 float，数字不会溢出，但是可能被舍入。
- 从 int 或 float 转换到 double，因为 double 有更大的范围（也就是可表示值的范围），也有更高的精度（也就是有效位数），所以能够保留精确的数值。
- 从 double 转换成 float，因为范围要小一些，所以值可能溢出为 正无穷或负无穷。另外，由于精度较小，它还可能被舍入。
- 从 float 或者 double 转换成 int，值将会向零舍入。

## 小结

计算机将信息编码为位（比特），通常组织为字节序列。有不同的编码方式用来表示整数、实数和字符串。不同的计算机模型在编码数字和多字节数据中的字节顺序时使用不同的约定。

C 语言的设计可以包容多种不同字长和数字编码的实现。64 位字长的机器逐渐普及，并正在取代统治市场长达 30 多年的 32 位机器。由于 64 位机器也可以运行为 32 位机器编译的程序，我们的重点就放在区分 32 位和 64 位程序，而不是机器本身。64 位程序的优势就是可以突破 32 位程序具有的 4GB 地址（内存）限制。

大多数机器对整数使用补码编码，而对浮点数使用 IEEE 标准 754 编码。在位级上理解这些编码，并且理解算术运算的数学特性，对于想使编写的程序能在全部数值范围上正确运算的程序员来说，是很重要的。

在相同长度的无符号和有符号整数之间进行强制类型转换时，大多数 C 语言实现遵循的原则是底层的位模式不变。在补码机器上，对于一个 w 位的值，这种行为是由函数 T2U 和 U2T 来描述的。C 语言隐式的强制类型转换会出现许多程序员无法预计的结果，常常导致程序错误。

由于编码的长度有限，与传统整数和实数运算相比，计算机运算具有非常不同的属性。当超出表示范围时，有限长度能够引起数值溢出。当浮点数非常接近于 0.0，从而转换为零时，也会下溢。

与大多数其他程序语言一样，C 语言实现的有限整数运算和真实的整数运算相比，有一些特殊的属性。例如，由于溢出，表达式 x*x 能够得出负数。但是，无符号数和补码的运算都满足整数运算的许多其他属性，包括结合律、交换律和分配律。这就允许编译器做很多的优化。例如，用 (x<<3)-x 取代表达式 7*x 时，我们就利用了结合律、交换律和分配律的属性，还利用了移位和乘以 2 的幂之间的关系。

我们已经看到了几种使用位级运算和算术运算结合的聪明方法。例如，使用补码运算，~x + 1 = -x。

浮点表示通过将数字编码为 `x * 2^y` 的形式来近似的表达实数。最常见的浮点表示方式是由 IEEE 标准 754 定义的。它提供了几种不同的精度，最常见的是单精度（32 位）和双精度（64 位）。IEEE 浮点数也能表示特殊值 正无穷大、负无穷大和 NaN。

必须非常小心地使用浮点运算，因为浮点运算只有有限的范围和精度，而且并不遵守普通的算术属性，比如结合性。
