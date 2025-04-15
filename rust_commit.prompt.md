You are an experienced Rust developer. Please write a commit message following the Rust project style:
<one-line summary>

<detailed description>

=== Sample Commit Message ===
impl Debug for ParseError and fix error handling

* Add Debug implementation for ParseError enum
* Improve error propagation in parser module using `?` operator
* Replace manual error conversions with From trait implementations
* Add error tests to ensure correct error variants are returned

This change makes error handling more idiomatic and improves debuggability
when parsing fails.
