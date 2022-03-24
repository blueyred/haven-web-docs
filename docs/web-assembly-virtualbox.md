---
layout: default
title: Web Assembly - Virtualbox
nav_order: 7
---
## Haven Protocol Web App
# Building Haven Wallet Web Assembly using Virtual Box

The wallet functionality is provided by a C++ library thats compiled to WebAssembly using [Emscripten](https://emscripten.org/) see the [index page for an overview of the ecosystem](index.md)

To build the web assemblies I'm using the latest stable Debian release (v10 at time of writing) has worked well. Alpine may also be a good choice, I had issues using the latest Ubuntu releases (20.04+), some of the libraries seem not to build correctly. Make sure you have plenty of disk space as setting up the build process will eat up 10-20G.

At the time of writing (Aug 2021) I'm using a minimal system to being with debian-10.10.0-amd64-netinst.iso as a minimal system and I'll add the various libraries form there. During installation I'm adding , standard system utilites and ssh.

I've setup with a 25G Virtual Hard disk, 4G ram, 4 cores and bridged networking.

Once booted up get your VMs ip address
```
ip addr
```
then ssh in (I find this easier, as you have your native clipboard, copy paste etc..)
```
ssh user@192.168.0.100
```
install the guest additions - 
```
su
apt install build-essential dkms linux-headers-$(uname -r)
mkdir -p /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
cd /mnt/cdrom
cp VBoxLinuxAdditions.run ~/VBoxLinuxAdditions.run
cd
./VBoxLinuxAdditions.run --nox11
```
create a mount point for a shared folder

```
mkdir /shared
chmod 777 /shared
```
add sbin paths into /root/.bashrc at bottom of file:
```
export PATH=$PATH:/sbin:/usr/sbin
```

I find building within a shared folder that both host and VM can access the most practicle option, however there's a small bit of house keeping we need to do first to ensure symbolic links work correctly. 

## VirtualBox enable symlinks for shared folders
Shared folders by default don't allow symbolic links
Inside the VM check (as things change) this by
```
> touch myfile.txt
> ln -s myfile.txt symbolicfile.txt
ln: failed to create symbolic link 'symbolicfile.txt': Operation not permitted
```
If you get the *Operation not permitted* then lets enable them.

**Important - shutdown the machine while you do this**

From your host system (not in the VM!)
```
> VBoxManage setextradata "{VM_Name}" VBoxInternal2/SharedFoldersEnableSymlinksCreate/{Folder_Name} 1
```
Replace {VM_Name} and {Folder_Name} with your own names. To get a list of names of your virtual machines, execute in the host console:
```
> VBoxManage list vms
```
 You can also list information about a specific machine with:
```
> VBoxManage showvminfo "VM_Name"
```
In my case 
```
VBoxManage setextradata "Debian10" VBoxInternal2/SharedFoldersEnableSymlinksCreate/shared 1
```
Now you can start your Linux guest OS and create the symbolic link
```
> ln -s myfile.txt symbolicfile.txt
```

### Install packages
Use the Dockerfile as a guide for the packages to install, you don't need the "RUN" or "set -ex" parts and the main install lines can be copied in as they are. [Dockerfile can be found here](scripts/Dockerfile)

add into /root/.bashrc at bottom of file:
```
export EMSCRIPTEN=/shared/emsdk/upstream/emscripten
source "/shared/emsdk/emsdk_env.sh"
```
Note: this can cause some error when running **npm install** in the haven-web-core directory, as it finds the node modules folder in the emsdk folder, so you may need to comment them out logout and *possibly reboot*.


Now build we can start building the projects
```
git clone --recursive https://github.com/haven-protocol-org/haven-web-core.git
```
Next we need to make sure the translation files are built for the main C++ project
```bash
> cd haven-web-core/external/haven-web-cpp/external/haven
mkdir build
mkdir build/release
cd build/release/
cmake ../..
make obj_common
cd ../..
cp build/release/translations/translation_files.h src/common/translation_files.h
```

before building the web worker you'll need the node modules so
```
npm install
npm install --save-dev webpack
npm install --save-dev webpack-cli
```
and 
```
apt install webpack
```


---
[Electron App]({{ site.baseurl }}{% link docs/electron-desktop-app.md %}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---
