# prerequisites

https://github.com/Sandhu-Sahil/bootstrapping-hyperledger

# steps to create 

- **Step 1:** Start the bootstrap server either by using the bootstraping-hyperledger repo or by running the following command in the terminal:
```
sudo docker run --name microfab --rm -ti -p 8080:8080 -e MICROFAB_CONFIG="${MICROFAB_CONFIG}" ibmcom/ibp-microfab
```

- **Step 2:** Create wallet and gateway connection profile:
```
curl -s http://console.127-0-0-1.nip.io:8080/ak/api/v1/components | weft microfab -w ./_wallets -p ./_gateways -m ./_msp -f
```

output:
```
Gateway profile written to : /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_gateways/producersorggateway.json 
Gateway profile written to : /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_gateways/sellersorggateway.json 
Added identity under label producersorgadmin to the wallet at /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_wallets/ProducersOrg 
Added identity under label producersorgcaadmin to the wallet at /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_wallets/ProducersOrg 
Added identity under label sellersorgadmin to the wallet at /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_wallets/SellersOrg 
Added identity under label sellersorgcaadmin to the wallet at /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_wallets/SellersOrg 
Added identity under label ordereradmin to the wallet at /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_wallets/Orderer 

Environment variables: 

For producersorgadmin @  ProducersOrg use these:
 
export CORE_PEER_LOCALMSPID=ProducersOrgMSP
export CORE_PEER_MSPCONFIGPATH=/media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_msp/ProducersOrg/producersorgadmin/msp
export CORE_PEER_ADDRESS=producersorgpeer-api.127-0-0-1.nip.io:8080

For producersorgcaadmin @  ProducersOrg use these:
 
export CORE_PEER_LOCALMSPID=ProducersOrgMSP
export CORE_PEER_MSPCONFIGPATH=/media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_msp/ProducersOrg/producersorgcaadmin/msp
export CORE_PEER_ADDRESS=producersorgpeer-api.127-0-0-1.nip.io:8080

For sellersorgadmin @  SellersOrg use these:
 
export CORE_PEER_LOCALMSPID=SellersOrgMSP
export CORE_PEER_MSPCONFIGPATH=/media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_msp/SellersOrg/sellersorgadmin/msp
export CORE_PEER_ADDRESS=sellersorgpeer-api.127-0-0-1.nip.io:8080

For sellersorgcaadmin @  SellersOrg use these:
 
export CORE_PEER_LOCALMSPID=SellersOrgMSP
export CORE_PEER_MSPCONFIGPATH=/media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_msp/SellersOrg/sellersorgcaadmin/msp
export CORE_PEER_ADDRESS=sellersorgpeer-api.127-0-0-1.nip.io:8080
Complete 
```

- **Step 3:** Create export variables of ‘producersorgadmin @ ProducersOrg’ for the peer:
```
export CORE_PEER_LOCALMSPID=ProducersOrgMSP
export CORE_PEER_MSPCONFIGPATH=/media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_msp/ProducersOrg/producersorgadmin/msp
export CORE_PEER_ADDRESS=producersorgpeer-api.127-0-0-1.nip.io:8080
```

- **Step 4:** Install binaries:
```
curl -sSL https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh | bash -s -- binary
```

- **Step 5:** export path:
```
export PATH=$PATH:${PWD}/bin
export FABRIC_CFG_PATH=${PWD}/config
```

### project scarffolding is done

# lifecycling chaincode

- **Step 1:** Package the chaincode:
```
peer lifecycle chaincode package mango.tgz --path ./mango-contract/ --lang golang --label mango_1
```

- **Step 2:** Install the chaincode:
```
peer lifecycle chaincode install mango.tgz
```

output:
```
2023-08-03 20:54:11.739 IST 0001 INFO [cli.lifecycle.chaincode] submitInstallProposal -> Installed remotely: response:<status:200 payload:"\nHmango_1:5af16379d37e69f875edd2f8259144f1aef932615170320544ad1aae4d2f63d0\022\007mango_1" > 
2023-08-03 20:54:11.740 IST 0002 INFO [cli.lifecycle.chaincode] submitInstallProposal -> Chaincode code package identifier: mango_1:5af16379d37e69f875edd2f8259144f1aef932615170320544ad1aae4d2f63d0
```

