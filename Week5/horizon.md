# Rust 视界周刊 Week 5 ｜ 驳 “Rust 等内存安全语言的安全性并不优于C++”
---

## 社区热点

### FOSDEM 2023 Rust 分享列表

[FOSDEM 2023](https://fosdem.org/2023/) 是软件开发人员见面、分享想法和协作的免费活动。每年，来自世界各地的数千名自由和开源软件开发人员齐聚布鲁塞尔。你不需要注册。只需出现并加入！

和 Rust 相关的分享视频列表如下：
- [video](https://fosdem.org/2023/schedule/event/building_an_actor_library_for_quickwits_indexing_pipeline/) 为 [Quickwit](https://github.com/quickwit-oss/quickwit) 的索引管道构建Actor 库，介绍分布式搜索引擎 Quickwit 为什么选择开发自己的 actor 框架，并讨论其实现的独特功能。
- [video](https://fosdem.org/2023/schedule/event/rust_building_a_distributed_search_engine_with_tantivy/) 使用 tantivy 构建分布式搜索引擎，介绍了 [lnx](https://github.com/lnx-search/lnx) 如何解决在 Rust 中构建分布式搜索引擎的挑战。
- [video](https://fosdem.org/2023/schedule/event/rust_aurae_a_new_pid_1_for_distributed_systems/) Aurae: 分布式运行时，介绍 Aurae 的动机、目标和架构。
- [video](https://fosdem.org/2023/schedule/event/rust_bastionlab/) BastionLab, 介绍 Rust 开源隐私框架，用于保密数据科学合作。
- [video](https://fosdem.org/2023/schedule/event/rust_neovim_and_rust_analyzer_are_best_friends/) Neovim 和 rust-analyzer 是最好的朋友，深入探讨了 Rust 的语言服务器协议实现以及如何在 rust-analyzer 和 Neovim 之间建立友好关系。
- [video](https://fosdem.org/2023/schedule/event/rust_a_rusty_cheri_the_path_to_hardware_capabilities_in_rust/) Rusty CHERI - 通往 Rust 中硬件能力的道路，是关于在 Rust 中支持 CHERI 架构的持续努力的状态报告。CHERI 定义了硬件扩展以对指针的访问约束进行编码，这样做提供了另一层保护。我们可以对编译器（和 OS/等）以硬件可以在运行时强制执行的方式了解的来源有效性、边界和其他访问限制的知识进行编码。这项工作可能会带来其他一些不错的好处。例如，运行时边界检查现在可以通过硬件而不是软件来完成。
- [video](https://fosdem.org/2023/schedule/event/rust_slint_are_we_gui_yet/) Slint: 我们有 GUI 了吗？“是的，使用 Slint”。该分享介绍 Slint 并展示如何在 Rust 中构建反应式 GUI。
- [video](https://fosdem.org/2023/schedule/event/rust_rust_api_design_learnings/) Rust API 设计心得。回顾从使用 Rust 构建内部 API 以及一些公共 API中学到的一些经验教训。它涵盖了从犯错误中吸取的教训，使用泛型进行更巧妙的抽象。
- [video](https://fosdem.org/2023/schedule/event/rust_a_deep_dive_inside_the_rust_frontend_for_gcc/) 深入探究 GCC 的 Rust 前端。探讨 gccrs 中的一些组件，并深入探讨项目生命周期中遇到的一些障碍，以及如何整合 Polonius 项目以在 gccrs 中执行借用检查，需要实现什么才能开始对 Rust-for-Linux 项目有用。
- [video](https://fosdem.org/2023/schedule/event/rust_merging_process_of_the_rust_compiler/) Rust 编译器的merge过程，介绍如何为 Rust 编译器合并补丁，整个过程如何发生以及合并后会发生什么，根据要合并的贡献类型和每个涉及的内容来讨论所涉及的工具和过程。
- [video](https://fosdem.org/2023/schedule/event/rust_lets_write_snake_game/) 让我们写贪吃蛇游戏。介绍了 Bevy Engine 库，它使我们能够以智能的方式创建简单的游戏。
- [video](https://fosdem.org/2023/schedule/event/rust_glidesort/) Glidesort： 新的稳定排序。排序是编程中最常用的算法之一，几乎每个标准库都包含它的例程。尽管这也是最古老的问题之一，但仍然发现了令人惊讶的巨大改进。其中一些是基本的新颖性，而另一些则是与现代硬件不断变化的性能格局相匹配的优化。本次演讲中介绍了 Glidesort，一种通用的内存中稳定比较排序。在对随机 32 位整数进行排序时，Glidesort 比 Rust 的标准库 Timsort 例程实现了 3 倍的加速，加速打破了现实低基数分布的数量级障碍。它在不使用 SIMD、特定于处理器的内在函数或关于被排序类型的假设的情况下实现了这一点：它是一种采用任意比较运算符的完全通用的排序。
- [video](https://fosdem.org/2023/schedule/event/rust_how_pydantic_v2_leverages_rusts_superpowers/) Pydantic V2 如何利用 Rust 的超级能力。Pydantic 现在被大约 10% 的专业 Web 开发人员使用！本次演讲中简要介绍了 Pydantic V2，然后再深入探讨 Rust 的使用如何彻底改变 Pydantic 的架构，使其更易于扩展和维护，同时显着提高性能。
- [video](https://fosdem.org/2023/schedule/event/rust_scalable_graph_algorithms_in_rust_and_python/) Rust 中的可扩展图算法（以及 Python）。图用于许多不同的应用程序，因为它们是表示实体之间复杂关系的直观方式，例如在社交、通信、金融或地理网络中。这些领域中的图可能非常大，可能跨越数百万甚至数十亿个节点和边。为了从这些结构中获得分析洞察力，图形算法的可扩展实现是必要的。Rust 是实现此类算法的理想语言，这归功于其众所周知的方面，例如“无所畏惧的并发”和内存安全性，以及出色的开箱即用性能和富有表现力的类型系统。本次分享介绍 [graph](https://github.com/s1ck/graph) crate，包括一个内存中图形表示、用于从各种数据源构建内存中图形的 API，以及一小部分高性能图形算法。
- [video](https://fosdem.org/2023/schedule/event/rust_using_rust_for_your_network_management_tools/) 让网络管理工具使用 Rust ! 本次演讲中介绍如何结合几个现有的库并创建自己的库来创建一个强大的网络工具 [nmstate](https://github.com/nmstate/nmstate)（以声明的方式管理主机网络设置），以及分享将项目从Python 重写为 Rust 的经验教训。
- [video](https://fosdem.org/2023/schedule/event/rust_backward_and_forward_compatibility_for_security_features/) 安全特性的向后和向前兼容性，本次演讲提供了有关需要处理向后和向前兼容性的安全库开发的反馈，因为安全功能与特定内核版本相关，以安全可靠的方式处理不同的用例。
- [video](https://fosdem.org/2023/schedule/event/rust_atuin_magical_shell_history_with_rust/) [atuin](https://github.com/ellie/atuin): 将每个命令及其周围的上下文（例如，它运行的目录、持续时间等）存储在 SQLite 数据库中，然后在此基础上提供模糊搜索。与 Atuin 同步服务器一起，可以在用户拥有的每台机器上提供此历史记录。
- [video](https://fosdem.org/2023/schedule/event/rustunikernel/) Rust 基础的模块化 Unikernel，用于 MicroVMs。本次演讲介绍了从基于 C 的 HermitCore 到基于 Rust 的 RustyHermit unikernel 的过渡。
- [video](https://www.youtube.com/watch?v=90Q5N1qT7BQ) 用现代语言 Rust 重新实现 Coreutils。Coreutils 是任何 Unix 的关键组件。现在一个社区产生了，以[用 Rust 重新实现 coreutils](https://github.com/uutils/coreutils/) 为目标，可以用它启动 Debian，构建 Firefox 或 LLVM 等。本次演讲介绍了他们为什么要造轮子，以及提及 Rust 在 Linux 内核中的用法。

相关视频在 Youtube 也有发布。


### Rust 社区对 Stroustrup 博士关于 "Rust 等内存安全语言的安全性并不优于C++" 言论的回应

美国国家安全局（NSA）最近发布了一份关于内存安全重要性的网络安全信息表，他们在其中建议从内存不安全的编程语言（如 C 和 C++）转移到内存安全的编程语言（如 Rust）。 C++ 之父 Bjarne Stroustrup 博士的回应“Rust 等内存安全语言的安全性并不优于C++”。

[Rust 社区针对Cpp之父这一言论作出了回应](https://www.reddit.com/r/rust/comments/10rnymj/my_reaction_to_dr_stroustrups_recent_memory/)，一些观点摘录如下：

- Cpp 之父请大家“认真考虑安全”，但他自己实际并没有认真考虑安全。Cpp 之父的一些观点非常陈旧，他没有意识到它们大多甚至与 NSA 的网络安全信息表无关，更不用说对其进行深思熟虑的反驳了。
- Stroustrup 博士反复强调的最有趣且概念上最相关的观点之一是，内存安全并不是唯一的安全类型。实际上，内存不安全是迄今为止内存不安全编程语言中安全漏洞和不稳定的最大来源，在某些情况下估计高达70%。这恰恰是 C++ 最严重缺陷的领域。换句话说，Stroustrup 博士认为内存不安全并不重要。而 Rust 语言则专注于解决内存不安全的问题。C++ 不强制将内存安全作为编程语言的一项功能。这在未来可能会改变（正如 Stroustrup 博士所讨论的那样），但这是目前的情况。Stroustrup 博士试图淡化这一点，但并不令人信服。
- Stroustrup 博士另一个观点是，提醒大家不要忽略了cpp 30 多年的进步。虽然 C 和 C++ 之间可能存在 30 多年的分歧，但 C++ 所谓的“进步”都没有涉及从 C++ 中删除内存不安全的 C 功能，其中许多功能仍在普遍使用，其中许多功能仍然使内存安全在 C++ 中几乎难以处理。这些旧的内存不安全功能仍在普遍使用。
- Stroustrup 博士认为通过静态分析工具也可以保障 Cpp 的内存安全。但他忽略了人性的善变。如果静态分析工具没有内置到编程语言中，它就能被跳过。认真对待内存安全的编程语言不会将其作为大多数人会忽略的可选附加组件提供，必须使用附加工具来解决语言的缺陷远非理想。

更多讨论请参考原贴。

### 将 Rust semver 检查速度提高 2000 倍以上

[cargo-semver-checks](https://crates.io/crates/cargo-semver-checks) 最新版本性能提升了 2000 多倍，具体来说，是从 5 小时缩短到 8 秒，快了 2272 倍。

虽然 semver-checking 对于大多数 crate来说都在一分钟左右完成，但 `aws-sdk-ec2` 该过程则花费了5 个多小时。cargo-semver-checks 它建立在 [Trustfall](https://github.com/obi1kenobi/trustfall) 之上，Trustfall 是一个查询引擎，支持通过特殊 API 插入来查询任何类型的数据源。最新版本中，[Trustfall 进行了优化](https://predr.ag/blog/speeding-up-rust-semver-checking-by-over-2000x/)，让性能得以提升。


## 应用实践

### 改进 Rust 编译时间以启用内存安全

[Prossimo 最近发布的文章《改进 Rust 编译时间以启用内存安全》](https://www.memorysafety.org/blog/remy-rakic-compile-times/)，介绍了由 Prossimo 赞助 Rémy Rakic 作为 Rust 编译器性能工作组的一员一直致力于改善构建时间的倡议。

Rust 编译时间是需要改进的领域，避免编译时间可能成为采用 Rust 的障碍，因此改进它们可以帮助扩大语言的影响。这是 Prossimo 的目标之一：提高更多内存安全软件的潜力，特别是在互联网基础设施的最关键部分（例如，网络、TLS、DNS、操作系统内核等）。

> [Prossimo](https://www.memorysafety.org/about/) 是互联网安全研究组 (ISRG) 的一个项目，目标是将 Internet 的安全敏感软件基础架构转移到内存安全代码。

Rémy Rakic 与 Prossimo 合作处理以下优先事项：

- 使流水线编译尽可能高效
- 提高原始编译速度
- 改进对持久、缓存和分布式构建的支持

关于改进编译时间的更多工作内容可以参考原文。


## 生态看点

### cargo-dist: 帮助开发者发布 Rust 二进制文件 的生产力工具

[cargo-dist](https://blog.axo.dev/2023/02/cargo-dist) 可以帮助开发者解决大多数在发布二进制文件中遇到的问题。

### Glidesort: Rust 实现的新的稳定排序

[glidesort](https://github.com/orlp/glidesort)是 Rust 实现的一种新的稳定排序，随机数据的速度提高了约 4 倍。

> Glidesort 是一种新颖的稳定排序算法，是一种基于比较的排序，支持任意比较运算符，虽然在具有模式的数据上表现出色，但对于随机数据也非常快。

详细算法介绍见 glidesort 库的 README。

P.S [glidesort和ipn_stable的性能分析](https://github.com/Voultapher/sort-research-rs/blob/main/writeup/glidesort_perf_analysis/text.md)参考。

### scaphandre: 测量计算能耗

[scaphandre](https://github.com/hubblo-org/scaphandre) 用于测量 IT 服务器和计算机的能耗：Windows 兼容性（实验性）、多传感器支持，以及更多。目前发布 0.5 版本。

scaphanre 计算每个进程的功耗的原理概述：

- 计算每个进程使用了多少 [jiffies](https://www.anshulpatel.in/posts/linux_cpu_percentage/) ，从而了解进程正在使用机器的多少资源。
- 通过某种传感器（比如 RAPL）来跟踪机器本身的用电量，即，对于每个 jiffy，及时检查在那些时刻消耗了多少功率。
- 综合计算上述得到的功耗数据，使用所用电力的排放因子，将功耗数据转化为相关碳排放量的估算值。

> jiffies，Linux 中的处理时间以 Jiffies 为单位进行测量，单位是“HZ”，jiffies的值可以通过系统调用获取。
> RAPL 传感器代表运行平均功率限制。这是嵌入在 2012 年之后生产的大多数 Intel 和 AMD x86 CPU 中的技术。

### icu4x: 针对客户端和资源受限环境的 i18n 解决方案

[icu4x](https://github.com/unicode-org/icu4x) 是[Unicode (ICU) 的国际化组件](http://site.icu-project.org/)的实现，旨在实现模块化、高性能和灵活。该库为所有软件提供了一层 API，以启用国际化功能。

该库由 Unicode 联盟（字符编码和国际化的标准机构）中 ICU-TC 的一个小组委员会开发。

### darkbird: 面向文档的实时内存数据库解决方案

[darkbird](https://github.com/Rustixir/darkbird) 是 Rust 实现的高并发、实时、内存数据库，其灵感来自于 Erlang 生态中的mnesia 数据库。目前发布 6.0 版本。

### async-rdma v0.5.0 发布

[async-rdma](https://github.com/datenlord/async-rdma) 是一个用于编写具有高级抽象和异步 API 的 RDMA 应用程序的框架。新的版本中添加了一些 API，使用户能够控制设备、连接和 async-rdma 后台框架的更多属性。

###  Rust GUI 基础框架 Masonry 0.1 发布

[masonry-rs](https://github.com/PoignardAzur/masonry-rs) 是一个旨在为 Rust GUI 库提供基础的框架。masonry 提供了一个平台来创建窗口（使用[Glazier](https://github.com/linebender/glazier)作为后端），每个窗口都有一个小部件(widgets)树。开发者可以在 Masonry 之上实现即时模式 GUI、Elm 架构、功能性反应式 GUI 等。该项目最初是 Druid 的一个分支，现在独立开发了。

值得一说的是，Masonry 作者认为 Rust 终有一天会彻底改变原生 GUI 的生态，Masonry 就是朝着这个目标迈出的一步。

更多内容可以参考[masonry-rs官方介绍](https://poignardazur.github.io/2023/02/02/masonry-01-and-my-vision-for-rust-ui/)。

### glow: GL Rust 绑定

[glow](https://github.com/grovesNL/glow) 提供了一组 Rust 绑定，让开发者可在任何地方运行 GL（Open GL、OpenGL ES 和 WebGL）。