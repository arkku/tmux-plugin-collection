# tmux plugins

This repository contains a selection of tmux plugins as submodules. Given that
this still requires manual installation, there is little benefit to using this
instead of the original plugins, unless you want exactly the same set as I.

## Installation

To install, first clone the repository and get the submodules:

``` sh
git clone https://github.com/arkku/tmux-plugin-collection.git
cd tmux-plugin-collection
git submodule init
git submodule update --init --recursive
```

Then load individual plugins from your `~/.tmux.conf` (or a file included from
it) the usual way. Personally I have this in my `~/.tmux.conf` (which is in
[a git repository](https://github.com/arkku/dotfiles) and the same for all my
computers):

``` tmux
if-shell 'test -s ~/.tmux/local.conf' 'source-file ~/.tmux/local.conf' ''
```

Then in `~/.tmux/local.conf` I load and set-up the plugins, which may be
different for some computers. My plugins are in `~/.tmux/plugins`.

## Plugins

### resurrect

[tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect) allows saving
and restoring tmux sessions, including windows, panes, layout, working
directories, and even select running processes. It doesn't overwrite existing
stuff, so an accidental restore is generally not a problem. It only saves on
command, but see _tmux-continuum_ below for autosaving.

``` tmux
set -g @resurrect-capture-pane-contents 'off'
set -g @resurrect-processes 'ssh mosh-client bat'
set -g @resurrect-hook-post-restore-all 'tmux resize-pane -R 1; tmux resize-pane -L 1'

if-shell 'test -d ~/.tmux/plugins/resurrect' 'run-shell ~/.tmux/plugins/resurrect/resurrect.tmux' ''
run-shell 'mkdir -p ~/.tmux/resurrect && chmod 700 ~/.tmux/resurrect'
```

* **prefix** + <kbd>Ctrl</kbd>–<kbd>S</kbd> – save the current session
* **prefix** + <kbd>Ctrl</kbd>–<kbd>R</kbd> – restore a saved session

#### continuum

[tmux-continuum](https://github.com/tmux-plugins/tmux-continuum/) adds periodic
auto-saving and (optional) auto-restore on launch to _tmux_resurrect_ (on which
it depends).

``` tmux
set -g @continuum-restore 'on'
set -g @continuum-save-interval '19'
if-shell 'test -d ~/.tmux/plugins/continuum' 'run-shell ~/.tmux/plugins/continuum/continuum.tmux' ''
```

Note that continuum never deletes saves, so you will accumulate quite a few
over time. They aren't very large, however, so a yearly manual purge should be
fine.

#### open

[tmux-open](https://github.com/tmux-plugins/tmux-open) adds a couple of key
bindings in copy mode to directly open URLs and files.

``` tmux
set -g @open-S 'https://stackoverflow.com/search?q='
set -g @open-G 'https://www.google.com/search?q='

if-shell 'test -d ~/.tmux/plugins/open' 'run-shell ~/.tmux/plugins/open/open.tmux' ''
```

With an active selection:

* <kbd>o</kbd> – open the file or URL (e.g., with `open` on macOS)
* <kbd>Ctrl</kbd>–<kbd>O</kbd> – open the file in `$EDITOR` (note that this
* <kbd>Shift</kbd>–<kbd>S</kbd> – search for the highlighted text online (in
  google, by default, but see above snippet on how to change it, but note that
  currently you cannot bind both upper- and lowercase of the same letter to
  a different search engine)

#### copycat

[tmux-copycat](https://github.com/tmux-plugins/tmux-copycat) adds the
capability to easily search for things and select them in copy mode. In
particular, there is a pre-defined search for URLs, which goes together well
with _tmux-open_.

``` tmux
if-shell 'test -d ~/.tmux/plugins/copycat' 'run-shell ~/.tmux/plugins/copycat/copycat.tmux' ''
```

* **prefix** + <kbd>/</kbd> – enter a regular expression to search for
* **prefix** + <kbd>Ctrl</kbd>–<kbd>U</kbd> – search for URLs
* **prefix** + <kbd>Ctrl</kbd>–<kbd>F</kbd> – search for file names
* **prefix** + <kbd>Ctrl</kbd>–<kbd>G</kbd> – search for files in `git status`
* **prefix** + <kbd>Ctrl</kbd>–<kbd>D</kbd> – search for digits (numbers)
* **prefix** + <kbd>Alt</kbd>–<kbd>H</kbd> – search for hashes or such
* **prefix** + <kbd>Alt</kbd>–<kbd>I</kbd> – search for IP addresses

Once in copy mode, press <kbd>n</kbd> to jump to the next match, or
<kbd>Shift</kbd>–<kbd>N</kbd> to jump to the previous match. Of course the
usual copy mode bindings apply at this stage.

**Note**: Currently (February 2020) this plugin seems to have some issues with
the latest versions of `tmux`, and thus I have disabled it my own
configuration. I'm keeping the submodule so I can easily track changes, since
it provides very useful features, but unfortunately the upstream repository
seems to be fairly inactive with several years old pull requests for fixes
pending…
