# Call TIS from Rust

## Barebones example of executing TIScript from Rust

Requires `sciter.dll` in the folder.

Intended as a solution for [this](https://sciter.com/forums/topic/call-tiscript-function-from-rust/) question.

```rust
#[macro_use] extern crate sciter;
use sciter::{HELEMENT, Element, Value};
use std::env;
struct EventHandler {}
impl sciter::EventHandler for EventHandler {
	fn document_complete(&mut self, root: HELEMENT, target: HELEMENT) {
		&Element::from(root).call_function("foo", &make_args!("Hello World!"));
	}
}
fn main() {
	let mut frame = sciter::Window::new();
	frame.event_handler(EventHandler { });
	let dir = env::current_dir().unwrap().as_path().display().to_string();
	let filename = format!("{}\\{}", dir, "index.htm");
	frame.load_file(&filename);
	frame.run_app();
}
```

```html
<html>
  <head>
    <script type="text/tiscript">
      function foo(x) { view.msgbox(x); }
    </script>
  </head>
  <body></body>
</html>
```