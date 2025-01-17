# 总结

- [总结](#总结)
  - [测试先行](#测试先行)
  - [一个测试一个断言](#一个测试一个断言)
  - [测试抽象](#测试抽象)
    - [不必要的测试代码](#不必要的测试代码)

本章将学习如何设计测试，以便提升回报并避免其成为维护负担。本章将介绍以下几个方法：

1. FIRST 助记符---审核测试的重要方法。
2. 一个测试一个断言---帮助限制测试的大小准则。
3. 测试抽象---保持测试可读性的核心原则。

## 测试先行

可以通过对照 FIRST 原则来审视编写的测试是否足够好：

- F 是[快速（Fast）](./7.md#快速-fast)：单元测试构建与执行应当非常快速。只有速度越快开发人员才会更加频繁的执行单元测试获取更多的反馈。
- I 是[独立（Isolated）](./7.md#独立-isolated)：每个测试应该验证一小段独立于外部因素的逻辑，只由一个原因导致失败方便问题的查找。测试不仅要独立于产品系统的外部因素，还要彼此独立。
- R 是[可重复（Repeatable）](./7.md#可重复)：高质量的单元测试是可重复的，即可以一遍遍地运行，并且总是获得相同的结果，无论其他测试（如果有的话）是否先运行。
- S 是[自我验证（Self-verifying）](./7.md#自我验证)：单元测试必须执行代码并验证代码能否在你不参与的情况下自动工作。一个单元测试至少有一个断言。
- T 是[及时（Timely）](./7.md#及时)：什么时候编写测试？及时编写意味着你要先编写测试。为什么？因为你在做TDD，而且正是因为它是保持高质量代码库的最好方法，所以你才使用它。

## 一个测试一个断言

在系统中测试驱动开发小而不相关联的行为，而规定行为以及验证此行为确实可行的方法——断言。另外测试必须清楚地陈述意图，而意图最重要的声明是测试名称，它应该澄清上下文和目标。因此当一个测试覆盖的行为越多，测试名称就越不能准确的描述行为。

要谨记一个关键的准则——一个测试只有一个行为。如果还不够清楚，那么可以这样认为，任何拥有条件逻辑（如if语句）的测试几乎都与此准则相悖。

## 测试抽象

抽象在测试和面向对象设计中同等重要。因为你要将测试当作文档来读，所以它们必须正中其意，以最清晰、最简洁的方式声明其意图。可以通过以下几个途径增加测试的抽象程度：

1. 内聚：一个测试一个断言，多个前置的参数断言可以合并成一个调用函数来完成。
2. 更好的命名：测试自身及内部的代码；
3. 抽象掉多余的东西：可利用 fixture 辅助函数或者 SetUp()。

### [不必要的测试代码](./7.md#不必要的测试代码)

1. 断言不为空：在做 TDD 时，想象一下在增量步伐中为指针为空检查编写一个断言。这样做没什么问题。但是，一旦你认为测试完成了，要回头看一下代码，消除一些不具有文档化意义的代码。通常而言，这类不必要的代码不仅仅只有指针为空检查的断言。
2. 异常处理：**大部分测试是为了常规代码路径，它们是不会产生异常的。这些不必要的异常处理代码只会影响测试的可读性，并增加维护成本。**而测试已知异常流程时，请使用带有校验异常的断言，而不应该使用 try-catch 捕获。
3. 断言失败的消息：带有消息的断言不能带来任何有价值的东西。就像正常的代码注释一样，应该尽量避免此类需求。如果一个断言不带失败消息就没意义的话，那么还是先解决测试中的其他问题吧！
4. 注释：在保留测试表达力的前提下，努力找到一个去除注释的方法。对前一个测试而言，去除所有注释并不影响理解。
5. 隐含之意：通常来说，你会将实现细节从测试移至 SetUp() 或另一个辅助函数。**但注意，不要隐藏太多！** 否则测试阅读者需要看得更深入才能知道答案。**合适的函数和变量名能使测试意图达到顾名思义的效果。**
6. 误导性的组织：测试代码组织的方式应当前后一致，且清晰可巡。测试代码结构要突出重点。
7. 晦涩的命名：**在软件设计中，良好的命名是你可以做的最重要的事情之一。通常而言，好的名称是测试关联问题的解决方案。反之，如果不能赋予设计一个简洁的名称，很可能意味着该设计存在问题。**
