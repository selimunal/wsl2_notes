# WSL2 internet connection problem, ws2 apt-get update time out problem in Windows 11. 


In the command prompt:

Method 1. This way, you use the internet in wsl1 and need to swich to wsl2 later. 
1. wsl -l -v     

( NAME            STATE           VERSION
 * Ubuntu-22.04    Running         2)

2.   wsl --set-version Ubuntu-22.04 1

3. bash 
4. sudo apt-get update 

This worked for me. 

Method 2. This way you keep using WSL, but need to apply below fixed and still might not work. 

Finally made it work in WS2. I am not really sure how i did it. I have pushed all buttons, somehow it worked I guess 🗡 

1. Control Panel\System and Security\Windows Defender Firewall -> Advanced Setting -> Outbound Rules, Click new Rule, choose program -> Next -> Select the wsl.exe in system.32 -> Select all profiles -> Save it with a fancy name. 

2. WSL settings -> Enable localhost forwarding, and Networking Mode-> VirtioProxy   (this can be done via .wslconfig file, needs to be in /Users/%Username% folder)
3. echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
sudo chattr +i /etc/resolv.conf  # Make resolv.conf immutable
4. sudo ip route del default via 192.168.1.1 dev eth1
5. source ~/.bashrc
4. sudo nano /etc/wsl.conf
5. add 
6. [network]
generateResolvConf = false
7.sudo ip route add default via 127.0.0.1 dev loopback0
8. wsl --shutdown


At some point I have done this:
sudo dhclient -r eth0
sudo dhclient eth0


you can use this command to check your connection speed, if this command works, you are done.

curl -v https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
