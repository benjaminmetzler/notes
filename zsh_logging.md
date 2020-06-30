### How to Log Your Entire zsh History
I rely on my command history for a lot of the things I do.  It allows me to "not remember" the exact syntax of commands.  I could try to google to find the page that showed me how to run that one obscure command that one time or I could just save all of my history.  Previously I had followed the instructions [here](https://spin.atomicobject.com/2016/05/28/log-bash-history/) to keep all of my commands in a dedicated log directory and it worked well (thought I kept forgetting to back it up).

In macOS Catalina, Apple replaced the default login shell with zsh.  I always wipe the drive when installing a new OS so when I installed Catalina and recreated my user, macOS set my shell to zsh.  I could switch back to bash (and I might) but I like to play with new toys so I stuck with zsh.  

Unfortunately the `history` command in zsh doesn't have the same functionality as bash's version so the above command didn't work. Do get it working I used the following command instead:
```
export ZSH_LOGS=~/.zsh_logs
[[ -d ${ZSH_LOGS} ]] || mkdir ${ZSH_LOGS}
export LOG_ZSH_COMMAND='if [ "$(id -u)" -ne 0 ]; then echo "$(date "+%Y-%m-%d.%H:%M:%S") $(pwd) $(history 1)" >> ${ZSH_LOGS}/zsh-history-$(date "+%Y-%m-%d").log; fi'
precmd() { eval "$LOG_ZSH_COMMAND" }
```
This does the following:
* `export ZSH_LOGS=~/.zsh_logs` - Points to the log directory.  Change as needed
* `[[ -d ${ZSH_LOGS} ]] || mkdir ${ZSH_LOGS}` - Creates the log directory if it doesn't exist.
* `export LOG_ZSH_COMMAND ...` - Does the actual logging.
* `precmd() { eval "$LOG_ZSH_COMMAND" }` - Uses zsh's `precmd` to run the log command.

To get it to work, add the above line to your `~/.zprofile` and then run `source ~/.zprofile`.  Note that this will capture the commannd after it has been run but before the prompt is rendered.  As a result it will show the last command run before the current.



