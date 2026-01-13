# Typestateパターン

状態をコンパイル時に型で表現し、不正な状態遷移を防ぐ。

## 基本形

```rust
// 状態を表す型
pub struct Draft;
pub struct Published;
pub struct Archived;

// 状態をジェネリクスで持つ
pub struct Article<State> {
    title: String,
    content: String,
    _state: std::marker::PhantomData<State>,
}

impl Article<Draft> {
    pub fn new(title: String) -> Self {
        Self {
            title,
            content: String::new(),
            _state: std::marker::PhantomData,
        }
    }

    pub fn set_content(&mut self, content: String) {
        self.content = content;
    }

    pub fn publish(self) -> Article<Published> {
        Article {
            title: self.title,
            content: self.content,
            _state: std::marker::PhantomData,
        }
    }
}

impl Article<Published> {
    pub fn archive(self) -> Article<Archived> {
        Article {
            title: self.title,
            content: self.content,
            _state: std::marker::PhantomData,
        }
    }
}

impl Article<Archived> {
    pub fn restore(self) -> Article<Draft> {
        Article {
            title: self.title,
            content: self.content,
            _state: std::marker::PhantomData,
        }
    }
}
```

## 使用例

```rust
let mut article = Article::new("Hello".to_string());
article.set_content("World".to_string());

// Draft -> Published
let published = article.publish();

// Published -> Archived
let archived = published.archive();

// Archived -> Draft
let draft = archived.restore();

// コンパイルエラー: DraftにはarchiveがないS
// article.archive();
```

## HTTPリクエストビルダー

```rust
pub struct NoUrl;
pub struct HasUrl;
pub struct NoMethod;
pub struct HasMethod;

pub struct RequestBuilder<Url, Method> {
    url: Option<String>,
    method: Option<String>,
    headers: Vec<(String, String)>,
    _url: std::marker::PhantomData<Url>,
    _method: std::marker::PhantomData<Method>,
}

impl RequestBuilder<NoUrl, NoMethod> {
    pub fn new() -> Self {
        Self {
            url: None,
            method: None,
            headers: vec![],
            _url: std::marker::PhantomData,
            _method: std::marker::PhantomData,
        }
    }
}

impl<M> RequestBuilder<NoUrl, M> {
    pub fn url(self, url: impl Into<String>) -> RequestBuilder<HasUrl, M> {
        RequestBuilder {
            url: Some(url.into()),
            method: self.method,
            headers: self.headers,
            _url: std::marker::PhantomData,
            _method: std::marker::PhantomData,
        }
    }
}

impl<U> RequestBuilder<U, NoMethod> {
    pub fn method(self, method: impl Into<String>) -> RequestBuilder<U, HasMethod> {
        RequestBuilder {
            url: self.url,
            method: Some(method.into()),
            headers: self.headers,
            _url: std::marker::PhantomData,
            _method: std::marker::PhantomData,
        }
    }
}

// URLとMethodが設定されている場合のみsend可能
impl RequestBuilder<HasUrl, HasMethod> {
    pub fn send(self) -> Response {
        // ...
    }
}
```

## 状態遷移のトレイト化

```rust
pub trait State {}
pub trait CanEdit: State {}
pub trait CanPublish: State {}

pub struct Draft;
pub struct Review;
pub struct Published;

impl State for Draft {}
impl State for Review {}
impl State for Published {}

impl CanEdit for Draft {}
impl CanPublish for Review {}

pub struct Document<S: State> {
    content: String,
    _state: std::marker::PhantomData<S>,
}

impl<S: CanEdit> Document<S> {
    pub fn edit(&mut self, content: String) {
        self.content = content;
    }
}

impl<S: CanPublish> Document<S> {
    pub fn publish(self) -> Document<Published> {
        Document {
            content: self.content,
            _state: std::marker::PhantomData,
        }
    }
}
```
