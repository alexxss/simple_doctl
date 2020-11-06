DigitalOcean bash Script
=====

[![hackmd-github-sync-badge](https://hackmd.io/6VxoRIWzQPOkpyVqNbI5Xw/badge)](https://hackmd.io/6VxoRIWzQPOkpyVqNbI5Xw)

This script makes it easier to use DigitalOcean's `doctl` tool by simplifying some commonly used Droplet-related commands into selections, so that you can spend less time using `--help` to check for syntax, and more time waiting for the Droplet spin-up time. :smiley: 

**NOTE:**  Written and tested on **Ubuntu 18.04** only. Reliability unknown in other environments. Below are some program/driver used by this script. Lacking these program would not affect the core functionaly of this script.
 - `notify-send` ([doc](http://manpages.ubuntu.com/manpages/xenial/man1/notify-send.1.html)) is used to send alert after long tasks have done running. (i.e. creating droplets, snapshots...)  
    ![](https://i.imgur.com/4vg1CvV.png)
- Similar to above, `aplay` is used to play an alert tone. The sound file used is `/usr/share/sounds/sound-icons/prompt`. If the file does not exist in your system, it simply will not play an alert tone, but the pop-up notification will still appear on screen.


##### About `doctl`: 
> `doctl` allows you to interact with the [DigitalOcean API](#About-DigitalOcean-API) via the command line. It supports most functionality found in the control panel. You can create, configure, and destroy DigitalOcean resources like Droplets, Kubernetes clusters, firewalls, load balancers, database clusters, domains, and more.
> -- [doctl CLI :: DigitalOcean documentation](https://www.digitalocean.com/docs/apis-clis/doctl/)

##### About DigitalOcean API:
> The DigitalOcean API allows you to manage Droplets and resources within the DigitalOcean cloud in a simple, programmatic way using conventional HTTP requests. The endpoints are intuitive and powerful, allowing you to easily make calls to retrieve information or to execute actions.
> -- [DigitalOcean API](https://developers.digitalocean.com/documentation/v2/)


Prerequisites
-----

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
> $ chmod +x simple-doctl/simple-doctl.sh
```
In the future, you can get the latest version of the script by simply using `git pull`. <sub>(That is, if the script will even be getting updates :p)</sub>

#### Using `wget`
Alternatively, you may also get the script via `wget` (why would you do this tho):
```bash
# the script will be saved in the foler 'simple-doctl' which
# will be created if it does not exist
# you may replace 'simple-doctl' with whatever name you like
> $ wget https://gist.githubusercontent.com/alexxss/9146292a1acee71104a65cbfe81eceec/raw/959574068fdebc321d6116444b6657a621860fe9/simple_doctl.sh -P simple-doctl
# then, chmod so that it becomes executable
> $ chmod +x simple-doctl/simple-doctl.sh
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
<pre><font color="#8AE234"><b>alex@DESKTOP</b></font>:<font color="#729FCF"><b>~</b></font>$ sdoctl

What do you want to do?
1) Show droplets
2) Show snapshots
3) New droplet
4) Destroy droplet
5) Create snapshot
6) Delete snapshot
7) Quit
#? 2
</pre>

Features
-----

### 1. show droplets
<pre><font color="#555753"><b>Obtaining list of droplets...</b></font>
ID           Name       Public IPv4       Status    Features
xxxxxxxxx    droplet    xxx.xxx.xxx.xx    active    monitoring,private_networking
</pre>

### 2. show snapshots
<pre><font color="#555753"><b>Obtaining list of snapshots...</b></font>
ID          Name                      Created at              Size
xxxxxxxx    asamplesnapshot1234567    2020-08-23T16:46:36Z    3.30 GiB
xxxxxxxx    snapshot123               2020-11-05T15:18:03Z    4.89 GiB
</pre>

### 3. new droplet
#### From snapshot ID
<pre>Snapshot ID? (leave blank to abort): xxxxxxxx
Droplet name? (leave blank to abort): droplet
<font color="#555753"><b>Creating droplet... (An alert will be sent when process is done. Go do other stuff!)</b></font>
ID           Name       Public IPv4       Private IPv4    Public IPv6    Memory    VCPUs    Disk    Region    Image                            VPC UUID                                Status    Tags    Features                         Volumes
xxxxxxxxx    droplet    xxx.xxx.xxx.xx    xx.xxx.x.x                     8192      2        25      sgp1      Ubuntu asamplesnapshot1234567    xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx    active            monitoring,private_networking    
</pre>

#### From list of snapshots
<pre>Select which snapshot to <font color="#4E9A06">create droplet</font> from: 
<font color="#555753"><b>Obtaining list of snapshots...</b></font>
   ID          Name
1) xxxxxxxx    asamplesnapshot1234567
2) xxxxxxxx    snapshot123
3) back
#? 2
Selected snapshot xxxxxxxx
Droplet name? (leave blank to abort): droplet
<font color="#555753"><b>Creating droplet... (An alert will be sent when process is done. Go do other stuff!)</b></font>
ID           Name       Public IPv4       Private IPv4    Public IPv6    Memory    VCPUs    Disk    Region    Image                 VPC UUID                                Status    Tags    Features                         Volumes
xxxxxxxxx    droplet    xxx.xxx.xx.xxx    xx.xxx.x.x                     8192      2        25      sgp1      Ubuntu snapshot123    xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx    active            monitoring,private_networking    
</pre>

