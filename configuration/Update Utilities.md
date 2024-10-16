# A word of warning.
You are entering Linux Contry. Its wild, Its keyboardy, Its not as hard as you think.

It also takes time. 


## Update Your OS [Mandatory]

Open your `/etc/sources.list` in your favorite editor, and replace this line:

```bash
deb http://deb.debian.org/debian buster-backports main contrib non-free
```
with this:
```bash
deb http://archive.debian.org/debian buster-backports main contrib non-free
```

Then, update your OS
```bash
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
```
## Python3 [Mandatory]

```bash
sudo apt update && sudo apt upgrade -y
sudo apt-get install lzma liblzma-dev libbz2-dev
```
This is a very interesting why to install python. Still takes forever but it worked. https://github.com/pyenv/pyenv
```bash
curl https://pyenv.run | bash
```
First, add the commands to ~/.bashrc by running the following in your terminal:
```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```
Now you can install whichever version you like; however you should strongly consider 3.10.14 (3.12 has breaking changes).
```
pyenv install 3.10.14
```
wait forever...
```bash
pyenv global 3.10.14
```

## Git [Optional]
If you are running an older version of git.
```bash
git version
git version 2.20.1
```
follow this guide:
https://www.digitalocean.com/community/tutorials/how-to-install-git-on-debian-10

You will need about 150MB of drive space. If /tmp is not large enough move the folder over to a partition that has space (what I did).
