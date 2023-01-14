# Rust 视界周刊 Week 2
---
2023/01/14

## 社区热点

### Rust 1.66.1 发布

Rust 1.66.1 修复了 Cargo 在使用 SSH 克隆依赖项或注册表索引时不验证 SSH 主机密钥的问题。此安全漏洞被跟踪为 [CVE-2022-46176](https://www.cve.org/CVERecord?id=CVE-2022-46176)。所有包含 1.66.1 之前的 Cargo 的 Rust 版本都容易受到攻击。

Rust 1.66.0 的补丁文件也可[在此处](https://github.com/rust-lang/wg-security-response/tree/main/patches/CVE-2022-46176)获得，用于定制工具链。

如果您还不能升级到 Rust 1.66.1，官方建议将 Cargo 配置为使用git 命令 而不是其内置的 git 支持。这样，所有 git 网络操作都将由git 命令 执行，不受此漏洞的影响。可以通过 Cargo 配置文件来实现：

```toml
[net]
git-fetch-with-cli = true
```

#### Cargo 安全公告 (CVE-2022-46176)

Rust 官方发布了[Cargo 安全公告 (CVE-2022-46176)](https://blog.rust-lang.org/2023/01/10/cve-2022-46176.html)，Rust 安全响应工作组获悉，Cargo 在通过 SSH 克隆索引和依赖项时未执行 SSH 主机密钥验证。攻击者可利用此漏洞执行中间人 (MITM) 攻击。

> 当 SSH 客户端与服务器建立通信时，为了防止 MITM 攻击，客户端应该检查它是否已经与该服务器通信过，以及当时服务器的公钥是什么。如果自上次连接以来密钥发生变化，则必须中止连接，因为可能会发生 MITM 攻击。

该漏洞是由 Julia 安全团队 提交给 Rust 安全团队。

###  Rust 1.68 中将更新 Android NDK

[原文](https://blog.rust-lang.org/2023/01/09/android-ndk-update-r25.html)

Rust 中的 Android 平台支持将在 Rust 1.68 中实现现代化，因为在 NDK r23 中，Android 切换到对所有架构使用 LLVM 的 `libunwind` 。展望未来，Android 平台将以最新的 LTS（长期支持） NDK 为目标，允许 Rust 开发人员更快地访问平台功能。这些更新应每年进行一次，并将在发行说明中公布。

> [Android 中 NDK 工具链说明](https://developer.android.com/ndk/guides/other_build_systems)


### 【辟谣】hyper 存在拒绝服务漏洞 ？？? Rust 项目易受 DoS 攻击？？？真相在这里

近日，社区疯狂流传一篇被标题为“Hyper 存在漏洞，Rust项目易受拒绝服务攻击”的文章。

当然，这篇文章来自于国外，原文是 JFrog （JFrog是 Rust基金会白金成员）官方博客发布的名为[“使用 Rust 流行的 Hyper 包时注意 DoS”](https://jfrog.com/blog/watch-out-for-dos-when-using-rusts-popular-hyper-package/) 的文章。

其实，JFrog 的人在几个月之前就联系过 Hyper 作者 seanmonstar ，seanmonstar 在 hyper 相关 [issues ](https://github.com/hyperium/hyper/issues/3111) 里说到：

> 我知道这篇文章，他们在几个月前私下联系了我，我试图解释它与 本质上是一样的`Read::read_to_end`，但他们觉得声称 hyper 易受攻击的大标题对他们有用。🤷

JFrog 作为 Rust 基金会白金成员之一，起这个标题，到目前为止我是可以理解的，至少这个标题可以呼吁 Rust 开发者在使用 Hyper 时要注意这个 Dos 风险。

但让人更无语的是，国内技术媒体翻译到国内以后，标题被改的已经失去了原文的初衷，扭曲了事实。

首先必须得承认，原文中所说的 hyper 当前最新稳定版（v0.14）中 [`to_bytes` ](https://docs.rs/hyper/latest/hyper/body/fn.to_bytes.html)函数确实存在 Dos 风险。但其实，**这并不是 hyper 的漏洞**。

> JFrog 安全研究团队不断在流行的开源项目中寻找新的和以前未知的漏洞和安全问题，以帮助改善他们的安全状况并保护更广泛的软件供应链。作为这项工作的一部分，我们最近发现并披露了流行的 Rust 项目（例如[Axum](https://github.com/tokio-rs/axum)、[Salvo](https://crates.io/crates/salvo)和[conduit-hyper](https://crates.io/crates/conduit-hyper) ）中的多个漏洞，这些漏洞源于相同的根本原因——在使用 Hyper 库时忘记对 HTTP 请求设置适当的限制。

这是原文中的开篇，清楚地写道：“漏洞是属于一些较为流行的 Rust 项目（例如[Axum](https://github.com/tokio-rs/axum)、[Salvo](https://crates.io/crates/salvo)和[conduit-hyper](https://crates.io/crates/conduit-hyper) ），这些漏洞源于相同的根本原因——在使用 Hyper 库时忘记对 HTTP 请求设置适当的限制。”

是这些 Rust 项目，使用 Hyper 库时，自己忘记了对 HTTP 请求设置做适当处理导致的。

而在 Hyper 的文档中，`to_bytes` 这个函数的文档，清清楚楚地写了注意事项：

> 如果远程数据不受信任，则需要小心。该函数不执行任何长度检查，恶意方可能会消耗任意数量的内存。检查 `Content-Length`是一种可能性，但并不严格要求必须存在。

文档里提醒开发者使用这个函数需要小心，并且也解释了为什么在这个函数内部没有对其做限制。那些 Rust 项目使用了该函数，引发了DoS 漏洞，完全是因为没有看该文档导致的。换句话说，是使用不当导致的。

所以，结论是，hyper 并不存在什么 DoS 漏洞。[更多细节可以参考本文](https://mp.weixin.qq.com/s/g2z8qpkoLThKxaaFGSPhoQ) 。


### Pre-RFC : 为了提升供应链安全建议使用 Sigstore 签名和验证 crate

对于任何构建和维护开源软件的公司而言，软件供应链安全性越来越重要。许多应用程序依赖于第三方依赖项而不知道谁编写了软件，这可能为供应链攻击敞开大门。因此有人提议使用 [Sigstore](https://www.sigstore.dev/) 对 crate 进行签名和验证，并撰写了 [Pre RFC](https://github.com/trustification/rust-rfcs/blob/sigstore-rfc/text/0000-sigstore-integration.md)。

使用 Sigstore 可以改善供应链攻击向量中 Upload modified package 和 Compromise package repo 攻击。当前，如果 crates.io 遭到破坏，则无法验证从 crates.io 中提取的 crates。将 crates.io 和 cargo 与 Sigstore 集成减少了这种妥协的中断，因为仍然可以验证单个箱子，并且不可变日志的存在有助于审计和评估攻击的影响。

Sigstore 是由开源安全基金会 (OpenSSF) 运行的一个开源项目，它提供了针对上述问题的解决方案。它提供了一个密钥管理服务 Fulcio，它与 OpenID Connect 服务集成以识别作者并生成用于签名的密钥。Sigstore 还提供了一个不可篡改的透明日志 Rekor，用于记录签名证书以及从 OpenID 令牌中提取的其他信息。以后可以审核这些记录。

而 Rust 官方安全团队早已在 9 年前（2014）就注意到了这个问题，并建立了 crate 安全模型的 [issue](https://github.com/rust-lang/crates.io/issues/75) 讨论，并提到将由 Tor 开发人员和学者共同开发的[更新框架（TUF）](https://blog.sigstore.dev/the-update-framework-and-you-2f5cbaa964d5)（TUF 是一组特定于软件分发系统的已定义攻击和威胁模型，以及一组巧妙设计的协议来抵御它们）集成到 Cargo 中，然而一直没有落地一个合适的方案。


## 【热议】什么场景下使用 C 比 Rust 更好？

[该问题]((https://www.reddit.com/r/rust/comments/108z0m4/when_is_c_better_a_better_choice_than_rust/) )来自于 `Reddit/r/rust` 频道。

**一个高赞回答如下：**

1. 最大的可移植性。很少有平台没有某种C语言工具链可用，无论是奇怪的大型机系统、老式工作站，还是一些可爱的嵌入式东西。
2. 合规性。有大量的规范有效地要求C语言；MISRA就是一个例子。
3. 工具化。C有大量围绕它的工具，比如frama-c和其他分析你的代码的工具，或者compcert编译器。虽然Rust有很多用于普通情况的工具，但对于更多的小众事物来说，它很难超越50年以上的生态系统。
4. 生命力持久。如果出于某种原因，你需要让你的代码在50年后仍可运行，那么我认为C是更好的选择。

上面四点，大部分人同意前三点，但是对于第四点，Rust 社区有人回复：“我不明白，为什么 Rust 不能运行 50 年？”。

“C 是过去 50 年的语言，Rust 是未来 50 年的语言”，这句话是 Rust 社区流行的一句话，现在通过这个帖子的讨论看来，大部分人并不认同这句话。这显然是可以理解的，因为让 Rust 成为未来 50 年的语言，目前还只是一个美好的愿望，并且 Rust 社区还在为这个目标努力。但是 C 语言，它已经达成了过去 50 年屹立不倒的成就了。

## Rust 应用实践

### RedoxOS 的 GUI 安装程序即将推出，用 iced 编写！

[RedoxOS](https://www.redox-os.org/) 是一个用 Rust 编写的类 Unix 操作系统，旨在将 Rust 的创新带到现代微内核和全套应用程序中。

之前 RedoxOS 的 UI 开发套件是 [orbtk](https://github.com/redox-os/orbtk)，三个月之前，orbtk作者宣布该项目即将落幕，RedoxOS 将换成 [iced](https://iced.rs/) 。目前 iced （以及 [slint](https://slint-ui.com/) ）提供与 Redox OS 兼容的渲染器无关工具包，但它们还支持比 OrbTk 更多的功能。


### 官宣：支持在 Chromium 项目中使用 Rust

[Google 安全博客官宣](https://security.googleblog.com/2023/01/supporting-use-of-rust-in-chromium.html) 将在 Chromium 项目中支持 Rust 第三方库。

目前 Chromium 团队正在积极地将 Rust 工具链集成到其构建系统中（实际上这项工作已经持续很久了），在明年（2024？）内将 Rust 代码包含在 Chrome 二进制文件中。

**为什么选择将 Rust 引入 Chromium？**

为了提供一种更简单（无 IPC）和更安全的方式来满足**加快开发速度**（更少的代码编写，更少的设计文档，更少的安全审查）并**提高Chrome的安全性**（增加没有内存安全错误的代码行数，降低代码的错误密度）的需求。

**Chromium 将如何支持 Rust 的使用？**

- 目前，Chromium 将只支持单一方向的互操作，即从 C++ 到 Rust。Chromium是用C++编写的，大部分的框架技术栈都是C++代码，通过将互操作限制在一个方向，可以控制依赖树的形状。Rust不能依赖C++，所以它不能知道C++的类型和函数，除非通过依赖注入。
- 暂时只支持 Rust 第三方库。第三方库是作为独立的组件编写的，它们不持有关于Chromium实现的隐含知识。这意味着它们的API更简单，而且专注于它们的单一任务。

**Chromium中 Rust 和 C++ 之间的互操作**

迄今为止，大多数成功的C/C++和Rust互操作故事都是围绕着通过单一(Narrow)的API（如QUIC或蓝牙的库，Linux驱动程序）或通过明显的隔离组件（如IDL，IPC）进行互操作。Chrome是建立在基础的但真正广泛的C++ API上的，在高层次上，团队发现，由于C++和Rust遵循不同的规则，事情很容易出岔子。例如，Rust通过静态分析来保证时间上的内存安全，静态分析依赖于两个输入：生命期（推断的或明确写入的）和独占的可变性。后者与Chromium的大部分C++的编写方式不兼容，在整个系统中持有冗余的可变指针，以及提供多条路径来到达可变指针的指针。这在Chrome浏览器进程中尤其如此，它包含了一个巨大的（可变）指针互连系统。如果这些C++指针也以复杂或长期的方式被用作 Rust的引用，这就要求C++作者理解Rust的别名规则，并防止违反这些规则的可能性。

总之，如果没有额外的互操作工具支持：
- 跨语言传递指针/引用是有风险的
- 语言之间单一(Narrow)的接口对于正确编写代码来说是至关重要的

Google 目前正在投入 [Crubit](https://github.com/google/crubit) 项目，这是一个关于如何提高C++和Rust之间互操作的保真度，并将每种语言的要求表达或封装给对方的实验。

Chromium 团队认为：

> Rust生态系统是非常重要的，特别是对于像Chromium这样以安全为重点的开源项目。这个生态系统是巨大的（crates.io上有96k+的crates），并且在不断增长，包括谷歌在内的整个系统开发行业都在投资。Chrome浏览器在很大程度上依赖于第三方代码，而我们需要跟上第三方投资的步伐。我们必须支持将Rust纳入Chromium项目，这一点至关重要。我们将遵循上述策略来建立规范，并通过第三方程序保持一定程度的API审查，同时我们展望未来的互操作支持，推动Rust和C++之间可能和合理的操作的界限。

内存不安全是整个行业当前面临的问题，而利用 Rust 是在这一领域推进战略的一个部分。


### dora-rs： 机器人中间件项目

[dora](https://github.com/dora-rs/dora) 是一个基于 Rust 的机器人框架，目标是成为一个低延迟、可组合和分布式的面向 SDV 和 无人驾驶的数据流计算平台。，旨在比当前机器人应用标准 ROS/ROS 2 好 10 倍，成为 ROS/ROS2/Autosar 的替代者。

Rust-os Blog的作者 Philip Opperman是 dora 主力开发者之一 。

dora 通信层暂时依赖于 [eclipse-zenoh/zenoh](https://github.com/eclipse-zenoh/zenoh)，关于zenoh 的介绍可以参考文章 [开源产品 | eclipse zenoh 助力雾计算和边缘计算](https://rustmagazine.github.io/rust_magazine_2021/chapter_4/zenoh.html)。[dora-rs 的通信层正在被重新设计](https://github.com/dora-rs/dora/pull/162)，目标是将数据面的控制和传输技术分离，比如算子都在一台机器部署的时候，就会用共享内存，这样延时很低。

更多文档参考：[https://dora-rs.github.io/dora/](https://dora-rs.github.io/dora/)，并且还配套有基于dora的自动驾驶入门套件 [dora-drives](https://github.com/dora-rs/dora-drives)。

虽然是早期项目，但发展不错，目前正在加入开放原子基金会的过程中，并且在 2023 年春季会基于 dora 开展国际智能驾驶大赛（Openatom Carsmos全球开源自动驾驶算法大赛）。

### BlackBerry 和 Elektrobit 通过支持 Rust 编程语言加强汽车安全路线图

[官方原文](https://www.blackberry.com/us/en/company/newsroom/press-releases/2023/blackberry-and-elektrobit-bolster-automotive-safety-roadmap-with-support-for-rust-programming-language)

BlackBerry 是将 Rust 语言集成到 BlackBerry QNX 微内核实时操作系统中，Elektrobit 与 BlackBerry QNX 在 Rust 项目上密切合作，贡献代码，确保代码质量，处理项目管理以及与 Rust 社区的互动。 Elektrobit 公司是AUTOSAR专家，深耕汽车软件行业，和 BlackBerry QNX 是很多年合作伙伴。

BlackBerry QNX 已在全球范围内获得超过 2.15 亿辆汽车的信赖，并在全球范围内部署在商用车、重型机械和其他市场等一系列行业的嵌入式系统中。其产品已通过多项行业安全标准的预认证，包括 ISO 26262、IEC 61508 和 IEC 62304，该公司还获得德国莱茵 TÜV 独立审计师的认可，成为全球首个通过 ASIL D 安全认证的商业管理程序。

Rust 可与 BlackBerry 经过安全认证的 BlackBerry QNX 产品组合集成，有能力塑造关键任务软件和软件定义车辆的未来

> 点评：这次合作比等待 Autosar 直接引入 Rust 的效率高多了。


### aquatic_udp 性能改进，达到每秒响应高达 225 万次

[aquatic](https://github.com/greatest-ape/aquatic) 当前已经完成了新一轮开放 UDP BitTorrent tracker 实施的基准测试。aquatic_udp的结果非常好，在 8 个 CPU 内核上运行时实现了[opentracker](http://erdgeist.org/arts/software/opentracker/)的两倍吞吐量，达到每秒响应高达 225 万次。


## 生态看点

### flux: Rust 精化类型库

[flux](https://github.com/liquid-rust/flux/) 是最新发布的一个 Rust 精化类型（Refinement Types）库。

> 精化类型 (refinement types) 在普通类型的基础上添加了对变量取值范围的约束，从而可以用于保证程序不存在除零、数组越界等错误，甚至完全验证程序的功能正确性。
> 精化类型的工作机制可以简单描述为：标注函数的输入和输出满足的条件（前置条件和后置条件），经过验证条件生成，使用 SMT 求解器半自动地验证正确性。精化类型可以看作依赖类型的一个子集，对于依赖和谓词的使用有一些约束以保证类型检查的可判定性。
> 详情可以参考[华为编程语言Lab - 精化类型简介](https://zhuanlan.zhihu.com/p/451297869)。

#### 为什么需要精化类型？

Rust 拥有强大的类型系统，可以在编译期捕捉很多常见的错误，以此来保证程序的正确性、内存安全性和并发安全性。然而，类型系统并不能捕捉全部的错误，还有一些常见的错误是类型系统无法捕捉的。例如，除零错误、数组越界、程序执行结果的正确性（类型系统可以保证返回值类型正确，但无法保证具体的值是否满足要求，比如返回的数组是否满足指定排序）。精化类型解决的主要是程序运行时的正确性问题。

flux 通过 Rust 过程宏将精化类型带到了 Rust 中，它在编译时进行检查。

```rust
#[flux::sig(fn() -> i32[10])]
pub fn mk_ten() -> i32 {
    5 + 4
}
```
上述代码，使用 `#[flux::sig(fn() -> i32[10])]` 标注的 `mk_ten` 函数，意味着它输出的值（后置条件）应该是 10 。

但是在编译时，flux 就会发现问题，告诉你当前函数执行结果不满足后置条件：

```
error[FLUX]: postcondition might not hold
 --> src/basics.rs:7:5
  |
7 |     5 + 4
  |     ^^^^^
```

再看另一个示例：

```rust
#[flux::sig(fn (b:bool[true]))]
pub fn assert(b:bool) {
    if !b { panic!("assertion failed") }
}
```
这次 `flux` 设置的是前置条件，即，assert 函数的参数必须输入 true，不能是 false。

```rust
fn test(){
    assert(2 + 2 == 4);
    assert(2 + 2 == 5); // fails to type check
}
```
测试代码的第二个测试就会检查失败，因为它不满足前置条件。

值得说明的是，[Flux 利用Rust的所有权机制](https://liquid-rust.github.io/2022/11/29/ownership-in-flux/)来跟踪共享（`&T`）和可变（`&mut T`）引用的属性，另外还增加了一个强（`&strg T`）引用 -- `&mut`的一个特例 -- 来支持类型本身被调用改变的情况。

flux 还提供了很多功能，更多详情可以参考[flux 官方介绍文章](https://liquid-rust.github.io/2022/11/14/introducing-flux/)。官方也提供了[在线体验 flux 的网站](http://goto.ucsd.edu:8091/index.html#?demo=basics.rs)。flux 作者来自 UC San Diego 大学，他也发布了相关论文[Flux: Liquid Types for Rust](https://arxiv.org/abs/2207.04034)。

### Veryl: 现代硬件描述语言

[veryl](https://github.com/dalance/veryl) 是 Rust 实现的现代硬件描述语言，目前正处于语言设计的探索阶段。

- [Veryl Playground](https://dalance.github.io/veryl/playground/)
- [Veryl Book](https://dalance.github.io/veryl/book/)

Veryl 的目标是 SystemVerilog 的替代品。

> SystemVerilog是一种硬件描述和验证语言（HDVL），它基于IEEE1364-2001 Verilog硬件描述语言（HDL），并对其进行了扩展，包括扩充了C语言数据类型、结构、压缩和非压缩数组、 接口、断言等等，这些都使得SystemVerilog在一个更高的抽象层次上提高了设计建模的能力。SystemVerilog由Accellera开发，它主要定位在芯片的实现和验证流程上，并为系统级的设计流程提供了强大的连接能力。

Veryl 遵循以下设计原则：
- Veryl 具有基于 SystemVerilog / Rust 的简化语法。“简化”有两个含义：一个用于解析器，另一个用于人类。SystemVerilog 的语法非常复杂（参见 IEEE Std 1800-2017 Annex A）。这导致 SystemVerilog 工具实现困难。
- 能转译到 SystemVerilog。Veryl 将具有与 SystemVerilog 几乎所有相同的语义。所以转译后的代码将是人类可读的 SystemVerilog。此外，Veryl 与 SystemVerilog 具有互操作性。Veryl可以在代码中使用SystemVerilog的`module/interface/struct/enum`，反之亦然。
- 现代编程语言默认具有 linter、格式化程序和语言服务器等开发支持工具。从开发之初，Veryl 也会拥有它们。

### h3o : h3 地理空间索引系统的 Rust 实现

[h3o](https://github.com/HydroniumLabs/h3o) 是对 Uber 开源的 h3 地理空间索引系统的纯 Rust 实现，100%覆盖 h3 4.0 API。目标是，在Rust项目中更容易集成，特别是在针对 WASM的时候，并且能提供更安全更快的API。

> [H3](https://github.com/uber/h3) 是一个针对地球的空间划分和空间索引系统。由 Uber 在 2018年初正式开源。h3 将全球划分为不同分辨率的六边形的蜂窝小区，通过 Uber 公司 H3 Core Libary API 可以将经纬度坐标转移映射到六边形小区，从而实现了卫星通信路由区。

h3o 的由911个测试案例组成的基准套件已经被开发出来了，涵盖了整个公共API，并将h3o的性能与H3参考实现（通过h3ron-sys crate，预计没有开销）进行比较，h3o更快。

### datacake ： 构建最终一致的分布式数据系统

[datacake](https://github.com/lnx-search/datacake) 是 [Lnx](https://github.com/lnx-search/lnx)（基于rust tantivy 搜索引擎实现的类似 ElasticSearch 的工具） 项目下的一个新项目，用于构建最终一致的分布式数据系统。

Datacake是作者尝试为 lnx 带来高可用性的成果，分布式共识算法采用 [Quickwit](https://quickwit.io/) 开发的 [chitchat](https://github.com/quickwit-oss/chitchat) ，它是 scuttlebutt 算法的实现。


### leptos: 全栈同构的 Rust Web 反应式（reactivity）框架

[leptos](https://github.com/leptos-rs/leptos) 是提供了构建现代 Web 应用程序所需的大部分内容：反应式系统、模板库以及可同时在服务器端和客户端运行的路由器。特点是，利用细粒度的反应式来构建用户界面。

目前仅发布了 0.1，可以关注。


### how-u-doin: Rust 的进度报告抽象库

![process](https://user-images.githubusercontent.com/13831379/211681673-7e0898b7-dded-4121-8876-cf261c2a124d.gif)

[how-u-doin](https://github.com/kdr-aus/how-u-doin)的架构类似于 `log` 库，`log`为日志输出提供了一个抽象的接口（基于 faced 设计模式），而 `howudoin` 则为进度（progress）报告提供抽象接口。该接口用于将进度的消费和显示分离。


### Fyrox 游戏引擎发布 0.29 版本

[Fyrox](https://github.com/FyroxEngine/Fyrox) 是 Rust 实现的游戏引擎，支持 3D 和 2D 游戏，目前发布了 0.29 版本。

0.29 版本特性摘要：
- 重新设计的动画系统。长期以来，引擎中的动画系统只能对场景节点的位置、旋转和缩放进行动画处理。现在它发生了变化——您可以使用反射为几乎任何数字属性设置动画。
- 新的动画编辑器。感兴趣可以参考[动画编辑器介绍](https://fyrox-book.github.io/fyrox/animation/anim_editor.html)
- 改进 WebAssembly 支持。引入 `fyrox-template`（一个生成游戏项目和脚本模板的简单工具）现在能够为游戏生成单独版本的 WebAssembly 执行器，几乎可以毫不费力地创建游戏的 WebAssembly 版本，并且也支持了一键部署 WebAssembly 。

还有很多重大特性更新，感兴趣的朋友[可以参考 fyrox 更多新特性介绍](https://fyrox.rs/blog/post/feature-highlights-0-29/)。

### cargo-show-asm 发布 0.2.10 版本

[cargo-show-asm](https://github.com/pacak/cargo-show-asm) 是一个快速查看 rustc 生成的 asm 汇编指令的工具。

查看 Rust 项目生成 asm 汇编指令的方式之前有两种方式：
- 使用 [godbolt.org](https://godbolt.org/)
- 通过 `cargo rustc -- --emit asm` 生成

现在又多了一种更方便的工具 cargo-show-asm ，它不仅支持多种平台，还支持显示 `Intel/A&T` 两种不同的汇编格式，可以展示 `llvm-ir`，`rustc MIR`，`wasm` 等多种指令，同时还实现了 `bash/zsh` 的自动补全、彩色输出等功能，是一个非常不错的命令行工具。

### Vello : 2D 图形的新型 GPU 加速渲染器改用 `wgpu`

[原文](https://raphlinus.github.io/rust/gpu/2023/01/07/requiem-piet-gpu-hal.html)

[Vello](https://github.com/linebender/vello) 是一种用于 2D 图形的新型 GPU 加速渲染器，其操作在很大程度上依赖于计算着色器。曾用名为 piet-gpu-hal，因为之前是基于 Piet 进行渲染，现在改名为 Vello 已经依赖于 [`wgpu`](https://wgpu.rs/) 了。

Vello 作者 Raphlinus 是 GUI 领域资深开发者。引导开发过 Xi-Editor 和 [druid](https://github.com/linebender/druid)。既然他决定将 Vello 大幅重写改用 wgpu，那基本上 `wgpu`(WebGPU API 的 Rust 实现) 可以看作是图形渲染领域 的 Rust 事实性基础设施了。


### 开源 Citadel 协议

> 值得一提的是，Citadel 协议在过去五年中，一直由作者单人开发维护，并未开源。但是，在最近他看到 [中国研究人员声称使用量子和经典计算机的混合体破解了低级 RSA 加密——用于全球安全数据传输的标准加密方法——令安全界感到惊讶](https://techmonitor.ai/hardware/quantum-encryption-rsa-cryptography) 的新闻，感到，开源 Citadel 协议是必须的了。。

> 来自中国郑州数学工程与先进计算国家重点实验室研究人员的新预印本论文引起了人们的关注。它提出了一种不同的算法，可以用低级量子计算机破解 2048 位 RSA。该团队表示，他们使用基于 10 量子位量子计算机的混合系统破解了 48 位 RSA，如果他们能够访问至少具有 372 量子位的量子计算机，则可以对 2048 位做同样的事情。这项工作反映了对混合量子/经典解决方案的更大推动，其中包括日本理研研究所将量子计算机连接到自己的超级计算机 Fugaku的工作。

> 中国科学家真的用量子计算机破解了RSA加密吗？科学家们表示，他们的方法可用于使用 372 量子位量子计算机来破解高级 2048 位 RSA 加密，这将具有重大的安全隐患。

[Citadel](https://github.com/Avarok-Cybersecurity/Citadel-Protocol) 协议是一种用 Rust 编写的高性能异步信号类协议，它通过使用多层棘轮、多层加密、后量子密钥交换、可配置的真正完美前向保密（每个数据包都有一个通过重新加密的唯一加密密钥）或尽力而为模式、文件传输加密、内置 NAT 遍历（无 libp2p）、可配置的凭据身份验证（通过 argon-2id）、设备相关身份验证或无密码身份验证等特征。

Citadel 协议建立在底层传输协议之上。TCP、TLS 或 QUIC 均可用于传输（混合加密的默认 TLS）。Citadel 协议用于在网络中的节点之间进行通信。

网络拓扑包含一个全局可访问的中央节点，默认情况下，该节点用于对等点发现和 NAT 遍历（该节点可以赋予它应用程序逻辑，但不是必需的）。对等点连接到这个中央服务器，并且在注册和连接后，能够在彼此之间甚至中央服务器之间传递消息。

由于使用 UDP 更容易执行 NAT 遍历，因此 QUIC 协议用于 P2P 连接，因为这是免费的。对于客户端到服务器的连接，服务器可以选择使用 TCP、TLS 或 QUIC 作为底层协议。

使用 Citadel SDK，Rust 开发人员可以轻松创建适用于客户端到服务器和 p2p 应用程序的超安全后量子应用程序。


### Zenoh 0.7 发布

代号为 Charmander 的 [Eclipse Zenoh 0.7](https://github.com/eclipse-zenoh/zenoh/releases/tag/0.7.0-rc) 版本发布。 引入了一些期待已久的功能：

- 双向 TLS 认证；
- MQTT插件；
- S3 存储后端；

更多详情参考[Zenoh 官方介绍](https://zenoh.io/blog/2023-01-10-zenoh-charmander/)。

### safecloset: 跨平台安全 TUI 私密信息存储器

[safecloset](https://github.com/Canop/safecloset) 将您的秘密保存在受密码保护的文件中。 SafeCloset旨在方便并避免常见的弱点，如外部编辑或写入磁盘的临时文件。

> 警告：SafeCloset未经独立审计，绝对不能提供任何保证。如果你丢失了存储在 SafeCloset 中的秘密，作者表示他也无能为力。

更多介绍可以参考[safecloset 文档](https://dystroy.org/safecloset/)。