### 4. destroy droplet
<pre><font color="#C4A000">Remember to backup/snapshot before deleting droplets!</font>
Select which droplet to <font color="#C4A000">delete</font>: 
<font color="#555753"><b>Obtaining list of droplets...</b></font>
   ID           Name
1) xxxxxxxxx    droplet
2) back
#? 1
</pre>
You will be prompted to take a snapshot before actually deleting the droplet.
<pre>Create snapshot of droplet? [y/N]:y
Snapshot name? (leave blank to abort): snapshot
<font color="#555753"><b>Creating snapshot... (An alert will be sent when process is done. Go do other stuff!)</b></font>
ID            Status       Type        Started At                       Completed At                     Resource ID    Resource Type    Region
xxxxxxxxxx    completed    snapshot    2020-11-06 11:14:38 +0000 UTC    2020-11-06 11:18:07 +0000 UTC    xxxxxxxxx      droplet          sgp1
<font color="#C4A000">Warning</font>: Are you sure you want to delete this Droplet? (y/N) ? y
<font color="#4E9A06">Success.</font>
</pre>
If you choose to not create snapshot, you will proceed to droplet deletion.
<pre>Create snapshot of droplet? [y/N]:n
<font color="#C4A000">Warning</font>: Are you sure you want to delete this Droplet? (y/N) ? y
<font color="#4E9A06">Success.</font>
</pre>

### 5. create snapshot
<pre>Select which droplet to <font color="#4E9A06">create snapshot</font> from: 
<font color="#555753"><b>Obtaining list of droplets...</b></font>
   ID           Name
1) xxxxxxxxx    droplet
2) back
#? 1
Snapshot name? (leave blank to abort): snapshot
<font color="#555753"><b>Creating snapshot... (An alert will be sent when process is done. Go do other stuff!)</b></font>
ID            Status       Type        Started At                       Completed At                     Resource ID    Resource Type    Region
xxxxxxxxxx    completed    snapshot    2020-11-06 11:09:05 +0000 UTC    2020-11-06 11:12:38 +0000 UTC    xxxxxxxxx      droplet          sgp1
</pre>

### 6. delete snapshot
<pre>Select which snapshot to <font color="#C4A000">delete</font>: 
<font color="#555753"><b>Obtaining list of snapshots...</b></font>
   ID          Name
1) xxxxxxxx    asamplesnapshot1234567
2) xxxxxxxx    snapshot123
3) 73007561    snapshot
4) back
#? 3
<font color="#C4A000">Warning</font>: Are you sure you want to delete this snapshot? (y/N) ? y
<font color="#4E9A06">Success.</font>
</pre>

TO-DO
-----
the [list](https://www.notion.so/simple-doctl-df618af3aaea49de9ea1fac2256d7677) is maintained on **Notion**. 
> The ability to embed Notion pages into Hackmd/Github readme would be nice 🥺 

