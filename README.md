# RagnaDocker

## Install
````
git clone https://github.com/n-devs/ro-db.git

git clone https://github.com/n-devs/ro-serve.git

git clone https://github.com/n-devs/ro-play.git

git clone https://github.com/n-devs/ro-wsproxy.git

git clone https://github.com/n-devs/ro-files.git

git clone https://github.com/n-devs/ro-fluxcp.git

git clone https://github.com/n-devs/ro-api.git

git clone https://github.com/n-devs/ro-ip.git
````

## Config RO Serve

#### 1 º - Baixe o emulador ou adicione o seu emulador na pasta [serve](https://github.com/rathena) como rathena.
EXP: 
````
  ./ro-serve
````
#### 2 º - Referencia do seu amulador.
EXP: 
````
- ./ro-serve:/ro-serve
````
#### 3 º - Nas configurações de acesso ao banco de dados que os emuladores utilizam como o arquivo ````inter_athena.conf````, substitua ````127.0.0.1```` por ````db````.
EXP-PASTA: 
````
  /ro-db/sql-file
    - item_db.sql
    - mob_db.sql
    - logs.sql
    - main.sql
````
EXP-docker-compose.yaml: 
````
  - ./ro-db/sql-files:/docker-entrypoint-initdb.d
````

#### 4 º - Nas configurações de acesso como ````char_athena.conf````, ````login_athena.conf```` e ````map_athena.conf```` substitua ````127.0.0.1```` por ````server````.
#### Mudar os arquivos
````
    ro-serve/conf/inter_athena.conf
    ro-serve/conf/char_athena.conf  
    ro-serve/conf/map_athena.conf
    ro-serve/conf/login_athena.conf
    ro-serve/conf/subnet_athena.conf
    ro-serve/src/custom/defines_pre.hpp
````
## Arquivos inter_athena.conf
Mude de as linhas ````_ip: 127.0.0.1```` para ````_ip: db````
EXP:
````login_server_ip: 127.0.0.1```` para  ````login_server_ip: db````
## Arquivos char_athena.conf
Mude as linhas
````
//login_ip: 127.0.0.1
//bind_ip: 127.0.0.1
//char_ip: 127.0.0.1
````
Para
````
login_ip: serve
bind_ip: serve
char_ip: serve
````
Lembrar de remover ````//```` os comentário
## Arquivos map_athena.conf
Mude as linhas
````
//char_ip: 127.0.0.1
//bind_ip: 127.0.0.1
//map_ip: 127.0.0.1
````
Para
````
char_ip: serve
bind_ip: serve
map_ip: serve
````
Lembrar de remover ````//```` os comentário
## Arquivos login_athena.conf
Mude as linhas
````
//bind_ip: 127.0.0.1
````
Para
````
bind_ip: serve
````
Lembrar de remover ````//```` os comentário
## Arquivos defines_pre.hpp
Antes da linha 
````#endif /* CONFIG_CUSTOM_DEFINES_PRE_HPP */````
adicione
````
#define PACKET_OBFUSCATION_KEY1 0x290551EA
#define PACKET_OBFUSCATION_KEY2 0x2B952C75
#define PACKET_OBFUSCATION_KEY3 0x2D67669B
#define PRERE
````
<details><summary><b>NEMO to diff ur client Packet Encryption</b></summary>
<p>

Use NEMO to diff ur client, and...

Do NOT select:

Disable Packet Encryption (Recommended)
Select:

Packet First Key Encryption, and following ur 1st key
Packet Second Key Encryption, and following ur 2nd key
Packet Third Key Encryption, and following ur 3rd key
Then make sure put your custom keys on db/[import/]packet_db.txt, in packet_keys_use: <key1>,<key2>,<key3>

[packet-keys](https://www.robrowser.com/prototype/packet-keys/)

</p>
</details>

#### Copile o seu emulador. 
Elembrar de acessar o conteiner do emulador ````docker exec -i -t serve-ragnarok /bin/bash````.

Exemplo:  ````./configure --enable-packetver=20141022 && make clean && make server```` ou ````./entrypoint.sh````

## Config Ro Files
#### ส่วนเสริม ที่ต้องนำ file ของตัวเองมาแก้อัพเดต
````
   ro-files/AI
   ro-files/BGM
   ro-files/data
   ro-files/resources
````



#### 4. passo
1 - ro-files/AI
  - update AI ของคุณ

2 - ro-files/BGM
  - update file .mp3 ของคุณ

3 - ro-files/data
  - update data ของคุณ โดยจะ export มาจาก file data.grf มาเป็น โฟล์เดอร์ data
 
4 - ro-files/resources
  - ในโฟล์เดอร์ ````resources```` จะประกอบไปด้วย ````data.grf```` และ ````DATA.INI```` หรือ file *.grf.

## Config Ro Play
#### ส่วนเสริม ที่ต้องนำ file ของตัวเองมาแก้อัพเดต
````
   ro-play/public/AI
   ro-play/public/BGM
   ro-play/public/System

````

# Comandos Mais usados 
#### Start o docker-composer
docker-compose -f "docker-compose.yaml" up -d --build
#### Stop o docker-composer
docker compose -f "docker-compose.yaml" down
#### Configurar o ro serve 
"./configure && make clean && make server" หรือ "./configure && make clean && make sql"
#### Start do ro serve 
"./athena-start start"

Obs:
````
	1 - Quando terminar de rodar os comandos é só abrir o navegador http://localhost:8080/
	2 - Os navegadores modernos impõem políticas de segurança mais rígidas, e conexões WebSocket devem ser feitas por meio de URLs seguras (WSS) quando a página principal é carregada por HTTPS. Utilize o ROBrowser no mesmo servidor do emulador para facilitar a comunicação.
	3  - Teste a sua conexão com o websocat. Tente instalar utilizando cargo install websocat e executando o comando como exemplo: "websocat ws://websocket.example.com:5999/127.0.0.1:690"	
````

# Step Run Command
 - docker exec -i -t serve-ragnarok /bin/bash
 - ./entrypoint.sh 
 - ./configure && make clean && make server
 - docker-compose -f "docker-compose.yaml" up -d --build
 - ./athena-start start
 - mysql -uragnarok -pragnarok;
 - USE ragnarok;
