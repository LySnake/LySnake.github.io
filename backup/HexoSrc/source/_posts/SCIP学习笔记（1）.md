---
title: SCIP学习笔记（1）
comments: true
categories:
  - archives
  - technology
tags:
  - Cocos
date: 2018-07-21 16:13:12
updated: {{ updated }}
description: 个人学习SICP的记录笔记
---

## 序和前言 ##

有关这些程序具体是关于什么的、服务于哪类应用等等的情况常常并不重要，重要的是它们的性能如何，在用于构造更大的程序时能否与其它程序平滑。程序员们必须同时追求具体部分的完美和汇合的适宜性。

人的大脑、计算机程序的集合以及计算机本身。每一个计算机程序都是现实中的或者精神中的某个过程的一个模型，通过人的头脑孵化出来。这些过程出现在人们的经验或者思维之中，数量上数不胜数，详情琐碎繁杂，任何时候人们都只能部分地理解它们。

计算机程序设计领域之令人兴奋的源就在于它所引直怕连绵不绝的发现，在我们的头脑之中，在由程序所表达的计算机制之中，以及在由此所导致的认识爆炸之中。

因为大的程序是从小东西成长起来的，开发出一个标准化的程序结构的塸库，并保证其中的每种结构的正确性--我们称它们为惯用法，再学会如何利用一些已经证明很有价值的组织技术，奖这些结构组合成更大的结构，这些都是到头重要的。本书中奖详尽地讨论这些技术。理解这些技术，对于参与这种被称为程序设计的具有创造性的事业是最最本质的。特别值得提出的是，发现并掌握强有力的组织技术，奖提升我们构造大型的重要程序的能力。

计算机永远都不够大也不够快。硬件技术的每一次突破都带来了更大规模的程序设计事业，新的组织原理，以及更加丰富的抽象模型。每个读者都应该反复地问自己“支哪里才是头儿，到哪里才是头儿？”

执行的时间和对数据存储的需求。程序员应该追求好的算法和惯用法。即使某些程序难以精确地描述，程序员也有责任去估计它们的性能，并要继续高潮去改进之。

在任何非常大的程序设计工作中，一条有用的组织原则就是通过发明新语言，去控制和隔离作业模块之间的信息流动。

一个计算机语言并不仅仅是让计算机去执行操作的一种方式，更重要的，它是一种表述有关方法学的思想的新颖的形式化媒介。因此，程序必须写得能够供人们阅读，偶尔地去供计算机执行。其次，我们相信，在这一层次的课程里，最基本的材料并不是特定的程序设计语言的语法，不是有效的计算某种功能的巧妙算法，也不是算法的数学分析或者计算的本质基础，而是一些能够用于控制大开支软件系统的智力复杂性技术。

Lisp语言扔重要优点：它们只有不多几种构造复合表达式的方式，几乎没有语法结构。所有的形式化性质都可以在一个小时里讲完，就像下象棋的规则似的。另一优势在于，与我们所知的任何其它语言相比，它可以支持（但并不是强制性的）更多的能用于以模块化的方式分解程序的大规模策略。

## 第一章 构造过程抽象 ##

奖若干简单的认识组合为一个复合的认识，由此产生出各种复杂的认识。将二个认识放在一起对照，不管它们如何简单或者复杂，在这样的做时并不奖它们合而为一。由此得到有关它们的相互关系的认识。将有关认识与那些在初建中和它们同在的所有基它认识隔离开，这就是抽象，所有具有普遍性的认识都是这样得到的。

Lisp是20世纪50年代后期发明的一种记法形式，是为了能对某种特定形式的逻辑表达式（称为递归方程）的使用做推理，递归方程可以作为计算的模型。一个Lisp解释器就像是一台机器，它能实现用Lisp语言描述的计算过程。第一个Lisp解释器是McCarthy在MIT电子研究实验室的人工智能组和MIT计算中心里他的同事和学生的帮助下实现的。Lisp的名字来自表处理（LISt Processing），其设计是为了提供符号计算的能力，以便能用于解决一些程序设计问题，例如代数表达式的符号微分和积分。它包含了适用于这类目的的一些新数据对旬，称为原子和表，这是它与那一时代的所有其它语言之间最明显的不同之处。

Lisp并不是一个刻意设计努力的结果，它以一种试验性的非正式的方式不断深化，以满足用户的需要和实际的各种考虑。Lisp的这种非官方深化持续了许多年，Lisp用户社团具有掏制定这一语言的“官方”定义企图的传统。本书使用的是Scheme方言。

