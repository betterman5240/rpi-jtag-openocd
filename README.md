# rpi-jtag-openocd

The Raspberry pi 2 can be a JTAG of using OPENOCD to simulate, and it can connect and debug with the Raspberry 3(bcm2837 chip).

# How to be a good tool like a JTAG? 

**The first step is installing the Openocd in the Rpi 2.**

please follow the below steps:
    
    cd ~
    
    sudo apt-get update
    
    sudo apt-get install git autoconf libtool make pkg-config libusb-1.0-0 libusb-1.0-0-dev
    
    git clone http://openocd.zylin.com/openocd
    
    cd openocd
    
    ./bootstrap
    
    ./configure --enable-sysfsgpio --enable-bcm2835gpio
    
    make
    
    sudo make install  (default install folder is /usr/local/share/openocd/)
    
if you have done these steps, please copy **interface/rpi2.cfg** to the **/usr/local/share/openocd/scripts/interface/**,
and also copy **target/rpi3.cfg** to the **/usr/local/share/openocd/scripts/target/**.

**The second step is the Rpi 2 pin headers and the Rpi 3 pin headers linking for each other, Following the below picture.**

![rpi2debugrpi3](https://user-images.githubusercontent.com/8576322/166696162-f1b5a7b3-8f52-4e94-a867-d199401d2a46.jpeg)


| feature       | Rpi 2            |   Rpi3     |
| ------------- |:----------------:| :-----------:|
| GND           |pin 9  (GND)      |    pin 9 (GND) |
| TDI           | pin 19 (GPIO 10) |   pin 37 (GPIO 26) |
| TDO           |pin 21 (GPIO 9)   |    pin 18 (GPIO 24) |
| TCK           | pin 23 (GPIO 11) |   pin 22 (GPIO 25) |
| TMS           | pin 22 (GPIO 25) |   pin 13 (GPIO 27) |
| TRST          |pin 26 (GPIO 7)   |    pin 15 (GPIO 22) |

**Why are definitions for these pin headers in the Rpi 2? Please reference the Rpi2.cfg configuration.**

    # Each of the JTAG lines need a gpio number set: tck tms tdi tdo
    # Header pin numbers: 23 22 19 21
    bcm2835gpio jtag_nums 11 25 10 9
    
    # If you define trst or srst, use appropriate reset_config
    # Header pin numbers: TRST - 26, SRST - 18

    bcm2835gpio trst_num 7
    reset_config trst_only

**Why are definitions for these pin headers in the Rpi 3? Please reference the below picture.**

<img width="1225" alt="Screen Shot 2022-04-30 at 18 28 49" src="https://user-images.githubusercontent.com/8576322/166702851-fe52a9f1-c31d-44b3-a17c-128628739d23.png">

When you have written the **enable_jtag_gpio=1** in the config.txt at the Rpi 3. the Rpi 3 will enable Alt4 configurations for next time.

**The third step is running the openocd command in the Rpi 2 and also trun on the Rpi 3 atfer you enable Alt4. Reference below commands.**

    cd /usr/local/share/openocd/scripts/
    
    openocd -f ./interface/rpi2.cfg -f ./target/rpi3.cfg


**As you can see the Openocd of the Jtag mode shows the Rpi 3 checking information in the Rpi 2 terminal. The below picture is a result of the running command**


<img width="895" alt="Screen Shot 2022-05-04 at 22 46 23" src="https://user-images.githubusercontent.com/8576322/166707270-a742729a-5f79-4d74-852f-f85a5fbaad10.png">


**The fourth step is using the telnet command to connect to the Openocd, and also try to run the *targets* and *reg* commands. if you see more information about the Rpi 3 and show all registers. the Rpi 2 has communicated the Rpi 3, It can start to debug the Rpi 3 SOC on the Rpi 2, It is like the below pictures.**


<img width="891" alt="Screen Shot 2022-05-04 at 22 56 06" src="https://user-images.githubusercontent.com/8576322/166710864-936d6a78-6aa7-4b6c-afcc-cc20033cc6b7.png">

<img width="291" alt="Screen Shot 2022-05-04 at 22 56 46" src="https://user-images.githubusercontent.com/8576322/166710876-289e1258-86ef-4277-8508-c77a54d11901.png">


# Reality environment


![7CACED77-D64C-477D-B692-911C3215420D](https://user-images.githubusercontent.com/8576322/166717596-4e1c1e29-1212-4cd2-9d8f-420d7b2380e2.jpeg)



    
