# 总结

- [总结](#总结)
  - [遗留代码](#遗留代码)
  - [处理遗留代码的法则](#处理遗留代码的法则)
  - [保持测试驱动开发的心态](#保持测试驱动开发的心态)
  - [被遗留代码转移注意力](#被遗留代码转移注意力)
  - [用 Mikado 方法大规模改动代码](#用-mikado-方法大规模改动代码)

## 遗留代码

针对遗留代码的态度，是选择让维护成本不断增加（再函数内增加各种各样的标志和内嵌的代码块）或者着手处理问题？或许大多数时候还是值得一拼的，不允许任何人让系统变得更糟。不值得这么做的唯一情况是，你面对的是一个封闭的系统或即将被淘汰的系统。

## 处理遗留代码的法则

1. **任何时候，只要可以就进行测试驱动**。使用测试驱动方法时，有可能的话就将需要改动的代码作为新的成员或新的类。
2. **不要让测试覆盖率缩水**。非常容易出现的情况是，改动一点代码，然后认为这只是一些简单的代码行，并对之不予过多考虑。如果没有测试，每一行新加的代码都会导致测试覆盖率降低。
3. **为了编写测试，必须改动现有代码！**由于对协作对象的依赖，大多数情况下不能方便地为遗留代码编写测试。在编写测试前，需要一个打破依赖的方法。
4. **可以在限制范围内实施微小的代码改动，这样会降低风险**。用一些技巧就可以手动地做出一些小而安全的代码变换。
5. **仔细斟酌再遗留代码上所作的修改**。你编写的每行代码都有风险，甚至一个敲错的字符都会引入潜在的缺陷，而这可能会浪费好几个小时。尽可能少写代码，每敲击一次键盘时都要想清楚。
6. **坚持小的、增量的改动**。这在TDD中是奏效的。同样，对遗留代码也是如此。步子迈得太大会使你陷入困境。
7. **一次只做一件事**。在处理遗留代码时，不要合并步骤或目标。例如，不要在重构的同时写测试。
8. **有些增量的改动可能会使代码变得丑陋，要接受这一点**。记住，一次只做一件事。可能需要几个小时做“正确”的事。不要等，现在就提交工作方案，因为这样或许就不用花费过多时间。最终，你可能要抽出时间回过头来整理代码，或许你不会，但依然是进步了。你已经向正确的方向迈进了，而且也证明了所有代码依然可以工作。
9. **增量地修改代码**。面对庞大的代码库时，仅仅为遇到的相关代码编写测试或许不能带来巨大改观。更重要的是你内心深知任何新加入的东西都需要经过测试。

## 保持测试驱动开发的心态

在应对遗留代码时依然是测试先行。即使已经编写完测试所涵盖的代码，仍需要写测试来描述已有行为的特征。同时，也要以 TDD 的方式添加新代码。

只需要测试即将被改动的代码，参考底线会告诉你什么时候破坏了已有的行为。虽然你要判定出哪些依赖代码可能会因此被破坏，但不需要测试超出改动范围的代码。

同时，要避免那些必须与文件系统直接打交道的测试，以便保持测试集快速运行并减少管理文件的烦恼。尽量使用存储于内存的流而非文件流，这样可以部分达成效果。

## 被遗留代码转移注意力

第三方库，会为测试制造许多困难。它们要么速度慢，要么需要大量的配置工作，还可能有副作用。如果你是第一次面对遗留代码，很有可能会花费大量时间以处理来自第三方库导致的类似问题。

规避第三方库的一个可行方法是链接替换。其中最终要的步骤便是使用库的头文件来编写一份 MOCK 代码：

- 转换头文件需要考虑的事情：
  1. 首先需要做的是，包含所创建的实现文件的地方头文件。
  2. 可能需要将实现文件放在一个命名空间中。
  3. 移除 virtual 和 static 关键字。
  4. 移除 public: 和其他一些访问修饰符。
  5. 移除成员变量和枚举值。
  6. 在移除预处理定义和 typedefs 时要小心。
  7. 如果函数有返回值，就返回一个最简单的，如 0、false、""、空指针或一个无参构造函数构造的实例。
  8. 如果必须要返回一个 const 引用，创建一个全局变量，并返回这个变量。
  9. 不要忘记将函数限定在合适的类。
  10. 不要太注重外观，重要的是能够编译。

## 用 Mikado 方法大规模改动代码

1. 制定 Mikado 目标以及想要达到的最终状态。
2. 以最稚拙的方法实现这一目标。
3. 找出所有的错误。
4. 找到解决错误的即时方案。
5. 将即时方案作为新的前提目标。
6. 如果有错误，将相应的代码回滚到最初状态。
7. 对每个方案重复步骤2至步骤6。
8. 检查代码是否存在错误；将前提目标标记为完成。
9. 完成了Mikado目标吗？如果是，任务完成！
