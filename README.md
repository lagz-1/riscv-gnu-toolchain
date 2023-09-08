# riscv-gnu-toolchain

**This tutorial is for setting up the environment related to the riscv-gnu-toolchain, and it serves as the foundational environment setup for the MIT 6.S081 course**

***

## 1. Download the file riscv-gnu-toolchain.tar.gz to your ubuntu
  I have provided the download link for the file below, so you don't have to pull the mirror from GitHub (which can be subject to your internet speed and sometimes fail to fetch). 
  Once you download and unzip it on your Ubuntu, you can proceed with the *configuration* (../configure) and *compilation* (make) directly.

  Download link:[https://mega.nz/file/Y70EmJLJ#Qq0Lgt6C8aRuMzT6vTEUXDVOwhI7SbPMOhJ2M-YlVyM](https://mega.nz/file/Y70EmJLJ#Qq0Lgt6C8aRuMzT6vTEUXDVOwhI7SbPMOhJ2M-YlVyM)

  The images after downloading and extracting is as follows.
	![](https://res.cloudinary.com/dogmynjzd/image/upload/v1694146186/Screenshot_from_2023-09-08_11-16-42_f8dbqd.png)

  Use the command 
  ```shell
  riscv32-unknown-elf-gcc -v
```
  to check your gcc compiler tool's version
  ![](https://res.cloudinary.com/dogmynjzd/image/upload/v1694146186/Screenshot_from_2023-09-07_19-57-43_fwetqr.png)

## 2. Configure Environment

- Open Terminal and Install Dependencies:
  
  ```shell
  sudo apt-get install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev \
  gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev
  ```

- add environment variables: `vim ~/.bashrc`
  Then add these lines:

	```vim  
	export RISCV="/path/to/your/riscv-gnu-toolchain unzip folder"  
	export PATH=$PATH:$RISCV/bin
	```

close ~/.bashrc , and input `source ~/.bashrc` in the terminal to apply your settings.

- Then create a folder named 'build',  
  
  ```shell
  mkdir build
  cd build
  ```

- begin to configure:
  
  ```shell
  ../configure --prefix=$RISCV --with-arch=rv32gc --with-abi=ilp32d
  ```
  
  at this step, you would wait for a long time, be patient  

- Then use command:
  ```shell
  make -j4
  ```
  
  you also should be patient

  If you have made successfully, you will see messages like 'make[2]', 'make[1]', etc., as shown in the image 
  ![](https://res.cloudinary.com/dogmynjzd/image/upload/v1694146186/Screenshot_from_2023-09-07_19-59-57_on9g7y.png)
  
 - Then use command:
   
   ```shell
   make install
   ```

## 3.Configure QEMU

- Download Dependencies

  ```shell
   sudo apt-get install libglib2.0-dev ninja-build  build-essential zlib1g-dev pkg-config libglib2.0-dev  \
binutils-dev libboost-all-dev autoconf libtool libssl-dev  libpixman-1-dev libpython-dev  \
 virtualenv libmount-dev   libpixman-1-dev 
 ```

```
 cd ./qemu  
 mkdir build  
 cd build  
```






"Now we can create a C language file to test the effect. In another location, create a folder for storing your code. Then, create a 'helloworld.c' file and use the 'kk' command to compile it.
  
