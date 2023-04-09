# Tutorial Taiko Node Airdrop Finder

<p style="font-size:14px" align="right">
<a href="https://t.me/airdropfind" target="_blank">Join Telegram Airdrop Finder<img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
</p>

<p align="center">
  <img height="auto" width="auto" src="https://raw.githubusercontent.com/bayy420-999/airdropfind/main/NavIcon.png">
</p>

## Referensi

* [Website](https://taiko.xyz/)
* [Discord](https://discord.gg/taikoxyz)
* [Dokumen resmi](https://taiko.xyz/docs/guides/run-a-node#view-the-live-data-streams-of-your-running-containers)
* [Daftar Alchemy (RPC)](https://auth.alchemy.com/signup?redirectUrl=https%3A%2F%2Fdashboard.alchemy.com%2Fsignup%2F%3Freferrer_origin%3DDIRECT)

## Spesifikasi Software & Hardware

### Persyaratan Hardware

| Komponen | Spesifikasi minimal |
|----------|---------------------|
|CPU|16 Cores|
|RAM|32 GB DDR4 RAM|
|Penyimpanan|250 GB SSD|
|Koneksi|10Mbit/s port|

| Komponen | Spesifikasi rekomendasi |
|----------|---------------------|
|CPU|32 Cores|
|RAM|64 GB DDR4 RAM|
|Penyimpanan|2 x 1 TB NVMe SSD|
|Koneksi|1 Gbit/s port|

### Persyaratan Software/OS

| Komponen | Spesifikasi minimal |
|----------|---------------------|
|Sistem Operasi|Ubuntu 16.04|

| Komponen | Spesifikasi rekomendasi |
|----------|---------------------|
|Sistem Operasi|Ubuntu 22.04|

## Setup

**Disclaimer: Buat run Node Taiko butuh VPS dengan spesifikasi minimal 8VCPU/32GB RAM, Kalo VPS kalian kentang mending gausah garap karena paling gak dapet block...** 

**Reward hanya diberikan kepada Node yang pertama kali approve block (fcfs)**

### Install dependensi Linux

```console
apt-get update && apt-get upgrade
apt-get install git
```

### Install Docker

1. Update `apt-get` dan install paket yang diperlukan
   ```console
   sudo apt-get update
   sudo apt-get install \
     ca-certificates \
     curl \
     gnupg \
     lsb-release
   ```
2. Tambahkan kunci GPG docker
   ```console
   sudo mkdir -p /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   ```
3. Setup repositori
   ```console
   echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```
4. Install docker
   ```console
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
   ```
5. Cek apakah docker sudah terinstall dengan benar
   ```console
   sudo docker run hello-world
   ```

### Buat Sepolia RPC

1. Daftar [Alchemy](https://auth.alchemy.com/signup?redirectUrl=https%3A%2F%2Fdashboard.alchemy.com%2Fsignup%2F%3Freferrer_origin%3DDIRECT)
2. Pilih `+CREATE APP`
3. Isi nama aplikasi (bebas)
4. Di bagian Network pilih Sepolia
5. Pencet `CREATE APP`
6. Cari App yang kalian buat tadi terus pilih `VIEW DETAILS`
7. Pilih `VIEW KEY`
8. Salin HTTP & WSS

### Install & jalankan Node

1. Download Node
   ```console
   git clone https://github.com/taikoxyz/simple-taiko-node.git
   cd simple-taiko-node
   ```
2. Salin .env
   ```console
   cp .env.sample .env
   ```
3. Edit .env
   ```console
   nano .env
   ```
   Ganti nilai `L1_ENDPOINT_HTTP` dan `L1_ENDPOINT_WS` dengan RPC dari alchemy

   Ganti nilai `ENABLE_PROVER` menjadi `true`
   
   Ganti nilai `L1_PROVER_PRIVATE_KEY` dengan private key kalian (saran make new wallet)
   
   Save file dengan CTRL + x + Y + enter
4. Update Node
   ```console
   docker compose pull
   ```
5. Jalankan Node
   ```console
   docker compose up -d
   ```
6. Cek log
   ```console
   docker compose logs -f 
   ```
7. Cek block yang kalian approve
   ```console
   docker compose logs -f | grep "💰 Your block proof was accepted"
   ```

   > Kalo setelah 2-3 hari masih belum ada block yang nyangkut mending matiin aja Nodenya, artinya VPS kalian gk kuat

## Perintah berguna
### Cek semua logs
```console
docker compose logs -f
```
### Cek prover logs
```console
docker compose logs -f taiko_client_prover_relayer
```
### Cek l2 execution engine logs
```console
docker compose logs -f l2_execution_engine
```
### Cek block yang kalian approve
```console
docker compose logs -f | grep "💰 Your block proof was accepted"
```
### Start Node
```console
docker compose up -d
```
### Restart Node
```console
docker compose restart
```
### Stop Node
```console
docker compose down
```
### Hapus Node
```console
docker compose down -v
cd ~
rm -rf simple-taiko-node
```

## Troubleshoot
Reserved
