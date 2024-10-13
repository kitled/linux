# Multiplex

A multiplexer allows multiple terminal sessions in the same window.  
Popular multiplexers include `screen` and `tmux`.


















## Over SSH

When working with SSH and needing to disconnect from a session while keeping the processes running on the remote machine, there are several tools designed specifically for this purpose. The most popular ones are `screen`, `tmux`, and using `nohup`. Here’s how you can use each:

### 1. `screen`
`screen` is a terminal multiplexer that allows you to manage several separate terminal sessions within a single window. It’s very useful for keeping remote sessions active even after disconnecting. Here’s how to use it:

- **Install Screen** (if it's not already installed):
  ```bash
  sudo apt-get install screen  # On Debian/Ubuntu
  sudo yum install screen      # On CentOS/RHEL
  sudo dnf install screen      # On Fedora
  ```

- **Start a Screen Session**:
  ```bash
  screen
  ```

- **Detach from Screen Session**:
  You can detach from the screen session and keep it running in the background by pressing `Ctrl-a` followed immediately by `d`.

- **List Running Sessions**:
  ```bash
  screen -ls
  ```

- **Reattach to a Screen Session**:
  ```bash
  screen -r [session ID or name]
  ```

### 2. `tmux`
`tmux` is another terminal multiplexer, similar to `screen` but with a more modern feature set. It allows multiple terminal sessions with more customizable options.

- **Install tmux** (if it's not already installed):
  ```bash
  sudo apt-get install tmux  # On Debian/Ubuntu
  sudo yum install tmux      # On CentOS/RHEL
  sudo dnf install tmux      # On Fedora
  ```

- **Start a tmux Session**:
  ```bash
  tmux
  ```

- **Detach from tmux Session**:
  Press `Ctrl-b` followed by `d` to detach from the session.

- **List Running Sessions**:
  ```bash
  tmux ls
  ```

- **Reattach to a tmux Session**:
  ```bash
  tmux attach-session -t [session name]
  ```

### 3. `nohup`
`nohup` is a POSIX command to ignore the HUP (hangup) signal, enabling the command to keep running after the user who issues the command has logged out.

- **Using nohup**:
  ```bash
  nohup command &
  ```

- **Redirect Output to a File if Needed**:
  ```bash
  nohup command > output.log 2>&1 &
  ```
  This command sends the stdout and stderr to `output.log`.

While `screen` and `tmux` offer rich features for managing multiple windows and sessions, `nohup` is simpler for ensuring that a single command continues running after disconnecting. The choice between these tools can depend on your specific needs such as whether you need to return to a full terminal session or just ensure a process continues to run.