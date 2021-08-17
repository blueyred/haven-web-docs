## Haven Protocol Web App
# Building Haven Wallet Web Assembly using Docker

The wallet functionality is provided by a C++ library thats compiled to WebAssembly using [Emscripten](https://emscripten.org/) see the [index page for an overview of the ecosystem](index.md)

To build the web assemblies I'm using Docker and I find the latest stable Debian release (v10 at time of writing) has worked well. Alpine may also be a good choice, I had issues using the latest Ubuntu releases (20.04+), some of the libraries seem not to build correctly. Make sure you have plenty of disk space as setting up the build process will eat up 10-20G.

First we'll build the docker container then go into the environment and build the project (change the naming and tagging for your setup)

The [Dockerfile can be found here](scripts/Dockerfile), copy it into your working directory, then inside the directory with the Dockerfile in,

```bash
> docker build -t blueyred/debianhaven:v10 .
```
This takes around 7 mins to complete for me
```
[+] Building 392.2s (12/12) 
```

Now we can enter the Docker container environment with the [run command](https://docs.docker.com/engine/reference/commandline/run/)

```bash
> docker run -ti blueyred/debianhaven:v10 /bin/bash
```
-ti sets up a command line (-t Allocate a pseudo-TTY and -i Keep STDIN open even if not attached).

You may want to add in --rm to automatically remove the container when it exits which will clean the container back to its original state.

Once inside the container, lets clone the project along with all its sub (and sub sub) modules

```bash
> git clone --recursive https://github.com/haven-protocol-org/haven-web-core.git
```
Next we need to make sure the translation files are built for the main C++ project

```bash
> cd haven-web-core/external/haven-web-cpp/external/haven
> make release
```
 
This will start the build process, the translation headers are all we need, so you can interupt it once they're built, for me they're built after about 30 seconds, once you see
```
[  3%] Completed 'generate_translations_header'
```
now we can copy them into place
```bash
> cp build/release/translations/translation_files.h src/common/translation_files.h
```
we can now build the web assemblies from the main haven-web-core folder

```bash
> cd ../../../..

```
First we need to modify a few of the build script files. There are a set of build shell scripts.
Add a space between the parrallel core option, these files are for a mac environment and we're in a linux one.

**build_openssl_emscripten.sh**
change
```emmake make -j${HOST_NCORES}``` 
to
```emmake make -j ${HOST_NCORES}```

**build_wasm_emscripten.sh**
change
```source ../../utils/emsdk/emsdk_env.sh```
to
```source ../emsdk/emsdk_env.sh```
I also comment out the patching of the version as we're likekly doing some development and it causes issues updating the version in the web app everytime you build the assembly, so comment out the version and set it manually, eg.

```
#NPM_VERSION=$(npm --no-git-tag-version version)
#NPM_VERSION=$(echo $NPM_VERSION | cut -c 2-)
NPM_VERSION=1.4.60
```

I also get build errors if I build with multiple cores so
```emmake cmake --build . -j$HOST_NCORES || exit 1```
to
```emmake cmake --build . || exit 1```
then from the main directory run
```bash
> bin/build_all.sh
```
once this has built correctly the first time then you can just run **build_wasm_emscripten.sh** and **build_web_worker.sh** as the boost and openssl have already been compiled.

The built files will be in the **dist** folder, something like HavenWebWorker1.4.60.js haven_offshore1.4.60.js  haven_offshore1.4.60.wasm.

For development you'll want to change the [emscripten compilation flags](https://emscripten.org/docs/tools_reference/emcc.html) to output debug information, there are some [notes on debugging in the emscripten docs](https://emscripten.org/docs/porting/Debugging.html)
Add the EMCC_DEBUG environemtn variable with 
```
export EMCC_DEBUG=1
```
you could add this into top of the **build_wasm_emscripten.sh** file to make sure its set.

To change the emscripten compiler flags edit the **CMakeLists.txt** in the top level directory, you can find the compiler switches around line 265 after *EMCC_LINKER_FLAGS_BASE*

I update 
```
-s ASSERTIONS=1 
-s EXCEPTION_DEBUG=1 
-s DEMANGLE_SUPPORT=1 
```
remove ```--g0 ```
and add
```
-O2 
-s WASM_BIGINT 
-gseparate-dwarf 
--source-map-base http://localhost:3000/ 
-gsource-map 
-g
```

#### Copy files to the host

On your host (not inside the container) find the id for your running docker container with
```
docker ps
```
which should output something like
```
CONTAINER ID   IMAGE                      COMMAND       CREATED       STATUS       PORTS     NAMES
4649156c832c   blueyred/debianhaven:v10   "/bin/bash"   2 hours ago   Up 2 hours             objective_snyder
```
In this example my container id is **4649156c832c**, then you need to copy each file individually
```
> docker cp 4649156c832c:/usr/local/haven-web-core/dist/haven_offshore1.4.50.js ~/Projects/haven-wasm-compiled/
```


<!---

For me this outputs
```
[+] Building 392.2s (12/12) FINISHED                                                                                                                   
 => [internal] load build definition from Dockerfile                                                                                              0.0s
 => => transferring dockerfile: 2.10kB                                                                                                            0.0s
 => [internal] load .dockerignore                                                                                                                 0.0s
 => => transferring context: 2B                                                                                                                   0.0s
 => [internal] load metadata for docker.io/library/debian:10-slim                                                                                 2.5s
 => [1/8] FROM docker.io/library/debian:10-slim@sha256:c8152821b158dd171b4acf92afb0a58fc2faa179a7e0af8ace358fbe1668e99d                           8.4s
 => => resolve docker.io/library/debian:10-slim@sha256:c8152821b158dd171b4acf92afb0a58fc2faa179a7e0af8ace358fbe1668e99d                           0.0s
 => => sha256:c8152821b158dd171b4acf92afb0a58fc2faa179a7e0af8ace358fbe1668e99d 1.85kB / 1.85kB                                                    0.0s
 => => sha256:ad3a7712a9e3ac324bee1ee2a214be0fdf365ac7ecdd979240fe24edfbd485b4 529B / 529B                                                        0.0s
 => => sha256:df0140a4030c28adc1eee2db92287eaaa736c9c354324506116301ce42588750 1.46kB / 1.46kB                                                    0.0s
 => => sha256:33847f680f63fb1b343a9fc782e267b5abdbdb50d65d4b9bd2a136291d67cf75 27.15MB / 27.15MB                                                  6.5s
 => => extracting sha256:33847f680f63fb1b343a9fc782e267b5abdbdb50d65d4b9bd2a136291d67cf75                                                         1.8s
 => [2/8] RUN set -ex &&     apt-get update &&     apt-get upgrade &&     apt-get --no-install-recommends --yes install     build-essential     173.8s
 => [3/8] RUN mkdir -p /usr/share/man/man1 /usr/share/man/man2                                                                                    0.3s 
 => [4/8] RUN set -ex &&  apt-get --no-install-recommends --yes install default-jdk                                                              85.0s 
 => [5/8] RUN set -ex && update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1 && update-alternatives --install /usr/bin/pyt  0.3s 
 => [6/8] WORKDIR /usr/local                                                                                                                      0.0s 
 => [7/8] RUN set -ex     && git clone https://github.com/emscripten-core/emsdk.git     && cd emsdk     && ./emsdk install latest-upstream      105.5s 
 => [8/8] RUN /bin/bash -c "source /usr/local/emsdk/emsdk_env.sh"                                                                                 0.6s 
 => exporting to image                                                                                                                           15.6s 
 => => exporting layers                                                                                                                          15.6s 
 => => writing image sha256:3caf70462acbd7a346e401edcd501f5a6e1aadc21aeb935eae31ae0f1ecc9a6c                                                      0.0s 
 => => naming to docker.io/blueyred/debianhaven:v10                                                                                               0.0s
 ```
-->



<!---
#
# Note if running in a Virtualbox shared folder
#
# test by doing this:
# touch myfile.txt
# ln -s myfile.txt symbolicfile.txt
# ln: failed to create symbolic link 'symbolicfile.txt': Operation not permitted
#
#
# VirtualBox 6: How to enable symlinks for shared folders
# For security reasons, creating symbolic links in a shared folder is disabled in the guest OS (ticket 10085 and manual 5.3 Shared Folders). 
# If you trust your Linux guest OS, you can enable symlinking from the host OS with the following command:
#
# ~ $ VBoxManage setextradata "{VM_Name}" VBoxInternal2/SharedFoldersEnableSymlinksCreate/{Folder_Name} 1
#
# Replace {VM_Name} and {Folder_Name} with your own names. To get a list of names of your virtual machines, execute in the host console:
# ~ $ VBoxManage list vms
#
# You can also list information about a specific machine with:
# ~ $ VBoxManage showvminfo "VM_Name"
# Now you can start your Linux guest OS and create the symbolic link
# ~ $ sudo ln -s /media/sf_Websites /var/www/html
-->

---
> [Web Assembly :arrow_right:](web-assembly.md)
---
