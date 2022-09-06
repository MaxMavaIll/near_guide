
# near_guide

[<img width="695" alt="Знімок екрана 2022-08-03 о 14 33 05" src="https://user-images.githubusercontent.com/102728347/182598105-4c3dd80a-2ef1-4db7-a46a-419dbdc0ccc6.png">](https://near.org/community/)

* [Создание кошелька](https://github.com/MaxMavaIll/near_guide/blob/main/andfat.md#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BA%D0%BE%D1%88%D0%B5%D0%BB%D1%8C%D0%BA%D0%B0)
* [Встановлення ноди через скрипт](https://github.com/MaxMavaIll/near_guide/blob/main/andfat.md#%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-node)
* []()
* []()
* []()
* []()
* []()
* []()

## Создание кошелька
Переходим по ссылке [create_wallet](https://wallet.shardnet.near.org/) и создаем кошелек.
Обязательно!!!
Не забудь сохранить сид фразы.



## Установка node

Устанавливаем node через скрипт

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

