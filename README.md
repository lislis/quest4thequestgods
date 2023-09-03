# Quest for the Quest Gods

This is a game to be played in a physcial space with (ideally) real people and a (preferably) blutooth-capable barcode scanner. (I had [this one](https://www.inateckoffice.com/pages/bcst-50-barcode-scanner-support-page) available for development). It is actively played by one person at a time but multiple people can be a part of it and play one after the other.

## Game prep

### 1. Barcode creation

### a) Default

Print the `_print_materials/barcodes.pdf` and cut out the individual codes. For historical reasons the words are in German.

### b) Custom

If you want to use your own list of words, adjust the `/public/input.txt` and generate a barcode (or qrcode if your scanner can handle it!) for each word (With some online barcode generation tool). Print the barcodes on physical paper and cut out the individual codes.

Build the app (see further down) and put it on a local or online webserver with your custom wordlist.

### 2. Barcode distribution

Place/ tape barcodes to people (eg to backs or upper arms. Ask first!) or somewhere in the space. People move, so that's fun! You don't have to want to be an active player to receive a barcode, but you can!

### 3. App and scanner

Pair the scanner with the phone that runs the app (aka has the website open).

## Game play

A player takes the phone and scanner and starts the Quest. They will run around and try to find the codes on other people (or places) and scan them.

After their time is up they get their total number of quested items. This can be used for a high score sheet if you want. (To compare successes and for bragging rights, of course!)

This is it! Have fun!



## Techie setup

This is a Vue3 app. Build it an run it on the device that is connected to the barcode scanner.

### Install dependencies

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
