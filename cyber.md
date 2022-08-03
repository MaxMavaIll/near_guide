
# near_guide

[<img width="695" alt="Знімок екрана 2022-08-03 о 14 33 05" src="https://user-images.githubusercontent.com/102728347/182598105-4c3dd80a-2ef1-4db7-a46a-419dbdc0ccc6.png">](https://near.org/community/)


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

<img width="1388" alt="Знімок екрана 2022-08-01 о 18 30 52" src="https://user-images.githubusercontent.com/102728347/182185558-36e9aa54-006f-40f3-aeff-cc5dba7320e5.png">


Провіряємо синхронізацію 
```
curl -s http://127.0.0.1:3030/status | jq .sync_info

```
Якщо синхронізація показує false тоді можна переходити до наступного розділу

<img width="553" alt="Знімок екрана 2022-08-01 о 18 34 27" src="https://user-images.githubusercontent.com/102728347/182186039-330f7703-d0a4-4151-93a1-91e99e845842.png">


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

## Активація node 

```
near login
# Водимо адрес в браузер https://wallet.shardnet.near.org/login/****
```

Підтвержуємо всі запити
<img width="1384" alt="Знімок екрана 2022-08-01 о 18 44 39" src="https://user-images.githubusercontent.com/102728347/182188439-e282a7e7-89cb-4f57-a11a-ade45ce4e75f.png">

<img width="1384" alt="Знімок екрана 2022-08-01 о 18 45 15" src="https://user-images.githubusercontent.com/102728347/182188450-5d4ce4c5-dcef-4288-bffb-6c8325a513d9.png">

<img width="386" alt="Знімок екрана 2022-08-01 о 18 45 42" src="https://user-images.githubusercontent.com/102728347/182188462-a11fcb15-5c8c-4eb6-b19c-a0deed819320.png">

Далі коли ви пройдете ви отримаєте помилку 403 Not Fount.

Після цього переходимо в термінал і водимо ваш ACCOUNT_ID
Це повинно виглядати так

<img width="1180" alt="Знімок екрана 2022-08-01 о 18 52 01" src="https://user-images.githubusercontent.com/102728347/182189875-ee3312a1-a697-4ba9-8278-ed9db2a13bab.png">



## Створення валідатора
Тепер створюєм ключі валідатора
```
near generate-key $ACCOUNT_ID

```
Робимо деякі зміни у створившимуся файлі 
```
sed -i 's/private_key/secret_key/' ~/.near-credentials/shardnet/$ACCOUNT_ID.json
sed 's/$ACCOUNT_ID/secret_key/' ~/.near-credentials/shardnet/$ACCOUNT_ID.json
```
Копіюємо ключ у іншу дерикторію і перейменовуємо
```
cp ~/.near-credentials/shardnet/$ACCOUNT_ID.json ~/.near/validator_key.json

```

І перезапускаємо ноду
```
sudo systemctl daemon-reload
sudo systemctl enable neard
sudo systemctl restart neard

```

Це ще не все для того щоб створити валідатора ми повинні виконати ще деякі дії
В даний скрипт ми повинні замінити `public_key` на ваш публічний ключ

```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "$MONIKER", "owner_id": "$ACCOUNT_ID", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="$ACCOUNT_ID" --amount=30 --gas=300000000000000

```

Також потрібно  закинути відповідну суму для того, щоб валідатор був у валідному сеті
```
near call andfat deposit_and_stake --amount <amount> --accountId $ACCOUNT_ID --gas=300000000000000 

```
