---
title: 软件工程部分要点
date: 2018-03-11
tags:
    - 中文
    - software engineering
---

## 1. 软件工程的定义

> Software engineering is “(1) the application of a systematic, disciplined, quantifiable approach to the development, operation, and maintenance of software, that is, the application of engineering to software,” and “(2) the study of approaches as in (1).” – IEEE Standard 610.12

根据IEEE标准，软件工程的含义包括两个层面：

- 系统的、秩序的、可度量地开发、操作、维护软件的方法的应用，就是软件的工程化应用。
- 对于上述应用的研究。

## 2. 何为Software Crisis

Software Crisis(软件危机)，是发生于上世纪六十年代的软件生产困境。随着计算机应用需求的驱动，软件生产的复杂性和成本急剧上升，而当时软件开发普遍应用“个人英雄主义”，使得大型软件的生产出现了很大的困难，出现了许多重大的问题，包括但不限于：

- 软件开发成本日益增长，超出预算
- 软件开发进度难以控制，实际进度与计划相比一再拖延
- 用户需求不明确，用户对使用的软件产品不满意的现象经常发生
- 软件产品质量不可靠
- 软件产品的可维护程度低
- 软件开发生产率跟不上日益增长的硬件性能和用户需求

软件工程就是为了解决软件危机所展示出来的问题，于上世纪六十年代末至七十年代初应运而生的工程学科。

## 3. 何为COCOMO模型

Constructive Cost Model(构造性成本模型)，是由Barry Boehm于上世纪八十年大提出的一种软件成本估算方法，其目的是在软件生产之前评估软件成本，以使软件生成流程得到更加精确的管理，规避不必要的风险。

早期的构造性成本模型在准确性方面有一定的局限性，后来出现的“中级COCOMO”和“详细COCOMO”加入了更多考量因素，直至今天也是软件工程中非常热门、实用的管理控制工具。

## 4. 软件生命周期

Software Development Life Cycle(SDLC, 软件开发生命周期)是起源于上世纪六十年代的一种软件开发过程模型，也是软件工程中最重要的模型之一。SDLC定义了软件生产的各个阶段过程，真正使得软件生产得以系统化，也孕育了许多软件生命周期模型，是现代软件工程开发过程的指导模型。

软件生命周期一般看作有如下6个阶段(Phases)：

1. 可行性分析与初期计划

    这一阶段要分析此软件生产项目的可行可行性，完成对成本、风险和回报粗略估计，并结合自身的团队力量状况来确定是否接手此软件项目。这一阶段的任务往往由顶层人员负责，要求其具有相当丰富的软件工程经验和管理分析能力。
·
2. 需求分析阶段

    分析用户的需求，确定软件系统的各项功能、性能需求和设计约束，确定对文档编制的要求，产生软件规格说明书、初步用户手册等一系列技术文档。

3. 设计阶段

    完成软件概要设计和详细设计，即根据软件需求形成软件模块、并确定每个模块的具体输入输出、数据结构和算法实现，产生结构设计说明、详细设计说明等文档。

4. 实现阶段

    根据模块详细设计对每一个模块进行实现，并编写用户手册、操作手册、测试计划等。

5. 测试阶段

    全面测试软件系统，包括单元测试、集成测试、系统测试、文档测试等等，产生进度总结报告。

6. 运行与维护阶段

    在此阶段软件已经提交给用户，在用户运行的过程中需要对其进行持续的维护，根据用户可能提出的新需求进行必要的扩充、删改和更新。

得益于软件生命周期的提出，出现了许多面向实际生产的软件生命周期模型，包括：

- Waterfall Model(瀑布模型)
- V Model and W Model(V模型与W模型)
- RAD Model(快速应用开发模型)
- Prototype Model(原型模型)
- Incremental Model(增量模型，又叫演化模型、迭代模型)
- Spiral Model(螺旋模型)
- Fountain Model(喷泉模型)
- CBSD Model(基于构件的开发模型)
- RUP Model(Rational统一过程模型)
- Agile Modeling and XP(敏捷开发模型与极限编程)
- ...

