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
ts-node: Entorno de ejecución de TypeScript en el cmd: npm install -g ts-node

Metaplex será nuestra via para subir y mintear los NFTs en Solana. 

En la linea de comandos (cmd): 
1. git clone -b v1.1.1 https://github.com/metaplex-foundation/metaplex.git ~/metaplex
2. yarn install --cwd ~/metaplex/js/

# PASO 4: Crear una nueva billetera en Solana. Guardar Pubkey y Passphrase

en el cmd: solana-keygen new --outfile ~/.config/solana/devnet.json  

resultado: pubkey: Er1Tm8zqaMLxAyuCSWm4qZykgbYxFZUEHsvdBsrhKJ9a
======================================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
bachelor accuse excite timber put reopen because strategy final fetch together illegal
Guardar ambas.

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
Ejemplo: (Lee más aquí https://docs.metaplex.com/candy-machine-v2/configuration)
{
  "price": 0.2, 
  "number": 10,
  "gatekeeper": { 
    "gatekeeperNetwork": "ignREusXmGrscGNUesoU9mxfds9AiYTezUKex2PsZV6",
    "expireOnUse": true
  },
  "solTreasuryAccount": "Er1Tm8zqaMLxAyuCSWm4qZykgbYxFZUEHsvdBsrhKJ9a", 
  "splTokenAccount": null, 
  "splToken": null, 
  "goLiveDate": "2 Feb 2022 09:30:00 UTC", 
  "endSettings": null, 
  "whitelistMintSettings": null, 
  "hiddenSettings": null, 
  "storage": "arweave-sol",
  "ipfsInfuraProjectId": null, 
  "ipfsInfuraSecret": null, 
  "awsS3Bucket": null, 
  "noRetainAuthority": false,
  "noMutable": false
}

# PASO 7: Carpeta Assets.
Deberá estar llena con nuestros pares de  .png + metadata. Puedes usar los que proporcionan de ejemplo desde metaplex o usar los tuyos.
{
    "name": "Number #0010",
    "symbol": "",
    "description": "Collection of 10 numbers on the blockchain. This is the number 10/10.",
    "seller_fee_basis_points": 500,
    "image": "9.png",
    "attributes": [
        {"trait_type": "Layer-1", "value": "0"},
        {"trait_type": "Layer-2", "value": "0"}, 
        {"trait_type": "Layer-3", "value": "1"},
        {"trait_type": "Layer-4", "value": "0"}
    ],
    "properties": {
        "creators": [{"address": "N4f6zftYsuu4yT7icsjLwh4i6pB1zvvKbseHj2NmSQw", "share": 100}],
        "files": [{"uri": "9.png", "type": "image/png"}]
    },
    "collection": {"name": "numbers", "family": "numbers"}
}
  Aquí un ejemplo de metadata.

# PASO 8: Subir nuestros Assets a la Candy Machine.
En el cmd:
ts-node ~/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts upload -e devnet -k ~/.config/solana/devnet.json -cp ~\metaplex\js\packages\cli\config.json -c example ~\metaplex\js\packages\cli\assets
Si tus archivos tienen otro nombre deberás adaptar este comando. por ejemplo config.json o assets
Este paso toma tiempo, una colección de 5000 dura aprox 6h en subirse.
Guardarse la Candy Machine Pub key: 
candy machine public key 59TgX6HXcNBD9sGTSMjTsKQQCbbsnqQYtZQUXUaqKCEo

# PASO 9: Actualizar Candy Machine
para actualizar la candy machine(digamos que has realizado un cambio en el config.json), cambiar upload por update_candy_machine en el comando anterior:
ts-node ~/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts update_candy_machine -e devnet -k ~/.config/solana/devnet.json -cp ~\metaplex\js\packages\cli\config.json -c example ~\metaplex\js\packages\cli\assets

# PASO 10: Verificar
para verificar, eliminar assets y cambiar update_candy_machine por verify_upload:
ts-node ~/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts verify_upload -e devnet -k ~/.config/solana/devnet.json -cp ~\metaplex\js\packages\cli\config.json -c example

# PASO 11: Comprobar nuestro balance.
Veamos como cambia nuestro balance en Devnet después de la subida.
En el CMD: solana balance
