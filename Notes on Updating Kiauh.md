Kiauh v6 needs at least python v3.8. My printer had 3.7... time for some scripting.

Maybe a few cups of coffee.

A danish...

```bash filename=update-python.sh
PY_VERSION=3.12.5

sudo apt update && sudo apt upgrade -y
sudo apt install wget software-properties-common build-essential libncursesw5-dev libreadline-gplv2-dev libssl-dev libsqlite3-dev tk-dev libc6-dev libbz2-dev libffi-dev -y 

wget https://www.python.org/ftp/python/${PY_VERSION}/Python-${PY_VERSION}.tgz
tar xvf Python-${PY_VERSION}.tgz

cd Python-${PY_VERSION}/
./configure --enable-optimizations
make install

python3 -V

echo "print('Hello, you are running python')" > test.py
chmod +x test.py
/usr/bin/python3 test.py
rm test.py

cd .. Rm -rf Python-3.1.* sudo aptitude clean
```

Run
```bash
chmod +x update-python.sh
sudo ./update-python.sh
```

Or not... pyenv seems like a better tool
https://github.com/pyenv/pyenv

