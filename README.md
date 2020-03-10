create droplet
copy ip address
in terminal, ssh root@ip_address
adduser username
create new password
verify user exists by typing id username
usermod -aG sudo username
su - username
whoami
mkdir ~/.ssh
chmod 700 ~/.ssh  making it only accessible to our user
nano ~/.ssh/authorized_keys  create a new file for te authorized keys
copy the ssh key > crtl+x, yes, enter
chmod 600 ~/.ssh/authorized_keys change permissions for it to only be accessible by the user
whoami
ssh username@ip_address at this point it should have locked out sign in from ssh root@ip_address
pwd should give /home/addeduser
sudo nano /etc/ssh/sshd_config get to the ssh config
input the sudo password
crtl + w for search
search for PermitRootLogin and disable it by changing yes to no
search again for PasswordAuthentication, uncomment it and change it to no
crtl x, yes, enter
# setting basic firewall stuff
sudo systemctl reload sshd this puts those edits that we made into effect.
To test: try loging in as ssh root@ip_address
sudo ufw allow OpenSSH   output Rules updated
sudo ufw allow http
sudo ufw allow https
sudo ufw enable   ignore the warning about ssh since we've already enabled it
sudo ufw status   allow anywhere in port 80 and 443
ssh username@ip_address
curl http://localost:port  the output will confirm to you whether it is working.
to fix the problem of automatically restarting the app that there's a server restart, we're going to use a process manager that is called pm2
sudo npm install -g pm2
make sure you're in the app's directory.
pm2 start pm2 just replaces the word node when starting, therefore if pm2 start doesn't work, try pm2 index.js
To make sure that the app will restart when the server restarts, pm2 startup systemd the output will give a command that will give the right environment variables and the command to update systemd running as sudo
Copy and paste it
move to home directory
sudo apt-get install bc
sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
cd /opt/letsencrypt/
but before this tool runs properly we need to have a DNS address pointing to our server
get the ip_address and log in to whatever tool you're using to manager your server he's using cloudflare
in terminal: ./letsencrypt-auto certonly --standalone 