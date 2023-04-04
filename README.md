### 用SQL查询CSV
将sql的ast转换成polars的ast,使polars具有sql查询的能力
并拓展数据源可以为网络资源(支持http下载啦)

### example
```rust
use anyhow::Result;
use queryer::query;

#[tokio::main]
async fn main() -> Result<()> {
    tracing_subscriber::fmt::init();

    let url = "https://raw.githubusercontent.com/owid/covid-19-data/master/public/data/latest/owid-covid-latest.csv";

    // 使用 sql 从 URL 里获取数据
    let sql = format!(
        "SELECT location name, total_cases, new_cases, total_deaths, new_deaths \
        FROM {} where new_deaths >= 50 ORDER BY new_deaths DESC LIMIT 2 OFFSET 1",
        url
    );
    let df1 = query(sql).await?;
    println!("{:?}", df1);

    Ok(())
}
```

```shell
cargo run --example covid
```