虽然Lisp还没有完全战胜有关它特别低效的诋毁，但它现在已被用于许多性能并不是最重要考虑因素的应用领域。例如，Lisp已经成为操作系统外壳语言的一种选择，作为编辑器和计算机辅助设计系统的扩充语言等等。

计算过程的Lisp描述（称为过程）本身又可以作为Lisp的数据来表示和操作。Lisp可以将过程作为数据进行处理的灵活性，使它成为探索这些技术的最方便的现存语言之一。能将过程表示为数据的能力，也使用Lisp成为编写那些必须将其它程序当作数据去操作的程序的最佳语言，如支持计算机语言的解释器和编译器。除了这些考虑之外，用Lisp编程本身也是极其有趣的。

### 1.1 程序设计的基本元素 ###

**组合式**：如`(+ 3 2)`就是用一对括号括起一些表达式，形成一个表，用于表示一个过程应用。在表里最左的元素称为运算符，其他元素都称为**运算对象**。要得到这种组合式的值，采用的方式就是将由去处符所刻画的过程应用于有关的**实际参数**，而所谓实际参数也就是那些运算的对象的值。

将运算符放在所有去处对象左边，这种形式称为前缀表示。前缀表示与掌击数学表示差别很大，而前缀表示也有一些优点，其中之一就是它完全适用于可能带有任意的过程。另一个优点是它可以直接扩充，允许出现组合式嵌套的情况。

```Scheme
(+ (* 3
      (+ (* 2 4)
         (+ 3 5)))
   (+ (- 10 7)
      6))
```

上面就是遵循一种称为美观打印的格式规则。按照这种规则，在写一个很长的组合式时，我们令其中的各个运算对象垂直对齐。这样排列的结果能很好地显示出表达式的结构。

在Scheme里，给事物命名通过**define(定义)**的方式完成，如`(define size 2)`定义变更，`(define (func) (+ 3 2 3))`定义函数。define是我们所用的语言里最简单的方法，它允许我们用一个简单的名字去引用一个组合运算的结果。一般而言，计算得到的对象完全可以具有非常复杂的结构，如果每次需要使用它们时，都必须记住并重复地写出它们的细节，那将是极端不方便的事情。

一般而言，我们应该把递归看做一种处理层次性结构的（像树这样的对象）极强有力的技术。事实上，“值向上穿行”形式的求值形式是一类更一般的计算过程的一个例子，这种计算过程称为树形积累。

**过程定义**：是一种威力更加强大的抽象技术，通过它可以为复合操作提供名字，而后就可以将这样的操作作为一个单元使用了，如`(define (square x ) (* x x ))`。

**正则序求值**：当函数的实参是一个组合式时，把组合式带入到函数体能所有形参位置作为替换，类型C的宏替换一样。可能会浪费运算次数，但也可以成为特别有价值的工具。第3章和第4章。

**应用序求值**：当函数的实参是一个组合式时，把组合式的结果值赋给形参进行函数运算。其它语言基本都是使用这种方式进行入参的。

```Scheme
//测试是使用正则序求值还是应用序求值
(define (p) (p))
(define (test x y )
   (if (= x 0)
       0
       y))
```

**cond表达式**:类似switch语句，当条件表达式的求值方式如下：首先求值谓语`<p1>`，如果它的值是false，那么就去求值`<p2>`，如果`<p2>`的值是false就去求值`<P3>`.这一过程将继续做下去，直到发现了某个谓词的值为true或全为false直接处理else子名，此时解释器就返回相应子句中的序列表达式`<e>`的值，以这个值作为整个条件表达式的值。如果没有找到为真的`<p>`，cond的值就没有定义。

```Scheme
(cond  (<p1> <e1>)      
	   (<p2> <e2>)
		...
	   (<pn> <en>)
       (else <e>)）
```

我们用术语**谓词**指那些返回真或假的过程，也指那些能求出真或者假的值的表达式。

```Scheme
(if <predicate> <consequent> <alternative>)
(define (abs x )
    (if (< x 0)
        (- x)
        x))
```

**if表达式**：解释器从求值其`<predicate>`部分开始，如果`<predicate>`为真，解释器就去求值`<consequent>`并返回其值，否则它就去求值`<alternative>`并返回其值。

**逻辑表达式**：

```Scheme
(and <e1> ... <en>)
(or <e1> ... <en>)
(not <e>)
```