## 5. 按照SWEBok的KA划分，“系统分析与设计”课程应该关注哪些KA或知识领域？

Software Engineering Body of Knowledge(SWEBOK)是一个国际标准，用于定义软件工程主体相关的重要领域。SWEBOK V3为软件工程划分了15个Knowledge areas(KAs)，包括：

- Software requirements
- Software design
- Software construction
- Software testing
- Software maintenance
- Software configuration management
- Software engineering management
- Software engineering process
- Software engineering models and methods
- Software quality
- Software engineering professional practice
- Software engineering economics
- Computing foundations
- Mathematical foundations
- Engineering foundations

(2004版的SWEBOK划分了10个KAs，就不在此一一列举)

系统分析与设计主要面向SDLC的设计阶段，即根据需求合理地设计软件模块与模块实现方案，那么与之相关的KA应当有：

- Software requirements(根据需求完成分析和设计)
- Software design(系统分析和设计的主体部分)
- Computing foundations(详细设计需要杰出的计算知识，包括数据结构、算法等)
- Mathematical foundations(模块设计常用到图论，详细设计用到的算法知识等)

## 6. CMMI的五个级别

Capability Maturity Model Integration(CMMI, 能力成熟度模型集成)，是一个过程改进方案，它给出对于软件成熟度的评估标准，用于开发团队在软件开发的过程中评估和改进软件产品。

- Level 1 - Initial : 无序，自发生产模式。(第一级被看作是没有通过成熟度评估的“初始级”)
- Level 2 - Managed : 已管理。
- Level 3 - Defined : 已定义。
- Level 4 - Quantitatively Managed : 已被量化地管理。
- Level 5 - Optimizing : 优化中。

## 7. 用自己的语言阐述，何为SWEBok

关注一个计算机科学中的技术、工具、标准、理论，首先关注其用于解决什么问题，软件工程是用于解决上世纪六十年代发生的软件危机问题，软件生命周期是为了解决软件开发过程的系统化问题，等等。Software Engineering Body of Knowledge(SWEBok)作为国际标准ISO/IEC TR 19759:2005，在我看来，是为了明确和分清软件开发过程的各个技术领域。

划分技术领域的工作与软件生命周期类似，使得软件开发能够泾渭分明，分工明确，提高效率；不同于软件生命周期以时间为线索，SWEBok在这方面的工作是从静态的技术角度出发，系统化地管理了技术领域，使得软件开发在时间和空间两方面都得到了层次分明的蓝图。

## 8. PSP各项指标及技能需求

Personal Software Process(PSP)，是针对个人开发人员的开发能力评估标准，由提出CMMI的团队所创造。PSP经过若干个版本的更迭，现在最新的PSP2.1对于个人开发者有如下指标：

- Planning(计划)
    - Estimate(估计任务所需要的时间)
- Development(开发)
    - Analysis(分析需求)
    - Design Specification(产生设计文档)
    - Design Review(设计复查)
    - Coding Standard(代码规范)
    - Design(设计)
    - Coding(编码)
    - Code Review(代码复查)
    - Test(测试)
- Record Time Spent(记录花费时间)
- Test Report(测试报告)
- Size Measurement(工作量计算)
- Postmortem(事后总结)
- Process Improvement Plan(提出过程改进计划)

对于一个软件工程师，在接到任务后须按照上述PSP2.1的流程走一遍，与以往自发开发不同的是，PSP2.1中加入了较多的时间估计、复查和总结。这不仅要求软件工程师有过硬的编码能力，其阅读和撰写文档的技能、自我评估的技能以及测试技能等都十分重要。

对于每一项指标，采用计时的方式产生统计数据，记录并保存，项目完成后依照自己的统计数据进行处理，产生完整的、多元的PSP指标分析结果。
