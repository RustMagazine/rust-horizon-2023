# Rust 1.67 稳定版语言特性一瞥

[Rust 1.67 稳定版发布了](https://github.com/rust-lang/rust/blob/master/RELEASES.md#version-1670-2023-01-26) 。

以下摘录三个值得关注的语言特性。

## 让 `Sized` trait 可共归纳（Coinductive）

在 1.67 之前，下面代码编译不通过：

```rust
struct Node<C: Trait<Self>>(C::Assoc);

trait Trait<T> {
    type Assoc;
}

impl<T> Trait<T> for Vec<()> {
    type Assoc = Vec<T>;
}

fn main() {
    let _ = Node::<Vec<()>>(Vec::new());
    // <Vec<()> as Trait<Node<Vec<()>>>>  
    // Node( <Vec<()> as Trait<Node<Vec<()>>>>::Assoc )
}
```

报错如下：

```rust
error[E0275]: overflow evaluating the requirement `Node<Vec<()>>: Sized`
  --> src/main.rs:12:13
   |
12 |     let _ = Node::<Vec<()>>(Vec::new());
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
note: required because of the requirements on the impl of `Trait<Node<Vec<()>>>` for `Vec<()>`
  --> src/main.rs:7:9
   |
7  | impl<T> Trait<T> for Vec<()> {
   |         ^^^^^^^^     ^^^^^^^

For more information about this error, try `rustc --explain E0275`.
```

这个[错误码`E0275`](https://doc.rust-lang.org/error_codes/E0275.html)表示在在解析某些类型限定时存在无限递归。具体到上面代码，是计算`Node<Vec<()>>: Sized` 的 `Sized` 限定时产生了无限递归。

上面代码 `Sized` 的正常推理逻辑是：

1. 如果 `<Vec<()> as Trait<Node<Vec<()>>>::Assoc` 是 `Sized`，则要求 `Node<Vec<()>>` 是 `Sized` 。
2. 这就要求 `Vec<Node<Vec<()>>` 是 `Sized`，因此再次需要 `Node<Vec<()>` 是 `Sized`。

这就造成了递归。在 1.67 stable 之后，[`Sized` 现在是共归纳（coinductive）](https://github.com/rust-lang/rust/pull/100386/)，就不会再次报错。

> 说明：共归纳（coinductive）是类型理论里的术语，用于处理递归类型。

###  `#[must_use]` 现在支持 `async fn`

`#[must_use]` 作用于 `async fn` 则会影响到 `Future::Output`，意味着下面异步函数：

```rust
#[must_use]
async fn bar() -> u32 { 0 }

async fn caller() {
    bar().await;
}
```

在未使用 `Future::output` 时会输出下面警告：

```rust
warning: unused output of future returned by `bar` that must be used
 --> src/lib.rs:5:5
  |
5 |     bar().await;
  |     ^^^^^^^^^^^
  |
  = note: `#[warn(unused_must_use)]` on by default
```

### `std::sync::mpsc` 合并了 `crossbeam_channel` 的 mpsc 

`std::sync::mpsc`现在被[切换为基于`crossbeam-channel`](https://github.com/rust-lang/rust/pull/93563/)。这次合并是为了解决 `std::sync::mpsc` 多年未解决的 Bug，以及提高了实现的性能和可维护性，并不包含任何 API 更改。

所以，如果你需要使用 `mpmc` channel，则还需要使用 `crossbeam-channel` 第三方库。使用 `mpsc` 的话，则用标准库的就可以了。




