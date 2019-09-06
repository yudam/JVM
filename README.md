## 第一章：Java内存区域与内存异常

### 1.1 运行时数据区域
Java虚拟机在执行Java程序的过程中会把会把它所管理的内存划分为若干个不同的数据区域。
#### 1.1.1 程序计数器
线程私有。一块较小的内存区域，可以看作是当前线程所执行的字节码的行号指示器，依赖程序计数器来执行字节码指令，异常处理，线程恢复等 。 
每条线程都有一个独立的程序计数器，是为了在线程切换回来的时候能够恢复到正确的执行位置。
#### 1.1.2 Java虚拟机栈
线程私有。描述的是Java方法执行的内存模型，每个方法在执行的时候都会创建一个栈帧，用来存储局部变量表，操作数栈，动态链接，方法出口等信息。
每一个方法的调用到执行完成都对应着一个入栈道出栈的操作。  
局部变量表中昂放入了编译器移植的各种基本数据类型,引用类型和returnAddress类型（一条字节码指令的地址）。其中64位的
long和double类型的数据占用两个局部变量空间，其余占用一个。
#### 1.1.3 本地方法栈
线程私有，与虚拟机栈发挥的作用类似，不同点在于虚拟机栈为虚拟机执行Java方法（字节码）服务，而本地方法栈为虚拟机使用到的Native方法服务。
#### 1.1.4 Java堆
线程共享。Java虚拟机所管理的内存区域最大的一块。此内存区域的唯一目的是存方对象实例，所有的对象和实例都在堆上分配。Java堆是垃圾收集器管理的主要区域。
Java堆可以划分为新生代，老年代等。Java堆可以处于物理上不连续的内存空间中，逻辑上是连续的即可。
#### 1.1.5 方法区
线程共享。存储被虚拟机加载的类信息，常量，静态变量，即使编译器编译后的代码等。
* 运行时常量池：方法区的一部分，用于存放编译器生成的各种字面亮和符号引用。
#### 1.1.6 直接内存
不是虚拟机运行时区域的一部分。直接内存的分配不受Java堆大小的限制，但是受到本机总内存和处理器寻址空间的制约。

### 1.2 对象
#### 1.2.1 对象的创建
* 当虚拟机接收到一个new指令，检查这个指令的参数能否在常量池中定位到一个类的符号引用，并检查这个符号引用代表的类是否已被夹在，解析，初始化。
如果没有，那必须先执行相应的类加载过程。
* 为新生的对象分配内存空间。
* 内存分配完成后，将分配到的内存空间都出实话为零值，保证了对象的势力字段在Java代码中可以不赋初始值就是直接使用。
* 对对象进行设置，例如该对象属于哪一个类，如何才能找到类的元素信息，对象的哈希码，对象的GC分代年龄等。

#### 1.2.2 对象的内存布局
对象在内存中存储的布局可以分为三块区域：对象头，实力数据和对其填充。
* 对象头包括两部分：第一部分用于存储对象自身的运行时数据，如哈希吗，GC分代年龄，锁状态标志等。
                 另一部分是类型指针，即对象指向它的类元数据的指针，虚拟机通过这个指针确定这个对象是哪个类的实力。
* 实例数据：对象真正存储的有效信息。
* 对其填充：只起到占位符的作用。

#### 1.2.3 对象的访问
Java程序通过栈上的reference数据来操作堆上的对象。对象的访问方式分为使用句柄和直接指针。
* 使用句柄：Java堆中划分一块区域来存储句柄池。句柄中包含了对象实力数据和对象类型数据的具体地址。
* 直接指针：refreence中存储的直接就是对象地址。
使用句柄好处在于refrerence中存储的是稳定地址。使用指针的好处在于访问速度更快，


### 1.3 OutOfMemoryError异常
#### 1.3.1 Java溢出
Java堆用于存储对象实例，只要不断的创建对象，对象数量到达最大堆的容量后就会产生内存溢出
#### 1.3.2 虚拟机栈和本地方法栈
* 如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常。
* 如果虚拟机在扩展栈内存时无法申请到足够的内存空间，则抛出OutOfMemoryError异常。

解决方法：虚拟机提供了参数来控制堆和方法区这两部分内存的最大值。减少最大堆和减少栈容量来换取更多的线程。

#### 1.3.3 方法区的溢出
方法去用于存放Class的相关信息。如类名，访问修饰符，常量池，基本思路是运行时产生大量的类去填满方法区。

#### 1.3.4 本机直接内存溢出
并没有真正向操作系统申请内存，而是通过计算得知内存无法分配。












