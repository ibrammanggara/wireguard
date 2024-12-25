---
title: Wireguard VPN
---
---
noted: p
---


# **membuat vpn dengan wireguard**

note : gunakan sudo untuk hak akses penuh pada terminal, tetapi jika anda masuk dengan user root, anda bisa menggunakan perintah langsung tanpa menambahkan perintah sudo

## **installasi wireguard**

pada kedua sisi (client & server os ubuntu) install wireguard, dengan perintah

```
sudo apt install wireguard
```

## **konfigurasi server**

pada server isikan perintah ini

```
sudo wg genkey | tee private.key
```

```
sudo ip link add wg0 type wireguard
```

```
sudo ip addr add 10.5.10.1/24 dev wg0
```

```
sudo wg set wg0 private-key ./private.key
```

```
sudo ip link set wg0 up
```

## **konfigurasi client**

pada sisi client isikan perintah

```
sudo wg genkey | tee private.key
```

```
sudo wg pubkey < private.key
```

```
sudo ip link add wg0 type wireguard
```

```
sudo ip addr add 10.5.10.2/24 dev wg0
```

```
sudo wg set wg0 private-key ./private.key
```

```
sudo ip link set wg0 up
```

## **membuat peer ke kedua sisi**

kalian terlebih dahulu untuk mengecek kedua sisi (client & server) pada interface wg0 yang telah di tambahkan dengan perintah

```
ip a
```

jika terdapat wg0 dengan alamat ip yang tertera, itu sudah berhasil di pasangkan, lanjut dengan perintah dibawah ini untuk mengecek konfigurasi wireguard dari interface yang di tambahkan

```
sudo wg
```

![](/img/Screenshot_20241225_104820.png)

berhubung saya sudah melakukan peer, di gambar juga tertera konfigurasi peer nya, sekarang lanjut kan untuk anda melakukan peer

### sisi server

```
sudo wg set wg0 peer v9BaLYuNB8dhzR2DinFegpXEwLsNFRrMu19yrBNufAk= allowed-ips 10.5.10.3/24 endpoint 192.168.0.7:58483
```

pada perintah di atas, ubah bagian

```
sudo wg set wg0 peer (publik key dari client) allowed-ips (alamat ip yang digunakan wg0 dari client) endpoint (ip:port dari ip local dan port yang telah di buka oleh wg tadi)
```

### sisi client

```
sudo wg set wg0 peer dJZ5zXKzlsitJea5gPKqAInfxGWOvp7YwHepLn/bjxI= allowed-ips 10.5.10.2/24 endpoint 192.168.0.5:42607
```

pada perintah di atas, ubah bagian

```
sudo wg set wg0 peer (publik key dari server) allowed-ips (alamat ip yang digunakan wg0 dari server) endpoint (ip:port dari ip local dan port yang telah dibuka oleh wg tadi)
```

# lakukan ping pada ip interface wg0
