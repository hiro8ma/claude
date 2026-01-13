# 並行処理

## スレッド

```rust
use std::thread;

// スレッド生成
let handle = thread::spawn(|| {
    println!("Hello from thread");
});

handle.join().unwrap();

// データの移動
let data = vec![1, 2, 3];
let handle = thread::spawn(move || {
    println!("{:?}", data);
});
```

## Send と Sync

- `Send`: 所有権をスレッド間で転送可能
- `Sync`: 複数スレッドから参照可能

```rust
// Rc は Send でも Sync でもない
use std::rc::Rc;
// let rc = Rc::new(5);
// thread::spawn(move || { rc });  // エラー

// Arc は Send かつ Sync
use std::sync::Arc;
let arc = Arc::new(5);
thread::spawn(move || { arc });  // OK
```

## Mutex

排他的アクセス。

```rust
use std::sync::{Arc, Mutex};

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let counter = Arc::clone(&counter);
    let handle = thread::spawn(move || {
        let mut num = counter.lock().unwrap();
        *num += 1;
    });
    handles.push(handle);
}

for handle in handles {
    handle.join().unwrap();
}
```

## RwLock

読み取り多数、書き込み少数の場合。

```rust
use std::sync::RwLock;

let lock = RwLock::new(5);

// 複数の読み取り可能
{
    let r1 = lock.read().unwrap();
    let r2 = lock.read().unwrap();
    println!("{}, {}", *r1, *r2);
}

// 排他的書き込み
{
    let mut w = lock.write().unwrap();
    *w += 1;
}
```

## チャネル

```rust
use std::sync::mpsc;

// 単一プロデューサ・複数コンシューマ
let (tx, rx) = mpsc::channel();

thread::spawn(move || {
    tx.send("hello").unwrap();
});

let received = rx.recv().unwrap();

// 複数プロデューサ
let (tx, rx) = mpsc::channel();
let tx2 = tx.clone();

thread::spawn(move || { tx.send(1).unwrap(); });
thread::spawn(move || { tx2.send(2).unwrap(); });
```

## async/await

```rust
use tokio;

async fn fetch_data(url: &str) -> Result<String, Error> {
    let response = reqwest::get(url).await?;
    response.text().await
}

#[tokio::main]
async fn main() {
    let data = fetch_data("https://example.com").await.unwrap();
    println!("{}", data);
}

// 並行実行
async fn fetch_all() {
    let (a, b) = tokio::join!(
        fetch_data("https://a.com"),
        fetch_data("https://b.com"),
    );
}
```

## Atomics

ロックフリーな操作。

```rust
use std::sync::atomic::{AtomicUsize, Ordering};

static COUNTER: AtomicUsize = AtomicUsize::new(0);

fn increment() {
    COUNTER.fetch_add(1, Ordering::SeqCst);
}

fn get() -> usize {
    COUNTER.load(Ordering::SeqCst)
}
```

## デッドロック回避

```rust
// 常に同じ順序でロックを取得
fn transfer(from: &Mutex<Account>, to: &Mutex<Account>, amount: u64) {
    // アドレスでソートして一貫した順序を保証
    let (first, second) = if (from as *const _) < (to as *const _) {
        (from, to)
    } else {
        (to, from)
    };

    let mut first = first.lock().unwrap();
    let mut second = second.lock().unwrap();
    // 転送処理...
}
```
