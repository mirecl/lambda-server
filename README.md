### PROPOSAL API (`lambda_server`)

```rust
use anyhow::Result;
use lambda_server::{Http, Context};
use serde::{Serialize, Deserialize};
use validator::{Validate, ValidationError};

#[derive(Deserialize, Debug, Validate)]
struct Request {
    #[validate(range(min = 1))]
    pub price: i32,
    #[validate(range(min = 1))]
    pub quantity: i32,
}

#[derive(Serialize, Debug)]
struct Response {
    pub amount: i32,
}

fn handler(ctx: Context, req: Request) -> Result<Response> {
    req.validate()?;

    let amount = req.price * req.quantity;
    Ok(Response{ amount })
}

#[tokio::main]
async fn main() -> Result<()> {
    Http::run(handler).await?;
    Ok(())
}
```
