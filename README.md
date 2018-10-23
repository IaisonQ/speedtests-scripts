# VPN Throughput Test

In order to enhance the customer experience, this project is aimed to monitor throughput speeds on all Private Internet Access regions in order to identify and resolve throughput issues as expidited as possible.

## Getting Started

By open sourcing the code base and instructions, this allows all users to replicate the test results to ensure accuracy, transparecy and faster issue replication and resolution. Any user can replicate the data in order to verify the results as some VPN providers sometimes compress data in order to achieve faster throughput speed results.


### Installing

Create a Vultr account.
Tests are run on Vultr VPS (Virtual Private Server) infrastructure. Each Vultr server can download at speeds much faster than most consumer internet connections. Servers cost less than $0.01 an hour and can be created in an instant and then destroyed when they're no longer needed. Additionally Vultr VPS servers are offered in a range of locations meaning that you can chose the server location lcosest to your actual location.

After logging and funding your Vultr account. You will be able to create a VPS by clicking the large + symbol in the top right.

Server Location: Choose your server location. For the most reflective results, select the region closest to your actual location. Not every country is currently available from Vultr however they expand regions.

Server Type: Choose Debian 9 x64 as your Server Type.

Server Size: Select cheapest (non-IPv6) as the Server Size.

Additional Features: No additional features are required.

Startup Script: Click 'Add New' and enter the following, then save:

    #!/bin/sh
    
    export DEBIAN_FRONTEND=noninteractive
    apt-get -y install unzip
    
    #replace this with a git clone
    cd /tmp/
    wget -O vpnspeedtest.zip https://github.com/IaisonQ/speedtests-scripts/archive/master.zip
    unzip -o vpnspeedtest.zip
    cp -R speedtests-scripts-master/* ~
    
    #setup working folders
    cd ~
    mkdir -p torrents
    mkdir -p logs
    mkdir -p vpn_auth
    
    #setup environment
    chmod 700 scripts/*
    ./scripts/vultr-debian-setup.sh
    
    #update the path to use the new curl we just built from source
    PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    export PATH
    
    python vpnspeedtest.py --help
	
SSH Keys: No SSH Keys are required.

Server Hostname & Label: You can leave Server Hostname & Label blank.

Click 'Deploy Now'.


## Running the tests

You will need to connect to the VPS you just created when the server has finished being built. You can check this by connecting via SSH to the server. The Vultr panel contain your username and password. You can check to see if the startup script has finished by typing:

    tail -n 40 -f /tmp/firstboot.log
	
When the script has finished, it will display the --help file. You can get back to the prompt by pressing Ctrl+C.

To run the speedtest, you need to enter the following password whilst replacing the username and password with your own account credentials.

    python vpnspeedtest.py --vpn=privateinternetaccess --auth-username=p1234567 --auth-password=password
	
Additional features will be added to allow for comparative speedtests and more regions.


## Deployment

Currently the system is built for single tests, however it will be expanded.

## Built With

* [Python](https://www.python.org/)
* [OpenVPN](https://www.openvpn.net/)

## Authors

* **VPNSpeedTest** - *Initial work* - [VPNSpeedTest](https://github.com/vpnspeedtest)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Thanks you to VPNSpeedTest who made the code available, their tweets and making my job easier!

