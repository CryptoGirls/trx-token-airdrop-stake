# TRX token airdrop stake software

Use: To airdrop TOKENS to the holders of a specific token based on their stake proportion on the total circulating supply of that token. If configured, the script gives a bonus to the holders (a proportion from their token balances) if they are voters of a specific SR.

## Dependencies
```
sudo apt install python3-pip

sudo pip3 install python-dateutil

sudo pip3 install python-dateutil --upgrade

sudo apt-get install postgresql postgresql-contrib

sudo apt-get install build-dep python-psycopg2

sudo pip3 install psycopg2-binary

sudo pip3 install termcolor

git clone https://github.com/CryptoGirls/trx-token-airdrop-stake

cd trx-token-airdrop-stake

sudo apt install nodejs

sudo apt install npm

npm install axios

- node doesn't work? Install a newer version:

curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -

sudo apt-get install -y nodejs

```

To not send the private key in plain text to the network, it's recommended to install and configure docker containers made by Rovak. If you won't use the docker, we strongly recommend to use a smaller wallet from which to send the tokens because it can be compromised.

```
apt install docker docker-compose

git clone https://github.com/tronscan/tronscan-docker

```

In docker-compose.yml file put a full node IP in full and solidity IPs:

```
      NODE_FULL_IP: "YOUR_SR_IP_HERE"
      NODE_FULL_PORT: "50051"
      NODE_SOLIDITY_IP: "YOUR_SR_IP_HERE"
      NODE_SOLIDITY_PORT: "50051"
      ENABLE_SYNC: "false"
      ENABLE_NETWORK_SCANNER: "false"
      SECRET_KEY: "aSLtAkzrIY9pTPyboOih"
```

Run it in screen:

```
./start.sh
```

Now you should be able to call the API from http://127.0.0.1:9000

## Configuration
Edit config.json and modify the lines with your settings:

- coin: TRX
- token: token name. It will be used to get the holders' wallets data that own this token
- sraddress: Super Representative's address. This field is mandatory ONLY if you want to give a bonus to the SR's voters
- owneraddress: The addres from where the token payments will be broadcasted
- node: node where you get data
- nodepay: node used for payments. It's recommended to change the nodepay to http://127.0.0.1:9000 after clone and install Rovak's docker containers repo (Please see the details in dependencies section). If you won't use the docker, don't change the nodepay but we strongly recommend to use a smaller wallet from which to send the tokens because it can be compromised.
- amount: total amount of tokens to distribute (bonuses will be included in this amount)
- percentagebonusforvoters: percentage you want to give as bonus to the SR's voters; 0 for no bonuses
- minpayout: the minimum amount for a payout, must be integer (no decimals). Note: All the transactions will be broadcasted with integer token amounts.
- pk: the private key of the address from which the payments will be sent
- donations: a list of objects (address: amount) for send static amount every payout
- donationspercentage: a list of objects (address: percentage) for send static percentage every payout
- skip: a list of addresses to skip. The script will get all the addresses in the network that own that specific token. To skip the issuer's wallet you must add that address here. Example: "skip": ["ISSUER_ADDRESS"]

Edit the following line in accounts.js by replacing TOKEN_NAME with the token name:

```
let {data} = await xhr.get("https://api.tronscan.org/api/token/TOKEN_NAME/address", {
```

Edit the following line in votes.js with the SR address (This is mandatory ONLY if you want to give a bonus to the SR's voters):

```
const CANDIDATE_ADDRESS = '';

```


### Private pool
If you want to run a private pool, you need to edit config.json and:
- private: set to true
- whitelist: put a list of addresses you wish to include


## Testing it

1. Run accounts.js for getting holders' wallets that own the token. You can run the following line as many times as you want, it will clear and fill the data in accounts.json file

```node accounts.js```

2. Run votes.js for getting SR's voters wallets (This step is mandatory ONLY if you want to give a bonus to the SR's voters). You can run the following line as many times as you want, it will clear and fill the data in voters.json file

```node votes.js```

3. Run tokenairdrop.py and press Y/N for saving or not the data. For multiple tests you have to save the original poollogs.json file before running this command. After every test, replace the new poollogs.json file with the original one to test it again. Otherwise, in this file the amounts of every holder will be added at every run.
Also, the script will prepare the payments to be broadcasted by creating a file called "payments.sh". After running the following command, you can check holders' amounts in poollogs.json file and verify payments commands in payments.sh

```python3 tokenairdrop.py```

The command can be run with autosave parameter:

```python3 tokenairdrop.py -y```

4. Open payments.sh file. You can pick a single line beginning with "curl..." with a small amount of tokens to run it directly from the terminal for testing.


## Running it

1. Run accounts.js for getting holders' wallets that own the token

```node accounts.js```

2. Run votes.js for getting SR's voters wallets (This is mandatory ONLY if you want to give a bonus to the SR's voters. Otherwise, skip this step):

```node votes.js```

3. Run tokenairdrop.py and press Y/N for saving or not the data (the script will create a file called "payments.sh")

```python3 tokenairdrop.py```

The command can be run with autosave parameter:

```python3 tokenairdrop.py -y```

The file "payments.sh" will have all the payments shell commands. Run this file with:

```bash payments.sh```

The payments will be broadcasted every second.


## Batch mode

The scripts are also runnable by cron.

- give rights to execute

`chmod +x accounts.sh`

`chmod +x voters.sh`

`chmod +x tokenairdrop.sh`

- execute the scripts

`./accounts.sh`

`./voters.sh`

`./tokenairdrop.sh`


## Author
This software is created by lisk delegate "dakk", please consider a small donation if you
use this software: 
- "2324852447570841050L" for lisk
- "7725849364280821971S" for shift
- "AZAXtswaWS4v8eYMzJRjpd5pN3wMBj8Rmk" for ark
- "8691988869124917015R" for rise

## Features added by CryptoGirls
- adapted the script for TRX tokens airdrops

Please consider a small donation if you use this software:
- TRX: "TQk7fK1WfRqothSdTQBoYf7o81Byohzb1Y"
- BTC: "3Qv3uRZufA5t7GEz6BBH2khKbeUc7967RJ"
- ETH: "0xD174B1A997d9CB3F7D2dE284EE37e77a5de030bE"

## License
Copyright 2017-2018 Davide Gessa

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

