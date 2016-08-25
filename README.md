Gridcoin-Attacks client
=============

Gridcoin-Attacks is an enhanced [Gridcoin-Research](https://github.com/gridcoin/Gridcoin-Research) client.

It contains various additional RPC functions and allows the user to reverse CPIDsv2 taken from the Blockchain. After a successfull reversion it is possible to steal the targets BOINC Credits to gain a illigetimately high Coin Reward without any BOINC contribution.

- - - -

##Build and Install

###Ubuntu | Arch
    Ensure that all dependencies are satisfied.
    Ubuntu:
    $ apt-get install ntp git build-essential libssl-dev libdb-dev libdb++-dev libboost-all-dev libqrencode-dev libcurl3-dev libzip-dev
    Arch:
    $ pacman -Sy base-devel boost openssl libzip qrencode db curl
    
    
    Change into src directory and build. Set number of jobs accordingly. **WARNING!** uses lots of RAM for compilation.
    $ cd src
    $ make -f makefile.unix -j4 USE_UPNP=-

    Install the binary.
    $ make install -m 755 gridcoinresearchd /usr/bin/gridcoinresearchd

###Configuration
    Create configuration directory.
    $ mkdir ~/.GridcoinResearch
    $ cd ~/.GridcoinResearch

    Create configuration file.
    $ nano gridcoinresearch.conf
    
    email=
    cpumining=
    rpcallowip=127.0.0.1
    rpcuser=grid
    rpcpassword=coin
    addnode=node.gridcoin.us

    Optional: Retrieve a snapshot of the Gridcoin Blockchain (saves time!)
    $ wget http://download.gridcoin.us/download/downloadstake/signed/snapshot.zip && unzip snapshot.zip && rm snapshot.zip

##First Run
    Execute the installed binary.
    $ gridcoinresearchd
    
    Ensure that the client connects to other clients to retrieve missing Blocks
    $ gridcoinresearchd getinfo

    If anything goes wrong, it might be helpful to check the debug log under ~/.GridcoinResearch/debug.log

##Attacking
    1. Choose a target
    2. Reverse one of the targets CPIDsv2
    3. Enter the targets credentials to steal Coin Reward
    4. Profit

To enable attacking and attack detection, the following RPC functions were added:
    * reversecpidv2 <number> - Reverses CPIDv2 for a given blockhight.
    * bulkreversecpidv2 - Attempts to reverse all CPIDv2s found in the blockchain.
    * findpluralcpids - Attempts to find eCPIDs used with multiple GRC addresses.
    * findpluraladdresses - Attempts to find GRC addresses used with multiple eCPIDs.

Once the target eCPID has been chosen get the corresponding e-mail address and iCPID by executing
    $gridcoinresearchd reversecpidv2 <number>
Where number has to be the blockheight of a Block, mined with the targets eCPID.

To mine with the targets eCPID add the obtained e-mail address, iCPID and BOINC projectname to the local gridcoinresearch.conf.
In addition set the parameters attack and cpumining to enable mining and the attack mode of the client.

    rpcallowip=127.0.0.1
    rpcuser=grid
    rpcpassword=coin
    addnode=node.gridcoin.us
    cpumining=true
    email=<e-mail>
    icpid=<iCPID>
    project=<projectname>
    attack=true

All created Blocks should now contain the targets eCPID and a valid CPIDv2.
This means in effect, that the targets accumulated Research Age is utilized to illegitimately increase our Coin Reward.

----------------------------------------
For further information and insight, please read the corresponding bachelor thesis. 
