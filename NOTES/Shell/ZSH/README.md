# Zsh (Z shell)

🏛️ https://www.zsh.org/  
🇺🇸 https://zsh.sourceforge.io/  

> Zsh is a shell designed for interactive use, although it is also a powerful scripting language. Many of the useful features of bash, ksh, and tcsh were incorporated into zsh; many original features were added.

**A User's Guide to the Z-Shell**: https://zsh.sourceforge.io/Guide/zshguide.html

**The Z Shell Manual**: https://zsh.sourceforge.io/Doc/Release/index.html  
*Original documentation by Paul Falstad*





## Setup

[Zsh](https://zsh.sourceforge.io/) is great for IT people and programmers. Productivity is increased by great plugins like `zsh-autosuggestions` (helps reuse & do variations on history commands).




### Installation

```sh
sudo apt install zsh zsh-doc
zsh --version    # Confirm proper installation
```

[Change your default user shell](#change-default-user-shell) if needed.  
Otherwise simply run `zsh` (right within `bash` or any other shell) to use it.

You will be greeted by the Z Shell config function for new users, `zsh-newuser-install`.  
*Hit <kbd>q</kbd> to quit if you're going to install Oh My Zsh next.*




### Configuration

Zsh is configured mostly through your **`~/.zshrc`** file, following the usual inheritance paths from the system-wide `/etc/zsh/zshrc` .  
Global Order: `zshenv`, `zprofile`, `zshrc`, `zlogin`

You can tweak lots of parameters.

📚 https://zsh.sourceforge.io/Doc/Release/Parameters.html#Parameters-Used-By-The-Shell

#### History

Zsh has a custom tool to present history in a human-friendly way from the raw `~/.zsh_history` file.

```zsh
history
```

Near-infinite history size: in `~/.zshrc` set these parameters to a high-enough value.

```zshrc
# Number of commands to keep in memory
HISTSIZE=1000000

# Number of commands to save to the history file    
SAVEHIST=1000000
```




### Oh My Zsh!

https://ohmyz.sh/


#### Installation

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```


#### Plugins

- Overview: <https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins-Overview>
- Full list: <https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins>

Enable a plugin by adding its name to the `plugins` array in your `.zshrc` file (found in the `$HOME` directory). For example, this enables the `rails`, `git` and `ruby` plugins, in that order:

```
plugins=(rails git ruby)
```

> [!NOTE]
> Elements in zsh arrays are separated by whitespace (spaces, tabs, newlines...).  
> **DO NOT use commas.**

Whenever you install a new terminal tool on a machine, check for its [OMZ plugin](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins).  
E.g., 
[`sudo`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/sudo),
[`cp`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/cp),
[`ssh`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/ssh),
[`ssh-agent`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/ssh-agent),
[`systemd`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/systemd) 
[`git`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git),
[`ufw`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/ufw),
[`1password`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/1password),
[`pip`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/pip),
[`ubuntu`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/ubuntu),
[`vscode`](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/vscode),
…

> [!Tip]
> Don't add too many (strict need-to basis), as it may slow down shell startup.


#### zsh-autosuggestions

https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#oh-my-zsh

1. Clone this repository into `$ZSH_CUSTOM/plugins` (by default `~/.oh-my-zsh/custom/plugins`)

```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

2. Add the plugin to the list of plugins for Oh My Zsh to load (inside `~/.zshrc`):  

```sh
plugins=( 
    # other plugins...
    zsh-autosuggestions
)
```

3. Restart zsh (such as by opening a new instance of your terminal emulator).


#### zsh-syntax-highlighting

https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md#oh-my-zsh

1. Clone this repository in oh-my-zsh's plugins directory:

```sh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

2. Activate the plugin in `~/.zshrc`.  
It **must** be last!

```sh
plugins=( [plugins...] zsh-syntax-highlighting)
```

3. Restart Zsh (such as by opening a new instance of your terminal emulator).


<!--
### Starship prompt

🏛️ https://starship.rs/  
🧬 https://github.com/starship/starship  

> ☄🌌️ The minimal, blazing-fast, and infinitely customizable prompt for any shell!

#### Installation

1. Install the latest version for your system:

    ```sh
    curl -sS https://starship.rs/install.sh | sh
    ```

1. Add the following to the end of `~/.zshrc`:

    ```sh
    eval "$(starship init zsh)"
    ```
-->

### Spaceship Prompt

🏛️ https://spaceship-prompt.sh/  
🧬 https://github.com/spaceship-prompt/spaceship-prompt  

> 🚀⭐ Minimalistic, powerful and extremely customizable Zsh prompt


#### Installation

🔗 https://spaceship-prompt.sh/getting-started/#__tabbed_1_3

1. Clone the repo:

   ```sh
   git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1
   ```

2. Symlink `spaceship.zsh-theme` to your oh-my-zsh custom themes directory:

   ```sh
   ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
   ```

3. Set `ZSH_THEME="spaceship"` in your `.zshrc`.

   ```sh
   nano ~/.zshrc
   ```

4. Restart Zsh.








## Resources

- Zsh: [The Z Shell Manual](https://zsh.sourceforge.io/Doc/Release/index.html)

- Oh My Zsh [documentation](https://github.com/ohmyzsh/ohmyzsh/wiki)




