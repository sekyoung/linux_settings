# linux_settings
Because I initialize workspace occasionally,I needed storage to back up some of important configurations

## To install packages & apps
 - git
 - texlive-full
 - vscode
 - oh-my-posh
 - matlab
 - miniconda
 - Firacode Nerd Font
 - nvidia driver & cuda toolkit(GPU acceleration kits)
  
## ssh commands for secure connection [source](https://chinensis.tistory.com/entry/SSH%EB%A1%9C-%EC%9B%90%EA%B2%A9-%EC%84%9C%EB%B2%84-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EC%A0%91%EC%86%8D-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0)

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

### Port forwarding for remote access <span style="color:red"> **WHY NOT WORKING??**</span>
1. assign static IP address to host device by setting in router.
2. create ssh key for secure connection by following command on **Client PC**<br>
```ssh-keygen -t Ed25519``` <br> and entering passphrase.(for better security)
3. connect to host server by ```ssh-copy-id username@host_ip```


### Change ssh config for more security
1. open ssh config by ```sudo gedit /etc/ssh/sshd_config```
2. change ```PasswordAuthentication yes``` to ```PasswordAuthentication no``` <br> need ssh key to login.
3. change ```PermitRootLogin prohibit-password``` to ```PermitRootLogin no```
4. restart ssh service by ```sudo systemctl restart ssh```
5. check status by ```sudo systemctl status ssh```  

### Other useful command for good ssh life
 - ```tail -f /var/log/auth.log``` for live log tracking.
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

