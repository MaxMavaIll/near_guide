
# near_guide

[<img width="695" alt="Знімок екрана 2022-08-03 о 14 33 05" src="https://user-images.githubusercontent.com/102728347/182598105-4c3dd80a-2ef1-4db7-a46a-419dbdc0ccc6.png">](https://near.org/community/)


## Cтворення гамнця
Переходимо по силці [create_wallet](https://wallet.shardnet.near.org/) і створюємо гамонець.
Обов'зково!!!
Не забуть зберегти сід фрази.

Замінюємо moniker на свій нік
```
MONIKER=<MONIKER>
```
```
POOL=$MONIKER.factory.shardnet.near
ACCOUNT_ID=$MONIKER.shardnet.near

```

## Встановлення node

Встановлюємо node через скрипт
```
wget https://raw.githubusercontent.com/MaxMavaIll/near_guide/main/near_install_node.sh && chmod +x near_install_node.sh && bash near_install_node.sh
  
```

Команда для провірики логів
Тут також повинно показуватися що іде завантаження headers
```
journalctl -u neard -f -o cat

```
Коли воно загрузиться ви побачите такі логи


Провіряємо синхронізацію 
```
curl -s http://127.0.0.1:3030/status | jq .sync_info

```
Якщо синхронізація показує false тоді можна переходити до наступного розділу

<img width="553" alt="Знімок екрана 2022-08-03 о 16 06 32" src="https://user-images.githubusercontent.com/102728347/182615247-d570fe14-b076-496a-ba6f-de621c6afb36.png">



## Активація node 

```
near login
# Водимо адрес в браузер https://wallet.shardnet.near.org/login/****
```

Підтвержуємо всі запити

<img width="360" alt="Знімок екрана 2022-08-03 о 16 24 59" src="https://user-images.githubusercontent.com/102728347/182619950-f869cc10-50d0-48e5-8fbc-deb6692769eb.png">

<img width="366" alt="Знімок екрана 2022-08-03 о 16 25 41" src="https://user-images.githubusercontent.com/102728347/182619431-3d40f82f-ff3d-4a97-b861-0c045c9afbf1.png">

<img width="386" alt="Знімок екрана 2022-08-01 о 18 45 42" src="https://user-images.githubusercontent.com/102728347/182188462-a11fcb15-5c8c-4eb6-b19c-a0deed819320.png">

Далі коли ви пройдете ви отримаєте помилку 403 Not Fount.

Після цього переходимо в термінал і водимо ваш ACCOUNT_ID
Це повинно виглядати так

<img width="675" alt="Знімок екрана 2022-08-03 о 16 34 16" src="https://user-images.githubusercontent.com/102728347/182621268-373c1fc2-e39b-4666-bcf3-740d96ec8412.png">

## Створення валідатора
Тепер створюєм ключі валідатора
```
near generate-key $ACCOUNT_ID

```
Робимо деякі зміни у створившимуся файлі 
```
sed -i 's/private_key/secret_key/' ~/.near-credentials/shardnet/$ACCOUNT_ID.json
sed -i s/$ACCOUNT_ID/$POOL/ ~/.near-credentials/shardnet/$ACCOUNT_ID.json

```
Копіюємо ключ у іншу дерикторію і перейменовуємо
```
cp ~/.near-credentials/shardnet/$ACCOUNT_ID.json ~/.near/validator_key.json

```

І перезапускаємо ноду
```
sudo systemctl restart neard

```

Це ще не все для того щоб створити валідатора ми повинні виконати ще деякі дії
В даний скрипт ми повинні замінити `public_key` на ваш публічний ключ

```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "$MONIKER", "owner_id": "$ACCOUNT_ID", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="$ACCOUNT_ID" --amount=30 --gas=300000000000000
```

Також потрібно  закинути відповідну суму для того, щоб валідатор був у валідному сеті
```
near call $MONIKER deposit_and_stake --amount <amount> --accountId $ACCOUNT_ID --gas=300000000000000 
```
Інші команди які можуть знадобитися

* Unstake and Unstake All
```
near call <staking_pool_id> unstake '{"amount": "<amount yoctoNEAR>"}' --accountId <accountId> --gas=300000000000000
```
```
near call <staking_pool_id> unstake_all --accountId <accountId> --gas=300000000000000
```
* Зняти або Зняти всю суму. 
```
near call <staking_pool_id> withdraw '{"amount": "<amount yoctoNEAR>"}' --accountId <accountId> --gas=300000000000000
```
```
near call <staking_pool_id> withdraw_all --accountId <accountId> --gas=300000000000000
```
* Доступний для виведення. Ви можете зняти кошти з контракту, лише якщо вони розблоковані.
```
near view <staking_pool_id> is_account_unstaked_balance_available '{"account_id": "<accountId>"}'
```
* Подивитися стейк баланс.
```
near view <staking_pool_id> get_account_staked_balance '{"account_id": "<accountId>"}'
```
* Розтейкання балансу.
```
near view <staking_pool_id> get_account_unstaked_balance '{"account_id": "<accountId>"}'
```

# Створюємо пінг
Встановлюємо такий скрипт
```
wget -P ~/ https://raw.githubusercontent.com/MaxMavaIll/near_guide/main/ping.sh && chmod +x ~/ping.sh 
mkdir -p $HOME/logs
```
Втановлюємо crontab якщо він у вас не встановлений і відкриваємо редактор де будемо задавати зміни
```
apt install crontab
crontab -e

```
Якщо ви раніше не користувалися ним вам дадуть вибір який використовувати редактор(я буду користуватися nano)
Тепер вставляємо таку фразу в кінець списку 
```
0 */2 * * * bash /root/ping.sh
```
Щоб побачити logs цих команд потрібно вести
```
nano ~/logs/all.log

```
