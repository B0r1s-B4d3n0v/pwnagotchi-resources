# Bash Alises - AKA pwnagotchi shortcuts

I'm writing some plugins and fixing some others.

Got tired of writing out the long commands - and even ctrl+r was getting annoying - made some aliases for the common control stuffs.

The file is automatically picked up during boot process by the core - so add it to your backup scripts and it should "just work" when redeployed.

## Copy and paste

Copy and paste the entire block of code (including the word "end"). It will create the file and add these contents to it.

```
sudo cat > /home/pi/.bash_aliases <<END
alias restart-auto='sudo touch /root/.pwnagotchi-auto && sudo rm -rf /root/.pwnagotchi-manual && sudo systemctl restart pwnagotchi'
alias restart-manu='sudo touch /root/.pwnagotchi-manual && sudo rm -rf /root/.pwnagotchi-auto && sudo systemctl restart pwnagotchi'
alias restart-manual='restart-manu'

alias shutdown='sudo systemctl stop pwnagotchi && sudo pwnagotchi --clear && sudo shutdown -h now'
alias reboot='shutdown'

alias stop='sudo systemctl stop pwnagotchi'
alias start='sudo systemctl start pwnagotchi'
alias restart='sudo systemctl restart pwnagotchi'

alias status='sudo systemctl status pwnagotchi'
alias ver='echo "This takes a min ..." >&2 && sudo pwnagotchi --version'
alias debug='sudo systemctl stop pwnagotchi && sudo pwnagotchi --debug'
END
```

Then restart bash service with 

`source ~/.bashrc`

and you can start using the aliases.

That's all there is to the install. 

To use it you can now control the gotchi with simple commands like restart-auto to restart the gotchi is Auto mode. Or just status to see what it's doing.
