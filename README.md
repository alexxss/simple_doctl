DigitalOcean bash Script
=====

[![hackmd-github-sync-badge](https://hackmd.io/6VxoRIWzQPOkpyVqNbI5Xw/badge)](https://hackmd.io/6VxoRIWzQPOkpyVqNbI5Xw)

This script makes it easier to use DigitalOcean's `doctl` tool by simplifying some commonly used Droplet-related commands into selections, so that you can spend less time using `--help` to check for syntax, and more time waiting for the Droplet spin-up time. :smiley: 


> Written and tested on **Ubuntu 18.04** only. Reliability unknown in other environments. Feel free to edit it to suit your needs.

##### About `doctl`: 
> `doctl` allows you to interact with the [DigitalOcean API](#About-DigitalOcean-API) via the command line. It supports most functionality found in the control panel. You can create, configure, and destroy DigitalOcean resources like Droplets, Kubernetes clusters, firewalls, load balancers, database clusters, domains, and more.
> -- [doctl CLI :: DigitalOcean documentation](https://www.digitalocean.com/docs/apis-clis/doctl/)

##### About DigitalOcean API:
> The DigitalOcean API allows you to manage Droplets and resources within the DigitalOcean cloud in a simple, programmatic way using conventional HTTP requests. The endpoints are intuitive and powerful, allowing you to easily make calls to retrieve information or to execute actions.
> -- [DigitalOcean API](https://developers.digitalocean.com/documentation/v2/)


Prerequisites
-----

Some program/driver used in this script as below. Lacking these program would not affect the core functionaly of this script.
 - `notify-send` ([doc](http://manpages.ubuntu.com/manpages/xenial/man1/notify-send.1.html)) is used to send alert after long tasks have done running. (i.e. creating droplets, snapshots...)
- Similar to above, `aplay` is used to play an alert tone. The sound file used is `/usr/share/sounds/sound-icons/prompt`. If the file does not exist in your system, it simply will not play an alert tone, but the pop-up notification will still appear on screen.

### 1. install & authorize `doctl`

Depending on your OS/usage requirements, instructions can be found [here](https://www.digitalocean.com/docs/apis-clis/doctl/how-to/install/#step-1-install-doctl).

### 2. get the script

The script is hosted [here](https://gist.github.com/alexxss/9146292a1acee71104a65cbfe81eceec). Below are a couple of suggested methods to download it.

#### Using `git clone`
After validating your `doctl` installation, download the script using `git clone`:
```bash
# the script will be saved in the folder 'simple-doctl' which
# will be created if it does not exist
# you may replace './simple-doctl' with whatever name you like
> $ git clone https://gist.github.com/9146292a1acee71104a65cbfe81eceec.git ./simple-doctl
# then, chmod so that it becomes executable
> $ chmod +x simple-doctl.sh
```
In the future, you can get the latest version of the script by simply using `git pull`. ~(That\ is,\ if\ the\ script\ will\ even\ be\ getting\ updates\ :p)~ 

#### Using `wget`
Alternatively, you may also get the script via `wget` (why would you do this tho):
```bash
# the script will be saved in the foler 'simple-doctl' which
# will be created if it does not exist
# you may replace 'simple-doctl' with whatever name you like
> $ wget https://gist.githubusercontent.com/alexxss/9146292a1acee71104a65cbfe81eceec/raw/959574068fdebc321d6116444b6657a621860fe9/simple_doctl.sh -P simple-doctl
# then, chmod so that it becomes executable
> $ chmod +x simple-doctl.sh
```

### Usage

make a hardlink from `/usr/bin` to `simple-doctl.sh`. (`sudo` priviledges needed) 'sdoctl' stands for 'simple doctl'
```sh
> $ sudo ln /usr/bin/sdoctl path/to/simple-doctl/simple-doctl.sh
```
edit `~/.bash_aliases` (or create the file if it doesn't exist) and add
```bash=0
alias sdoctl='/usr/bin/sdoctl'
```
changes take effect after you open a new bash. now you can start the script like this:
```
> $ sdoctl
What do you want to do?
1) Show droplets    3) Destroy droplet	5) Delete snapshot
2) New droplet      4) Create snapshot	6) Quit
#? 
```

Features
-----

### show droplets

### new droplet

### destroy droplet

### create snapshot

### delete snapshot

TO-DO
-----
the [list](https://www.notion.so/simple-doctl-df618af3aaea49de9ea1fac2256d7677) is maintained on **Notion**. 
> The ability to embed Notion pages into Hackmd would be nice 🥺 

