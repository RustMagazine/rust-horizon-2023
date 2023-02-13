# Rust 视界周刊 Week 6 ｜ 黑莓公司宣布：汽车行业需要 Rust
---


## 社区热点

### 谁“拥有” Rust 语言？

[Reddit 有人发帖发问：谁“拥有” Rust 语言？](https://www.reddit.com/r/rust/comments/10z6qnu/who_owns_rust/) 。因为他最近听说 Google 打算在 Golang pipline 中内置 telemetry， 这是他不可接受的，他想寻找一门完全开源不受某个公司控制的编程语言。

Rust 语言的创造者 Graydon 亲自回复如下。

目前和 Rust 语言相关有两个组织：

- Rust基金会，这是一家非营利性综合（特拉华州）公司，拥有章程、员工和正常的合法存在。它拥有商标和域名，在需要时充当法律和行政联络点，并且我认为对基础设施（crates.io、CI 等）的运营和资金负责。该基金会的成员几乎都是捐钱（有时捐人）以推进其使命的企业赞助商。这里涉及相当广泛的公司：微软、谷歌、亚马逊、Meta、华为等。
- Rust项目是一个非法律实体的人员集合（一些受雇于公司，一些是志愿者），他们自组织成团队（主要选择他们自己未来的成员）通过互联网协作，每个人都有自己的职责范围。他们有自己的流程和治理系统（在上一代流程遇到一些问题后，他们目前正在进行相当大的改革）。该项目实际上负责所有技术决策，包括语言的内容、工具和流程的工作方式、文档中的内容、更改的功能以及更改时间等。将做出诸如“我们应该将telemetry放入编译器”之类的决定，而不是基础。

Rust项目不接受基金会的指导，基金会也不接受项目的指导。它们并行存在，并且是大部分独立的实体，具有大部分独立的责任领域，尽管有几个人在两者中都扮演着角色。到目前为止，据我所知，他们之间没有发生任何严重的冲突，我也不太清楚如果有会发生什么。但基金会的任务是支持该项目，所以如果他们发现自己发生冲突，情况会有些奇怪。

### 有人认为 stc 这个项目不道德，对于 typescript 社区是毒药，你怎么看？

stc 是用 Rust 编写的 TypeScript 类型检查器 ，目标是性能更好，使用 Rust 实现。 该 [stc 的 issue](https://github.com/dudykr/stc/issues/647) 的作者认为，stc如果做好了，会因此而获得控制 TS 规范的能力。并且让stc作者转而去实现另外一个 类型化js的实现。

stc作者回复：“typescript 本身没有规范，所以我无法控制它。如果 TypeScript 有规范，情况会有所不同，但事实并非如此，如果有，tsc它本身就是规范。”

我的观点： 人们肯定期望更好的工具，并且它会带走流量，这是自然规律。 这个 issues 作者太奇葩了，人们不能阻止竞争对手的发展来迎合自己的用户，这才是不道德的。

### Reddit 讨论：`#[must_use]`没有成为函数的默认值违背了“Rust 具有最安全的默认值”的原则

[reddit 一篇题为 《`#[must_use]`没有成为函数的默认值违背了“Rust 具有最安全的默认值”的原则》](https://www.reddit.com/r/rust/comments/10wz3am/must_use_not_being_the_default_on_functions_goes/)的文章，引起了社区的讨论。

作者认为，Rust 犯了与 C++17 的`[[nodiscard]]`属性相同的错误，他认为：

1. 大多数返回值的函数，无论它们是否有副作用，都应该以某种方式使用它们的返回值
2. 一些返回值和有副作用的函数可能在不检查返回值的情况下被正确使用
3. 当函数的返回值未被使用时，编译器发出警告是有益的，因为在大多数情况下这是一个错误
4. 因此，作为一个负责任的程序员，除了我有意决定丢弃返回值不是错误的地方外，我应该把它放在`#[must_use]`下

reddit 下面一些回复摘要：

- “`F#` 中的这是默认值。它之所以有效，是因为 `F#` 主要是一种函数式语言，因此您通常创建纯函数，结果应该用于进一步的计算。然而，在 `F#` 中编写命令式代码时，您经常需要使用`ignore`。考虑到纯函数很少而且介于两者之间，这将成为 Rust 的一大麻烦”。
- “`#[must_use]`与安全无关，它是为了明确某些返回值（少数）值得处理。如果它成为默认值，它就失去了意义。更糟糕的是，如果成本高到人们禁用 lint，带来的结果也许更糟糕。”
- “这其实是对 Rust 的普遍误解！Rust 并不努力成为“默认情况下完全安全”，Rust 并没有试图实现“完美的安全性”，但它试图让您的程序在一个由人类编写程序的世界中尽可能安全可靠。”

更多的讨论内容可以参考原帖。


## 应用实践

### 黑莓公司宣布：汽车行业需要 Rust

[blackberry 官方平台发文宣称](https://blogs.blackberry.com/en/2023/02/this-is-the-kind-of-rust-the-automotive-industry-needs)，软件定义汽车(SDV) 领域需要 Rust 语言带来更多安全优势。

> BlackBerry和 [Elektrobit](https://www.elektrobit.com/) 最近汇集了他们的专业知识，共同支持 Rust，使开发人员能够构建安全、可靠和高效的汽车软件。Elektrobit 和BlackBerry QNX作为合作伙伴有着悠久的历史，并且拥有强大的、经过生产验证的业绩记录。Elektrobit 在 Rust 项目上与 BlackBerry QNX 密切合作，贡献代码，确保代码质量，处理项目管理，并与 Rust 程序员互动。


### GitHub 用 Rust 重写搜索引擎

在[GitHub 新代码搜索背后的技术](https://github.blog/2023-02-06-the-technology-behind-githubs-new-code-search/)一文中，提到 Github 目前的代码搜索引擎基于 Rust 实现。具体来说，是用 Rust 完全从零开始构建一个专门用于代码搜索领域的引擎，代号为 Blackbird。它创建并增量维护一个由 Git blob 对象 ID 分片的代码搜索索引，最终 Blackbird 满足了大家的性能目标：速度非常快，索引也非常紧凑，重量约为（去重）语料库大小的 1/3。

在 GitHub CodeSearch 首页上，展示的代码图片是 `rust-lang/rust` 项目中的 `wtf8.rs` 模块。[WTF-8](https://simonsapin.github.io/wtf-8/)（Wobbly Transformation Format − 8-bit）是UTF-8的超集。


## 生态看点

### gptcommit ： 让 GPT-3 辅助编写git commit信息

最近一周 ChatGPT 爆火，微软也已经宣布将 ChatGPT 集成到最新的 Bing 搜索引擎中。ChatGPT 确实令人惊艳，不同于以往的人工智障，它好像是可以作为我们日常工作中的一个助手了。

[gptcommit](https://github.com/zurawiki/gptcommit) 就是利用 OpenAI 的模型来辅助帮助开发者来自动编写 git commit 信息。

gptcommit 背后原理简单描述：

- 并行地对每个提交的文件通过 OpenAI 的模型提取要点，生成要点列表
- 指示模型为提交的更改生成一个简短的单行标题列表
- 将标题列表总结为一个总标题

类似于下面条目：

```rust
[title]

- [summary point 1]
- [summary point 2...]

[/changed/file A]
- [file summary point 1]
- [file summary point 2...]
[/changed/file B...]
- [file summary point 1...]
```

说明，使用 gptcommit 你需要一个 OpenAI API 密钥（默认模型是 GPT-3）。更多信息可以参考[gptcommit作者的博客](https://zura.wiki/post/never-write-a-commit-message-again-with-the-help-of-gpt-3/)。

### sdkman-cli-native：sdkman 命令行工具使用 Rust 实现

SDKMAN 是一款管理多版本 SDK 的工具，可以实现在多个版本间的快速切换。

[sdkman-cli-native](https://github.com/sdkman/sdkman-cli-native) 现在使用 Rust 实现。


### Meilisearch 发布 1.0 版本

[Meilisearch](https://github.com/meilisearch/meilisearch) 是一个轻量的搜索引擎，可以轻松集成到你的应用和网站中。

近日，其发布了第一个稳定版本 1.0，完善了中文和韩文的语言支持，提高了索引和搜索的速度，提供了一键升级的特性，详细内容可以参考[Meilisearch 发布日志](https://blog.meilisearch.com/whats-new-in-v1-0/)。

### Qdrant: 用于下一代 AI 应用程序的矢量搜索引擎和数据库

[Qdrant](https://github.com/qdrant/qdrant) 是用于下一代 AI 应用程序的矢量搜索引擎和数据库，目前发布 1.0 版本。Qdrant 可用于语义文本搜索、相似图片搜索和电商商品分类等领域。

### s3s： S3服务适配器

这个实验项目旨在提供一个符合人体工程学的适配器，用于构建与 S3 兼容的服务。

- [s3s](https://github.com/Nugine/s3s)，以通用的 hyper 服务形式实现 Amazon S3 REST API。 S3 兼容的服务可以专注于 S3 API 本身而不必关心 HTTP 层。
- [s3s-aws]()，提供有用的类型并与 aws-sdk-s3 集成。
- s3s-fs，实现了基于文件系统的 S3 API，作为示例实现。它专为集成测试而设计，可用于模拟 S3 客户端。它还提供了一个用于调试的二进制文件。

### timeblok: 声明式日历语言

[timeblok](https://github.com/JettChenT/timeblok) 一个简单的声明式 DSL，它结合了纯文本的多功能性和可扩展性以及数字日历的便利性，可用于个人日历规划。

比如可以使用以下内容来描述工作日的一天安排：

```rust
2023-1-1
7:30am wake up & eat beakfast
8am~11:30 work on TimeBlok
- Write Technical Documentation
2pm~6pm Study for exams
8pm~10pm Reading
- Finish an entire book
```
timeblok 将上述内容“编译”为 .ics 文件，这是一种数字日历文件格式，大多数日历应用程序都支持这种格式，人类几乎无法阅读，但可以直接导入到日历中。

### stackdump: 用于嵌入式栈跟踪的工具

[stackdump](https://github.com/tweedegolf/stackdump) 是基于 DWARF 格式用于栈转存和栈跟踪的调试库，目前仅支持 Cortex M。

更多信息可以参考[stackdump 官方的详细文章](https://tweedegolf.nl/en/blog/82/crash-and-now-what)。

### man-in-the-middle-proxy: Rust 实现的中间人代理工具

[man-in-the-middle-proxy](https://github.com/emanuele-em/man-in-the-middle-proxy)是一个中间人代理，旨在提供网络流量可见性。目前，它同时显示 HTTP 和 HTTPS 请求和响应，但未来的目标是允许为更高级的用例操纵流量。该工具是对 [mitmproxy](https://mitmproxy.org/) （Python 实现）的 Rust 实现，但项目还处于早期。

GitHub 其他 Rust 实现: [mitmproxy_rs](https://github.com/mitmproxy/mitmproxy_rs) 和 [hudsucker](https://github.com/omjadas/hudsucker) 。


