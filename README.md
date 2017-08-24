kb_builder
==========

**NOTE: The CAD engine (KAD) which powers the current [builder.swillkb.com](http://builder.swillkb.com) builder has been open sourced and is available here [github.com/swill/kad](https://github.com/swill/kad).  Enjoy...**

`kb_builder` is the original engine that ran [builder.swillkb.com](http://builder.swillkb.com).  That service has been completely rewritten in Golang, but this project is still a useful environment to hack on.  

This tool uses the JSON layout generated by the [Keyboard Layout Editor](http://www.keyboard-layout-editor.com/).  There are a lot of hidden features that you can do when building plates.  Check the `?` next to the `Plate Layout` text field in the UI for more details.

Here are some examples of the types of features that are exposed. The following features can be applied by modifying the source copied from the keyboard layout editor. All of these features are applied to individual keys by adding or modifying the object before the key. EG: rotate switch for "Enter" would be done by entering `{_r:90},"Enter"`.

* `{_t:<0-2>}`: Change switch cutout type. Numbers are in the same order as the images. EG: `{_t:0},""`
* `{_s:<0-2>}`: Change stabilizer cutout type. Numbers are in the same order as the dropdown list. EG: `{_s:2},""`
* `{_k:<mm>}`: Specify a kerf value for this specific switch/stabilizer cutout which overrides the default. Values are in mm (without the * unit). EG: `{_k:0.05},""`
* `{_r:<degrees>}`: Rotate the switch cutout independent of the stabilizer cutout (assuming there is one). EG: `{_r:90},""`
* `{_rs:<degrees>}`: Rotate the stabilizer cutout independent of the switch cutout. EG: `{_rs:180},""`

This tool is implemented as a webserver and exposes a UI to be consumed in the browser, but it is not fit for actual web traffic because it can not handle drawing more than one layout at a time.  This however should not be a limitiation for personal use.


## Installation and Configuration


### Quick Start Guide

The easiest way to run this application is locally on a
[VirtualBox](https://virtualbox.org) VM that is deployed with
[Vagrant](https://vagrantup.com).


**Instructions**

``` bash
$  # = host
$$ # = guest vm

# get the kb_builder
$ git clone https://github.com/swill/kb_builder.git
$ cd kb_builder

# deploy kb_builder
$ vagrant up
$ vagrant ssh
$$ cd kb_builder && sudo ./kb_builder.py
# access the kb_builder on the host at http://localhost:8080

# to stop the kb_builder do `Ctrl + \` in the guest terminal, then
$$ exit
$ vagrant halt
```

On the `guest` VM you will have a copy of code checked out in the
home directory.  Test changes and hack the source here. The builder
is accessible on the `host` machine at the URL `http://localhost:8080`.



### Manual Install

This tool is easiest to run on a Ubuntu VM.  You can run it on your laptop with
either VirtualBox or VMware Fusion.  I have had trouble getting the FreeCAD lib
to work correctly on Mac OSX natively, so if you get it working, please contribute
documentation.  Here are the Ubuntu install instructions.


**Install the OS dependencies**

``` bash
$ sudo add-apt-repository --yes ppa:freecad-maintainers/freecad-daily
$ sudo apt-get update
$ sudo apt-get --yes install build-essential python-dev python-pip git freecad unzip
```


**Get the source and install python dependancies**

``` bash
$ git clone https://github.com/swill/kb_builder.git
$ sudo pip install --requirement kb_builder/requirements.txt
```


**Install the Draft-dxf-importer**

This is a quick start guide.  Review the [full docs
here](https://github.com/yorikvanhavre/Draft-dxf-importer)

``` bash
$ cd ~/
$ sudo freecad
	# 'sudo' so it creates the correct '/root/.FreeCAD' directory
	# note the version number
	# download the appropriate version from here: 
	# 	https://github.com/yorikvanhavre/Draft-dxf-importer
	# i will assume freecad version v16 and the latest importer (v15)
$ wget https://github.com/yorikvanhavre/Draft-dxf-importer/archive/1.38.zip
$ unzip 1.38.zip
	# this will create the 'Draft-dxf-importer-1.38/' directory
$ cp Draft-dxf-importer-1.38/* /root/.FreeCAD/
	# Note: we are putting this import into the 'root' users '.FreeCAD' 
	#	not your users because we are running with 'sudo'
```


**Run the source**

``` bash
$ cd ~/kb_builder && sudo ./kb_builder.py
```


## License

```
kb_builder builds keyboard plate and case CAD files using JSON input.

Copyright (C) 2015-2016  Will Stevens (swill)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
```
