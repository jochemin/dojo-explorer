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

We need to edit Dojo torrc file (this file is located in /samourai-dojo-master/docker/my-dojo/tor)  to create a hidden service (v3) to connect to our explorer. Add this lines under the services:

```
HiddenServiceDir /var/lib/tor/hsv3explorer
HiddenServiceVersion 3
HiddenServicePort 80 172.29.1.6:3002
```

### 2.- Clone this repository  ###
```
git clone https://github.com/jochemin/dojo-explorer
```

### 3.- Edit .env file ###

Edit .env file and insert your Bitcoin Core RPC credentials
<p align="center">
  <img src="img/env_file.png?raw=true" alt=".env file"/>
</p>



