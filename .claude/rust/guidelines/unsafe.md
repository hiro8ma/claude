# Unsafe Rust

## unsafeが必要なケース

1. 生ポインタのデリファレンス
2. unsafe関数の呼び出し
3. 可変静的変数のアクセス
4. unsafeトレイトの実装
5. ユニオンフィールドへのアクセス

```rust
// 生ポインタ
let mut num = 5;
let r1 = &num as *const i32;
let r2 = &mut num as *mut i32;

unsafe {
    println!("r1: {}", *r1);
    *r2 = 10;
}
```

## unsafeの原則

### 最小化

unsafeブロックは必要最小限に。

```rust
// Bad - 広すぎるunsafe
unsafe {
    let ptr = get_ptr();
    let value = *ptr;
    let result = process(value);  // safeなコードも含む
    save(result);
}

// Good - 必要な部分のみ
let ptr = get_ptr();
let value = unsafe { *ptr };
let result = process(value);
save(result);
```

### 安全な抽象化

unsafeを安全なインターフェースでラップ。

```rust
pub struct SafeBuffer {
    ptr: *mut u8,
    len: usize,
}

impl SafeBuffer {
    pub fn new(size: usize) -> Self {
        let ptr = unsafe { alloc(size) };
        Self { ptr, len: size }
    }

    pub fn get(&self, index: usize) -> Option<u8> {
        if index < self.len {
            Some(unsafe { *self.ptr.add(index) })
        } else {
            None
        }
    }
}

impl Drop for SafeBuffer {
    fn drop(&mut self) {
        unsafe { dealloc(self.ptr, self.len) };
    }
}
```

### ドキュメント化

unsafeコードには安全性の根拠をコメント。

```rust
/// # Safety
///
/// `ptr`は有効なメモリを指し、少なくとも`len`バイトが
/// 読み取り可能でなければならない。
pub unsafe fn read_bytes(ptr: *const u8, len: usize) -> Vec<u8> {
    // SAFETY: 呼び出し元が有効なポインタと長さを保証
    std::slice::from_raw_parts(ptr, len).to_vec()
}
```

## FFI（外部関数インターフェース）

```rust
// C関数の宣言
extern "C" {
    fn strlen(s: *const c_char) -> usize;
}

// 安全なラッパー
pub fn safe_strlen(s: &CStr) -> usize {
    unsafe { strlen(s.as_ptr()) }
}

// Rustから公開
#[no_mangle]
pub extern "C" fn rust_function(x: i32) -> i32 {
    x * 2
}
```

## MaybeUninit

未初期化メモリの安全な扱い。

```rust
use std::mem::MaybeUninit;

let mut array: [MaybeUninit<i32>; 10] = unsafe {
    MaybeUninit::uninit().assume_init()
};

for (i, elem) in array.iter_mut().enumerate() {
    elem.write(i as i32);
}

// すべて初期化後に変換
let array: [i32; 10] = unsafe {
    std::mem::transmute(array)
};
```
