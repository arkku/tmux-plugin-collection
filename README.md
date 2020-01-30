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
