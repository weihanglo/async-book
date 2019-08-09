# `async`/`.await` 入門

`async/.await` 是 Rust 內建編寫非同步函式的工具，讓非同步函式寫起來像同步。
`async` 會將一塊程式碼轉換為一個實作 `Future` 特性的狀態機。相較於在同步的方法
中呼叫一個阻塞函式（blocking function）會阻塞整個執行緒，反之，被阻塞的
`Future` 會釋出執行緒的控制權，讓其他 `Future` 可以繼續運作。

使用 `async fn` 語法來建立一個非同步函式：

```rust
async fn do_something() { ... }
```

`async fn` 的返回值是一個 `Future`。要讓事情發生，`Future` 需要在一個執行器
（executor）上面運作。

```rust
{{#include ../../examples/01_04_async_await_primer/src/lib.rs:7:19}}
```

在一個 `async fn` 裡，可以使用 `.await` 來等待另一個實作 `Future` 特性的型別完
成任務，例如其他 `async fn` 的輸出。和 `block_on` 不同的事，`.await` 不會阻塞當
前的執行緒，取而代之的是非同步地等待這個 future 完成，若這個 future 當下不能有
所進展，也能允許其他任務繼續執行。

舉例來說，想像有三個 `async fn`：`learn_song`、`sing_song` 與 `dance`：

```rust
async fn learn_song() -> Song { ... }
async fn sing_song(song: Song) { ... }
async fn dance() { ... }
```

有個方法可以執行學習、唱歌和跳舞，就是分別阻塞每一個函式：

```rust
{{#include ../../examples/01_04_async_await_primer/src/lib.rs:32:36}}
```

然而，這樣並沒有達到最佳的效能，我們同時只做了一件事而已！很明顯地，我們需要在
唱歌之前學習歌曲，但能夠同時跳著舞有學習並唱歌。為了達成這個任務，我們建立兩個
獨立可並行執行的 `async fn`：

```rust
{{#include ../../examples/01_04_async_await_primer/src/lib.rs:44:66}}
```

這個範例中，學習歌曲必須發生在唱歌之前，但無論唱歌或學習都能與跳舞同時發生。如
果我們在 `learn_and_sing` 中使用 `block_on(learn_song())` 而不是
`learn_song().await`，整個執行緒在 `learn_song` 執行期間無法做其他事，這種情況
下要同時跳舞是不可能的任務。利用 `.await` 來等待 `learn_song` future，就會允許
其他任務在 `learn_song` 阻塞時接管當前的執行緒。這讓在同個執行緒下並行執行多個
future 變為可能。

現在，你學會了基礎的 `async/await`，讓我們來嘗嘗實戰範例。
