# `tmux`


## Quick start

`tmux` is another terminal multiplexer, similar to `screen` but with a more modern feature set. It allows multiple terminal sessions with more customizable options.

- **Install `tmux`**

  ```sh
  sudo apt-get install tmux  # On Debian/Ubuntu
  sudo yum install tmux      # On CentOS/RHEL
  sudo dnf install tmux      # On Fedora
  ```

- **Start a tmux Session**:

  ```sh
  tmux
  ```

- **Detach from tmux Session**:

  Press <kbd>Ctrl</kbd>+<kbd>b</kbd> followed by <kbd>d</kbd> to detach from the session.

- **List Running Sessions**:

  ```sh
  tmux ls
  ```

- **Reattach to a tmux Session**:

  ```sh
  tmux attach-session -t [session name]
  ```



## Preserve sessions across reboots

`tmux-resurrect` and `tmux-continuum` are plugins for `tmux` that enhance its capabilities, particularly in regards to saving and restoring session states, and automatically doing so. These tools address one of the common limitations of `tmux`, namely the inability to preserve the session state through reboots or system shutdowns.

### 1. `tmux-resurrect`

**Features**

- Save the `tmux` environment so it can be completely restored after a system restart.
- Restore not just the tmux sessions and windows, but also pane layouts, current working directory, and even processes like `vim`, `top`, `htop`.

**How it works**

- You manually save the `tmux` session state to a file by using a pre-configured key binding.
- You can restore the session state manually after the system starts or after `tmux` is launched.

**Setup:**

1. **Install tmux-resurrect**
   If you are using TPM (Tmux Plugin Manager), you can add the following line to your `.tmux.conf`:

   ```tmux
   set -g @plugin 'tmux-plugins/tmux-resurrect'
   ```

   Then hit `prefix + I` in `tmux` to fetch and source the plugin.

2. **Save and restore sessions:**

   - Save: `prefix`, <kbd>Ctrl</kbd><kbd>s</kbd>
   - Restore: `prefix`, <kbd>Ctrl</kbd><kbd>r</kbd>

### 2. `tmux-continuum`

**Features**

- Automatic continuous saving of tmux sessions and environments.
- Automatic tmux start when the computer/server starts.
- Automatic restore of the tmux environment when `tmux` is started after reboot. 

**How it works**

- The plugin works in tandem with `tmux-resurrect`. It frequently saves the session state using the mechanism provided by `tmux-resurrect`.
- This enables a form of "set and forget" for tmux session management, where sessions are not only restored manually but also backed up at regular intervals automatically.

**Setup:**

1. **Install tmux-continuum**

   If you use TPM, add this line to your `.tmux.conf`:

   ```tmux
   set -g @plugin 'tmux-plugins/tmux-continuum'
   ```

   Then reload tmux environment (`prefix + I`).

2. **Configure settings in `.tmux.conf`**

   - To set the automatic saving interval (default is every 15 minutes):

     ```
     set -g @continuum-save-interval '15'
     ```

   - To enable restoring sessions on tmux start:

     ```
     set -g @continuum-restore 'on'
     ```

By using both `tmux-resurrect` and `tmux-continuum`, you can achieve near seamless tmux session persistence across system reboots, without needing to manually save and restore your sessions. These tools significantly enhance the usability of tmux for long-term and persistent session management.



## Prefix key

In `tmux`, the "prefix" key acts as a control or trigger that precedes other command sequences to manage sessions, windows, and panes. Essentially, it's a key combination that signals to `tmux` that the following keystrokes form a command for `tmux` itself that it should capture, rather than being sent to the terminal session.

### Default Prefix Key

By default, the prefix key in `tmux` is <kbd>Ctrl</kbd>+<kbd>b</kbd>. This means that to execute commands within `tmux`, you usually need to hit <kbd>Ctrl</kbd>+<kbd>b</kbd> first, followed by another specified key (or key combination) that represents a specific `tmux` command. 

For example, to create a new window in `tmux`, you would press:
- <kbd>Ctrl</kbd>+<kbd>b</kbd> (prefix key),
- followed immediately by <kbd>c</kbd> (command to create a new window).

### Common Commands Using the Prefix

Here are a few commonly used `tmux` commands that require the prefix key:

- Create a new window: <kbd>Ctrl</kbd>+<kbd>b</kbd> then <kbd>c</kbd>
- Split pane horizontally: <kbd>Ctrl</kbd>+<kbd>b</kbd> then <kbd>%</kbd>
- Split pane vertically: <kbd>Ctrl</kbd>+<kbd>b</kbd> then <kbd>"</kbd>
- Switch to next window: <kbd>Ctrl</kbd>+<kbd>b</kbd> then <kbd>n</kbd>
- Detach from session: <kbd>Ctrl</kbd>+<kbd>b</kbd> then <kbd>d</kbd>

### Customizing the Prefix Key

The default prefix (<kbd>Ctrl</kbd>+<kbd>b</kbd>) can be changed to something else if desired. Many users prefer to change it to <kbd>Ctrl</kbd>+<kbd>a</kbd> because it's similar to `screen` (another terminal multiplexer), or simply because it might be easier to reach or remember.

You can configure a different prefix by modifying your `.tmux.conf` file, which is `tmux`'s configuration file. To change the prefix to <kbd>Ctrl</kbd>+<kbd>a</kbd>, you'd add the following line:

```tmux
unbind C-b  # unbind the default prefix
set -g prefix C-a  # set the new prefix
bind C-a send-prefix  # bind the new prefix key
```

After changing the prefix key, you would need to reload the `tmux` configuration with these new settings by either restarting `tmux` or using source command from within tmux: <kbd>Ctrl</kbd>+<kbd>b</kbd> (if still active) or <kbd>Ctrl</kbd>+<kbd>a</kbd> (if already switched) then `:source ~/.tmux.conf`.






