# Setup GIT On Ubuntu

## Install Git

```bash
# sudo apt update
```

```bash
# sudo apt install git
```

```bash
# git --version
```

```bash
Output:
git version 2.25.1
```

## Setting Up Git

- You have to configure your username and email

```bash
# git config --global user.name "Your Name"
# git config --global user.email "youremail@domain.com"
```

```bash
# git config --list
```

```text
Output :
user.name=Your Name
user.email=youremail@domain.com
...
```

- After this step you can check your name and email add in gitconfig file

```bash
# nano /root/.gitconfig
```

```bash
[user]
  name = Your Name
  email = youremail@domain.com
```

## Add the Official GitLab Repository For Gitlab-Runner

```bash
# curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
```

## Install Gitlab-runner

```bash
# sudo apt-get install gitlab-runner
```

- Command to check gitlab-runner version

```bash
# sudo gitlab-runner -version
```

```text
OUTPUT :
Version:      13.4.1
Git revision: e95f89a0
Git branch:   13-4-stable
GO version:   go1.13.8
Built:        2020-09-25T20:03:43+0000
OS/Arch:      linux/amd64
```

- Now check the status of Runner

```bash
# sudo gitlab-runner status
```

```bash
Runtime platform                                    arch=amd64 os=linux pid=29368 revision=e95f89a0 version=13.4.1
gitlab-runner: Service is running!
```

- Here is the command for runner start, stop, restart and status

```bash
# sudo gitlab-runner start
```

```bash
# sudo gitlab-runner stop
```

```bash
# sudo gitlab-runner restart
```

```bash
# sudo gitlab-runner status
```

## Grant sudo Permission to GitLab Runner User

```bash
# cd /home/
```

```bash
# sudo visudo
```

- Add the gitlab-runner user in sudoers group and set NOPASSWD as shown below

```bash
# gitlab-runner ALL=(ALL) NOPASSWD: ALL
```

```bash
OUTPUT :

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL
gitlab-runner ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
gitlab-runner ALL=(ALL) NOPASSWD: ALL
```

## Now Register a new runner in git

```bash
sudo gitlab-runner register
```

Then After,  

![runner](./image/gitlab-runner.jpg)

1. First login to GitLab Server with Username and Password.
2. Go to Project and click on projct settings.
3. Navigate to Settings and click on CI/CD

![runner](./image/runner-details.jpg)

1. Enter your GitLab instance URL (also known as the gitlab-ci coordinator URL).
2. Enter the token you obtained to register the runner.
3. Enter a description for the runner. You can change this value later in the GitLab user interface.
4. Enter the tags associated with the runner, separated by commas.
5. Provide the runner executor. enter ssh
6. Enter server username.
7. Enter server password.
8. Then enter your id_rsa key path.

- After create runner successfully you can see the runner detils in

```bash
# /etc/gitlab-runner/config.toml
```
