# Spin `pluginify` - a Spin plugin to help build Spin plugins

This is a [Spin](https://developer.fermyon.com/spin/index) plugin that helps with the inner loop of Spin plugin development by creating the tar file and manifest for you.

## Prerequisites

* [Rust](https://www.rust-lang.org/tools/install)
* [Spin](https://developer.fermyon.com/spin/install)

## Installation

```bash
cargo run -r -- --install
```

## Usage

### Preparation

For your plugin, create a `spin-pluginify.toml` file with the following content:

```toml
name = "<PLUGIN-NAME>"
version = "0.1"
spin_compatibility = ">=0.7"
license = "Apache-2.0"
package = "<./PATH/TO/EXECUTABLE>"
```

You can find examples in this repo and in https://github.com/fermyon/spin-trigger-sqs.

### Updating

When you have a new build of your plugin ready:

* Run `spin pluginify`
  * It should create or update a `.tar.gz` file and a `<PLUGIN-NAME>.json` manifest
* Run `spin plugins install --file <PLUGIN-NAME>.json --yes`

If you want to save keystrokes, you can use `spin pluginify --install` to do both steps at once.

Your plugin should then be installed in Spin and ready to test.

### Testing

```mermaid
flowchart LR
    pluginify --> uninstall
    uninstall --> install
    install --> run
    run --> uninstall
    uninstall --> pluginify
```

To test your plugin, you can use `spin pluginify -iru` to uninstall, install, run, and again uninstall your plugin in one go. This is useful for testing your plugin in a clean environment.

That's a shortcut for:

```bash
spin pluginify --install --run --uninstall
```

Which in turn should be roughly equivalent to:

```bash
spin pluginify
spin plugins uninstall my-plugin
spin plugins install --file my-plugin.json --yes
spin my-plugin
spin plugins uninstall my-plugin
```

If your plugin takes arguments, you should specify them last in the command line after `--`:

```bash
spin pluginify --install --run --uninstall -- --my-arg
```

## Troubleshooting

Error handling is non-existent right now so, uh, sorry.
