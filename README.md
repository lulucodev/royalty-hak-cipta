# RoyaltyChain – Sistem Manajemen Royalti Hak Cipta Musik

RoyaltyChain adalah prototipe aplikasi berbasis blockchain untuk mencatat data lagu sebagai NFT dan mengelola penyetoran serta pembagian royalti musik melalui smart contract. Project ini dibuat untuk pembelajaran dan demonstrasi menggunakan blockchain lokal Hardhat.

> **Status project:** distribusi royalti pada versi saat ini masih dilakukan secara manual melalui tombol **Distribusikan** di frontend. Otomatisasi distribusi bulanan belum diimplementasikan.

## Fitur

- **Registrasi lagu:** menyimpan ID lagu, judul, nama pencipta, dan alamat wallet pencipta.
- **Tokenisasi NFT:** setiap lagu yang berhasil didaftarkan akan dibuatkan NFT dengan standar ERC-721.
- **Pengaturan pihak terkait:** menetapkan nama dan alamat wallet artis, label, serta platform.
- **Aktivasi streaming:** mengubah status lagu menjadi streaming aktif.
- **Penyetoran royalti:** wallet yang ditetapkan sebagai platform dapat menyetorkan ETH ke saldo royalti lagu.
- **Distribusi royalti:** wallet yang ditetapkan sebagai label dapat menjalankan pembagian saldo royalti.
- **Dashboard dan riwayat:** menampilkan daftar lagu, status, saldo royalti, pihak terkait, hasil distribusi, dan riwayat aktivitas.

## Teknologi yang digunakan

- **Solidity** – bahasa pemrograman smart contract.
- **Hardhat** – pengembangan, kompilasi, deployment, dan blockchain lokal.
- **OpenZeppelin ERC-721** – implementasi standar NFT.
- **Ethers.js v6** – menghubungkan frontend dengan wallet dan smart contract.
- **MetaMask** – wallet untuk menandatangani transaksi.
- **HTML, CSS, JavaScript** – membangun antarmuka aplikasi.
- **Live Server** – menjalankan frontend secara lokal.

## Struktur folder

```text
royalty-hak-cipta/
├── contracts/
│   └── RoyaltyHakCipta.sol
├── scripts/
│   └── deploy.js
├── frontend/
│   ├── index.html
│   ├── style.css
│   ├── app.js
│   └── contract.js
├── hardhat.config.js
├── package.json
├── package-lock.json
└── README.md
```

Nama atau susunan file dapat berbeda apabila struktur repository telah diubah.

## Persyaratan

Sebelum menjalankan project, siapkan:

1. Node.js dan npm.
2. Git.
3. MetaMask pada browser.
4. Visual Studio Code.
5. Ekstensi **Live Server** di Visual Studio Code.

## Instalasi

### 1. Clone repository

```bash
git clone URL_REPOSITORY
cd royalty-hak-cipta
```

Ganti `URL_REPOSITORY` dengan URL repository GitHub project ini.

### 2. Install dependency

```bash
npm install
```

### 3. Jalankan blockchain lokal

Buka terminal pertama pada folder utama project, lalu jalankan:

```bash
npx hardhat node
```

Hardhat akan menjalankan node lokal pada alamat RPC `http://127.0.0.1:8545` dan menampilkan beberapa akun pengujian beserta private key dan saldo ETH.

**Biarkan terminal ini tetap berjalan** selama aplikasi digunakan.

### 4. Compile smart contract

Buka terminal kedua pada folder utama project, kemudian jalankan:

```bash
npx hardhat compile
```

Perintah ini mengompilasi `contracts/RoyaltyHakCipta.sol`. Jika berhasil, Hardhat akan membuat artifact kompilasi di folder `artifacts/`.

### 5. Deploy smart contract

Masih dari terminal kedua, jalankan:

```bash
npx hardhat run scripts/deploy.js --network localhost
```

Setelah deployment berhasil, salin alamat kontrak yang ditampilkan di terminal.

### 6. Periksa konfigurasi frontend

Buka `frontend/contract.js` dan pastikan:

- `contractAddress` berisi alamat hasil deployment terbaru.
- `contractABI` berisi ABI dari hasil kompilasi smart contract yang digunakan.

ABI dapat diambil dari file:

```text
artifacts/contracts/RoyaltyHakCipta.sol/RoyaltyHakCipta.json
```

Salin bagian `abi` dari artifact tersebut ke konfigurasi `contract.js` sesuai format file yang sudah ada.

### 7. Atur MetaMask

Tambahkan jaringan lokal Hardhat di MetaMask dengan konfigurasi berikut:

| Pengaturan | Nilai |
|---|---|
| Network name | Hardhat Local |
| RPC URL | `http://127.0.0.1:8545` |
| Chain ID | `31337` |
| Currency symbol | `ETH` |

