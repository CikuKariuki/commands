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
sudo systemctl reload sshd > this puts those edits that we made into effect.
To test: try loging in as ssh root@ip_address
sudo ufw allow OpenSSH   output Rules updated
sudo ufw allow http
sudo ufw allow https
sudo ufw enable   ignore the warning about ssh since we've already enabled it
sudo ufw status   allow anywhere in port 80 and 443
sudo apt-get install git 
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install nodejs
node --version
mkdir directory name >>I used Apps
cd directory name
pwd output is home/user/directory-name
git clone app into directory >>>>Cloned Deals from github into Apps
cd cloned-app -go into the cloned app
npm install - in case you have any dependencies
>>>>
curl http://localhost:port  the output will confirm to you whether it is working. CONNECTION REFUSED!!(curl: (7) Failed to connect to localhost port 3000: Connection refused)

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
but before this tool runs properly we need to have a DNS(domain name system) address pointing to our server
get the ip_address and log in to whatever tool you're using to manager your server he's using cloudflare
in terminal: ./letsencrypt-auto certonly --standalone 
enter your email
agree by hitting enter
get name of your dns 20:10
enter
./letsencrypt-auto renew -renews certificate of letsencrypt if it's within 30 days of expiry. However you can set it up automatically using cron:
sudo crontab -e
choose option with nano
go all the way to the bottom
once at the bottom, type :    00 1 * * 1 /opt/letsencrypt/letsencrypt-auto renew >> /var/log/letsencrypt-renewal.log
this means it will run this command for renewing every monday at 1 am and send the output to var/log ...
30 1 * * 1 /bin/systemctl reload nginx  we run this 30 minutes after the renewal because we need nginx to restart so as to get the new script
ctrl x, yes, enter
sudo apt-get install nginx because nginx is easier to cnfigure tan node js itself
sudo nano /etc/nginx/sites-enabled/default
ctrl + k
copy paste:    #HTTP - redirect all traffic to HTPPS
server {
    listen 80;
    listen [::]:80 default_server ipv6only=on;
    return 301 https://$host$request_uri;
}
crtl + x, Y, enter
 sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048    to increase our security by verifying the ssl certificate
 head over to cipherli.st and copy the nginx ssl setup DO NOT PASTE YET!!
 sudo nano /etc/nginx/snippets/ssl-params.conf 
 paste
 go to resolver $DNS-IP-1 line and erase, refer to 27:39 replace with 8.8.8.8.8.8.4.4
 at the bottom, 28:20 add: 
 ssl_dhparam /etc/ssl/certs/dhparam.pem;
 ctrl+x, y, enter
 sudo nano /etc/nginx/sites-enabled/default
 underneath, add: 29:10 paste it listen up until 30:32
 sudo nginx -t
 sudo systemctl start nginx
 get the domain name. blah.com and see if you were successful
 qualys'ssl labs tester: check if your ssl cert rating is a+