# Git Testament

![BSD 3 Clause](https://img.shields.io/github/license/kinnison/git-testament.svg)
![Main build status](https://github.com/kinnison/git-testament/workflows/main/badge.svg)
![Latest docs](https://docs.rs/git-testament/badge.svg)
![Crates.IO](https://img.shields.io/crates/v/git-testament.svg)

`git-testament` is a library to embed a testament as to the state of a git
working tree during the build of a Rust program. It uses the power of procedural
macros to embed commit, tag, and working-tree-state information into your program
when it is built. This can then be used to report version information.

```rust
use git_testament::{git_testament, render_testament};

git_testament!(TESTAMENT);

fn main() {
    println!("My version information: {}", render_testament!(TESTAMENT));
}
```

## Reproducible builds

In the case that your build is not being done from a Git repository, you still
want your testament to be useful to your users.  Reproducibility of the binary
is critical in that case.  The [Reproducible Builds][reprobuild] team have defined
a mechanism for this known as [`SOURCE_DATE_EPOCH`][sde] which is an environment
variable which can be set to ensure the build date is fixed for reproducibilty
reasons.  If you have no repo (or a repo but no commit) then `git_testament!()`
will use the [`SOURCE_DATE_EPOCH`][sde] environment variable (if present and parseable
as a number of seconds since the UNIX epoch) to override `now`.

[reprobuild]: https://reproducible-builds.org
[sde]: https://reproducible-builds.org/docs/source-date-epoch/

## Use in `no_std` scenarios

If you turn on the `git-testament/no-std` feature then the crate will not link to
anything in the standard library.  You can still generate a `GitTestament` struct
though it'll be less easy to work with.  Instead it'd be recommended to use the
`git_testament_macros!()` macro instead which provides a set of macros which produce
string constants to use.  This is less flexible/capable but can sometimes be easier
to work with in these kinds of situations.
