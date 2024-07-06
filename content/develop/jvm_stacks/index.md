+++
title = 'JVM stacks'
date = '2024-07-06'
author = 'aiclr'
categories = ['develop']
tags = ['jvm','java','stacks']
Summary='Java Virtual Machine Stacks'
+++

![image](../pcregister/img/运行时数据区jdk8.svg) \
[Oracle 官方文档](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.5.2)

2.5.2. Java Virtual Machine Stacks \
&nbsp;&nbsp;Each Java Virtual Machine thread has a private Java Virtual Machine stack, created at the same time as the thread.A Java Virtual Machine stack stores frames ([§2.6](#stack-frame)).A Java Virtual Machine stack is analogous<sub>类似的</sub> to the stack of a conventional<sub>传统的</sub> language such as C: it holds local variables and partial<sub>传统的</sub> results, and plays a part in method invocation<sub>调用</sub> and return.Because the Java Virtual Machine stack is never manipulated<sub>操作</sub> directly except<sub>除…之外</sub> to push and pop frames, frames may be heap allocated<sub>分配的</sub>.The memory for a Java Virtual Machine stack does not need to be contiguous<sub>连续的</sub>.

&nbsp;&nbsp;*In the First Edition<sub>版本</sub> of The Java® Virtual Machine Specification<sub>规范</sub>, the Java Virtual Machine stack was known as<sub>被称为</sub> the Java stack.*

&nbsp;&nbsp;This specification<sub>规范</sub> permits<sub>允许</sub> Java Virtual Machine stacks either to be of a fixed<sub>固定的</sub> size or to dynamically<sub>动态的</sub> expand<sub>扩大</sub> and contract<sub>缩小</sub> as required by<sub>根据需要</sub> the computation<sub>计算</sub>.If the Java Virtual Machine stacks are of a fixed size, the size of each Java Virtual Machine stack may be chosen<sub>选择</sub> independently<sub>独立地</sub> when that stack is created.

&nbsp;&nbsp;*A Java Virtual Machine implementation<sub>实现</sub> may provide the programmer<sub>程序员</sub> or the user control over the initial<sub>初始</sub> size of Java Virtual Machine stacks,as well as, in the case of dynamically expanding or contracting Java Virtual Machine stacks, control over the maximum and minimum sizes.*

&nbsp;&nbsp;The following exceptional<sub>异常的</sub> conditions<sub>情况</sub> are associated<sub>有关联的</sub> with Java Virtual Machine stacks:
- If the computation<sub>计算</sub> in a thread requires a larger Java Virtual Machine stack than is permitted<sub>被允许</sub>, the Java Virtual Machine throws a **StackOverflowError**.
- If Java Virtual Machine stacks can be dynamically expanded,

and expansion<sub>扩大</sub> is attempted<sub>尝试</sub> but insufficient memory<sub>内存不足</sub> can be made available to effect<sub>实现</sub> the expansion,or if insufficient memory can be made available to create the initial<sub>初始</sub> Java Virtual Machine stack for a new thread,the Java Virtual Machine throws an **OutOfMemoryError**.

不同平台cpu架构不同，不能基于寄存器来设计java指令集。为了实现跨平台，java指令集根据`LIFO`<sub>last-in-first-out</sub>stack设计
    - 优点：跨平台、指令集小、编译器容易实现
    - 缺点：性能下降、实现同样功能需要更多的指令

与`pc register`一样，`JVM Stack`是**线程私有**的，生命周期与线程相同。\
`JVM Stack` 描述的是Java方法执行的线程内存模型：每个方法被执行的时候，JVM都会同步创建一个`Stack Frame`

`Stack Frame`用于存储
- `Local Variables`<sub>局部变量表，影响`stack frame`大小</sub>
- `Operand Stacks`<sub>操作数栈，影响`stack frame`大小</sub>
- `Dynamic Linking`<sub>动态链接，指向运行时常量池的方法引用</sub>
- `Method Invocation Completion`<sub>方法调用结束</sub>
    - `Normal Method Invocation Completion`<sub>方法调用正常结束</sub>
    - `Abrupt Method Invocation Completion`<sub>方法调用异常结束</sub>

jvm Stack 特点：
- 每一个方法被调用直至执行完毕的过程，就对应着一个`Stack Frame`在`JVM Stack`中从**入栈**到**出栈**的过程。
    - 方法执行-**入栈**
    - 方法执行结束-**出栈**
- jvm Stack 是一种快速有效的分配存储方式，访问速度仅次于The pc Register
- jvm Stack 没有 GC

jvm Stack 可能出现的内存错误；jvm规范允许jvm Stack的大小固定不变或动态扩展
- `StackoverflowError`：jvm Stack的大小固定，每个线程的jvm Stack大小在线程创建时独立设置，如果线程请求分配的jvm Stack大小超过jvm stack允许的最大容量，JVM将抛出StackOverflowError
- `OutOfMemmoryError`
    - 动态扩展的jvm stack，尝试扩展时无法申请到足够的内存，JVM将抛出OutOfMemoryError
    - 创建新的线程时没有足够的内存去创建对应的jvm stack，JVM将抛出OutOfMemoryError

jvm Stack 是运行时的单位，而 Heap 是存储的单位 \
jvm Stack 解决程序的运行问题,即程序如何执行。参与方法的调用和返回，每个线程在创建时都会创建自己的jvm Stack,内部保存一个个stack frame，对应一次次的方法调用 \
Heap 解决数据存储问题，即数据怎么存放，存放到哪。new创建的对象实例都存放在Heap \
方法嵌套调用的次数由jvm stack的大小决定
- jvm stack越大，方法嵌套调用次数越多
- 对一个**函数**来说，**参数**和**局部变量**越多，`local variables`越大，则其`stack frame`越大，该函数调用会占用更多的`jvm stack`空间，导致嵌套调用次数减少
- `local variables`中的**局部变量**只在**当前方法**调用中有效
    - 方法执行时，JVM通过使用`local variables`来完成**参数值**到**参数变量列表**的传递。
    - 方法调用结束后，随着`stack frame`的出栈销毁，`local variables`也会随之销毁

## Stack Frame

[Oracle 官方文档](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6) \
&nbsp;&nbsp; A frame is used to store data and partial<sub>部分的</sub> results, as well as to perform<sub>执行</sub> dynamiclinking, return values for methods, and dispatch<sub>调遣</sub> exceptions.

A new frame is created each time a method is invoked. A frame is destroyed when its method invocation<sub>调用</sub> completes, whether<sub>或者…（或者）</sub> that completion is normal or abrupt (it throws an uncaught<sub>未捕获</sub> exception). Frames are allocated<sub>分配</sub> from the **Java Virtual Machine stack** ([§2.5.2](#java-virtual-machine-stacks)) of the thread creating the frame. Each frame has its own **array** of **local variables** ([§2.6.1](#local-variables)), its own **operand stack** ([§2.6.2](#operand-stacks)), and **a reference to the runtime constant pool** ([§2.5.5](TODO)) of the class of the current method.

*A frame may be extended<sub>扩展</sub> with additional<sub>附加的</sub> implementation-specific<sub>具体实现</sub> information, such as debugging information.*

The sizes of the **local variable array** and the **operand stack** are determined<sub>确定的</sub> at compile-time<sub>编译时期</sub> and are supplied<sub>提供</sub> along with<sub>随同…一起</sub> the code for the method associated with<sub>与…有关</sub> the frame ([§4.7.3](TODO)).Thus<sub>因此</sub> the size of the frame data structure depends only on the implementation<sub>实现</sub> of the Java Virtual Machine, and the memory for these structures can be allocated<sub>分配</sub> simultaneously<sub>同时</sub> on method invocation<sub>调用</sub>.

Only one<sub>唯一</sub> frame, the frame for the executing method, is active at any point in a given thread of control. This frame is referred to<sub>被称为</sub> as the current frame, and its method is known as<sub>被称为</sub> the current method. The class in which the current method is defined<sub>定义</sub> is the current class. Operations<sub>操作</sub> on **local variables** and the **operand stack** are typically<sub>通常</sub> with reference to<sub>关于</sub> the current frame.

A frame ceases<sub>结束</sub> to be `current`<sub>当前帧</sub> if its method invokes another method or if its method completes. When a method is invoked, a new frame is created and becomes `current`<sub>当前帧</sub> when control<sub>控制权</sub> transfers<sub>转让</sub> to the new method. On method return, the current frame passes<sub>沿某方向移动</sub> back the result of its method invocation, if any<sub>如果有的话</sub>, to the previous<sub>先前的</sub> frame. The current frame is then discarded<sub>丢弃</sub> as the previous<sub>先前的</sub> frame becomes the `current`<sub>当前帧</sub> one. 

Note that a frame created by a thread is local to that thread and cannot be referenced<sub>引用</sub> by any other thread.

stack frame是一个内存区块，是一个数据集，维系着方法执行过程中的各种数据信息 \
在一条活动线程中，一个时间点上只会有一个活动的栈帧<sub>栈顶栈帧，即当前正在执行的方法的栈帧</sub>是有效的，这个栈帧称为`current`<sub>当前栈帧</sub> \
`current frame` 对应的方法是`current method` \
定义`current method`的类是`current class` \
`Execution Engine`<sub>执行引擎</sub>运行的所有**字节码指令**只针对`current frame`进行操作 \
在`a`方法中调用`b`方法：
`b`方法对应的`stack frame`会被创建并**入栈**，成为**栈顶栈帧**，`b`方法对应的`stack frame`成为新的`current`。 \
当`b`方法正常执行完，`b`方法对应的`stack frame`**出栈**，则`a`方法对应的`stack frame`重新变为**栈顶栈帧**，成为新的`current`

JAVA方法两种返回函数方式<sub>stack frame出栈</sub>
- 函数正常返回，使用`return`**指令**
- 抛异常<sub>未用`try-catch`捕获处理</sub>

在`stack frame`中与**性能调优**有关的主要是`local variables`

`stack frame`中允许携带与JVM实现有关的一些**附加信息**：对程序调试提供支持的信息

### Local Variables

[Oracle 官方文档](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6.1) \
Each frame ([§2.6](#stack-frame)) contains an array of variables known as its local variables.\
The length of the local variable array of a frame is determined<sub>确定</sub> at compile-time<sub>编译时期</sub>\
and supplied<sub>提供</sub> in the binary representation<sub>二进制表示法</sub> of a class or interface along with the code for the method associated with<sub>与…有关</sub> the frame ([§4.7.3](TODO)).

**A single local variable** can hold a value of type `boolean`, `byte`, `char`, `short`, `int`, `float`, `reference`, or `returnAddress`.\
**A pair of local variables** can hold a value of type `long` or `double`.

**Local variables** are addressed by **indexing**.\
The index of the first local variable is **zero**.\
An integer is considered<sub>经过深思熟虑的</sub> to be an **index** into the local variable array if and only if<sub>当且仅当</sub> that integer is between **zero** and one less than the size of the local variable array.

A value of type `long` or type `double` occupies<sub>占用</sub> two consecutive<sub>连续的</sub> local variables.\
Such a value may only be addressed using the lesser index.\
For example, a value of type **double** stored in the local variable array at index `n` actually occupies<sub>占用</sub> the local variables with indices<sub>索引</sub> `n` and `n+1`;\
however, the local variable at index `n+1` cannot be loaded from. It can be stored into. However, doing so invalidates<sub>使无效</sub> the contents<sub>内容</sub> of local variable `n`.

The Java Virtual Machine does not require `n` to be even<sub>偶数</sub>.\
In intuitive terms<sub>直观地说</sub>, values of types `long` and `double` need not be **64-bit** aligned<sub>对齐</sub> in the local variables array.\
Implementors<sub>实现者</sub> are free to decide<sub>决定</sub> the appropriate<sub>合适的</sub> way to represent<sub>表示</sub> such values using the two local variables reserved<sub>保留</sub> for the value.

The Java Virtual Machine uses local variables to pass parameters on method invocation.\
On class method invocation, any parameters are passed in consecutive<sub>连续的</sub> local variables starting from local variable `0`.\
On **instance method** invocation, local variable `0` is always used to pass a **reference to the object** on which the instance method is being invoked (`this` in the Java programming language).\
Any parameters are subsequently<sub>随后</sub> passed in consecutive<sub>连续的</sub> local variables starting from local variable `1`.

`Local Variables`是一个`Array`，存储**方法参数**和定义在**方法体内的局部变量**，数据类型包括：**编译期可知**的各种**JVM基本数据类型、reference、returnAddress**
**JVM基本数据类型**：`byte`、`boolean`、`short`、`char`、`int`、`float`、`long`、`double`\
**reference对象引用类型**：并不等同于对象本身，可能是一个指向对象**起始地址的引用指针**，也可能是指向一个代表对象的**句柄**或者其他与此对象相关的位置\
**returnAddress**：指向一条**字节码指令地址**

`Local Variables`建立在线程上是**线程的私有数据**，因此**不存在数据安全问题**\
`Local Variables`中的存储单位以`Slot`<sub>变量槽</sub>来表示。数据从`Local Variables Array` 的索引`0`位置开始存放,占用`64bit`的`long`和`double`类型数据会占用**两个Slot**\；使用时，用其占用的第一个Slot的index,其余的数据只占用**一个Slot**。

`Local Variables`所需的内存空间在**编译期**完成分配，并保存在**方法**的`Code`属性的`maximum local variables`数据项中\
当进入一个方法时，这个方法需要在`Stack Frame`中分配多大的`Local Variables`空间是**完全确定**的，\
在**方法运行期间不会改变**`Local Variables`的大小<sub>**大小指Slot的数量**</sub>，\
JVM真正使用多大的内存空间来实现一个Slot，由具体的JVM实现自行决定<sub>譬如按照一个Slot占用32bit、64bit，或者更多</sub>。
 
当一个实例方法被调用时，方法参数和方法体内部定义的局部变量将会按照声明顺序放置到`local variables array`。如果`current frame`是由**构造方法**或者**实例方法**创建的，那么该对象引用`this`将会存放在索引为`0`的Slot，其余变量按照位置顺序继续排列。**静态方法**不存在对象引用`this`，其`local variables`不会保存`this`,所以**静态方法中**不能使用`this`

如果一个局部变量过了其作用域，那么在其作用域之后申明的新的局部变量就很可能会复用过期局部变量的`slot`,从而达到节省资源的目的

`local variables`中的变量也是重要的**垃圾回收根结点**，只要被`local variables`中**直接**或**间接**引用的对象都**不会被回收**

### Operand Stacks

[Oracle 官方文档](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6.2) \
Each frame ([§2.6](#stack-frame)) contains a `LIFO`<sub>last-in-first-out</sub> stack known as its `operand stack`.\
The maximum depth of the operand stack of a frame is determined<sub>确定</sub> at compile-time<sub>编译时期</sub> and is supplied<sub>提供</sub> along with the code for the method associated with<sub>与…有关</sub> the frame ([§4.7.3](TODO)).

Where it is clear<sub>明确的</sub> by context<sub>上下文</sub>, we will sometimes refer<sub>称…(为)</sub> to the operand stack of the current frame as simply<sub>简单地</sub> the operand stack.

The operand stack is empty when the frame that contains it is created. \
The Java Virtual Machine supplies<sub>提供</sub> instructions<sub>(计算机的)指令</sub> to load `constants`<sub>常量</sub> or `values from local variables` or `fields`<sub>字段</sub> onto the operand stack.\
Other Java Virtual Machine instructions<sub>(计算机的)指令</sub> take operands from the operand stack, operate on them, and push the result back onto the operand stack.\
The operand stack is also used to prepare<sub>把…预备好</sub> parameters to be passed to methods and to receive method results.

For example, the `iadd` instruction ([§iadd](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-6.html#jvms-6.5.iadd)) adds two int values together.\
It requires that the int values to be added be the top two values of the operand stack, pushed there by previous<sub>先前的</sub> instructions.\
Both of the int values are popped from the operand stack.They are added, and their sum is pushed back onto the operand stack.\
Subcomputations<sub>子计算</sub> may be nested<sub>嵌套</sub> on the operand stack, resulting in<sub>导致</sub> values that can be used by the encompassing<sub>涉及</sub> computation<sub>计算</sub>.

Each entry on the `operand stack` can hold a value of any Java Virtual Machine type,including a value of type `long` or type `double`.

Values from the operand stack must be operated upon<sub>在……上</sub> in ways appropriate to<sub>适用于</sub> their types.\
It is not possible, for example, to push two `int` values and subsequently<sub>随后</sub> treat<sub>把…看作</sub> them as a `long` or to push two `float` values and subsequently add them with an `iadd` instruction.\
A small number of<sub>少数</sub> Java Virtual Machine instructions<sub>the `dup` instructions (§dup) and `swap` (§swap)</sub>operate on run-time data areas as raw<sub>原始的</sub> values without regard<sub>关注</sub> to their specific<sub>具体的</sub> types;\
these instructions<sub>(计算机的)指令</sub> are defined in such a way<sub>必须如此</sub> that they cannot be used to modify<sub>修改</sub> or break up individual<sub>单独的</sub> values.\
These restrictions<sub>限制规定</sub> on operand stack manipulation<sub>操作</sub> are enforced<sub>强制性的</sub> through<sub>通过</sub> class file verification<sub>验证</sub> (§4.10).

At any point in time, an operand stack has an associated<sub>相关的</sub> depth, where a value of type `long` or `double` contributes<sub>添加</sub> two units<sub>单位</sub> to the depth and a value of any other type contributes<sub>添加</sub> one unit.

《深入理解Java虚拟机》\
JVM的解释执行引擎被称为“基于栈<sub>`operand stack`</sub>的执行引擎<sub>`execution engine`</sub>

在概念模型中，两个不同的`Stack frame`作为不同方法的`jvm stack`元素，是完全相互独立的。\
但是在大多虚拟机的实现里都会进行一些优化处理，令两个`Stack frame`出现一部分重叠。\
让下面`stack frame`的部分`operand stack`与上面`stack frame`的部分`local variables`重叠在一起，这样做不仅节约了一些空间，\
更重要的是在进行方法调用时就可以直接共用一部分数据，无须进行额外的参数复制传递\
![两个stack frame数据共享](img/%E4%B8%A4%E4%B8%AAstack%20frame%E6%95%B0%E6%8D%AE%E5%85%B1%E4%BA%AB.svg)

`operand stack` 在方法执行过程中，根据**字节码指令**，往栈中写入数据或提取数据，即入栈<sub>`push`</sub>或出栈<sub>`pop`</sub>。某些字节码指令将值压入`operand stack`，其余的字节码指令将操作数取出`operand stack`，使用<sub>复制、交换、求和</sub>后把结果压入`operand stack`

如果被调用的方法带有**返回值**，其返回值将会被压入`current stack frame`的`operand stack`中，并更新`The pc Register`中下一条需要执行的字节码指令\
`operand stack`中元素的数据类型必须与字节码指令**严格匹配**，这由**编译器**在**编译期间**进行验证，同时在**类加载过程**中的**类检验阶段**的**数据流分析阶段**要再次验证\
`operand stack`主要保存计算过程的中间结果，同时作为计算过程中变量临时的存储空间\
`operand stack`是`JVM execution engine`的一个工作区，当一个方法刚开始执行的时候，一个新的`stack frame`也会随之被创建，这个方法的`operand stack`是空的（已创建）\
每一个`operand stack` 都会拥有一个明确的栈深度用于存储数值，其所需的最大深度在**编译期**确定，保存在方法的`Code`属性中，为`max_stack`的值，与`local variables`大小无关\
`operand stack`中任何一个元素都是任意的Java数据类型，与`local variables`的`slot`类似
- `32bit`的类型占用一个`operand stack`单位深度
- `64bit`的类型占用两个`operand stack`单位深度

`operand stack`使用数组实现, 不使用访问数组索引进行数据访问，只能通过标准的入栈<sub>`push`</sub>和出栈<sub>`pop`</sub>来完成数据访问

### Dynamic Linking

[Oracle 官方文档](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6.3) \
Each frame ([§2.6](#stack-frame)) contains a reference to the `run-time constant pool` ([§2.5.5](TODO)) for the type of the current method to support dynamic linking of the method code.The class file code for a method refers<sub>表示</sub> to methods to be invoked and variables to be accessed<sub>访问</sub> via<sub>通过</sub> symbolic<sub>符号</sub> references. Dynamic linking translates<sub>转换</sub> these symbolic method references into concrete<sub>具体的</sub> method references, loading classes as necessary to resolve<sub>解决</sub> as-yet-undefined<sub>尚未定义</sub> symbols, and translates variable accesses<sub>访问</sub> into appropriate<sub>恰当的</sub> offsets<sub>位置</sub> in storage<sub>存储</sub> structures associated with<sub>与…有关</sub> the run-time location of these variables.

This late binding<sub>延迟绑定</sub> of the methods and variables makes<sub>使</sub> changes in other classes that a method uses less likely<sub>可能的</sub> to break this code.

《深入理解Java虚拟机》每个`Stack Frame`都包含一个指向`run-time constant pool`<sub>运行时常量池</sub>中该`Stack Frame所属方法的引用`，持有这个引用是为了支持方法调用过程中的`Dynamic Linking`<sub>比如`invokedynamic`指令</sub>。

Java源文件编译为字节码文件时，所有的**变量**和`method references`<sub>方法引用</sub>都作为`symbolic reference`<sub>符号引用</sub>保存在`class文件的常量池`中。**字节码**中的**方法调用指令**以`class文件的常量池`里指向方法的`symbolic reference`<sub>符号引用</sub>作为参数。这些`symbolic reference`<sub>符号引用</sub>一部分会在**类加载阶段**或者**第一次使用**的时候就被转化为**concrete<sub>具体的</sub> method references**，这种转化被称为**静态解析**。另外一部分将在**每一次运行期间**都转化为**concrete<sub>具体的</sub> method references**，这部分就称为`Dynamic Linking`。

`Dynamic Linking` 通过`symbolic reference`<sub>符号引用</sub>指向`run-time constant pool`<sub>运行时常量池</sub>中的`method references`<sub>方法引用</sub>

#### 方法的调用--多态

JVM 将`symbolic reference`<sub>符号引用 `#3`、`#2`...</sub>转换为直接引用与方法的**绑定机制**有关

- `static linking`<sub>静态链接</sub>
    - 当一个字节码文件被装载进JVM内部时，如果被调用方法在**编译期**可知，且运行期保持不变时。将被调用方法的`symbolic reference`<sub>符号引用</sub>转换为**直接引用**的过程称为`static linking`<sub>静态链接</sub>
- `dynamic linking`<sub>动态链接</sub>
    - 如果被调用方法在编译期无法被确定下来，也就是说只能够在**程序运行期**将调用方法的`symbolic reference`<sub>符号引用</sub>转换为**直接引用**。由于这种引用转换过程具备动态性因此也被称为`dynamic linking`<sub>动态链接</sub>

方法的**绑定机制**：是一个**字段**、**方法**、**类**在`symbolic reference`<sub>符号引用</sub>被替换为**直接引用**的过程，仅发生一次
- `early binding`<sub>早期绑定</sub>：被调用的目标方法在**编译期**可知，且运行期保持不变<sub>即可将这个方法与所属的类型进行绑定</sub>，因此可以使用`static linking`<sub>静态链接</sub>的方式将`symbolic reference`<sub>符号引用</sub>转换为**直接引用**
- `late binding`<sub>延迟绑定</sub>：如果被调用的方法在**编译期**无法被确定下来只能在**程序运行期**根据实际的类型，绑定相关的方法，这种绑定方式就是`late binding`<sub>延迟绑定</sub>

**虚函数**：Java中任何一个普通方法其实都具备**虚函数**的特征，相当于`c++`语言中的**虚函数**<sub>`c++`中需要使用关键字`virtual`来显示定义</sub>。如果java程序中不希望某个方法拥有虚函数的特征，使用关键字`final`修饰<sub>不能被重写，编译期确定，不再具备多态性</sub>

多态<sub>类继承，且重写方法</sub>
- 子类对象的多态性前提：
    - 类的继承
    - 方法的重写
- 面向对象的高级语言，尽管在语法风格上存在差异，但是都支持**封装**、**继承**、**多态**等面向对象特性
    - 封装
    - 继承
    - 多态
- 具备多态性，就具备`early binding`和`late binding`两种绑定方式，可以在编译期确定具体调用哪个方法

</br>

- 虚方法
  - 具备多态性的方法
  - 除了**静态方法**、**私有方法**、**final方法**、**实例构造器**、**父类方法**
  - `invokevirtual`指令：调用所有**虚方法**<sub>`final`修饰的方法为非虚方法，也使用`invokevirtual`指令</sub>
  - `invokeinterface`指令：调用**接口方法**
- 非虚方法
  - 不具备多态性的方法
  - `invokestatic`和`invokespecial`指令调用的方法称为非虚方法，其余的<sub>`final`修饰的方法为非虚方法</sub>称为虚方法
  - 方法在编译期确定具体的调用版本，这个版本在运行时不可变
  - **静态方法**、**私有方法**、**final方法**、**实例构造器**、**父类方法**都是非虚方法
  - `invokestatic`指令：调用**静态方法**，`ClassLoaderSubSystem.Linking.Resolve`阶段<sub>解析阶段</sub>确定唯一方法版本
  - `invokespecial`指令：调用`<init>`方法、**私有方法**、**父类方法**，`ClassLoaderSubSystem.Linking.Resolve`阶段<sub>解析阶段</sub>确定唯一方法版本
- 动态调用指令
  - `invokedynamic`指令：动态解析出需要调用的方法，然后执行。**支持由用户确定方法版本**。
  - `invokevirtual`、`invokeinterface`、`invokestatic`、`invokespecial`指令固化在JVM内部，方法的调用执行**不可人为干预**。

#### 方法重写的本质

- 找到`operand stack`栈顶元素所执行的对象的实际类型，记作`c`<sub>当调用一个对象的方法时，会先把该方法的对象压入`operand stack`，通常为`invokevirtual`指令</sub>
- 如果在类型`c`中找到与**常量池**中**描述符**、**简单名称**都相符的方法<sub>查找c中有没有该方法</sub>，则进行**访问权限校验**
    - 如果访问权限校验通过，则返回这个方法的**直接引用**，查找过程结束。
    - 如果访问权限校验不通过，则返回`java.lang.IllegalAccessError`异常。
- 如果在类型`c`中没找到与**常量池**中**描述符**、**简单名称**都相符的方法<sub>查找c中有没有该方法</sub>，按照**继承关系从下往上**依次对`c`的各个父类进行`第2步`的搜索和验证
- 如果始终没有找到合适的方法，则抛出`java.lang.AbstractMethodError`异常

`java.lang.IllegalAccessError`异常<sub>jar冲突可能会出现</sub>: 程序试图访问或修改一个属性或调用一个方法，当这个属性或方法没有权限访问，一般会引起编译器异常，这个错误如果发生在运行时，就说明一个类发生了不兼容的改变。

#### 虚方法表

- 在面向对象编程OOP，频繁的使用到动态分派，若每次动态分派的过程都要重新在类的方法元数据中搜索合适的目标，会影响执行效率
- JVM采用在类的方法区建立一个`virtual method table`<sub>虚方法表</sub>，使用索引表来代替查找。非虚方法不会出现在表中。
- 每个类中都有一个`virtual method table`<sub>虚方法表</sub>，存放着各个方法的实际入口
- `virtual method table`<sub>虚方法表</sub>在`ClassLoaderSubSystem.Linking.resolve`阶段<sub>将常量池内的符号引用转换为直接引用</sub>被创建并开始初始化，类的变量初始值准备完成后，JVM会把该类的方法表也初始化完毕

### Method Invocation Completion

《深入理解Java虚拟机》当一个方法开始执行后，只有两种方式退出这个方法。
- 第一种方式是**执行引擎**遇到任意一个[方法返回的字节码指令](#返回字节码指令)，这时候可能会有返回值传递给上层的方法调用者<sub>调用当前方法的方法称为调用者或主调方法</sub>，方法是否有返回值以及返回值的类型将根据遇到何种[方法返回的字节码指令](#返回字节码指令)来决定，这种退出方法的方式称为[Normal Method Invocation Completionsub](#normal-method-invocation-completion)<sub>正常调用完成</sub>。
- 另一种退出方式是在方法执行的过程中遇到异常，且这个异常没有在方法体内得到妥善处理。无论是JVM内部产生的异常，还是代码中使用`athrow`字节码指令产生的异常，只要在本方法的**异常表**中没有搜索到匹配的**异常处理器**，就会导致方法退出，这种退出方法的方式称为[Abrupt Method Invocation Completion](#abrupt-method-invocation-completion)<sub>异常调用完成</sub>。一个方法使用[Abrupt Method Invocation Completion](#abrupt-method-invocation-completion)的方式退出，不会给他的上层调用者提供任何返回值。方法执行过程中抛出异常的**异常处理器**，存储在一个**异常处理表**，方便在发生异常的时候找到处理异常的代码。

无论采用何种退出方式，在方法退出后，必须返回到最初方法被调用时的位置，程序才能继续执行，方法返回时可能需要在`stack frame`种保存一些信息，用来帮助恢复他的上层主调方法的执行状态。一般来说，方法正常退出时，主调方法的`pc register`的值就可以作为**返回地址**<sub>即调用该方法的指令的下一条指令的地址</sub>，`stack frame`中很可能会保存这个`pc register value`。而方法异常退出时，返回地址是要通过**异常处理器表**来确定的，`stack frame`中**一般不会**保存这部分信息，异常退出不会给上层调用者生产任何的返回值。

方法退出的过程实际上等同于把当前`stack frame` **出栈**，因此退出时可能<sub>基于jvm规范讨论，具体执行哪些操作由具体jvm实现来确定</sub>执行的操作有：恢复上层方法的`local variables`和`operand stack`，把返回值<sub>如果有返回值</sub>压入调用者`stack frame`的`operand stack`，调整`pc register`的值以指向方法调用指令后面的一条指令等。

#### 返回字节码指令

- `ireturn`
  - boolean、byte、char、short、int
- `lreturn`
  - long
- `freturn`
  - float
- `dreturn`
  - double
- `areturn`
  - 引用类型
- `return`
  - void 方法、实例初始化方法、类、接口的初始化方法

#### Normal Method Invocation Completion

[Oracle 官方文档](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6.4) \
A method invocation completes normally<sub>正常地</sub> if that invocation does not cause<sub>造成</sub> an exception ([§2.10](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.10)) to be thrown, either directly<sub>直接地</sub> from the Java Virtual Machine or as a result of executing<sub>执行</sub> an explicit<sub>明确的</sub> throw statement<sub>语句</sub>. If the invocation of the current method completes normally, then a value may be returned to the invoking method. This occurs<sub>发生</sub> when the invoked method executes<sub>执行</sub> one of the return instructions<sub>(计算机的)指令</sub> ([§2.11.8](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.11.8)), the choice<sub>选择</sub> of which must be appropriate<sub>合适的</sub> for the type of the value being returned (if any).

The current frame [§2.6](#stack-frame)) is used in this case to restore<sub>恢复</sub> the state of the invoker<sub>调用者</sub>, including its `local variables` and `operand stack`, with the `program counter` of the invoker<sub>调用者</sub> appropriately<sub>适当地</sub> incremented<sub>递增</sub> to skip past<sub>跳过</sub> the method invocation instruction<sub>(计算机的)指令</sub>. Execution<sub>执行</sub> then continues normally<sub>正常地</sub> in the invoking method's frame with the returned value (if any) pushed onto the `operand stack` of that frame.

#### Abrupt Method Invocation Completion

[Oracle 官方文档](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6.5) \
A method invocation<sub>调用</sub> completes abruptly<sub>意外地</sub> if execution<sub>执行</sub> of a Java Virtual Machine instruction<sub>(计算机的)指令</sub> within the method causes the Java Virtual Machine to throw an exception ([§2.10](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.10)), and that exception is not handled within the method. Execution<sub>执行</sub> of an `athrow` instruction ([§athrow](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-6.html#jvms-6.5.athrow)) also causes<sub>引起</sub> an exception to be explicitly<sub>明确地</sub> thrown and, if the exception is not caught<sub>抓住</sub> by the current method, results in abrupt<sub>意外地</sub> method invocation completion. A method invocation that completes abruptly<sub>意外地</sub> never returns a value to its invoker<sub>调用者</sub>.

### 附加信息

《深入理解Java虚拟机》JVM规范运行jvm实现增加一些规范里没有描述的信息到stack frame中，例如与调试、性能收集相关的信息，这部分信息完全取决于具体jvm实现。

## 指令集架构

- 基于栈式架构（JVM）
    - 设计和实现更**简单**，适用于**资源受限**的系统
    - 避开寄存器的分配难题：使用**零地址指令**方式分配
    - 指令流中的指令大部分是零地址指令，其执行过程依赖于**操作栈**，指令集更小，编译器容易实现
    - 不需要硬件支持，**可移植性**更好，更好实现**跨平台**
- 基于寄存器架构
    - 典型的应用是x86的二进制指令集<sub>传统PC以及Android的Davlik虚拟机</sub>
    - 指令集架构完全依赖**硬件**，**可移植性差**
    - 性能优秀和执行更高效
    - 花费**更少**的指令去完成一项操作
    - 在大部分情况下，基于寄存器架构的指令集往往都以**一地址指令**、**二地址指令**和**三地址指令**为主，而基于栈式架构的指令集却是以零地址指令为主

## ToS

栈顶缓存 `ToS`<sub>`Top-of-Stack Cashing`</sub>\
`JVM`基于**栈式架构**，使用的**零地址指令**更加紧凑，完成一项操作的时候必然需要使用更多的入栈和出栈指令，意味着将需要更多的指令分派<sub>`instruction dispatch`</sub>次数和内存读写次数\
由于操作数是存储在**内存**中，因此频繁地执行内存读写操作必然会影响执行速度，为了解决此问题，`HotSpot JVM`的设计者们提出**栈顶缓存技术**<sub>`ToS Top-of-Stack Cashing`</sub>

将**栈顶元素**全部缓存在**物理CPU寄存器**中<sub>CPU寄存器:指令更少，执行速度快</sub>，降低对内存的读写次数，提升`execution engine`的执行效率。*ToS还需要在HotSpot JVM具体进行测试才能运用*

## 拓展

jvm stack 溢出的情况：
`Xss10M` 设置 `jvm stack size`\
jvm stack固定分配大小，当最后一个stack frame所占内存大于stack的剩余容量即会出现StackOverFlowError\
jvm stack动态分配，当尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的JVM stack，则OutOfMemoryError

调整stack大小，能保证不出现溢出吗?
stack frame不确定，方法嵌套调用深度不确定，故扩大stack size不能100%保证不出现溢出，最多延迟溢出的时间\
eg： 递死归

分配stack越大越好吗
- 不是，要综合考虑
- 挤占线程空间
- 挤占其他空

Garbage Collection 是否会涉及到 jvm stack
- 不会涉及jvm stack

方法中定义的局部变量（local variable）是否线程安全？
- 不一定，如果局部变量一直在当前方法内存活，则线程安全\
- 如果局部变量作为**参数**传入，如果**多线程**调用此方法，则该局部变量不安全\
- 如果局部变量作为**返回值**返回，并被其他方法使用时，如果**多线程**，也不安全

**变量分类**
- 按照**数据类型**分类
    - **基本数据类型**
    - **引用数据类型**
- 按照在类中**声明的位置**分类
    - **成员变量**: 使用前都经历过，默认初始化赋值
        - **类变量**<sub>`static`修饰又称为**静态变量**</sub>：`linking`的`prepare`阶段会给类变量赋**默认值** `-->` `Initialization`阶段**显示赋值**<sub>静态代码块赋值</sub>
        - **实例变量**：随着对象的创建，会在`heap`空间中分配**实例变量空间**并进行**默认赋值**
    - **局部变量**：使用前**必须显示赋值**，否则**编译不通过**