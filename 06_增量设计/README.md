# 总结

- [总结](#总结)
  - [简单设计](#简单设计)
  - [何时探讨设计](#何时探讨设计)
  - [阻碍重构的因素](#阻碍重构的因素)

## 简单设计

- 在使用 TDD 时需要考虑三条规则：
  1. 确保代码具备很强的可读性和表达力。
  2. 在和第一条规则不冲突的情况下，消除所有的重复。
  3. 不要向系统引入不必要的复杂性。避免猜测行的结构关系和不能增加系统表达力的抽象。

- 重复代码或许使维护代码库最大的开销，因此时刻谨记将增量重构作为 TDD 环节的一部分，可以避免系统级的退化，文中以设计一个股票交易系统为例。
  1. 不仅仅是生产代码，测试代码中避免重复同样重要。
  2. 获取在代码层面没有明显重复，但是在逻辑上是有多余的点，例如例子中判断是否持仓为空与查询股票数量的关系，通过去除 m_isEmpty 变量，让 IsEmpty() 查询股票的数量，我们可以消除这一概念重复。
       - 算法的重复（解决同一问题的不同方法或问题的不同部分）会随系统增长演变为重大问题。通常来说，随着对一个实现的改动未能编写进其他实现，重复代码会演化为不经意的变体。
  3. 抽离出公用的代码，例如[<更多的重复>](./6.md#更多的重复)
  4. 提取短[小的方法](./6.md#小方法的好处)有许多好处，职责单一，功能逻辑清晰，表达力强，同时也是避免重复的强力工具，只有将这些子功能提取出才有复用的可能。

## 何时探讨设计

- 预先设计是一个很好的起始路线图。围绕此设计的讨论有助于发现软件中必须要做的东西，以及最初该怎样设计软件。但是，打造系统所需要的大量细节会发生变化。TDD 允许你基于当前的业务需求，保持一个可能的最简设计。如果一直保持设计尽量简洁，那么就可以最大可能地引入新的、从未被考虑过的功能。相反，如果任由系统退化（并有大量的重复代码和晦涩难懂的代码），未来有任何新的需求时，你将痛苦万分。

- [简单设计原则与经典设计原则的冲突](./6.md#简单设计原则和经典设计理念冲突)：
  1. 访问性：如何抉择应不应该为测试方便，放松对于成员私有化的标准。
  2. 及时性：老派的设计希望你能尝试获得尽可能完美的设计。在简单设计中，更应该考虑如何通过简单、增量的设计应对持续的变化。

## [阻碍重构的因素](./6.md#阻碍重构的因素)
