# riscv-gnu-toolchain

**This tutorial is for setting up the environment related to the riscv-gnu-toolchain, and it serves as the foundational environment setup for the MIT 6.S081 course**

***

## 1. Download the file to your ubuntu
  I have provided the download link for the file below, so you don't have to pull the mirror from GitHub (which can be subject to your internet speed and sometimes fail to fetch). 
  Once you download and unzip it on your Ubuntu, you can proceed with the *configuration* (../configure) and *compilation* (make) directly.

  Download link:[https://mega.nz/file/Y70EmJLJ#Qq0Lgt6C8aRuMzT6vTEUXDVOwhI7SbPMOhJ2M-YlVyM](https://mega.nz/file/Y70EmJLJ#Qq0Lgt6C8aRuMzT6vTEUXDVOwhI7SbPMOhJ2M-YlVyM)

  The images after downloading and extracting is as follows.
	![](https://res.cloudinary.com/dogmynjzd/image/upload/v1694146186/Screenshot_from_2023-09-08_11-16-42_f8dbqd.png)

 Use the command
 
 ```shell
 sudo apt-get update
```
to update system package list

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
  sudo apt-get install libglib2.0-dev ninja-build  build-essential zlib1g-dev pkg-config libglib2.0-dev  \ binutils-dev libboost-all-dev autoconf libtool libssl-dev  libpixman-1-dev libpython-dev  \ virtualenv libmount-dev   libpixman-1-dev   
  cd ./qemu  
  mkdir build  
  cd build  
  ```

- Check your Dependencies' version  
 use command:  

  ```
  re2c --version  
  ninja --version  
  ```

to check the version of the above software  

If there is no output from the command line, download both software.Otherwise, the following error message is displayed:  

  ```
  ERROR: Cannot find Ninja  
  ```  

[  
Ps:  
re2c:[https://github.com/skvadrik/re2c/releases](https://github.com/skvadrik/re2c/releases)  
ninja:[https://github.com/ninja-build/ninja/releases](https://github.com/ninja-build/ninja/releases)  

If you choose to install ninja manually, please move the executable file of ninja to one of the executable file paths of the system so that you can access it globally in the terminal.  Common executable paths include */usr/local/bin* or */usr/bin*. Use the command:  

 ```
 sudo mv ninja /usr/local/bin/  
 ```

or

 ```
 sudo mv ninja /usr/bin/   
 ```

then  

 ```
 ninja --version  
 ```
]

- Begin to configure  
Use this command:  

 ```
 ../configure  
 ```

then waiiiiiiiiiiiiiiiiiiiiting...  
***

You may encounter many problems during the process,I list some i encountered below:  
[  
 Q:**ERROR: pkg-config binary 'pkg-config' not found**  
 A:This error message indicates that a binary file called pkg-config, which is commonly used to find and get information about dependent libraries when configuring and building software, is not found on your system. To solve this problem, you can install pkg-config by following one of the following steps:  

  ```
  sudo apt-get install pkg-config  
  ```

or you can install it manually:  

 its official website: [https://pkgconfig.freedesktop.org/releases/](https://pkgconfig.freedesktop.org/releases/)

 you can download its newest version:  

 ```shell
 wget https://pkg-config.freedesktop.org/releases/pkg-config-0.29.2.tar.gz  
 #into the folder  
 cd pkg-config-0.29.2  
 #use these commands  
 ./configure  
 make  
 #if failed to run, change into 'sudo make install'  
 make install  
 ```
 
finally,use this command to check your the pkg-config's version  

 ```shell
 pkg-config --version  
 ```


Q:**No package 'glib-2.0' found**  
A:This may be because pkg-config cannot find the file 'glib-2.0.pc' , which contains information about glib-2.0.  
Try adding the directory path containing the glib-2.0.pc file to the PKG_CONFIG_PATH environment variable. This tells pkg-config to look for .pc file in the specified directory.  

Search:  

 ```shell
  sudo find / -name "glib-2.0.pc" 2>/dev/null  
 ```

This will search the system for the glib-2.0.pc file and list its path. Once you find the path to the glib-2.0.pc file, you can add it to the PKG_CONFIG_PATH environment variable. Assuming that the found path is /path/to/glib-2.0-pc, use the following command to add it to the environment variable:  

 ```shell
 vim ~/.bashrc
 #add this line  
 export PKG_CONFIG_PATH=/path/to/glib-2.0-pc:$PKG_CONFIG_PATH     
 source ~/.bashrc  
 ```

Then, run the pkg-config command again:  

 ```shell
 pkg-config --modversion glib-2.0  
 ```

Q:**error: Either a previously installed pkg-config or "glib-2.0 >= 2.16" could not be found. Please set GLIB_CFLAGS and GLIB_LIBS to the correct values or pass --with-internal-glib to configure to use the bundled copy.**   
A:This error message indicates that the. /configure script cannot find the required dependencies when configuring the software, especially pkg-config and glib-2.0.  
 You can use this command to download:  

  ```shell
 sudo apt-get install pkg-config libglib2.0-dev  
 ```

or you can change the command `./configure` into  

 ```
 ./configure --with-internal-glib  
 ```

Q:**ERROR: glib-2.56 gthread-2.0 is required to compile QEMU**  
A:This problem is divided into two situations.  

- Your versions of GLib and GThread are too old  
 Use these commands:  
  
 ```shell
 pkg-config --modversion glib-2.0  
 pkg-config --modversion gthread-2.0  #Use this command to check the version  
 sudo apt-get install libglib2.0-dev  #If the version is lower than needed, download  
 ```

- Clear files downloaded in the folder 'build' when the previous configurator was running  
  When using the. /configure directive, it might download some configuration files.  When an error occurs, the configuration process terminates.  When we troubleshoot the error and configure again, the downloaded files may block the program and report this error.So delete those files in folder 'build'.  

- Other situation  
  If you upgrade GLib, but still encounter problems, you may need to update the library path so that the compiler can find the new version of GLib. You can use the following command to update the library cache:  
 
 ```
 sudo ldconfig  
 ```
]  

After the configuration is complete,Use these commands:  

 ```shell
 make -j4   #and wait  
 sudo make install  
```

During this process, your disk space may be insufficient, the error is as follows:  

 ```shell
 /usr/bin/ld: final link failed: No space left on device  
 collect2: error: ld returned 1 exit status  
 [5720/9608] Compiling C object libqemu-sparc-softmmu.fa.p/target_sparc_cc_helper.c.o  
 [5721/9608] Compiling C object libqemu-sparc-softmmu.fa.p/hw_sparc_sun4m.c.o  
 [5722/9608] Compiling C object libqemu-sparc-softmmu.fa.p/target_sparc_cpu.c.o  
 ninja: build stopped: subcommand failed.  
 make: *** [Makefile:162: run-ninja] Error 1  
 ```

expand your disk or clean up some files  

If 'sudo make install' is completed, it means that your qemu installation is successful.  


## 4.Test  
 Now we can create a C language file to test the effect.  
 In another location, create a folder for storing your code.  
 Then, create a 'helloworld.c' file and use the command  

 ```shell
 riscv32-unknown-elf-gcc helloworld.c -o helloworld  
 ```

 to compile it.  

Then ue the command:  

 ```shell
 ./qemu-riscv32 helloworld  
 ```
 to run it.
![](https://res.cloudinary.com/dogmynjzd/image/upload/v1694146185/Screenshot_from_2023-09-07_23-27-38_swwx5v.png)

To streamline this process, we can add 'qemu-riscv32' to the environment variables, eliminating the need to navigate to the 'qemu-riscv32' path and enabling us to run 'qemu-riscv32' directly from the terminal:

 ```shell
 vim ~/.bashrc
 #add these lines,the file 'qemu-riscv32' is in the folder './qemu/build'
 export RISCV32= "path/to/the/file/qemu-riscv32"	
 export PATH=$PATH:$RISCV/bin:$RISCV32
 ```

 ```shell
 #close .bashrc 
 source ~/.bashrc
 cd path/to/qemu/build
 chmod +x qemu-riscv32
 ```

Open the folder where the helloworld file is located and run it directly:

 ```shell
 qemu-riscv32 helloworld
 ```

![](https://res.cloudinary.com/dogmynjzd/image/upload/v1694146186/Screenshot_from_2023-09-08_00-00-30_qrhkiu.png)

if it messages **"bash: ./helloworld: cannot execute binary file: Exec format error"** ,try 

 ```shell
 sudo dpkg --add-architecture i386
 sudo apt-get update
 sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386
 ```

and re-run.

## If you can see 'hello world!', it means that you have successfully configured it.Congratulations!
