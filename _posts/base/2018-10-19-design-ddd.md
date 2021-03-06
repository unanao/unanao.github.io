---
layout:     post
title:      "领域驱动设计"  
subtitle:   ""
author:     Sun Jianjiao
header-img: "img/bg/default-bg.jpg"
catalog: true
tags:
    - 架构设计
---

领域驱动设计提供了一个框架，人们普遍接受的一些最佳实践。很多应用的主要复杂性并不在技术上，而是在领域本身、用户的互动或业务。 领域驱动设计是一种思维方式，也是一组优先任务，它旨在加速那些必须处理复杂领域的软件项目的开发。

- 何时应该为了节省时间而放弃某个方面
- 何时应该坚持不懈直到找到一个干净利落的解决方案

领域驱动的是指是**消化吸收大量的知识，最后产生一个反映深层次领域知识并聚焦关键概念的模型**。

微服务设计往往会面临**边界如何划定**的问题。领域驱动设计主要关注：从业务领域视角划分领域边界，**构建通用语言进行高效沟通**。通过业务抽象，建立领域模型，维持**业务和代码的一致性**。

DDD不是架构，而是一种架构的设计方法论，它试图分离技术实现的复杂性，并且**围绕业务概念构建领域模型**来控制业务的复杂性，**解决软件难以理解，难以演进的问题**。 DDD不仅可以用于微服务设计，还可以很好的应用于企业中台的设计。

# 1 基本概念

1. 领域：用来确定业务范围和边界的，也就是边界内要解决的问题。
2. 子域：领域进一步划分为子领域，简称子域。对应更小的问题域，或更小的业务范围。

领域建模和微服务建设的过程和方法基本类似，其核心思想就是**将问题域逐步分解，降低业务理解和系统实现的复杂度**。

子域根据自身重要性和功能属性划分为3个子域：核心域，通用域和支撑域。

- 核心域：决定产品和公司核心竞争力的子域
- 通用域：被多个子域通用功能的子域。比如权限，认证等，没有企业特点。
- 支撑域：不是核心域和通用域，具有企业特性，但不具有通用性。

建立领域模型的时候，我们需要结合公司的战略重点和商业模式，找到核心域，并且重点关注核心域。

通过领域划分，区分不同子域在公司内的不同功能属性和重要性，从而该公司可对不同子域采取不同的资源投入和建设策略。

1. 通用语言：在事件风暴中，通过团队交流达成共识的，能够**简单、清晰、准确描述业务含义和规则**的语言就是通用语言。也就是**团队统一的语言**，解决交流障碍的问题。
2. 限界上下文：限界就是领域的边界，上下文就是语义环境。这个边界**定义了模型使用的范围**。 领域边界就是由边界上下文来定义的。

限界上下文是微服务拆分的主要依据。在领域模型中，如果不考虑技术异构，团队划分等外部因素，一般情况下，一个限界上下文就可以设计为一个微服务。

## 1.1 战略设计向战术设计过度

- 实体：领域模型中的实体是多个属性、操作或行为的载体。对应代码模型就是**实体类**，这个类包含了实体的属性和方法。
- 值对象：值对象就是一个集合。包含了用于描述目的，具有整体概念和不可修改的属性。值对象用于保证属性归类的清晰和概念的完整性，避免属性零碎。

实体是看得到、摸得着的实实在在的业务对象，实体具有业务属性，业务行为和业务逻辑。值对象只是若干个属性的集合，只有数据初始化操作和有限的不涉及修改数据的行为，基本不包含业务逻辑。

![实体和值对象](/img/post/base/ddd/entity-value-object.jpg)

## 1.2 聚合和聚合根

在事件风暴中，我们会根据一些业务操作和行为找出实体(Entity)或值对象(ValueObject), 进而**将业务关联紧密的实体和只对象进行组合**，构成聚合。再根据业务语义将多个聚合划定到同一个限界上下文中，并在限界上下文内完成领域建模。

### 1.2.1 聚合

实体和对象都只是个体化的对象，他们的行为表现出来的是个体的能力。

聚合就是有业务和逻辑紧密关联的实体和值对象组合而成的。能让实体和值对象协同工作的组织就是聚合，它用来确保这些领域对象在实现共同的业务逻辑时，能保证数据的一致性。

### 1.2.2 聚合根

把聚合比作组织，那么聚合根就是这个组织的负责人，是聚合的管理者。

- 它是实体，拥有实体的属性和业务行为，实现自身的业务逻辑。
- 聚合的管理者，在聚合内部负责协调实体和值对象按照固定的业务规则协同完成共同的业务逻辑。
- 聚合之间，它还是聚合对外的接口人，以聚合根ID关联的方式接受外部任务和请求。

### 1.2.3 怎样设计聚合

DDD领域建模通常采用事件风暴，包括用例分析，场景分析和用户旅程分析等方法。

- 通过头脑风暴列出所有可能得业务行为和事件
- 然后找出产生这些行为的领域对象
- 梳理领域对象之间的关系，找出聚合根
- 找出与聚合根业务紧密关联的实体和对象，将聚合根、实体和值对象租车，构成聚合。

![实体和值对象](/img/post/base/ddd/aggregation.png)

### 1.2.4 聚合设计的一些原则

1. **在一致性边界内建模真正的不变条件**。聚合用来封装不变性。边界之外的任何点东西都与该聚合无关，这就是聚合能实现高内聚的原因。
2. **设计小聚合**。如果聚合设计的过大，聚合会因为包含过多的实体，导致实体之间的管理过于复杂。
3. **通过唯一标识引用其他聚合**。通过ID的方式引用，而不是直接对象引用的方式。这样就降低了聚合之间的耦合。
4. **边界之外使用最终一致性**。聚合内数据强一致性，聚合之间数据采用最终一致性。
5. 通过应用层实现跨剧和的服务调用。避免跨聚合的服务调用和跨聚合的数据库表关联。

聚合的特点：高内聚，低耦合，他是领域模型中最底层的边界，可以最为微服务的最小单位，但是不建议对微服务过度拆分。一个服务可以包含多个聚合。

聚合根的特点: 聚合根是实体，有实体的特点，具有全局唯一标识，有独立的声明周期。一个聚合只有一个聚合根。对内组合和协调实体和值对象。对外进行聚合之间的协同

实体的特点：有ID标识，通过ID判断是否相等，ID在聚合内唯一。声明周期由聚合根管理。

值对象的特点：没有ID，不可以变，没有生命周期，用于描述实体的状态和特征。

## 1.3 领域事件

如果在**发生某事件后，会触发进一步的操作**，这很可能就是领域事件。 例如：

- 如果发生......, 则......
- 当做完......的时候，请通知......
- 发生......时，则......

边界之外使用最终一致性，为了保证领域模型的解耦。

- 微服务之内：服务调用的方式完成聚合的访问，通常应用于实时性和数据一致性要求高的场景。
- 微服务之间: 也可以采用服务直接调用的方式，也可以通过订阅发布的方式。

