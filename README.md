# Dojo-Explorer
We will add [BTC-RPC-EXPLORER](https://github.com/janoside/btc-rpc-explorer "BTC-RPC-EXPLORER") to [Samourai Dojo](https://github.com/Samourai-Wallet/samourai-dojo "Samourai Dojo")

<a name="architecture"/>

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
                 ______________________________ | _________________________________________________________
                |                               |                                                          |
                |                      -------------------                                                 |
                |                     |   Tor Container   | - - - - - - - - - - - - - - - -                |
                |                      -------------------                                 |               |
                |                             |        |                                   |               |
                |             -------------------      |                                   |               |
                |            |  Nginx Container  |     |                     dmznet        |               |
                |             -------------------      |                                   |               |
                |- - - - - - - - - - - | - - - - - - - | - - - - - - - - - - - - - - - - - | - - - - - - - |
                |     --------------------          --------------------          ------------------       |
                |    |  Nodejs Container  | ------ | Bitcoind Container | ------ | BTC-RPC-Explorer |      |
                |     --------------------          --------------------          ------------------       |
                |               |                                                                          |
                |    -------------------                                                                   |
                |   |  MySQL Container  |                           dojonet                                |
                |    -------------------                                                                   |
                |__________________________________________________________________________________________|
