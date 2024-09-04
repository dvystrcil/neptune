# Updating Klipper, Moonraker, Fluidd and Kiauh

The goal is to bring the N4 series up to date. There are still some outstanding tasks, but it's looking very promising.

Why I chose Kiauh instead of a completely scripted solution is to keep the sources of truth for all these tools. I don't want to document how to install all these parts whole cloth, and maintaining them would be painful. If there are differences, and I expect there will be since many of us have bricked in the past, what are they? If we can understand how Elegoo did things we can repeat them, and perhaps improve on them.

The bottom line, let the contributors of Klipper, Moonraker, Fluidd, and Kiauh do their thing. Our focus is to alter just the bits the printer needs to function.

![image](https://github.com/user-attachments/assets/e2ce18da-573f-48e7-ba5b-96bbbc1b12ec)

Note: Consider the work here at a BETA stage. Use at your own risk. Restoring can be as simple as coping a folder back.

## Tasks:
- [x] Update python to 3.12.5
- [x] Use the alpha Kiauh
- [x] Update Fluidd
- [x] Update Moonraker
- [x] Update Klipper
- [ ] See what breaks, document it, and create patches?
- [ ] What is so Elegoo about the their Klipper?
- [ ] What happens when you do a firmware update? There are files for Klipper and Moonraker in the extras.
- [ ] Education. It's OK if you have untracked files reported in Fluidd. You might have installed something like autotune or califlower

Side Note: This effort does not supersede the excellent work being done over on the [OpenNept4une](https://github.com/OpenNeptune3D/OpenNept4une). At some point, the N4 series will be EOL by Elegoo and OpenNept4ne will be the path forward.

## Step 1) Install the mandatory tools
https://github.com/dvystrcil/neptune/blob/main/Update%20Utilities.md

## Step 2) Update Kiauh
```bash
cd ~/kiauh
pyenv local 3.10.14 # using the local option will pin it for that folder.
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
Note: It is still not known if there are more files than these, or even if we need all of them.
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
