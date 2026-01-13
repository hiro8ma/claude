# Builderパターン

## 基本形

```rust
#[derive(Debug)]
pub struct Server {
    host: String,
    port: u16,
    timeout: Option<Duration>,
    max_connections: usize,
}

#[derive(Default)]
pub struct ServerBuilder {
    host: String,
    port: u16,
    timeout: Option<Duration>,
    max_connections: usize,
}

impl ServerBuilder {
    pub fn new() -> Self {
        Self {
            host: "localhost".to_string(),
            port: 8080,
            timeout: None,
            max_connections: 100,
        }
    }

    pub fn host(mut self, host: impl Into<String>) -> Self {
        self.host = host.into();
        self
    }

    pub fn port(mut self, port: u16) -> Self {
        self.port = port;
        self
    }

    pub fn timeout(mut self, timeout: Duration) -> Self {
        self.timeout = Some(timeout);
        self
    }

    pub fn max_connections(mut self, max: usize) -> Self {
        self.max_connections = max;
        self
    }

    pub fn build(self) -> Server {
        Server {
            host: self.host,
            port: self.port,
            timeout: self.timeout,
            max_connections: self.max_connections,
        }
    }
}
```

## 使用例

```rust
let server = ServerBuilder::new()
    .host("0.0.0.0")
    .port(3000)
    .timeout(Duration::from_secs(30))
    .build();
```

## バリデーション付き

```rust
impl ServerBuilder {
    pub fn build(self) -> Result<Server, BuildError> {
        if self.port == 0 {
            return Err(BuildError::InvalidPort);
        }
        if self.host.is_empty() {
            return Err(BuildError::EmptyHost);
        }

        Ok(Server {
            host: self.host,
            port: self.port,
            timeout: self.timeout,
            max_connections: self.max_connections,
        })
    }
}
```

## deriveマクロ（typed-builder）

```rust
use typed_builder::TypedBuilder;

#[derive(TypedBuilder)]
pub struct Server {
    #[builder(default = "localhost".to_string())]
    host: String,

    #[builder(default = 8080)]
    port: u16,

    #[builder(default, setter(strip_option))]
    timeout: Option<Duration>,
}

// 使用
let server = Server::builder()
    .host("0.0.0.0")
    .port(3000)
    .build();
```

## 必須フィールドの強制

```rust
pub struct ServerBuilder<Host, Port> {
    host: Host,
    port: Port,
    timeout: Option<Duration>,
}

pub struct Missing;
pub struct Set<T>(T);

impl ServerBuilder<Missing, Missing> {
    pub fn new() -> Self {
        Self {
            host: Missing,
            port: Missing,
            timeout: None,
        }
    }
}

impl<P> ServerBuilder<Missing, P> {
    pub fn host(self, host: String) -> ServerBuilder<Set<String>, P> {
        ServerBuilder {
            host: Set(host),
            port: self.port,
            timeout: self.timeout,
        }
    }
}

impl<H> ServerBuilder<H, Missing> {
    pub fn port(self, port: u16) -> ServerBuilder<H, Set<u16>> {
        ServerBuilder {
            host: self.host,
            port: Set(port),
            timeout: self.timeout,
        }
    }
}

// buildはすべてSetの場合のみ呼び出し可能
impl ServerBuilder<Set<String>, Set<u16>> {
    pub fn build(self) -> Server {
        Server {
            host: self.host.0,
            port: self.port.0,
            timeout: self.timeout,
        }
    }
}
```
