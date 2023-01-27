# Rust 视界周刊 Week 3
---

## 社区热点

### 趣谈：你“不应该”使用 Rust 的十大理由

[fasterthanlime 的油管视频介绍了你“不应该”使用 Rust 的十大理由](https://www.youtube.com/watch?v=ul9vyWuT8SU) ：

1. It's bad for job security. Rust 会不利于你的工作保障。虽然 Rust 说可以在编译期避免很多错误，但是你们想想，现在有多少人在用像 Go 这样的语言一遍一遍地修复相同的错误。而这些错误都是让人衣食无忧的保障。
2. It's moved to slow. Rust 已经存在了十二年，稳定发布了八年，但是它仍然缺乏对 Null 的支持。几乎所有语言都支持 null，比如 C/C++/Java/C#/Python/Ruby/Javascript，但是 Rust 不支持。Rust 有 Option，它会逼迫你处理Some 和 None 两种情况，这让事情变得更加复杂了。
3. It's moved to fast. Rust 每六周发布一次，每三年发布一次新的 Edition 。变化太快了，比如 Rust 的宏语法，在 2015 edition 之前，你还得写 `extern crate xxxmacro`，但是 2018 年你就无需写这个了。但是对于学习者来说，该如何跟踪这种语法变化呢？
4. It's too opinionated. Rust 太自以为是了。rustfmt正在压制关于代码格式化的正确性方法的建设讨论，以“协作”名义扼杀个体开发人员的创造力。在 Rust 中也只能使用 Cargo，但是 C 中，我们可以使用 make、cmake、qmake、premake、autotools、scons、GN、bazel、buck、ninja、meson等多种选择。
5. It's not opinionated enough. Rust 也不够自以为是。Async Rust 可以适用于高性能 Web 应用和嵌入式硬件，这意味着你只需要在 Cargo.toml 中添加一行 `tokio={version="1", features=[full]"`，这字母可太多了。
6. 此条在国内过于敏感，请看原视频，不喜勿喷。
7. It's not inclusiv enough. 包容性不够。Rust 社区不欢迎那些威胁到社区成员的不友好分子（见 Rust 社区行为准则条款），那么那些人该去哪里交流 Rust 呢？难道仅仅是因为他们可能伤害人类同胞的心理或身体就驱逐他们吗？这样是不是不太公平？
8. It's bad for the economy. 对经济不利。英特尔、AMD和苹果每年都在推出越来越快的硬件，但是 Rust 在十年前的硬件上依旧良好运行。诚然，编译很慢，但这仅仅是涉及开发人员，他们只占市场的一小部分。
9. It's bad for the environment. 对环境不利。当 Rust 团队对 Rust 编译器或标准库进行更改时，他们通常会使用[`crater run`](https://github.com/rust-lang/crater)命令，他们会编译每个已经发布的 Rust 项目，看看它们是否引入了性能回归，这占用了大量宝贵的计算资源，我们本来可以用它挖掘加密货币的。
10. It's bad for security researchers. 对安全研究人员不利。使用 C/Cpp，你可以看到很多有趣的代码，缓冲区溢出漏洞基本上就在你掌心里了。而 Rust 则让安全人员加倍工作，以至于不禁发问：他们必须隐藏点什么呢？


### 2023 年的 Rust：成长

Rust 语言团队 Leader Niko 发布了一篇博文，介绍了 [Rust 2023 年最重要的事情](http://smallcultfollowing.com/babysteps/blog/2023/01/20/rust-in-2023-growing-up/)。

1. 实现：让任何函数异步化、在任何地方使用`impl Trait` 以及充分使用泛型关联类型（GAT）。
2. 为Rust 2024 Edition 做计划；开始制定 Rust 语言规范（specification）（相关 [RFC PR #3355](https://github.com/rust-lang/rfcs/pull/3355)），并将其纳入流程。
3. 为 Unsafe 代码定义规则，并为检查你是否遵循这些规则提供方便的工具。为此官方在计划创建[操作语义团队(Operational Semantics Team)](https://github.com/rust-lang/rfcs/pull/3346)
4. 支持在大学和其他地方教授 Rust。
5. 改进产品规划和用户反馈流程。
6. 完善团队治理结构，为专门的领域提供专门的团队，为广泛的监督提供更多可扩展的结构，以及更密集的入职培训。

延伸阅读：

- [Do we need a "Rust Standard"?](https://blog.m-ou.se/rust-standard/)
- [Ferrocene Rust Language Specification](https://spec.ferrocene.dev/)
- [Rust 类型系统的“正式模型” a-mir-formality](https://github.com/nikomatsakis/a-mir-formality)
- [Announcing: MiniRust](https://www.ralfj.de/blog/2022/08/08/minirust.html)
- [Safe Transmute RFC](https://rust-lang.github.io/rfcs/2835-project-safe-transmute.html)
- [rust-book in brown edu](https://rust-book.cs.brown.edu/)
- [Governance Update](https://blog.rust-lang.org/inside-rust/2022/10/06/governance-update.html)


### 《C++ DG 关于 ISO C++ 安全的意见》论文在 Reddit Rust 社区引起很大争议

[《C++ DG (Direction Group) 关于 ISO C++ 安全的意见》论文](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2023/p2759r0.pdf)在 Rust 社区引起很大争议。

论文中对于 Rust 的描述有很多不严谨的说法，甚至还认为 Rust 最初是构建于 Cpp 之上，他们（Cpp 委员会）甚至都不屑于去了解下 Rust 。 [感兴趣可以参考 Reddit 更多讨论](https://www.reddit.com/r/rust/comments/10gi09w/dg_opinion_on_safety_for_iso_c/)。

> 当他们试图通过忽略两种语言的优势并说“当狗屎发生时 Rust 和 C++ 一样糟糕”来争论 C++ 和 Rust 一样安全时，这是一个奇怪的争论点。

比如，把供应链攻击称为语言漏洞。

> 没有称为“RUST”（全部大写）的语言。这通常由对编程一无所知的人使用（例如人力资源人员和糟糕的经理）。或者，它由不了解 Rust 的人使用。

C++ DG 论文中似乎认为 C++ 的不安全只是一个公众形象问题

> C++ 似乎，至少在公众形象中，在安全方面不如其他语言有竞争力……C++ 受益于拥有规范、活跃的用户和实施者社区。其他“安全”语言甚至可能没有任何规范，至少现在还没有。这些重要的安全特性被忽略了，因为我们不太关心广告。在将近半个世纪的时间里，C++ 也在数百万行代码中经过了时间和战斗的考验。其他语言不是。众所周知，无可争辩的事实是，所有语言都存在漏洞，但这通常需要时间。较新的语言具有较少的漏洞，因为它们还没有经过时间的考验。

### ISLE 在 Cranelift 中被完全采用

ISLE 是一种支持类型系统的指令选择器 DSL。ISLE 的引入使 Cranelift 开发者能够以极其高效的方式开发编译器中关于后端目标体系结构中每一种的指令降低模式以及与机器无关的优化重写。

> 指令选择器 DSL ([ISLE](https://github.com/bytecodealliance/wasmtime/blob/main/cranelift/isle/docs/language-reference.md), Instruction Selection Lowering Expressions)。指令选择(instruction selection)是将中间语言转换成汇编或机器代码的过程。ISLE 是一种领域特定语言，用于编写指令选择和重写规则。ISLE 源文本被编译成 Rust 代码。ISLE 的目标是表示指令降低模式。指令降低模式是一种规范，即 IR (CLIF) 中的某种运算符组合，当在特定条件下组合时，可以编译成特定的机器指令序列。

Cranelift [ISLE 模块作者的这篇文章](https://cfallin.org/blog/2023/01/20/cranelift-isle/)详细阐述了 ISLE 的细节。

> Cranelift 是一个底层的可重定向代码生成器，Cranelift 目前被用在 wasmtime 中支持 WebAssembly，也被用于 Rust Debug 模式下编译后端。

在 [Reddit Cranelift 相关讨论中](https://www.reddit.com/r/rust/comments/10h4gd5/cranelifts_instruction_selector_dsl_isle/) 有人提到一个问题：craneflit是否会提供能匹配 LLVM/GCC O2/O3/O4 级别优化的短期或长期目标？

Cranelift 的开发者回复：“Cranelift 可能永远达不到 LLVM 或 gcc 的`-O3`级别，因为实现它需要的东西太多了，但确实有计划在现有的基础上添加更多优化，比如 `-o`或`-o2`级别。现在支持了 ISLE ，会对此有更多帮助”。


### Cargo 开始集成 gitoxide

[gitoxide](https://github.com/Byron/gitoxide) 是 Rust 重新实现的 Git，它也可以作为库被嵌入到项目中。

[Cargo fetch 集成 gitoxide 的 PR](https://github.com/rust-lang/cargo/pull/11448) 已经创建。因为 gitoxide 的性能比 git 更好。当该 PR 合并和启用时，crates-index 或 git 依赖项的克隆将快约 2.2 倍，而不会受到增量分辨率减慢的影响。

更多详细参考[gitoxide 讨论贴](https://github.com/Byron/gitoxide/discussions/694)。

### GitHub 和Rust 基金会正在合作帮助保护您免受crates.io密钥泄露的侵害

[GitHub 官宣](https://github.blog/changelog/2023-01-19-the-crate-io-registry-is-now-a-github-secret-scanning-integrator/)，GitHub 将扫描每个提交到公共存储库 crates.io 的公开的密钥。这项工作是和 Rust 基金会合作的，用于帮助保护 Rust 开发者免受crates.io密钥泄露的侵害。

### Rust 开发者生态系统 2022 年现状：发现近期趋势

来自 [jetbrains 的调查报告](https://blog.jetbrains.com/rust/2023/01/18/rust-deveco-2022-discover-recent-trends/)显示 ：

1. 在工作中使用 Rust 的开发人员比例从2020 年和2021 年的 16% 增长到 2022 年的 18%。
2. 根据调查，24% 的 Rustacean 已经使用 Rust 一年以上。这比去年增加了 4 个百分点，但仍然不容易找到经验丰富的 Rust 开发人员。 
3. JavaScript / TypeScript 仍然是与 Rust 一起使用的最流行的语言，并且其份额正在逐渐增加。 
4. 使用 `println!` 或 `dbg!` 宏调试代码的人从 60% 下降到 55%，而在 IDE 中进行 UI 调试的比例从 23% 上升到 27%。
5. Rust 开发人员使用的最流行的性能分析工具是 perf，但绝大多数 Rustecean（82%）根本不使用性能分析工具。 
6. 开发人员主要将 Rust 用于构建 CLI 工具、系统编程和 Web 开发。（鉴于公众对 Rust 的看法是区块链行业中有很多 Rust 工作，然而在统计报告中的区块链比例 6% 甚至低于嵌入式和学术用途 7%）
7. Linux 作为主要的 Rust 目标平台位居首位。随着最初的 Rust 支持在 10 月份被合并到 Linux 内核中，Rust 继续集成到 Linux 世界中。而 WebAssembly 平台占 22%。
8. 使用 rust-analyzer 的开发者比例从 25% 增加到 45%。42% 的开发人员使用 IntelliJ Rust，而去年这一比例为 47%。

### Rust 类型团队成立

[Rust 官方正式宣布类型团队](https://blog.rust-lang.org/2023/01/20/types-announcement.html)成立。

在过去的几年里，Rust 在许多指标上都有显着的增长：用户、贡献者、特性、工具、文档等等。随着它的发展，人们想用它做的事情清单也以同样快的速度增长。除了强大且符合人体工程学的功能之外，对 IDE 或语言学习工具等强大工具的需求也越来越明显。正在编写新的编译器（前端和后端）。而且，最重要的是，官方希望 Rust 继续保持其核心设计原则之一：安全性。

现在GitHub 上有很多关于 Rust 语言、编译器和类型相关的 issues，包括还有 62 个开放的[不健全（Unsound）问题](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AI-unsound)。

因此，特别成立了类型团队，用于解决和类型系统相关的边缘问题，包括类型检查器、trait求解、借用检查、Unsafe 代码准则等。在 [RFC 3254-types-team](https://rust-lang.github.io/rfcs/3254-types-team.html) 中定义了类型团队的职责范围。

### Servo 项目在 2023 年重新启动

[Servo 官方宣布](https://servo.org/blog/2023/01/16/servo-2023/)，得益于新的外部资金，一组开发人员将积极致力于 Servo 的开发。

第一项任务是重新激活该项目及其周围的社区，这样我们就可以为该项目吸引新的合作者和赞助商。2023 年的重点是改善 Servo 布局系统的情况，初步目标是让基本的 CSS2 布局工作。

## 应用实践

### CodeSandbox 中现在已经支持 Rust

[CodeSandbox 官宣](https://codesandbox.io/blog/announcing-rust-support-in-codesandbox) ，现在可以在 CodeSandbox 中启动 Rust 开发环境了。

CodeSandbox 是一个云开发平台，可以非常轻松地启动云环境并与他人共享。CodeSandbox 平台架构也使用 Rust 开发。

一位 Web 框架作者在 reddit 相关讨论中留言：“我认为这将真正改变 Rust 库和文档的世界，可以使用 CodeSandbox 来设置更加丰富的交互式示例“。

在 GitHub Codespaces 中，不能与他人共享（除非共享你的 git secrets）并且不可分叉。使用 CodeSandbox，你可以与其他人共享沙箱，其他人可以分叉它以继续开发他们自己的版本。所以它更专注于共享和协作。

### Azure Sphere IoT 平台上应用 Rust

开发人员现在可以在 Azure Sphere 平台上为联网设备创建应用程序时使用Rust编程语言了。

Azure Sphere 已经包含了用于联网设备的内置安全功能，并且包含基于联发科芯片和基于 Linux 的操作系统构建的硬件。此外，它还包括基于云的 Azure Sphere 安全服务 (AS3)，可在设备与互联网或云之间建立安全连接。

AS3 确保安全启动、设备身份验证、软件的信任以及设备运行可信代码的证明。它还使 Microsoft 能够安全地将更新下载到设备上的 Azure Sphere 操作系统和应用程序。将 Rust 引入 Azure Sphere 增加了更多的安全功能。
微软在 GitHub 上提供了一个指向 [Azure Sphere Rust 项目的链接](https://github.com/Azure/azure-sphere-gallery/tree/main/Rust)，其中包括 API、示例和许可条款。

### 西门子在一次内部Rust Meetup上介绍了Rust在列车控制网络中的应用

来源：推特 [Daniel Bovensiepen Li @bovensiepen](https://twitter.com/bovensiepen/status/1616367973475966976)

> Today's Rust Meetup at Siemens introduced the application of Rust in train control networks. Things are moving 🥰

没有更多信息了。

### VMware Lab 的 Wasm Workers 服务器支持 Rust workers

[rust-workers](https://workers.wasmlabs.dev/docs/tutorials/rust-workers) 会被编译为 WASM 模块。然后，它们由 Wasm Workers Server 加载并开始处理请求。

[Wasm Workers Server](https://github.com/vmware-labs/wasm-workers-server) 是使用被称为“workers”的轻量级结构运行无服务器代码的项目，也是纯 Rust 实现，基于 wasmtime。


## 生态看点

### hyper 回顾与展望

hyper 在 2022 年回顾：

- 做了很多迈向 1.0 的工作，目前 hyper 1.0 即将要发布了。相关链接见[hyper issues #3088](https://github.com/hyperium/hyper/issues/3088)。
- hyper in curl 的工作持续推动，curl 的大型 HTTP 套件中只有少数剩余测试未通过，目前需要社区贡献。
- [h3](https://github.com/hyperium/h3) crate，提供对任何 QUIC 实现通用的 HTTP/3，目标是直接集成到 hyper 中。
- tower 与 hyper 集成，以及[tower Service 1.0](https://github.com/tower-rs/tower/issues/636)规划。

2023 年展望：
- 发布 1.0
- 改进中间件
- 集成 http/3
- h2 性能改进

更多请看[hyper 官方文章](https://seanmonstar.com/post/706802392260362240/hyper-ish-2022-in-review)。


### justjson: 高效 json 解析器

[justjson](https://github.com/khonsulabs/justjson/) 是一个高效的 json 解析器，相比 serde-json 避免了更多的额外的分配。

作者写了这篇文章介绍他[为什么不喜欢用 serde-json](https://ecton.dev/rust-json-ecosystem/)，也介绍了 justjson 的设计理念。也可以参考[reddit 相关讨论](https://www.reddit.com/r/rust/comments/10gfvh2/surprises_in_the_rust_json_ecosystem/)。

### rune : 实验性 Emacs lisp 解释器

[rune](https://github.com/CeleritasCelery/rune/) 是 Rust 实现的实验性 Emacs lisp 解释器，是作者的一个学习型项目。作者写了详细的[设计文档](https://github.com/CeleritasCelery/rune/blob/master/design.org)和[设计博客](https://coredumped.dev/2023/01/17/design-of-emacs-in-rust/)。

> 另外一个同名的 Rust 项目是 [rune-rs/rune](https://github.com/rune-rs/rune)，是 Rust 实现的动态语言。

### astra: 构建于 hyper 之上的阻塞式 HTTP 服务器

[astra](https://github.com/ibraheemdev/astra) 是构建于 hyper 之上的阻塞式 HTTP 服务器。

在当下异步成为 Rust 网络 I/O 默认选择的今天，为什么还需要阻塞式 HTTP 服务器？

虽然异步对于大量应用程序非常有用，但它确实增加了一定程度的复杂性。正因为如此，阻塞式 crate 的受众仍然很大。然而生态系统中严重缺乏阻塞式HTTP服务器。因此 astra 应运而生。

但是 hyper 是在 tokio 异步运行时基础上构建的，而 astra 是绕过了 tokio，基于 mio 自己实现一个 Reactor 线程池。

> 据说，Hyper 1.0 正计划避免依赖 tokio 的 trait。

更多参考[astra 介绍文章](https://ibraheem.ca/posts/announcing-astra/)。


###  Tantivy 0.19 已发布

Tantivy 是一个用 Rust ( [benchmarks](https://tantivy-search.github.io/bench/) )编写的高性能全文搜索引擎库。该库受到 Apache Lucene 的启发，作为构建搜索引擎的基础，分布式搜索引擎 [Quickwit](https://github.com/quickwit-oss/quickwit/) 基于 tantivy 构建。

tantivy 1.9 新功能：

- 添加 IP 地址字段类型，用于查询和存储 IP 地址。IP 地址字段类型能够处理IPv4和IPv6，还支持复杂查询。
- 文档存储改进，现在使用一个单独的线程来压缩块存储。在 Quickwit 中主要摄取大量非结构化日志。压缩级别在这里对于降低 S3 的成本至关重要。
- 扩展聚合支持
- 扩展查询语言

### asteroids-genetic：交互式游戏AI训练模拟器

[asteroids-genetic](https://github.com/sparshg/asteroids-genetic) 使用神经网络和使用自然选择规则进化的遗传算法训练游戏AI 玩小行星游戏。

（测试分支包含不完整的 flappy birds AI）。

### Coerce-rs：异步（async/await）Actor 运行时和分布式系统框架

[coerce-rs](https://github.com/LeonHartley/Coerce-rs)  可以让开发者方便地实现基于Actor模型的分布式系统。

### syntactic-for: 句法 "for" 循环宏

[syntactic-for](https://github.com/xlambein/syntactic-for)提供了 `syntactic_for!` 宏，可以帮助开发者消除一些重复。

比如：

```rust
let array = b"oh my, I am getting summed!";
let mut acc = 0u32;
let mut i = 0;
while i <= array.len()-4 {
    syntactic_for!{ offset in [ 0, 1, 2, 3 ] {$(
        acc += array[i + $offset] as u32;
    )*}}
    i += 4;
}
```
展开后：

```rust
fn main(){
    let array = b"oh my, I am getting summed!";
    let mut acc = 0u32;
    let mut i = 0;
    while i <= array.len() - 4 {
        acc += array[i + 0] as u32;
        acc += array[i + 1] as u32;
        acc += array[i + 2] as u32;
        acc += array[i + 3] as u32;
        i += 4;
    }
}
```