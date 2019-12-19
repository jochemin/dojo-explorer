# Dojo-Explorer
We will add [BTC-RPC-EXPLORER](https://github.com/janoside/btc-rpc-explorer "BTC-RPC-EXPLORER") to [Samourai Dojo](https://github.com/Samourai-Wallet/samourai-dojo "Samourai Dojo")

## Architecture ##


                -------------------    -------------------      --------------------
               |  Samourai Wallet  |  |     Sentinel      |    | Bitcoin full nodes |
                -------------------    -------------------      --------------------
                        |_______________________|_______________________|
                                                |
                                          ------------

                                          Tor network

                                          ------------
                                                |
                  Host machine                  | (Tor hidden services)
                 ______________________________ | ___________________________________________________________________
                |                               |                                                                    |
                |                      -------------------                                                           |
                |                     |   Tor Container   | - - - - - - - - - - - - - - - -                          |
                |                      -------------------                                 |                         |
                |                             |        |                                   |                         |
                |             -------------------      |                                   |                         |
                |            |  Nginx Container  |     |                     dmznet        |                         |
                |             -------------------      |                                   |                         |
                |- - - - - - - - - - - | - - - - - - - | - - - - - - - - - - - - - - - - - | - - - - - - - - - - - - |
                |     --------------------          --------------------          ----------------------------       |
                |    |  Nodejs Container  | ------ | Bitcoind Container | ------ | BTC-RPC-Explorer Container |      |
                |     --------------------          --------------------          ----------------------------       |
                |               |                                                                                    |
                |    -------------------                                                                             |
                |   |  MySQL Container  |                           dojonet                                          |
                |    -------------------                                                                             |
                |____________________________________________________________________________________________________|


## Instructions

### 1.- Edit Samourai Dojo torrc file ###

We need to edit Dojo torrc file (this file is located in /samourai-dojo-master/docker/my-dojo/tor)  to create a hidden service (v3) to connect to our explorer. Add these lines under the services:

```
HiddenServiceDir /var/lib/tor/hsv3explorer
HiddenServiceVersion 3
HiddenServicePort 80 172.29.1.6:3002
```
<p align="center">
  <img src="img/torrc.png?raw=true" alt="torrc file"/>
</p>


### 2.- Stop Dojo  ###
```
dojo.sh stop
```

### 3.- Delete TOR image  ###
```
docker rmi $(docker images |grep 'samouraiwallet/dojo-tor')
```

### 4.- Start Dojo  ###
```
dojo.sh start
```

### 4.- Clone this repository  ###
```
git clone https://github.com/jochemin/dojo-explorer && cd dojo-explorer
```
### 5.- Edit .env file ###

Edit .env file.

Insert your Bitcoin Core RPC credentials
<p align="center">
  <img src="img/env_file.png?raw=true" alt=".env file"/>
</p>

Save the file

### 4.- Launch the container ###
```
docker-compose up -d
```

### 5.- View your explorer onion address
```
docker exec -it tor cat /var/lib/tor/hsv3explorer/hostname
```

Now you have your own Bitcoin explorer attached to your Dojo.
