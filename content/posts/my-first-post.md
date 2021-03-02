---
title: "Linux Note"
date: 2021-03-02T21:43:57+08:00
draft: false
images: ["/images/seal.jpg"]

---

This article offers a sample of basic Markdown syntax that can be used in Hugo content files, also it shows whether basic HTML elements are decorated with CSS in a Hugo theme.
<!--more-->


### jupyter notebook add kernal
```sh
python3 -m ipykernel install --user --name=tf-2.2.0
jupyter kernelspec list
jupyter kernelspec remove mykernel
```

### 移除一天以前修改的 folder
```
find path -type d -ctime +1 -exec rm -rf {} \;
```

### grep
```
grep 'keyword' file.txt
-n 行號
-i 忽略大小寫
-v 反向選擇
-A3 -B2 同時輸出前三行後兩行
```

### delete all file except one
```
ls | grep -v file.txt | xargs rm
find . ! -name "*.c" | xargs rm -r
```

### find and remove
```shell
find /path/to -name “*.log” -exec rm {} \;
```

### wget
```
wget http://ftp.gnu.org/gnu/wget/wget-1.19.tar.gz # 下載檔案

wget http://ftp.gnu.org/gnu/wget/wget-1.19.tar.gz \
  http://ftp.gnu.org/gnu/wget/wget-1.19.tar.gz.sig
  
-O # 指定檔名
-c # 檔案續傳
-b # 背景下載
```
with account and password
```
wget --http-user=my_user \
     --http-password=my_password \
     http://www.example.com/my_file

wget --ftp-user=my_user \
     --ftp-password=my_password \
     ftp://ftp.example.com/my_file
```

download file from ftp server
```
wget -r --user="user@login" --password="Pa$$wo|^D" ftp://server.com/
```

### curl
download file
```
curl -o download_name https://...
curl -O https://...
```

### Compressed
#### unzip
For almost every kind of file
```
bsdtar xf file_name
```
#### zip
壓縮資料夾並命名為 file
```
zip file data/*
```

### Edit $PATH
```
export PATH=$PATH:/your/new/path/here
```

### Change bash
```
cat /etc/shells
chsh -s /bin/bash
```

### Count word
```
wc -l file #行數
wc -w file #字數
wc -c file #位元組數
```

### find the original file a link point to
``` bash
# show the root file
readlink -f /path/file

# show only next level of the link
readlink /path/file
```

### pacman
``` bash
# uninstall package
pacman -R package-name
# uninstall package and all it's depedence
pacman -Rcns package-name
```

# Git
### Rename
* ```git mv original_name new_name```
* rename name which only different in uppercase and lowercase
* ```git mv Original_name tmp && git mv tmp original_name```

### Go back to prrversion
go to some commit
```git checkout f5e9273```
go back to master
```git checkout master```

### Delete all the change which aren't commited
We add some file, edit some file, delete some file after commit. Some of them are added. We want to go back to the status of last commit. 
```git reset -- hard```
We can't use ```git checkout HEAD``` since checkout only can be uesd when everything is commited.

### Detailed commit hostory
```git log -p file.html```

### Who wrote the code
```git blame index.html```

只輸出 5 ~ 10 行
```git blame -L 5,10 index.html```


### Refusing to merge unrelated histories
Problem:
1. Create a repo on the github with README.md and commit with initial commit.
2. Create some file in a local directory.
3. Commit in local directory.
4. ```git remote add origin https://github.com/myrepo``` at local
5. git push origin master -> ask for pulling
6. git pull origin master -> Refusing to merge unrelated histories

Solution: 
```git pull --allow-unrelated-histories origin master```

### Ignore the rule of .gitignore
```git add -f .```

### Untrack file
A file had been tracked by git, and it's now added to gitignore.
If we modifiy the file, git can still see the change and it can be commit.
To move the file out of git's control. ```git rm --cached file.txt```

### Delet untracked file
```
git clean
-n 演習
-f remove untracked file (except files in gitignore)
-d remove untracked directory
-x remove untracked file and directory even if it's in gitiignore.
```

### delete file from git history
```
git rm --cached file_name
git commit --amend -CHEAD
# Amend the previous commit with your change
# Simply making a new commit won't work, as you need
# to remove the file from the unpushed history as well
git push origin master
```


# Logic Design Laboratory
### Testbench
Add below code to the testbench
```
initial begin
    $fsdbDumpfile("Mealy_Sequence_Detector_t.fsdb");
    $fsdbDumpvars;
end
```
### Compile
```
ncverilog file.v file_t.v +access+r
```
### Simulation
```
nWave
```

# Arch linux installation


### install llvm and clang
* LLVM with clang full package (include openMP)
``` bash
# Download source code from official website
# --depth 1 specify only download the newest version without history commit
git clone https://github.com/llvm/llvm-project.git lllvm --depth 1

# create a folder under .pkg to for installed llvm
mkdir llvm-9.0.0

# create a forlder build/ for store file during the build phase
cd llvm-9.0.0.src
mkdir build
cd build

# generate makefiles with cmake
cmake -GNinja ../ -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_PROJECTS="openmp;clang" -DCMAKE_INSTALL_PREFIX=$HOME/.pkg/llvm-9.0.0

# compile and install
ninja
ninja install

# add llvm to $PATH
# this command is for fish terminal
set PATH $HOME/.pkg/llvm-9.0.0/bin $PATH
set -Ux LD_LIBRARY_PATH $HOME/.pkg/llvm-9.0.0/lib
```

