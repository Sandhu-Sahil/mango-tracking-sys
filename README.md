# prerequisites

https://github.com/Sandhu-Sahil/bootstrapping-hyperledger

# steps to create 

- **Step 1:** Start the bootstrap server either by using the bootstraping-hyperledger repo or by running the following command in the terminal:
```
sudo docker run -e MICROFAB_CONFIG -p 8080:8080 ibmcom/ibp-microfab
```

- **Step 2:** Create wallet and gateway connection profile:
```
curl -s http://console.127-0-0-1.nip.io:8080/ak/api/v1/components | weft microfab -w ./_wallets -p ./_gateways -m ./_msp -f
```

output:
```
Gateway profile written to : /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_gateways/org1gateway.json 
Added identity under label ordereradmin to the wallet at /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_wallets/Orderer 
Added identity under label org1admin to the wallet at /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_wallets/Org1 
Added identity under label org1caadmin to the wallet at /media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_wallets/Org1 

Environment variables: 

For org1admin @  Org1 use these:
 
export CORE_PEER_LOCALMSPID=Org1MSP
export CORE_PEER_MSPCONFIGPATH=/media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_msp/Org1/org1admin/msp
export CORE_PEER_ADDRESS=org1peer-api.127-0-0-1.nip.io:8080

For org1caadmin @  Org1 use these:
 
export CORE_PEER_LOCALMSPID=Org1MSP
export CORE_PEER_MSPCONFIGPATH=/media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_msp/Org1/org1caadmin/msp
export CORE_PEER_ADDRESS=org1peer-api.127-0-0-1.nip.io:8080
```

- **Step 3:** Create export variables for the peer:
```
export CORE_PEER_LOCALMSPID=Org1MSP
export CORE_PEER_MSPCONFIGPATH=/media/sandhu-sahil/New-Volume/CODING/LFX-Hyperledger/private-BC/mango-tracking-sys/_msp/Org1/org1caadmin/msp
export CORE_PEER_ADDRESS=org1peer-api.127-0-0-1.nip.io:8080
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

