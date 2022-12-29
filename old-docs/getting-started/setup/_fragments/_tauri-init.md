import Tabs from '@theme/Tabs'
import TabItem from '@theme/TabItem'
import Command, { InstallTauriCli } from '@theme/Command'

At the heart of every Tauri app is a Rust binary that manages windows, the webview, and calls to the operating system through a Rust crate called `tauri`. This project is managed by [Cargo], the official package manager and general-purpose build tool for Rust.

Our Tauri CLI uses Cargo under the hood so you rarely need to interact with it directly. Cargo has many more useful features that are not exposed through our CLI, such as testing, linting, and formatting, so please refer to their [official docs][cargo commands] for more.

:::info Install Tauri CLI

If you haven't installed the Tauri CLI yet you can do so with one of the below commands. Aren't sure which to use? Check out the [FAQ entry].

<InstallTauriCli />

:::

To scaffold a minimal Rust project that is pre-configured to use Tauri, open a terminal and run the following command:

<Tabs groupId="package-manager">
<TabItem value="npm">

```shell
npm run tauri init
```

</TabItem>
<TabItem value="Yarn">

```shell
yarn tauri init
```

</TabItem>
<TabItem value="pnpm">

```shell
pnpm tauri init
```

</TabItem>
<TabItem value="Cargo">

```shell
cargo tauri init
```

</TabItem>
</Tabs>

It will walk you through a series of questions:

<ol>
  <li>
    <i>What is your app name?</i>
    <br />
    This will be the name of your final bundle and what the OS will call your
    app. You can use any name you want here.
  </li>
  <br />
  <li>
    <i>What should the window title be?</i>
    <br />
    This will be the title of the default main window. You can use any title you
    want here.
  </li>
  <br />
  <li>
    <i>
      Where are your web assets (HTML/CSS/JS) located relative to the{' '}
      <code>&lt;current dir&gt;/src-tauri/tauri.conf.json</code> file that will
      be created?
    </i>
    <br />
    This is the path that Tauri will load your frontend assets from when
    building for <b>production</b>.{' '}
    {props.destDirExplination && (
      <div dangerouslySetInnerHTML={props.destDirExplination} />
    )}
  </li>
  <br />
  <li>
    <i>What is the URL of your dev server?</i>
    <br />
    This can be either a URL or a file path that Tauri will load during{' '}
    <b>development</b>.{' '}
    {props.devPathExplination && (
      <div dangerouslySetInnerHTML={props.devPathExplination} />
    )}
  </li>
  <br />
  <li>
    <i>What is your frontend dev command?</i>
    <br />
    This is the command used to start your frontend dev server.{' '}
    {props.beforeDevCommandExplination && (
      <div dangerouslySetInnerHTML={props.beforeDevCommandExplination} />
    )}
  </li>
  <br />
  <li>
    <i>What is your frontend build command?</i>
    <br />
    This is the command to build your frontend files.{' '}
    {props.beforeBuildCommandExplination && (
      <div dangerouslySetInnerHTML={props.beforeBuildCommandExplination} />
    )}
  </li>
</ol>

:::info

If you're familiar with Rust, you will notice that `tauri init` looks and works a lot like `cargo init`. You can just use `cargo init` and add the necessary Tauri dependencies if you prefer a fully manual setup.

:::

The `tauri init` command generates a folder called `src-tauri`. It's a convention for Tauri apps to place all core-related files into this folder. Let's quickly run through the contents of this folder:

- **`Cargo.toml`**  
  Cargo's manifest file. You can declare Rust crates your app depends on, metadata about your app, and much more. For the full reference see [Cargo's Manifest Format][manifest-format].

- **`tauri.conf.json`**  
  This file lets you configure and customize aspects of your Tauri application from the name of your app to the list of allowed APIs. See [Tauri's API Configuration][api config] for the full list of supported options and in-depth explanations for each.

- **`src/main.rs`**  
  This is the entry point to your Rust program and the place where we bootstrap into Tauri. You will find two sections in it:

  ```rust title=src/main.rs
   #![cfg_attr(
      all(not(debug_assertions), target_os = "windows"),
      windows_subsystem = "windows"
   )]

  fn main() {
  tauri::Builder::default()
     .run(tauri::generate_context!())
     .expect("error while running tauri application");
  }
  ```

  The line beginning with [`cfg! macro`][cfg macro] serves just one purpose: it disables the command prompt window that would normally pop up on Windows if you run a bundled app. If you're on Windows, try to comment it out and see what happens.

  The `main` function is the entry point and the first function that gets invoked when your program runs.

- **`icons`**  
  Chances are you want a snazzy icon for your app! To get you going quickly, we included a set of default icons. You should switch these out before publishing your application. Learn more about the various icon formats in Tauri's [icons feature guide][icons].

[manifest-format]: https://doc.rust-lang.org/cargo/reference/manifest.html
[cfg macro]: https://doc.rust-lang.org/rust-by-example/attribute/cfg.html
[api config]: ../../../../api/config.md
[icons]: ../../../features/icons.md
[prerequisites]: ../../prerequisites.md
[cargo]: https://doc.rust-lang.org/cargo/
[cargo commands]: https://doc.rust-lang.org/cargo/commands/index.html
[faq entry]: ../../../faq.md#node-or-cargo