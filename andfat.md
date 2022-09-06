
# near_guide

[<img width="695" alt="Знімок екрана 2022-08-03 о 14 33 05" src="https://user-images.githubusercontent.com/102728347/182598105-4c3dd80a-2ef1-4db7-a46a-419dbdc0ccc6.png">](https://near.org/community/)

* [Создание кошелька](https://github.com/MaxMavaIll/near_guide/blob/main/andfat.md#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BA%D0%BE%D1%88%D0%B5%D0%BB%D1%8C%D0%BA%D0%B0)
* [Встановлення ноди через скрипт](https://github.com/MaxMavaIll/near_guide/blob/main/andfat.md#%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%B0%D0%B2%D0%BB%D0%B8%D0%B2%D0%B0%D0%B5%D0%BC-node-%D1%87%D0%B5%D1%80%D0%B5%D0%B7-%D1%81%D0%BA%D1%80%D0%B8%D0%BF%D1%82)
* [Последовательная установка](https://github.com/MaxMavaIll/near_guide/blob/main/andfat.md#%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-node-%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE)
* [Активация node](https://github.com/MaxMavaIll/near_guide/blob/main/andfat.md#%D0%B0%D0%BA%D1%82%D0%B8%D0%B2%D0%B0%D1%86%D0%B8%D1%8F-node)
* [Создание валидатора](https://github.com/MaxMavaIll/near_guide/blob/main/andfat.md#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D0%B2%D0%B0%D0%BB%D0%B8%D0%B4%D0%B0%D1%82%D0%BE%D1%80%D0%B0)
* [Создание пинг](https://github.com/MaxMavaIll/near_guide/blob/main/andfat.md#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%B5%D0%BC-%D0%BF%D0%B8%D0%BD%D0%B3)


## Создание кошелька
Переходим по ссылке [create_wallet](https://wallet.shardnet.near.org/) и создаем кошелек.
Обязательно!!!
Не забудь сохранить сид фразы.



## Установка node

### Устанавливаем node через скрипт

Устанавливаем новую версию ветви
Узнать это можно перейдя по этой ссылке [last_version](https://github.com/near/stakewars-iii/blob/main/commit.md)
```
exempl 68bfa84ed1455f891032434d37ccad696e91e4f5
checkuot=<new_version>
```

```
wget https://raw.githubusercontent.com/MaxMavaIll/near_guide/main/near_install_node.sh && chmod +x near_install_node.sh && bash near_install_node.sh
  
```
Update cli (эта команда может значиться позже)
```
sudo npm install -g near-cli

```
### Установка node последовательно

Заходим в root пользователя
```
sudo su
```

```
sudo apt update && sudo apt upgrade -y

```
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
sudo apt install build-essential nodejs
PATH="$PATH"

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

Проверяем версии
```
node -v && npm -v

```
<img width="225" alt="Знімок екрана 2022-07-30 о 16 18 48" src="https://user-images.githubusercontent.com/102728347/181916319-705da541-48ac-4d09-bda7-d38980ddfdc6.png">

Устанавливаем NEAR-CLI(также таким образом можно переустановить near-cli)
```
sudo npm install -g near-cli

```
<img width="532" alt="Знімок екрана 2022-07-30 о 16 21 56" src="https://user-images.githubusercontent.com/102728347/181916300-e026ef96-07f6-4a7c-8a5e-b8c4e7e31880.png">

Настраиваем окружение. Текущий тест проходит в сети shardnet. Вводим название сети как переменное
```
export NEAR_ENV=shardnet
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc

```


Устанавливаем и настраиваем Python pip
```
sudo apt install python3-pip
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"

```

Устанавливаем Rust
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



Устанавливаем репозиторий
```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
#Обов'язково провіряємо чи це нова версія
git checkout c1b047b8187accbf6bd16539feb7bb60185bdc38

```

Собираем бинарные файлы
```
cargo build -p neard --release --features shardnet

```
<img width="550" alt="Знімок екрана 2022-07-30 о 17 18 32" src="https://user-images.githubusercontent.com/102728347/181918497-a957bb7a-47e8-4e33-8ffa-f8441a65ad2d.png">

Инициализируем ноду и загружаем генезис файл
```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis

```

Загружаем файл конфигураций
```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json

```

Загружаем снепшот
```
sudo apt-get install awscli -y
cd ~/.near
aws s3 --no-sign-request cp s3://build.openshards.io/stakewars/shardnet_noarchive/data.tar.gz .
tar -xzvf data.tar.gz

```

Запускаем ноду
```
cd ~
cd nearcore 
./target/release/neard --home ~/.near run

```
Вы должны увидеть как пошла загрузка и проценты обязательно должны увеличиваться

<img width="1307" alt="Знімок екрана 2022-08-01 о 16 49 51" src="https://user-images.githubusercontent.com/102728347/182184400-d5277b2c-5949-4434-906c-5ee3638f55f5.png">

Теперь можно остановить Cntr+C и создаем сервисник
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

Запускаем сервисник
```
sudo systemctl daemon-reload
sudo systemctl enable neard
sudo systemctl restart neard

```



Команда для проверки логов
Здесь также должно показываться что идет загрузка headers
```
journalctl -u neard -f -o cat

```
Когда оно загрузится вы увидите следующие логи


Проверяем синхронизацию
```
curl -s http://127.0.0.1:3030/status | jq .sync_info

```
Если синхронизация показывает false, тогда можно переходить к следующему разделу

<img width="553" alt="Знімок екрана 2022-08-03 о 16 06 32" src="https://user-images.githubusercontent.com/102728347/182615247-d570fe14-b076-496a-ba6f-de621c6afb36.png">



## Активация node

```
near login
# Водимо адрес в браузер https://wallet.shardnet.near.org/login/****
```

Подтверждаем все запросы

<img width="360" alt="Знімок екрана 2022-08-03 о 16 24 59" src="https://user-images.githubusercontent.com/102728347/182619950-f869cc10-50d0-48e5-8fbc-deb6692769eb.png">

<img width="366" alt="Знімок екрана 2022-08-03 о 16 25 41" src="https://user-images.githubusercontent.com/102728347/182619431-3d40f82f-ff3d-4a97-b861-0c045c9afbf1.png">

<img width="386" alt="Знімок екрана 2022-08-01 о 18 45 42" src="https://user-images.githubusercontent.com/102728347/182188462-a11fcb15-5c8c-4eb6-b19c-a0deed819320.png">

Далее когда вы пройдете вы получите ошибку 403 Not Fount.

После этого переходим в терминал и водим ваш ACCOUNT_ID
Это должно выглядеть так

<img width="675" alt="Знімок екрана 2022-08-03 о 16 34 16" src="https://user-images.githubusercontent.com/102728347/182621268-373c1fc2-e39b-4666-bcf3-740d96ec8412.png">

## Создание валидатора

Заменяем moniker на свой ник
```
MONIKER=<MONIKER>
```
```
POOL=$MONIKER.factory.shardnet.near
ACCOUNT_ID=$MONIKER.shardnet.near

```

Теперь создаем ключи валидатора
```
near generate-key $ACCOUNT_ID

```
Делаем некоторые изменения в создавшемся файле
```
sed -i 's/private_key/secret_key/' ~/.near-credentials/shardnet/$ACCOUNT_ID.json
sed -i s/$ACCOUNT_ID/$POOL/ ~/.near-credentials/shardnet/$ACCOUNT_ID.json

```
Копируем ключ в другую дерикторию и переименуем
```
cp ~/.near-credentials/shardnet/$ACCOUNT_ID.json ~/.near/validator_key.json

```

И перезапускаем ноду
```
sudo systemctl restart neard

```

Это еще не все для того, чтобы создать валидатора, мы должны выполнить еще некоторые действия.
В данный скрипт мы должны заменить `public_key` на ваш публичный ключ

```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "$MONIKER", "owner_id": "$ACCOUNT_ID", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="$ACCOUNT_ID" --amount=30 --gas=300000000000000
```

Также нужно забросить соответствующую сумму для того, чтобы валидатор находился в валидном сете.
```
near call $MONIKER deposit_and_stake --amount <amount> --accountId $ACCOUNT_ID --gas=300000000000000 
```
Другие команды, которые могут понадобиться

* Unstake and Unstake All
```
near call <staking_pool_id> unstake '{"amount": "<amount yoctoNEAR>"}' --accountId <accountId> --gas=300000000000000
```
```
near call <staking_pool_id> unstake_all --accountId <accountId> --gas=300000000000000
```
* Снять или Снять всю сумму. 
```
near call <staking_pool_id> withdraw '{"amount": "<amount yoctoNEAR>"}' --accountId <accountId> --gas=300000000000000
```
```
near call <staking_pool_id> withdraw_all --accountId <accountId> --gas=300000000000000
```
* Доступный для вывода. Вы можете снять деньги с контракта, только если они разблокированы.
```
near view <staking_pool_id> is_account_unstaked_balance_available '{"account_id": "<accountId>"}'
```
* Посмотреть стейк баланс.
```
near view <staking_pool_id> get_account_staked_balance '{"account_id": "<accountId>"}'
```
* Растейка баланса.
```
near view <staking_pool_id> get_account_unstaked_balance '{"account_id": "<accountId>"}'
```

# Создаем пинг
Устанавливаем такой скрипт
```
wget -P ~/ https://raw.githubusercontent.com/MaxMavaIll/near_guide/main/ping.sh && chmod +x ~/ping.sh 
mkdir -p $HOME/logs

```
Устанавливаем crontab если он у вас не установлен и открываем редактор, где будем задавать изменения
```
apt install crontab
crontab -e

```
Если вы раньше не пользовались им вам дадут выбор какой использовать редактор(я буду пользоваться nano)
Теперь вставляем такую фразу в конец списка 
```
0 */2 * * * bash /root/ping.sh
```
Чтобы увидеть logs этих команд нужно вести
```
nano ~/logs/all.log

```

