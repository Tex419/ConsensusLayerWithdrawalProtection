
# Consensus Layer Withdrawal Protection

Submit a pull request with your Ethereum validator change withdrawal credential message to have the CLWP community broadcast it as early as possible for security.

## Acknowledgements

 - [EthDo](https://github.com/wealdtech/ethdo)
 - [EthStaker](https://ethstaker.cc/)

## Documentation

For full documentation on ethdo usage, please refer to [Change Withdrawal Credentials](https://github.com/wealdtech/ethdo/blob/master/docs/changingwithdrawalcredentials.md). Below are the excerpt steps required to join the CLWP voluntary broadcast protection. For your own protection, please run the steps using your own node and execute on an airgapped machine for signing processes. However, for demonstration purposes, I have included an excerpt of the offline-preparation.json files in compressed format.

If your validator has not set an execution layer withdrawal address, your withdrawal credentials will start with "0x00". If they have been set, they will start with "0x01". You may easily check your validator status using beaconchain, such as here:
https://goerli.beaconcha.in/validator/99a29d72501fc49a748d11367b0b2b80be2e5c93cc28a512e06fb40142666e206590ee637ba1bf1e8adfd0e9de3665d5#deposits

At the launch of the Capella/Shanghai hardfork, every validator with Withdrawal Credentials starting with 0x00 will be allowed to perform **a one time** operation to change withdrawal credentials from 0x00 to an execution layer address. You will need validator mnemonic seed phrase (or withdrawal private key) to sign this transaction.

## Demo

Video Demo: https://bit.ly/clwp-demo

Presentation: https://bit.ly/clwp-presentation

## Steps

2023-01-12 - Apologies, but CLWP submissions are temporarily halted until the Ethereum core devs finalize the fork version handling for signatures. https://github.com/ethereum/consensus-specs/pull/3206

2023-01-21 - We are accepting CLWP submissions using ethdo version 1.27 or later. Fork version offline-preparation files have been updated for Capella

### Mainnet

```
# Download and upack the latest ethdo release from https://github.com/wealdtech/ethdo/releases/
# In the same directory as ethdo, unpack the offline-preparation.json file (or you may generate your own using --prepare-offline and your own beacon node)
tar -zxf ../offline-preparation.json.mainnet.tar.gz
cp offline-preparation.json.mainnet offline-preparation.json
# Use ethdo to generate the change-operations.json file. Perform this step on an air gapped, clean and secure computer (Linux USB recommended) and triple check your withdrawal address.
./ethdo validator credentials set --offline --withdrawal-address 0xAnExecutionLayerAddress --mnemonic "your seed phrase"
# Clear your command line history
history -c 
```

Move the resulting change-operations.json file into the mainnet folder with individual files for each validatorIndex.json, verify the withdrawal address again, and submit a pull request to have it included in CLWP protection. If you need help or prefer not to link your GitHub account to your validator, reach out to an admin on the Support below and we can assist. If you have multiple validators, you will need to split the file to have a single submission manually using a text editor. Never modify the validator index, public key, or withdrawal address, or signature or it will invalidate the submission. If you have many validators, you may install "jq" on linux and split the change-operations.json file using it:

```
for ((i=0; i<`jq -ec '.|length' change-operations.json`;i++)); do validator=`jq -ec ".[$i].message.validator_index|tonumber" change-operations.json`; echo "`jq -ec [".[$i]"] change-operations.json`" > ${validator}.json;done
``` 
 
Volunteer to run the broadcast of change-operations-clwp-mainnet.json on your node to help protect the community.  

### Goerli

The steps above are tested to work with Goerli as well. Use the offline-preparation.json.goerli.tar.gz file, and place your change-operations.json file in the goerli directory as validatorIndex.json per each validator. 


## Support

For community support, create an issue or join our OffChain Discord channel #ethereum-consensus-layer-withdrawal-protection https://discord.gg/pwuPA6K4zg


