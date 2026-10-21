# SSH from Termux to Linux Mint — UFW Timeout Fix
## Symptom
i was trying to connect termux ssh client to linux ssh server but the thing happening was that the termux kept saying connection timed out even when i had already install ssh client or even wrote the correct ip adress. but it still were saying that connection timed out,
## Investigation
then tried this **systemctl status ssh** and found out that ssh was inactive but the ssh.socket was listening and active, so that's meant that it is not dead, it wasn't that ssh couldn't connect there something else that was blocking ssh,
## Root Cause
then i checked that it should be firewall cause that this is not working like it supposed to,
## Fix
i have tried to make a rule to excempt the port 22 which is ssh port, this is the command i wrote it.**sudo ufw allow from 192.168.0.0/24 to any port 22 proto tcp**
where it can listen to connect request so it can connect,
## Result
 when i did that it worked and i was able to connect termux to linux. 
## Lessons
