<p align="center">
  <img alt="GSMEVIL 2 Title" src="https://raw.githubusercontent.com/sharyer/gsmevil2/master/img/logo.png" style="max-width:100%;max-height:100%;" />
</p>

**GSMEVIL 2** is python web-based tool which use for capturing IMSI numbers and SMS and also you able to see SMS and IMSI on any device using your favorite browser and it's make easy to capture SMS and IMSI numbers for those who not have much knowledge about GSM packets capturing.

# Disclaimer

This program was made to understand how GSM network works. Not for bad hacking!
We are not responsible for any illegal activity!

# Setup

## Install GSMEvil

1. Clone this repository and go into it:

    ```shell
    $ git clone https://github.com/sharyer/gsmevil2.git
    $ cd gsmevil2
    ```
	
2. Create and activate `python3` virtual environment (`venv`) named `gsmevil_venv`:

    Virtual environment allows us to avoid cluttering the system with specific 
	versions of packages needed for this project (which can also be managed
    externally (via `apt`) and cannot be installed in other way).

    ```shell
    $ sudo apt update
    $ sudo apt install python3-venv python3-pip

    $ python3 -m venv .gsmevil_venv
    $ . .gsmevil_venv/bin/activate
    ```
   
2. Install dependencies (in virtual environment):

   ```shell
   (.gsmevil_venv) $ python -m pip install -r ./requirements.txt
   ```

## Install TShark

`TShark` is needed, since its used by `pyshark` (python wrapper around it, dependency of this project)

```shell
$ sudo apt install tshark
```

## Install GR-GSM (For receiving GSM transmissions)

```shell
$ sudo add-apt-repository -y ppa:ptrkrysik/gr-gsm
$ sudo apt update
$ sudo apt install gr-gsm
```

If `gr-gsm` failed to setup - follow this guide: https://github.com/ptrkrysik/gr-gsm/wiki/Installation  

## Install Kalibrate (For finding frequencies)

```shell
$ sudo apt install kalibrate-rtl
```

or build it from sources:

```shell
$ sudo apt install build-essential libtool automake autoconf librtlsdr-dev libfftw3-dev
$ git clone https://github.com/steve-m/kalibrate-rtl
$ cd kalibrate-rtl
$ ./bootstrap && CXXFLAGS='-W -Wall -O3'
$ ./configure
$ make
$ sudo make install
```

# Preparation

1. You need GSM frequency on which you capture SMS or IMSI. By using `kalibrate-rtl` you will get all your near GSM Base Stations (BS) frequencies.

    ```shell
    $ kal -s GSM900
    ```
  
    Expected output:
    ```text
    kal: Scanning for GSM-900 base stations.
    GSM-900:
	    chan: 4 (935.8MHz + 320Hz)    power: 1829406.95
	    chan: 11 (937.2MHz + 308Hz)   power: 4540354.88
    ...
    ```

2. Now you need to capture GSM traffic using `gr-gsm` on frequency of your any GSM Base Station which you get from `kalibrate-rtl`.

    ```shell
    $ grgsm_livemon -f <your_frequency>M
    ```

    Example:

    ```shell
    $ grgsm_livemon -f 935.8M
    ```

    If you see output that's mean you getting GSM packets than continue other setups else change frequency.
    ```
    2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b
    2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b
    2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b
    2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b 2b
    ...
    ```

	This traffic will be send into `lo` network-interface by default, which `GSMEvlil` will listen.

3. Now every thing is ready. You can start capturing SMS or IMSI numbers using `GSMEvil`.
    
	You able to run IMSI catcher and SMS sniffer both at same time using 2 separate terminals for capture IMSI numbers and SMS both at same time.
	
    ```shell
    $ cd <your gsmevil2 folder> # Example: $ cd gsmevil2
    ```

# Usage

- Run this command to quick start Gsm Evil 2.
  
    This will start web-server on `localhost:80`, listening for `gr-gsm` packets on `lo` network-interface .
	
    **NOTE:** Listening network-interface requires `sudo` or setting permission once (`sudo setcap cap_net_raw,cap_net_admin=eip <executable>`).

    ```shell
    $ sudo .gsmevil_venv/bin/python GsmEvil.py 
    ```

    Options:
    ```
    $ .gsmevil_venv/bin/python GsmEvil.py --help
    Usage: GsmEvil.py: [options]

    Options:
      -h, --help            show this help message and exit
      -i IFACE, --iface=IFACE Interface (default : lo)
      -p PORT, --port=PORT  Port (default : 80)
      --host=HOST           Host (default : localhost)
    ```
	
- For changing host port:

    ```shell
    $ sudo .gsmevil_venv/bin/python GsmEvil.py -p 8080
    ```
	
- For changing hostname:

    ```shell
    $ sudo .gsmevil_venv/bin/python GsmEvil.py --host=localhost
    ```

Open `localhost` or `127.0.0.1` in your favorite browser and use now.

# Requirements

- Linux operating system (Kali linux)
- [rtl-sdr (RTL2832U)](https://osmocom.org/projects/sdr/wiki/rtl-sdr) with antenna (less than 15$) or [HackRF](https://greatscottgadgets.com/hackrf/) 

# Links

|                      |      |
| :------------------- | :--- |
| Frequency            | https://www.worldtimezone.com/gsm.html or https://en.wikipedia.org/wiki/GSM_frequency_bands  
| Sdr                  | https://en.wikipedia.org/wiki/Software-defined_radio  
| Sms                  | https://en.wikipedia.org/wiki/SMS#GSM  
| Imsi                 | https://fr.wikipedia.org/wiki/International_Mobile_Subscriber_Identity  
| Cell Id              | https://en.wikipedia.org/wiki/Cell_ID or https://unwiredlabs.com/  
| GSM                  | https://en.wikipedia.org/wiki/GSM  
| Frequency Calculator | https://www.cellmapper.net/arfcn  
| GR-GSM               | https://github.com/ptrkrysik/gr-gsm 

# Donations

Bitcoin : `192bG3RRAGdbTPSUWqxbTBaAnKyvALm84g`

# Contact

|          |      |
| :------- | :--- |
| Website  | https://www.ninjhacks.com
| Facebook | https://www.facebook.com/ninjhacks
| Twitter  | https://twitter.com/ninjhacks
| Discord  | https://discord.gg/ninjhacks<br/>
| Email    | help@ninjhacks.com
