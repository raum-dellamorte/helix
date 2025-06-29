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

Replace occurances of `~/git` with your prefered project/build folder.

The `touch` lines are for initiating the config files if you've never run Helix before. Optional.
Having no config files results in default behaviour.

The symlink to the `runtime` folder is part of enabling syntax highlighting for supported languages, and, 
because Helix supports a great many, it's a chonky 1.4GB. Feel free to `mv` instead of `ln -s` if you don't 
intend to keep the source folder ~~like a digital hoarder~~ for making your own changes to helix.

The `hx -g` lines are for updating and building the language profiles which live in the `runtime` folder.

```sh
cd ~/git # Change me if building elsewhere
git clone https://github.com/raum-dellamorte/helix.git
cd helix
git checkout keep_indentation
cargo install --locked --path helix-term
# Add $HOME/.cargo/bin to $PATH
mkdir -p ~/.config/helix/themes
touch ~/.config/helix/config.toml
touch ~/.config/helix/languages.toml
ln -s ~/git/helix/runtime ~/.config/helix/ # Change me if building elsewhere
hx -g fetch # These last 2 steps are to ensure the grammars are up to date.
hx -g build # Probably built already, but just in case
```

In order to notice any difference between this fork and vanilla Helix, ~/.config/helix/config.toml must contain:
```toml
[editor]
discard-auto-indented-whitespace = false
```
My personal config.toml:
```
theme = "kanabox" # need 'kanabox.toml' in .config/helix/themes/

[editor]
line-number = "relative" # All the cool devs are doing it
mouse = false # keeps the cursor from following the mouse
bufferline = "multiple" # Hide the bufferline when only one buffer/file is open
auto-format = false # For the demanding control freak, I will format the document myself, thank you.
soft-wrap.enable = true
discard-auto-indented-whitespace = false # The reason for the fork!

[editor.cursor-shape]
insert = "bar" # to make it more visually obvious when you're in 'insert' mode

[editor.whitespace] # pairs well with 'discard-auto-indented-whitespace = false'
render = "all"      # helps me to vertically track indentation with my stupid eyes

[keys.normal] # I 
esc = ["collapse_selection", "keep_primary_selection"]
p = ["paste_before","move_char_right"] # paste at left side of cursor by default
P = "paste_after"
C-v = ["paste_clipboard_before","move_char_right"] # Ctrl-v paste from system clipboard
C-c = "yank_to_clipboard" # Ctrl-c copy to system clipboard
C-s = ":w!" # Ctrl-s save like nano and other editors
C-x = ":q!" # Ctrl-x close like nano. Breaks Ctrl-xcv cut copy paste but I think 'close' before I think 'cut' for Ctrl-x bc nano
C-S-s = "save_selection" # this is the default behaviour of C-s above. I _think_ reassigning is necessary to make the above work
A-d = "delete_selection" # I _think_ this has to be declared first to make the following work
d = "delete_selection_noyank" # I just want to delete stuff without it replacing what I deliberately yanked.
A-c = "change_selection" # same as A-d
c = "change_selection_noyank" # it never felt intuitive for 'change_selection' to replace what I deliberately yanked.
"ret" = ["open_below", "normal_mode"] # I'm still test driving this, you may hate it, idk
"*" = ["move_char_right", "move_prev_word_start", "move_next_word_end", "search_selection", "search_next"] # vim like search word under cursor
"#" = ["move_char_right", "move_prev_word_start", "move_next_word_end", "search_selection", "search_prev"] # and reverse

[keys.insert]
C-s = ["normal_mode", ":w!"]
C-v = ["paste_clipboard_before","move_char_right"]
C-c = "yank_to_clipboard"
C-x = ["yank_to_clipboard","delete_selection_noyank"]
"ret" = "normal_mode" # pressing 'enter' returns to normal mode. takes some getting used to but I reach for the escape key less often
"S-ret" = "insert_newline" # paired with above. similar to using 'shift-enter' to get a newline when in a text entry field that defaults to submitting on 'enter'
"S-left" = "extend_char_left" # these 4 are to accommodate my expectation of being able to select text by holding shift in insert mode. there are issues by I can't remember them atm 
"S-right" = "extend_char_right"
"S-up" = "extend_visual_line_up"
"S-down" = "extend_visual_line_down"
"A-w" = ["select_mode", "extend_char_left", "surround_add", "insert_mode"]

[keys.select] # some things have to be repeated in select mode (aka visual mode) so you don't have to change your mental map bc you've got something highlighted
C-c = "yank_to_clipboard"
A-d = "delete_selection"
d = "delete_selection_noyank"
A-c = "change_selection"
c = "change_selection_noyank"
p = "replace_with_yanked"
P = "replace_selections_with_clipboard"
"(" = "@ms(" # select mode surround shortcuts, VSCode conveniences
"[" = "@ms["
"{" = "@ms{"
'"' = '@ms"'
"'" = "@ms'"
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
