# linux_settings
Because I initialize workspace occasionally,I needed storage to back up some of important configurations

## To install packages & apps
 - git
 - texlive-full
 - vscode
 - oh-my-posh
 - terminator
 - miniconda
 - Firacode Nerd Font
 - nvidia driver & cuda toolkit(GPU acceleration kits)
   - reboot after install, check by ```nvidia-smi```. <br> tip) install by dpkg,(web based not local one), install driver first and cuda toolkit second.

  
## ssh commands for secure connection [1](https://chinensis.tistory.com/entry/SSH%EB%A1%9C-%EC%9B%90%EA%B2%A9-%EC%84%9C%EB%B2%84-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EC%A0%91%EC%86%8D-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0)

### installation
1. make sure ssh is installed by running ```sudo apt install openssh-server``` 
2. enable it by ```sudo systemctl enable ssh``` ; enable ssh automatically when boot up 
3. and, make sure all(two) green dots are shown by ```sudo systemctl status ssh```
4. option) ```sudo systemctl start ssh``` to run service manually 

### Firewall setting
1. ```sudo ufw enable``` and run ```sudo ufw allow ssh``` (it will be port 22)

2. ```sudo ufw status``` to check the status. make sure 22 or ssh port is opened.

### Same network connection

1. check host local ip on terminal by ip addr <br> it's not 127.0.0.1, should located in inet under link/ether line.(this one show mac address.)
2. For same network access, ```ssh (username)@(host local ip)```. username can be found by ```whoami```.

### Port forwarding for remote access  
1. assign static IP address to host device by setting in router.
2. create ssh key for secure connection by following command on **Client PC**<br>
```ssh-keygen``` <br> and entering passphrase.(for better security) 
3. for linux:connect to host server by ```ssh-copy-id username@host_ip``` <span style="color:red"> **make sure IP is not private IP but PUBLIC IP** </span>
3. for window:connect to host server by '''ssh -i (path to key) username@host_ip'''
4. After initial connection, make sure public key is added by ```cat ~/.ssh/authorized_keys```. <br> Otherwise, add it manually by using nano or vim (Should enable PasswordAuthentication for initial connection?)
5. restart ssh service by ```sudo systemctl restart ssh```

### Change ssh config for more security
1. open ssh config by ```sudo gedit /etc/ssh/sshd_config```
2. change ```PasswordAuthentication yes``` to ```PasswordAuthentication no``` <br> need ssh key to login.
3. change ```PermitRootLogin prohibit-password``` to ```PermitRootLogin no```
4. change LogLevel to  **VERBOSE** 
5. restart ssh service by ```sudo systemctl restart ssh```
6. check status by ```sudo systemctl status ssh```  

### install fail2ban[1](https://monkeybusiness.tistory.com/696)
1. install using command ```sudo apt install fail2ban```
2. enable and start by ```sudo systemctl start fail2ban && sudo systemctl enable fail2ban```
3. copy config file and edit paste one; by ```sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local```
4. edit file by ```sudo gedit(nano) /etc/fail2ban/jail.local```. _conf should not be modified_. should be by .local file.
5. Nothing is enough to be changed, but if there is, make sure to restart it by ```sudo systemtl restart fail2ban```
6. followings are usefull command when using fail2ban
    - ```sudo systemctl status fail2ban```
    - ```sudo fail2ban-client status [jail name]``` e.g. ```sudo fail2ban-client status sshd``` if sshd in jail name
    - ```sudo fail2ban-client unban (IP address)``` for accidently banned
    - ```sudo cat /var/log/fail2ban.log``` for logging for live logging. ```tail -f /var/log/fail2ban.log```

### Other useful command for good ssh life
 - ```tail -f /var/log/auth.log``` or ```journalctl -fu ssh``` for live log tracking.
 - ```journalctl -u ssh --since yesterday``` for log since yesterday
 - ```sudo systemctl status ssh``` check everything is green.
 -  


## oh my posh command
after download, add following code to bashrc, but to change username(installation terminal should show the appropriate sentence)
```
    # >> oh-my-posh settings >>>
    export PATH=$PATH:/home/sekyoung/.local/bin
    #eval "$(oh-my-posh init bash)"
    eval "$(oh-my-posh init bash --config ~/jandedobbeleer.omp.json)"
    # <<< oh-my-posh settings<<<
```
to enable posh to show conda env, then add following command  [source](https://medium.com/@birendrasharma0226/conda-environment-is-not-displaying-in-oh-my-posh-f71c1d5a09a0)

```json
{
 "background": "#bbbbbb",
 "foreground": "#193549",
 "powerline_symbol": "\ue0b0",
 "style": "diamond",
 "properties": {
 "fetch_virtual_env": true,
 "display_mode": "environment",
 "home_enabled": true,
 "display_virtual_env": true,
 "display_default": true
 },
 "template": " \ue235 {{ .Venv }} ",
 "type": "python"
 },
 ```
to enable maximum performance of ph-my-posh and vscode experience, change(or edit) setting json by follwing under
```json
    "editor.fontFamily": "FiraCode Nerd Font",
    "editor.fontLigatures": true,
    "terminal.integrated.fontFamily": "FiraCode Nerd Font",
```
