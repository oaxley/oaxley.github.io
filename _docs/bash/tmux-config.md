---
title: "Tmux configuration"
type: doc
subtype: Bash
order: 2
---

## Alias

By default `tmux` does not support `XDG_CONFIG_HOME` as an alternate directory for configuration.  
To overcome this limitation, you can create an alias:

``` bash
$ alias tmux='tmux -f "${XDG_CONFIG_HOME}/user/tmux.conf"'
```

## My TMUX configuration

``` bash
# last window on C-b C-b, previous on Alt-h, next on Alt-l
bind C-b last-window
bind -n M-h previous-window
bind -n M-l next-window

# new window is C-n directly
unbind c
bind -n C-n new-window

# unbind current pane splitting
unbind %
unbind '"'

# bind pane splitting on more meaningful keys
# new pane opens up in same current directory
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"

# activate pane synchronization on C-x
bind -n C-x setw synchronize-panes \; display "#{?pane_synchronized,Pane Synchronization ON,Pane Synchronization OFF}"

# status bar with load average and uptime
set -g status-bg black
set -g status-fg white
set -g status-left '#[fg=green]#S '
set -g status-right-length 120
set -g status-right "#(uptime | cut -d' ' -f 4-5 | tr -d ",") | Load: #(cut -d' ' -f 1-3 /proc/loadavg) | %a %h-%d %H:%M"

# 256 colors TERM
set -g default-terminal "screen-256color"

# windows activity
set -g base-index 1
# tmux 3.3a
setw -g window-status-current-style fg=white,bg=red
# tmux < 3.0
#set -g window-status-current-bg red

set -g visual-activity on
set -g monitor-activity on
setw -g automatic-rename

# history limit
set -g history-limit 10000

# configuration reload
bind R source-file ~/.config/user/tmux.conf \; display-message "Config reload done"
```