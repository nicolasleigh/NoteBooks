## Full Stack for Frontend

[PPT slides](https://static.frontendmasters.com/assets/courses/2023-04-18-fullstack-v3/fullstack-v3-slides.pdf)

### Connect to the remote server

On your local computer: 

- Move into the ~/.ssh directory

  ```sh
  cd ~/.ssh
  ```

- Generate a key using `ssh-keygen`

  ```sh
  ssh-keygen
  ```

- SSH into your remote server

  ```sh
  ssh root@<your_IP>
  ```

- SSH into your server by using your private key

  ```sh
  ssh -i ~/.ssh/<your_private_key> root@<your_IP>
  ```

- Make sure the keychain is active

  ```sh
  vi ~/.ssh/config
  ```

- Add the private key to the keychain

  ```
  ssh-add --apple-use-keychain <your_private_key>
  ```

Updating and restarting

- SSH into your server

- Update software

  ```sh
  apt update
  apt upgrade
  ```

- Restart your server

  ```
  shutdown now -r
  ```

Creating and updating users

- Create a new user

  ```sh
  adduser <your_username>
  ```

- Add user to sudo group

  ```sh
  usermod -aG sudo <your_username>
  ```

- Switch user

  ```sh
  su <your_username>
  ```

- Check sudo access

  ```sh
  sudo cat /var/log/auth.log
  ```

Enable login as a new user

- Create authorized_keys file

  ```
  ~/.ssh/authorized_keys
  ```

- Paste your public key

- Exit

- Login as the new user

  ```sh
  ssh <your_username>@<your_IP>
  ```

Security

- Change file permission

  ```sh
  chmod 644 ~/.ssh/authorized_keys
  ```

- Disable root login

  ```sh
  sudo vi /etc/ssh/sshd_config
  
  # Change 'yes' to 'no'
  # PermitRootLogin yes
  ```

- Disable password login

  ```sh
  sudo vi /etc/ssh/sshd_config
  
  # Change 'yes' to 'no'
  # PasswordAuthentication yes
  ```

- Restart the ssh daemon

  ```sh
  sudo service sshd restart
  ```

### Using firewall

- Check firewall status

  ```sh
  sudo ufw status
  ```

- Allow ssh

  ```sh
  sudo ufw allow ssh
  ```

- Allow http

  ```sh
  sudo ufw allow http
  ```

- Enable firewall

  ```sh
  sudo ufw enable
  ```