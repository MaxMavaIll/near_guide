# near_guide

* [Встановлення node](https://github.com/MaxMavaIll/near_guide/blob/main/README.md#%D0%B2%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-node)
* [Створення гаманця]()
* [Активування node]()
* [Cтворення валідатора]()



## Встановлення node

Заходимо в root користувача
```
sudo su
```

```
sudo apt update && sudo apt upgrade -y

```


```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config llvm cargo

```
```
sudo apt install clang build-essential make

```
```
sudo apt install curl jq
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  

```
```
sudo apt install nodejs
PATH="$PATH"

```

Перевіряємо версії
```
node -v && npm -v

```
<img width="225" alt="Знімок екрана 2022-07-30 о 16 18 48" src="https://user-images.githubusercontent.com/102728347/181916319-705da541-48ac-4d09-bda7-d38980ddfdc6.png">

Устанавливаем NEAR-CLI
```
sudo npm install -g near-cli

```
<img width="532" alt="Знімок екрана 2022-07-30 о 16 21 56" src="https://user-images.githubusercontent.com/102728347/181916300-e026ef96-07f6-4a7c-8a5e-b8c4e7e31880.png">

Налаштовуємо оточення. Поточний тест проходить у мережі shardnet. Вводимо назву мережі як змінну
```
export NEAR_ENV=shardnet
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc

```


Встановлюємо та налаштовуємо Python pip
```
sudo apt install python3-pip
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"

```

Встановлюємо Rust
```
sudo apt install curl build-essential gcc make -y
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
# Вибираємо -> 1) Proceed with installation (default)
```
```
source ~/.profile
source ~/.cargo/env

```
<img width="576" alt="Знімок екрана 2022-07-30 о 16 36 54" src="https://user-images.githubusercontent.com/102728347/181917044-1449dbb5-49a1-4aa7-bb56-0b85c096f3dc.png">



Встановлюємо репозиторій
```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
git checkout c1b047b8187accbf6bd16539feb7bb60185bdc38

```

Збираємо бінарні файли
```
cargo build -p neard --release --features shardnet

```
<img width="550" alt="Знімок екрана 2022-07-30 о 17 18 32" src="https://user-images.githubusercontent.com/102728347/181918497-a957bb7a-47e8-4e33-8ffa-f8441a65ad2d.png">

Ініціалізуємо ноду та завантажуємо генезис файл
```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis

```

Завантажуємо файл конфігурацій
```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json

```
Створюємо сервісник
```        
echo "[Unit]
Description=NEARd Daemon Service

[Service]
Type=simple
User=root
WorkingDirectory=/root/.near
ExecStart=/root/nearcore/target/release/neard run
Restart=on-failure
RestartSec=30
KillSignal=SIGINT
TimeoutStopSec=45
KillMode=mixed

[Install]
WantedBy=multi-user.target"  | tee -a /etc/systemd/system/neard.service

```

Запускаємо сервісник
```
sudo systemctl daemon-reload
sudo systemctl enable neard
sudo systemctl restart neard

```

Команда для провірики логів
```
journalctl -u neard -f -o cat

```
<img width="1380" alt="Знімок екрана 2022-07-30 о 23 23 07" src="https://user-images.githubusercontent.com/102728347/181995063-5575e37f-109c-4f33-9db7-670d783d2ed7.png">


## Cтворення гамнця
Переходимо по силці [create_wallet](https://wallet.shardnet.near.org/). Створюємо гаманець і зберігаємо сід фрази.

## Активація node 

Запускаємо команду
```
near login

```

## Створення валідатора
