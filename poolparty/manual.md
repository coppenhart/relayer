1. **Clone, checkout and install the latest release ([releases page](https://github.com/cosmos/relayer/releases)).**

   *[Go](https://go.dev/doc/install) needs to be installed and a proper Go environment needs to be configured*

    ```shell
    git clone https://github.com/cosmos/relayer.git
    git checkout v2.0.0
    cd relayer && make install
    ```
   
2. **Download configuration file**

   ```shell
   wget -qO $HOME/.relayer/config/config.yaml "https://github.com/coppenhart/relayer/raw/main/poolparty/config.yaml"
   ```
   
3. **Set memo with discord ID**
   ```shell
   USERNAME="your_discord_username"
   sed -i -e "s/memo: *"".*/memo: \"$USERNAME\"/" $HOME/.relayer/config/config.yaml
   ```

3. **Import wallet** 

  To import wallet you can use command below.
  ```shell
  rly keys restore stride wallet "your_mnemonic_phrases"
  rly keys restore gaia wallet "your_mnemonic_phrases"
  ```
  
5. **Create service**

  ```shell
  sudo tee /etc/systemd/system/rly.service > /dev/null <<EOF
  [Unit]
  Description=Relayer
  After=network-online.target

  [Service]
  User=$USER
  ExecStart=$(which rly) start poolparty --processor events --log-format logfmt
  Restart=on-failure
  RestartSec=3
  LimitNOFILE=65535

  [Install]
  WantedBy=multi-user.target
  EOF
  ```
  
6. **Enable service and start service**

  ```shell
  sudo systemctl daemon-reload
  sudo systemctl enable rly
  sudo systemctl restart rly && journalctl -fu rly
  ```
  
7. **Check logs**

  ```shell
  journalctl -fu rly
  ```
