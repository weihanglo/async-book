## 非同步 Rust 的現況

隨著時間推移，非同步 Rust 生態系歷經了許多演進，所以不易了解什麼工具可用，什麼函式庫適合投資，或是什麼文件可以閱讀。幸虧，標準函式庫中的 `Future` trait 和 `async/await` 最近穩定了。整個生態系處於遷移至新穩定 API 的過程中，此後流失會顯著降低。

然而，目前生態系仍正在快速開發中，且尚未打磨非同步 Rust 的體驗。大多數函式庫依然使用 0.1 `futures` crate 的定義，這代表開發者經常要使用 0.3 `futures` 的`compat` 功能，才能讓 0.1 版相容地運作。`async/await` 語言特性還非常新。重要的擴充功能，如在 trait method 中的 `async fn` 語法仍未實作，且當前的編譯器錯誤訊息非常難以解析。

Rust 正在通往支援最高效能與人因工程的非同步程式設計的道路上，如果你不懼怕掉進坑裡，來好好享受 Rust 非同步程式設計的世界吧！