further install:
```
export CC_PACKAGE_ID=mango_1:5af16379d37e69f875edd2f8259144f1aef932615170320544ad1aae4d2f63d0
```

- **Step 3:** Approve the chaincode:
```
peer lifecycle chaincode approveformyorg -o orderer-api.127-0-0-1.nip.io:8080 --channelID mango-channel --name mango --version 1 --sequence 1 --waitForEvent --package-id ${CC_PACKAGE_ID}
```

output:
```
2023-08-03 21:00:41.182 IST 0001 INFO [chaincodeCmd] ClientWait -> txid [fab1bf66915fcbadc4539338e16ec776b51d1e0ae472d6a1be82ce8abd4beb20] committed with status (VALID) at producersorgpeer-api.127-0-0-1.nip.io:8080
```

- **Step 4:** Commit:
```
peer lifecycle chaincode commit -o orderer-api.127-0-0-1.nip.io:8080 --channelID mango-channel --name mango --version 1 --sequence 1
```

output:
```
2023-08-03 21:01:39.315 IST 0001 INFO [chaincodeCmd] ClientWait -> txid [73bc9ecd80ee1efbef639e110d02b11aac05618bd4243191c726d2643066b2d1] committed with status (VALID) at producersorgpeer-api.127-0-0-1.nip.io:8080
```

### Evoke and Query
Evoke means to call a function of chaincode which alters any data with the help of orderers, and query means to read the data from the ledger.

### Make Transactions/ Invoke and Query

- **Create a mango:**
```
peer chaincode invoke -o orderer-api.127-0-0-1.nip.io:8080 --channelID mango-channel -n mango -c '{"function":"CreateMango","Args":["MANGO1","1","mango corp","5000","10000"]}'
```

- **Query a mango:**
```
peer chaincode query  -o orderer-api.127-0-0-1.nip.io:8080 --channelID mango-channel -n mango -c '{"function":"ReadMango","Args":["MANGO1"]}'
```

## Update chaincode

- **Step 1:** Package the chaincode:
```
peer lifecycle chaincode package mango.tgz --path ./mango-contract/ --lang golang --label mango_2
```
name as mango_2 because we are updating the chaincode from mango_1 to mango_2, or you can name it as mango_1.1 as versions

- **Step 2:** Install the chaincode:
```
peer lifecycle chaincode install mango.tgz
```

output:
```
2023-08-03 22:38:56.809 IST 0001 INFO [cli.lifecycle.chaincode] submitInstallProposal -> Installed remotely: response:<status:200 payload:"\nHmango_2:8aa4967cd1561b59d6c256ca9cf1d227ad05f7db8effaf312b330a3d750b47b8\022\007mango_2" > 
2023-08-03 22:38:56.810 IST 0002 INFO [cli.lifecycle.chaincode] submitInstallProposal -> Chaincode code package identifier: mango_2:8aa4967cd1561b59d6c256ca9cf1d227ad05f7db8effaf312b330a3d750b47b8
```

further install:
```
export CC_PACKAGE_ID=mango_2:8aa4967cd1561b59d6c256ca9cf1d227ad05f7db8effaf312b330a3d750b47b8
```

- **Step 3:** Approve the chaincode:
```
peer lifecycle chaincode approveformyorg -o orderer-api.127-0-0-1.nip.io:8080 --channelID mango-channel --name mango --version 2 --sequence 2 --waitForEvent --package-id ${CC_PACKAGE_ID}
```
change the version to 2 and sequence to 2, this means we are updating the chaincode from mango_1 to mango_2, sequence is the number of times the chaincode is updated

- **Step 4:** Commit:
```
peer lifecycle chaincode commit -o orderer-api.127-0-0-1.nip.io:8080 --channelID mango-channel --name mango --version 2 --sequence 2
```

- **Step 5:** Invoke/change ower of mango:
```
peer chaincode invoke -o orderer-api.127-0-0-1.nip.io:8080 --channelID mango-channel -n mango -c '{"function":"SellMango","Args":["MANGO1","mango corp","mango shop"]}'
```

- **Step 6:** Query the mango:
```
peer chaincode query  -o orderer-api.127-0-0-1.nip.io:8080 --channelID mango-channel -n mango -c '{"function":"ReadMango","Args":["MANGO1"]}'
```

