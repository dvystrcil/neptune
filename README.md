# How to Update Neptune 4 Using krakjoe's Method

Laying down some expectations... I put this repo together to accomplish an few goals
- A place to back up my configuration
- A place to update my Neptune 4 Pro using automation
- A place to practice using Ansible

I am leaning heavily on the work done by krakjoe a user out on the Elegoo Discord. He was able to provide and update Klipper and Moonraker, as well as steps and configs to get the Neptune 4 up to spec with the world. I am not an expert in 3D printers, or Klipper, or Moonraker... this is a hobby I enjoy dabbling in. 

If you ask me for support, please read that last sentence again. I can only help with things that I have personally written in this repo.

What to expect from this repo.
- A guide to install krakjoe work. I hope you can leaverage the ansible playbooks I have written.
- A veiw into my personal printer configs. Nothing special here folks, your printer config will likely be different. Move along...

## Installation

### Requirements

You need to do a few things before you begin
- Install Ansible
- Fork or pull this repo to where you are doing the work. Cloning to your printer is OK if you have the space available.
- Update your printer to the latest firmware. If you know what you are doing you could probably use older firmware, but I started by updating to `NEPTUNE 4 PRO - APP_V1.1.3.1-UI_1.2.14-20240705-EN` which is for my printer (Neptune 4 Pro).

### Step 0 The Automated Way of 1 thru 3

Clone this repo
```bash
git clone https://github.com/dvystrcil/neptune.git
cd neptune
```

Change directory to the `ansible` folder, make `run-update.sh` executable, and run it
```bash
cd ansible
sudo chmod +x run-update.sh
./run-update.sh
```

Let ansible do its thing. It should take about 5 minutes. If a reboot occurs then a kernal update was applied (rare occurance). Just run-update.sh again.

#### Extra Credit: Correct your Time Zone
Edit the `playbooks/system/time-zone.yaml` so the `name: America/Los_Angeles` reflects the zone you want. Then come back to the ansible directory and run:

```bash
ansible-playbook -i inventory.ini playbooks/system/time-zone.yaml
```

More Extra Credit: You may have caught on that the playbooks are fairly granular. This means you can pick what task you want, even mix them around, or edit them to your liking. There are tons of resources about ansible out there and to be honest their documenation is pretty good.

### Step 1 Update Your OS

So you seem to be someone who likes to type a lot. Fair enough.

Update your sources, because if they are original, its broken.

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

### Step 2 Update Klipper

Here is where you get to see how we are leaning into krakjoe's work. 

```bash
cd /home/mks/klipper
git init
git remote add n4.klipper https://git.krakjoe.dev/n4.klipper.git
git fetch n4.klipper n4/master
git reset --hard FETCH_HEAD
git pull n4.klipper n4/master
```

### Step 3 Update Moonraker

```bash
cd /home/mks/moonraker
git init
git remote add n4.moonraker https://git.krakjoe.dev/n4.moonraker.git
git fetch n4.moonraker n4/master
git reset --hard FETCH_HEAD
git pull n4.moonraker n4/master
./scripts/data-path-fix.sh
```

A note about `data-path-fix.sh`: This is a script which corrects all the open source Klipper and Moonraker pathing to Elegoo's requirements. If you update either Klipper or Mookraker using krakjoe work, its important to run this again.

### Step 4 Update Kiauh and Fluidd

Here is where I stopped trying to automate things. As it is Kiauh is already a nice tool why not use it?

```bash
cd /home/mks/kiauh
```
If it asks to update Kiauh, say yes. Run it again if needed. When you get to the menu you want to choose Update, and then Fluidd. If you update Klipper or Mookraker, you are getting the non-elegoo versions. It will overwrite all the work you have just done in steps 2 and 3.

![image](https://github.com/user-attachments/assets/1c17d1ec-3113-4e1a-a31e-3fe74707f77c)

![image](https://github.com/user-attachments/assets/c7a26acb-00e7-4659-ae49-eef40338e6b6)



### Step 5 Update Your Printer Configuations

TBD - lots of compare edit work

I automated some parts

- moonraker.config
- printer.conf

![image](https://github.com/user-attachments/assets/cc4007b7-ddab-4326-9132-b35f0ee12ec6)