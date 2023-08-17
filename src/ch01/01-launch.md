# Launch the game

- Install Rust: [Rust official guide](https://www.rust-lang.org/learn/get-started), [Bevy Book installing rust and OS dependencies](https://bevyengine.org/learn/book/getting-started/setup/#rust-setup)
- Start a new project:
  ```bash
  cargo new learn_bevy_game
  cd learn_bevy_game
  ```
- Add Bevy as dependency (with a link-time optimization):
  ```bash
  cargo add bevy --features dynamic_linking
  ```
- Apply an optimization for debugging experience: add the following lines to `Cargo.toml`
  ```toml
  [profile.dev.package."*"]
  opt-level = 3
  ```
- Test the game (which is just a hello world program for now): **`ch01/step-1`**
  ```bash
  cargo run # this could take some time; later builds would be a lot faster
  ```
- Read more about the optimizations and building commands: [bevy book](https://bevyengine.org/learn/book/getting-started/setup/), [unofficial bevy cheatbook](https://bevy-cheatbook.github.io/pitfalls/performance.html)
