Kiauh v6 needs at least python v3.8. My printer had 3.7... time for some scripting.

Maybe a few cups of coffee.

A danish...

```bash
sudo apt update && sudo apt upgrade -y
sudo apt-get install lzma liblzma-dev libbz2-dev
```

This is a very interesting why to install python. Still takes forever but it worked.
https://github.com/pyenv/pyenv

```bash
curl https://pyenv.run | bash
pyenv install 3.10.14
pyenv global 3.10.14
pip3 install --upgrade pip
```

https://klipper.discourse.group/t/process-for-migrating-to-python3/5292/7

```bash
pip3 install --upgrade virtualenv
virtualenv -p python3 klippy-env
```