Import salah satu akun pengujian menggunakan private key yang ditampilkan oleh terminal `npx hardhat node`. Pastikan jaringan Hardhat Local dipilih ketika aplikasi digunakan.

### 8. Jalankan frontend

Di Visual Studio Code:

1. Buka folder `frontend`.
2. Klik kanan `index.html`.
3. Pilih **Open with Live Server**.
4. Browser akan membuka aplikasi RoyaltyChain.
5. Klik **Hubungkan Wallet** dan setujui koneksi MetaMask.

Pastikan node Hardhat masih berjalan dan alamat kontrak di `contract.js` sesuai dengan deployment yang aktif.

## Alur penggunaan aplikasi

1. Hubungkan MetaMask ke jaringan Hardhat Local.
2. Isi form registrasi lagu dengan ID lagu, judul lagu, nama pencipta, dan alamat wallet pencipta.
3. Klik **Daftarkan Lagu** dan konfirmasi transaksi di MetaMask.
4. Pilih lagu pada bagian **Manajemen Royalti**.
5. Tetapkan artis, label, dan platform menggunakan nama serta alamat wallet masing-masing.
6. Aktifkan streaming.
7. Beralih ke akun wallet yang ditetapkan sebagai **Platform**, lalu setorkan sejumlah ETH sebagai royalti.
8. Beralih ke akun wallet yang ditetapkan sebagai **Label**, lalu klik **Distribusikan**.
9. Periksa saldo, hasil pembagian, status, dan riwayat aktivitas.

## Aturan pembagian royalti

Smart contract membagi saldo royalti dengan komposisi berikut:

| Penerima | Persentase |
|---|---:|
| Pencipta | 50% |
| Artis | 30% |
| Label | 20% |
| **Total** | **100%** |

Contoh: jika saldo yang didistribusikan sebesar `1 ETH`, pencipta menerima `0.5 ETH`, artis menerima `0.3 ETH`, dan label menerima `0.2 ETH`.

## Status lagu

Status yang digunakan smart contract adalah:

| Nilai enum | Status di aplikasi | Keterangan |
|---:|---|---|
| 0 | Terdaftar | Lagu berhasil didaftarkan. |
| 1 | Streaming aktif | Streaming lagu telah diaktifkan. |
| 2 | Royalti terkumpul | Royalti telah disetorkan ke kontrak. |
| 3 | Terdistribusi | Saldo royalti telah dibagikan. |

## Catatan penting dan batasan versi saat ini

- Project ini menggunakan **blockchain lokal Hardhat**, bukan jaringan blockchain publik.
- ETH yang digunakan merupakan ETH pengujian pada jaringan lokal, bukan aset sungguhan.
- Setiap kali proses `npx hardhat node` dihentikan dan dijalankan ulang, keadaan blockchain lokal dapat kembali ke awal. Kontrak perlu di-deploy ulang dan alamat di `frontend/contract.js` perlu diperbarui.
- NFT saat registrasi dicetak kepada wallet yang memanggil fungsi registrasi (`msg.sender`). Alamat pencipta yang dimasukkan disimpan sebagai data pihak pencipta, tetapi bukan otomatis penerima NFT.
- Hanya wallet Platform yang ditetapkan yang dapat memanggil fungsi penyetoran royalti.
- Hanya wallet Label yang ditetapkan yang dapat memanggil fungsi distribusi royalti.
- Distribusi masih dipicu secara manual dari frontend. Penjadwalan bulanan, backend scheduler, dan otomasi distribusi belum tersedia pada versi ini.
- Data riwayat aktivitas disimpan melalui struktur `History` pada smart contract. Struktur tersebut bukan Solidity event (`emit`).
- Project ini merupakan prototipe pembelajaran dan belum diaudit untuk penggunaan produksi. Pengaturan hak akses pada fungsi penetapan pihak dan aktivasi streaming masih perlu diperketat sebelum digunakan pada jaringan publik.

## Keamanan

- **Jangan pernah mengunggah seed phrase atau private key pribadi ke GitHub.**
- Private key bawaan Hardhat hanya untuk akun pengujian lokal dan tidak boleh digunakan untuk menyimpan aset sungguhan.
- Jangan memasukkan private key ke dalam file frontend, source code, atau repository.
- Pastikan file `.env`, `node_modules/`, `artifacts/`, dan `cache/` tidak ikut di-commit jika tidak diperlukan.

## Kontribusi

Untuk kerja kelompok, anggota dapat melakukan fork atau clone repository, membuat branch untuk perubahan masing-masing, lalu mengajukan Pull Request. Sebelum menguji fitur blockchain, setiap anggota perlu menjalankan node Hardhat lokal dan melakukan deployment pada komputernya sendiri.

## Lisensi

Lisensi belum ditentukan. Tambahkan file `LICENSE` apabila project akan didistribusikan dengan ketentuan lisensi tertentu.