## Gitlab installation on ubuntu server :

### Pre-requisites : 
- Root previledges
- 4 cores for your CPU
- 4GB of RAM for memory

#
### Steps to install :
- <b>Update packages</b>
```bash
sudo apt update
```
  - <b>Install the dependencies</b>
  ```bash
  sudo apt install ca-certificates curl openssh-server postfix tzdata perl
  ```

#
- <b>Installing GitLab</b>
```bash
cd /tmp
```

#
  - <b>Download the installation script</b>
  ```bash
  curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
  ```

 #
  - <b>Run the script</b>
  ```bash
  sudo bash /tmp/script.deb.sh
  ```

 #
  - <b>Install the actual GitLab application</b>
  ```bash
 sudo apt install gitlab-ce
  ```

#
- <b>Editing the GitLab Configuration File</b>
> Note: replace <b>external_url</b> with your server's PublicIP</b>
 ```bash
 sudo vi /etc/gitlab/gitlab.rb
  ```
> Note: Edit <b>letsencrypt['contact_emails']</b> with your contact email in the same file <b>/etc/gitlab/gitlab.rb</b>
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/gitlab-http.png" />
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/gitlab-http-2.png" />

#
- <b>Reconfigure GitLab</b>
```bash
sudo gitlab-ctl reconfigure
  ```

#
- <b>Logging In for the First Time</b>
```bash
https://your_domain
  ```
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/gitlab-dash-vm.png" />

#
- <b>GitLab generates an initial secure password for you. It is stored in a folder that you can access as an administrative sudo user</b>
```bash
sudo nano /etc/gitlab/initial_root_password
  ```

# 
- <b>Username and Password : </b> 
- Username: root
- Password: [the password listed on /etc/gitlab/initial_root_password]
<img src="https://github.com/DevMadhup/GitLab-Zero-to-Hero/blob/main/Assets/gitlab-main.png" />

#
<b>Congratulation, you have successfully configured Gitlab on Ubuntu Server</b>
