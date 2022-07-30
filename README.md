# near_guide

Оновлюємо пакети
```
sudo apt update && sudo apt upgrade -y

```

Встановлюємо інструменти розробника, Node.js, npm та інші необхідні пакети
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

![image_2022-07-18_18-18-09](https://user-images.githubusercontent.com/102728347/181916218-6c0ac491-1f2b-4d79-bf56-f4bd3e7260b0.png)

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
```
Вибираємо -> 1
```
source ~/.profile
source ~/.cargo/env

```

