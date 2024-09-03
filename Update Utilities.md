## Git
If you are running an older version of git
```bash
git version
git version 2.20.1
```
follow this guide:
https://www.digitalocean.com/community/tutorials/how-to-install-git-on-debian-10

## Python3

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
Now you can install whichever version you like:
```
pyenv install 3.12.5
```
wait forever...
```bash
pyenv global 3.12.5
```
