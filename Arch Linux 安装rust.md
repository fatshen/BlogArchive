# Arch Linux 安装rust

## 0. 参考

[Rust Toolchain 反向代理使用帮助](https://mirrors.ustc.edu.cn/help/rust-static.html)

## 1. 安装

安装rustup和toolchain

```shell
yaourt -S rustup
rustup install stable
rustup default stable
```

```shell
# 如果要安装nightly
rustup install nightly
rustup default nightly
```



## 2. 测试

```rust
 fn main() {
     println!("Hello, World!");
 }
```



## 3. 补全需要

```shell
rustup component add rust-src
cargo install racer
cargo install rustfmt
```



## 4. 设置env 

以zsh为例，设置.zshenv为以下内容，可以帮助补全和设置rust为使用国内的源。

```shell
RUST_SRC_PATH=(~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src)
RUSTUP_DIST_SERVER=(https://mirrors.ustc.edu.cn/rust-static)
RUSTUP_UPDATE_ROOT=(https://mirrors.ustc.edu.cn/rust-static/rustup)
```

```shell
# 如果要安装nightly
RUST_SRC_PATH=(~/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src)
RUSTUP_DIST_SERVER=(https://mirrors.ustc.edu.cn/rust-static)
RUSTUP_UPDATE_ROOT=(https://mirrors.ustc.edu.cn/rust-static/rustup)
```

