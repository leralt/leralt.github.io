## iter collect()
`collect()`可以指定收集完成的类型，如`collect::<String>()`或者`collect::<Vec<String>>()`，但是如果不指定，Rust则会根据一些信息推断类型，如返回值
```rust
// "hello" -> "Hello"
pub fn capitalize_first(input: &str) -> String {
    let mut c = input.chars();
    match c.next() {
        None => String::new(),
        Some(first) => first.to_uppercase().collect::<String>() + c.as_str(),
    }
}

// ["hello", "world"] -> ["Hello", "World"]
pub fn capitalize_words_vector(words: &[&str]) -> Vec<String> {
    words.iter().map(|word| capitalize_first(word)).collect()
}

// ["hello", " ", "world"] -> "Hello World"
pub fn capitalize_words_string(words: &[&str]) -> String {
    words.iter().map(|word| capitalize_first(word)).collect()
}
```
正如这段代码，相同的代码可以给出不同的返回值，确实很奇妙
## iter  fold()
```rust
pub fn factorial(n: u64) -> u64 {
    // match n {
    //     0 | 1 => 1,
    //     _ => n * factorial(n - 1),
    // }
    (1..=n).fold(1, |acc, x| acc * x)
}
```

## operator
`..=`类似与`..`，只不过前者包含右边，后者是左开右闭

## 时间
忘了过去是好事吗？不用再花时间的精力去怀念、追忆