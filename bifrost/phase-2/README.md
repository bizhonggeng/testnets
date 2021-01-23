# Bifrost-2

If you haven't pariticipate in Phase-1 of Bifrost testnet, please fill in this [form](http://nyancat-irisnet.mikecrm.com/JsLCZ4H) to complete your sign-up process, and remember to set your validator moniker to `<your-name>-<pgp-id>`, identity to `<pgp-id>`. You need to complete the tasks use your validator account.

## How to use keybase

The `keybase ID` is a 16-digit string that is generated with a keybase.io account. It's a cryptographically secure method of verifying your identity across multiple online networks. The Keybase API allows us to retrieve your Keybase avatar. This is how you can add a logo to your validator profile

1. Go to https://keybase.io/
2. Click `login`，then` Join Keybase` and finish the sign up process
3. Click `add a PGP key`，then choose `I need a public key`，then click `Ok，got it`。
4. After filling the form, you should click on `Let the math begin`，then you could get your own key, then the whole registration process is finished. 

### Claim Ownership of a GitHub account：

1. Click`prove your GitHub identity`，then type your GitHub accoun.
2. Click `continue`，choose`in the browser`，then click `continue` again. Choose `github popup` to open gist。
3. Create a `keybase.md` file in gits, and fill in the contents you need, then choose to `creat public gist` named `keybase.md`
4. Next, you need to confirm that you have complete the chaim, Then you should see your **PGP ID**. This ID is used to identify yourself in IRISnet incentivized  testnet game. 

## How to Join

### install go

#### Install git, gcc and make
```
sudo apt install git build-essential --yes
```
#### Install Go with Snap
```
sudo snap install go --classic
```

- set your $GOPATH, $GOBIN, and $PATH environment variables, for example:
```
echo "export GOPATH=$HOME/go" >> ~/.bashrc
source ~/.bashrc
echo "export GOBIN=$GOPATH/bin" >> ~/.bashrc
source ~/.bashrc
echo "export PATH=$PATH:$GOBIN" >> ~/.bashrc
source ~/.bashrc
```


### Install iris
```
git clone --branch v1.0.0-rc0 https://github.com/irisnet/irishub
cd irishub
make install
iris version
```
### Join The Testnet
```
iris init <moniker> --chain-id="bifrost-2"
curl -o ~/.iris/config/config.toml https://gist.githubusercontent.com/maiyude2018/fde654512a32ba04347ee883c9b48dfd/raw/05966b6e2ef565b7165e2e6d09da0de35164d3ca/config.toml
curl -o ~/.iris/config/genesis.json https://gist.githubusercontent.com/maiyude2018/87f515f581189094ba8221309f480b55/raw/b1ebc23116019953df44016bc0d8a021ae76dcaf/genesis.json

```
- edit config.toml
```
nano ~/.iris/config/config.toml
```
link-11：

moniker = "CHANGE THIS TO YOUR MONIKER"，Make it your own moniker

```
iris start
```
```
iris status
```


### Upgrade to Validator Node

#### Create a Wallet
```
iris keys add <key-name> --keyring-backend "os"
```
**Faucet**

Unfortunately we do not have a public faucet for bifrost, however you are welcome to ask for the test tokens in our validator communities:

- [Riot English community](https://matrix.to/#/!bmimZgJrUWSmxqQEmG:matrix.org?via=matrix.org&via=t2bot.io)
- [QQ Chinese community](https://jq.qq.com/?_wv=1027&k=5BeP3tJ)

Waiting for nodes to catchup

#### Create Validator
- Only if your node has caught-up, you can run the following command to upgrade your node to be a validator.

```
iris tx staking create-validator \
  --amount=90000000ubif \
  --pubkey=$(iris tendermint show-validator) \
  --moniker="<moniker> " \
  --chain-id="bifrost-2" \
  --commission-rate="1" \
  --commission-max-rate="1" \
  --commission-max-change-rate="1" \
  --min-self-delegation="1" \
  --gas="auto" \
  --gas-adjustment="1.2" \
  --gas-prices="1ubif" \
  --from="<key-name> " --keyring-backend "os"
```


#### (Optional) Configure the service

To allow your iris instance to run in the background as a service you need to execute the following command

```
tee /etc/systemd/system/iris.service > /dev/null <<EOF  
[Unit]
Description=iris Full Node
After=network-online.target

[Service]
User=root
ExecStart=/root/go/bin/iris start
Restart=always
RestartSec=3
LimitNOFILE=4096 # To compensate the "Too many open files" issue.

[Install]
WantedBy=multi-user.target
EOF
systemctl enable iris

#Start node
systemctl start iris
#view log
journalctl -u iris.service -f
```
- WARNING
If you are logged as a user which is not root, make sure to edit the User and ExecStart values accordingly

### Resources

**Explorer**

https://bifrost.irisplorer.io

**Documentation**

https://bifrost.irisnet.org/docs/


## Task list and reward

### Network upgrade task list

| Task                                        | Details                                                              | Completion standard                         | Badge     |
| ------------------------------------------- | -------------------------------------------------------------------- | ------------------------------------------- | --------- |
| Testnet parameter-change voting   | Vote for bifrost-2 parameter-change proposal (activating IBC bidirection transfer)               | Successfully voted for bifrost-2 parameter-change proposal   | Bronze *1 |
| Testnet upgrade voting   | Vote for bifrost-2 upgrade proposal (adding SDK modules)               | Successfully voted for bifrost-2 upgrade proposal   | Bronze *1 |
| Testnet upgrade | Upgrade the validator node according to the upgrade proposal | The upgraded validator node signs at least 1 block in the first 200 blocks after the testnet upgraded  | Silver *1 |


### IBC task list

| Task                                        | Details                                                              | Completion standard                         | Badge     |
| ------------------------------------------- | -------------------------------------------------------------------- | ------------------------------------------- | --------- |
| Send IBC transfer tx to other testnets      | Send IBC transfer tx from bifrost-2 to other testnets                        | Successfully send at least one IBC tx to other testnets  | Gold *1 |
| Receive IBC transfer tx from other testnets      | Receive IBC transfer tx from other testnets to bifrost-2       | Successfully receive at least one IBC tx to the your registered address  | Gold *1 |

**NOTE:**

IBC transfer can be done very easily through the clients, for example, you can send tokens from `Bifrost-2` to other testnets:

```bash
iris tx ibc-transfer transfer transfer <src-channel> <receiver> <amount> \
    --absolute-timeouts \
    --packet-timeout-timestamp 0 \
    --from <sender> \
    --chain-id bifrost-2
```

 For more details, please refer to the client docs:

```bash
iris tx ibc-transfer transfer --help
```

