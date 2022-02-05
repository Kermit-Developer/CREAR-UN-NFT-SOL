# CREAR UN NFT EN SOLANA CON METAPLEX Y CANDY MACHINE V2 05/02/2022
Crear un NFT y subirlo a la Red de Solana paso a paso


# PASO 1: Descargar e instalar VSCode y Node.js

https://code.visualstudio.com/
https://nodejs.org/es/

# PASO 2: Instalar Solana
Abrimos el CMD como admin y vamos a nuestro escritorio con "cd": 
C:\WINDOWS\system32>cd ../../

C:\>cd Users

C:\Users>cd Carlo

C:\Users\carlo>cd Desktop

C:\Users\carlo\Desktop> 

Una vez en el escritorio insertar para instalar solana: (source: https://docs.solana.com/es/cli/install-solana-cli-tools)
curl https://release.solana.com/v1.9.5/solana-install-init-x86_64-pc-windows-msvc.exe --output C:\solana-install-tmp\solana-install-init.exe --create-dirs
C:\solana-install-tmp\solana-install-init.exe v1.9.5 

# PASO 3: Crear una carpeta e instalar Metaplex. Debemos instalar varias herramientas aquí también. 
Necesitaremos:
git: para clonar el repositorio: https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Instalaci%C3%B3n-de-Git
node: JavaScript runtime. instalado anteriormente
yarn: administrador de paquetes para instalar las dependencias requeridas. En el cmd: npm install -g yarn
ts-node: Entorno de ejecución de TypeScript: https://www.npmjs.com/package/ts-node#installation

Metaplex será nuestra via para subir y mintear los NFTs en Solana. 

En la linea de comandos (cmd): 
1. git clone -b v1.1.1 https://github.com/metaplex-foundation/metaplex.git ~/metaplex
2. yarn install --cwd ~/metaplex/js/

# PASO 4: Crear una nueva billetera en Solana. Guardar Pubkey y Passphrase

en el cmd: solana-keygen new --outfile ~/.config/solana/devnet.json  

resultado: 
pubkey: Er1Tm8zqaMLxAyuCSWm4qZykgbYxFZUEHsvdBsrhKJ9a
======================================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
bachelor accuse excite timber put reopen because strategy final fetch together illegal

# PASO 5: Configurar la billetera con Solana
Apuntaremos hacia la devnet.
En el cmd: solana config set --url https://api.devnet.solana.com
Luego probaremos a enviarle SOL:
En el cmd: solana airdrop 2

Ahora nuestra billetera en devnet tendrá 2 SOL. Esto se puede repetir tantas veces como se quiera, el SOL en la devnet es inútil solo lo usaremos para pruebas por lo que con 2 SOL es suficiente por ahora.


# PASO 5: Preparar nuestros Assets y el Candy Machine
Iremos a Visual Studio y abriremos nuestra carpeta donde tenemos metaplex.
js - packages - cli - example assets (cambiaremos el nombre a "assets" para que sea más fácil luego)
example-candy-machine-upload-config.json (cambiaremos el nombre a config.json para que sea más fácil de llamar más adelante)

# PASO 6: Editar el Config.json.
Ejemplo: 
{
  "price": 0.2, #precio que tendrán nuestros NFTs
  "number": 10, #Número de NFTs que tiene nuestra colección
  "gatekeeper": { #Se usa a modo de protección, dejar por default
    "gatekeeperNetwork": "ignREusXmGrscGNUesoU9mxfds9AiYTezUKex2PsZV6",
    "expireOnUse": true
  },
  "solTreasuryAccount": "Er1Tm8zqaMLxAyuCSWm4qZykgbYxFZUEHsvdBsrhKJ9a", #MUY IMPORTANTE, es el wallet al que irá todo el dinero del mint.
  "splTokenAccount": null, #nada importante
  "splToken": null, #nada importante
  "goLiveDate": "2 Feb 2022 09:30:00 UTC", #la fecha en la que será el mint, ese es el formato, se puede ir editando más adelante.
  "endSettings": null, #nada importante
  "whitelistMintSettings": null, #nada importante
  "hiddenSettings": null, #nada importante
  "storage": "arweave",
  "ipfsInfuraProjectId": null, #nada importante
  "ipfsInfuraSecret": null, #nada importante
  "awsS3Bucket": null, #nada importante
  "noRetainAuthority": false, #nada importante
  "noMutable": false #si queremos hacer nuestros NFTs mutables o no
}


