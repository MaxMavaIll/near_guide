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
node -v
```
```
npm -v
```

Устанавливаем NEAR-CLI
```
sudo npm install -g near-cli

```

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

