### This fork exists to provide a config option for disabling automatic removal of whitespace indentation on otherwise blank lines.
```toml
[editor]
discard-auto-indented-whitespace = false # default: true
```
Behaviour defaults to that of upstream Helix.

When you open a new line wherein the cursor position is automatically indented, if you then `Esc` to normal mode
or immediately proceed to another new line by pressing `Return`, the default behaviour is to discard the whitespace
used for the indentation of the otherwise blank line. Setting `discard-auto-indented-whitespace` to `false` prevents
the removal of that whitespace.

When paired with `[editor.whitespace] render = "all"`, this can help some people visually follow indented lines
vertically by giving the eye something to follow.

This also allows pre-indenting lines for planning purposes or pasting text on a new line (`o`pen a new line,
`Esc` to normal mode, and `P`aste_before) without then having to re-indent the pasted text.

If you want the above behaviour but don't want your saved document polluted with unnecessary whitespace, Helix
default behaviour is to format on save if a formatter is available, which typically removes trailing whitespace.

This config option is unlikely to be accepted into Helix proper.
See [this reply.](https://github.com/helix-editor/helix/discussions/10075#discussioncomment-8967564)

#### The change does not exist in `master`!

It's in the `keep_indentation` branch which is forked from Helix 25.01.1

### To use:
You'll need to have Rust/Cargo installed, along with base development packages for your distro (such as GCC).

Be sure that `~/.cargo/bin` is in $PATH.

Replace `~/git` with your git repo folder of choice and otherwise adapt to your setup.

The `touch` lines are for initiating the config files if you've never run Helix before. Optional.
Having no config files results in default behaviour.

The symlink to the `runtime` folder is part of enabling syntax highlighting for supported languages, of which Helix
supports a great many.
The `hx -g` lines are for updating and building the language profiles which live in the `runtime` folder.

```sh
cd ~/git
git clone https://github.com/raum-dellamorte/helix.git
cd helix
git checkout keep_indentation
cargo install --locked --path helix-term
mkdir -p ~/.config/helix/themes
touch ~/git/helix/config.toml
touch ~/git/helix/languages.toml
ln -s ~/git/helix/runtime ~/.config/helix/
hx -g fetch
hx -g build
```

In order to notice any difference between this fork and vanilla Helix, ~/.config/helix/config.toml must contain:
```toml
[editor]
discard-auto-indented-whitespace = false
```

<div align="center">

<h1>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="logo_dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="logo_light.svg">
  <img alt="Helix" height="128" src="logo_light.svg">
</picture>
</h1>

[![Build status](https://github.com/helix-editor/helix/actions/workflows/build.yml/badge.svg)](https://github.com/helix-editor/helix/actions)
[![GitHub Release](https://img.shields.io/github/v/release/helix-editor/helix)](https://github.com/helix-editor/helix/releases/latest)
[![Documentation](https://shields.io/badge/-documentation-452859)](https://docs.helix-editor.com/)
[![GitHub contributors](https://img.shields.io/github/contributors/helix-editor/helix)](https://github.com/helix-editor/helix/graphs/contributors)
[![Matrix Space](https://img.shields.io/matrix/helix-community:matrix.org)](https://matrix.to/#/#helix-community:matrix.org)

</div>

![Screenshot](./screenshot.png)

A [Kakoune](https://github.com/mawww/kakoune) / [Neovim](https://github.com/neovim/neovim) inspired editor, written in Rust.

The editing model is very heavily based on Kakoune; during development I found
myself agreeing with most of Kakoune's design decisions.

For more information, see the [website](https://helix-editor.com) or
[documentation](https://docs.helix-editor.com/).

All shortcuts/keymaps can be found [in the documentation on the website](https://docs.helix-editor.com/keymap.html).

[Troubleshooting](https://github.com/helix-editor/helix/wiki/Troubleshooting)

# Features

- Vim-like modal editing
- Multiple selections
- Built-in language server support
- Smart, incremental syntax highlighting and code editing via tree-sitter

Although it's primarily a terminal-based editor, I am interested in exploring
a custom renderer (similar to Emacs) using wgpu or skulpin.

Note: Only certain languages have indentation definitions at the moment. Check
`runtime/queries/<lang>/` for `indents.scm`.

# Installation

[Installation documentation](https://docs.helix-editor.com/install.html).

[![Packaging status](https://repology.org/badge/vertical-allrepos/helix-editor.svg?exclude_unsupported=1)](https://repology.org/project/helix-editor/versions)

# Contributing

Contributing guidelines can be found [here](./docs/CONTRIBUTING.md).

# Getting help

Your question might already be answered on the [FAQ](https://github.com/helix-editor/helix/wiki/FAQ).

Discuss the project on the community [Matrix Space](https://matrix.to/#/#helix-community:matrix.org) (make sure to join `#helix-editor:matrix.org` if you're on a client that doesn't support Matrix Spaces yet).

# Credits

Thanks to [@jakenvac](https://github.com/jakenvac) for designing the logo!
