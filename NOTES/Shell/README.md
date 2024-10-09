# Shell


## OS (shell-agnostic)

### Change default user shell

1. Use `chsh -s path/to/shell`

   For instance, to set Zsh as default:

   ```sh
   chsh -s $(which zsh)
   ```

2. You may also want to change the startup command.

   For instance in your Konsole profile:  
**change `/bin/bash` to `/bin/zsh`**

3. Then logout ( <kbd>Ctrl</kbd> + <kbd>D</kbd> ),  
and login again (relaunch your terminal emulator).


