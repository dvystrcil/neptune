# Updating Klipper, Moonraker, Fluidd and Kiauh

The goal is to bring the N4 series up to date. There are still some outstanding tasks, but it's looking very promising.

## Tasks:
[x] Update python to 3.12.5
[x] Use the alpha Kiauh
[x] Update Fluidd
[x] Update Moonraker
[x] Update Klipper
[ ] See what breaks, document, create patches?
[ ] What is so Elegoo about the their Klipper?
[ ] What happens when you do a firmware update? There are files for Klipper and Moonraker in the extras.
[ ] Education. Its OK if you have untracked files reported in Fluidd. You might have installed something like autotune or califlower

## Step 1) Install the mandatory tools
https://github.com/dvystrcil/neptune/blob/main/Update%20Utilities.md

## Step 2) Update Kiauh
```bash
cd ~/kiauh
pyenv local 3.10.14
./kiauh.sh
```
If it asks you about the new alpha v6 it's safe to try it out. I used it exclusively through this whole exercise.

## Step 3) Update Fluidd
Do this from Kiauh. No need to wait.

## Step 4) Update Moonraker
Note: If you have followed my previous guild that installs Krakjoe's repos, you must delete the `.git` folder.
```bash
cd ~/moonraker
# sudo rm -rf .git # see note
git init
git remote add origin https://github.com/Arksine/moonraker.git
git fetch origin master
git reset --hard FETCH_HEAD
git pull origin master
./scripts/data-path-fix.sh
```

## Step 5 Update Klipper

### Part A Back up your folders
```bash
cd ~
sudo mv klipper klipper.bak
sudo mv klipper-env klipper-env.bak
```

### Part B Update the python venv
```bash
pyenv global 3.10.14
pip3 install --upgrade pip
pip3 install --upgrade virtualenv
virtualenv -p python3 klippy-env
```

### Part C Get the latest Klipper
```bash
cd ~/klipper
# sudo rm -rf .git # see note
git init
git remote add origin https://github.com/Klipper3d/klipper.git
git fetch origin master
git reset --hard FETCH_HEAD
git pull origin master
```

### Part D Restore any Elegoo specific files
Note: It is still not known if there are more files than these
```bash
cd ~/klipper.bak
cp -R -u -p . ~/klipper
```

Restart your klipper
```bash
sudo systemctl restart klipper
```
Other commands are:
- systemctl status klipper
- sudo systemctl stop klipper
- sudo systemctl start klipper