* LLVM
``` bash
cd ~
mkdir .pkg
cd .pkg/

# Download source code from official website
wget http://releases.llvm.org/9.0.0/llvm-9.0.0.src.tar.xz

# unzip
bsdtar xf llvm-9.0.0.src.tar.xz

# create a folder under .pkg to for installed llvm
mkdir llvm-9.0.0

# create a forlder build/ for store file during the build phase
cd llvm-9.0.0.src
mkdir build
cd build

# generate makefiles with cmake
cmake -GNinja ../ -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=$HOME/.pkg/llvm-9.0.0/

# compile and install
ninja
ninja install

# add llvm to $PATH
# this command is for fish terminal
set PATH $HOME/.pkg/llvm-9.0.0/bin $PATH
```

* clang
``` bash
cd ~/.pkg/

# Download source code from official website
wget http://releases.llvm.org/9.0.0/cfe-9.0.0.src.tar.xz

# unzip
bsdtar xf cfe-9.0.0.src.tar.xz

# create a folder under .pkg to for installed llvm
mkdir cfe-9.0.0

# create a forlder build/ for store file during the build phase
cd cfe-9.0.0.src
mkdir build
cd build

# generate makefiles with cmake
# note that we need to temporarily set $LLVM_HOME before configure cfe, where is the prefix of LLVM
env LLVM_HOME=$HOME/.pkg/llvm-9.0.0/ cmake -GNinja ../ -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=$HOME/.pkg/cfe-9.0.0

# compile and install
ninja
ninja install

# add clang to $PATH
# this command is for fish terminal
set PATH $HOME/.pkg/cfe-9.0.0/bin $PATH
```

### install modules
``` bash
cd ~/.pkg/
wget https://sourceforge.net/projects/modules/files/Modules/modules-4.3.1/modules-4.3.1.tar.gz
bsdtar xf modules-4.3.1.tar.gz
mkdir modules-4.3.1-gcc-9.2.0
cd modules-4.3.1/

# generate makefile with configure
./configure --prefix=$HOME/.pkg/module-4.3.1-gcc-9.2.0

# compile and install
make -j8
make install

# init module under fish terminal
cd ~/.pkg/module-4.3.1-gcc-9.2.0/init/
source fish

# add this to config.fish 
vim .config/fish/config.fish
source $HOME/.pkg/module-4.3.1-gcc-9.2.0/init/fish
```

### install open MPI
``` bash
cd ~/.pkg/
wget https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.2.tar.bz2
bsdtar xf openmpi-4.0.2.tar.bz2
mkdir openmpi-4.0.2-clang-900
cd openmpi-4.0.2/

# generate makefile with configure
# it will use clang to compile openmpi
env CC=clang CXX=clang++ ./configure --prefix=$HOME/.pkg/openmpi-4.0.2-clang-900/

# compile and install
make -j8
make install

# add this to config.fish 
vim .config/fish/config.fish
set PATH $HOME/.pkg/openmpi-4.0.2-clang-900/bin $PATH
```

### install HDF5 (for openmc)
``` bash
mkdir hdf5-1.10.5-mpicxx-900/
cd hdf5-1.10.5/

# create a forlder build/ for store file during the build phase
mkdir build
cd build

# generate makefiles with cmake
# note that we need to temporarily set $LLVM_HOME before configure cfe, where is the prefix of LLVM
env CXX=mpicxx cmake -GNinja ../ -DCMAKE_BUILD_TYPE=Release -DHDF5_PREFER_PARALLEL=on -DCMAKE_INSTALL_PREFIX=$HOME/.pkg/hdf5-1.10.5-mpicxx-900/

# compile and install
ninja
ninja install

# add clang to $PATH
# this command is for fish terminal
set PATH $HOME/.pkg/hdf5-1.10.5-mpicxx-900/bin $PATH
```

### install openmc
``` bash
cd ~
mkdir .pkg
cd .pkg/

# Download source code from git
git clone https://github.com/openmc-dev/openmc.git
cd openmc
git checkout master

# create a folder under .pkg to for installed llvm
mkdir openmc-mpicxx-900/

# create a forlder build/ for store file during the build phase
cd openmc/
mkdir build
cd build

# generate makefiles with cmake
env CC=mpicc CXX=mpicxx HDF5_ROOT=$HOME/.pkg/hdf5-1.10.5-mpicxx-900/ cmake -GNinja ../ -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=$HOME/.pkg/openmc-mpicxx-900/

# compile and install
ninja
ninja install

# add llvm to $PATH
# this command is for fish terminal
set PATH $HOME/.pkg/openmc-mpicxx-900/bin $PATH
```
