# 🚀 Setup Git & Push ke Repository dengan SSH

mengatur SSH key agar bisa push ke repo GitHub tanpa perlu setup ulang setiap kali.

## 🔹 1. Buat SSH Key (Jika Belum Ada)
Cek apakah sudah ada SSH key:

```bash
ls -la ~/.ssh
```

Jika belum ada `id_rsa.pub`, buat SSH key baru:

```bash
ssh-keygen -t rsa -b 4096 -C "user.email@gmail.com"
```

Tekan Enter untuk lokasi default, dan kosongkan passphrase.

## 🔹 2. Tambahkan SSH Key ke SSH Agent
Jalankan perintah berikut agar key bisa digunakan:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Agar otomatis aktif saat login, tambahkan ke `.bashrc` atau `.zshrc`:

```bash
echo 'eval "$(ssh-agent -s)"' >> ~/.bashrc
echo 'ssh-add ~/.ssh/id_rsa' >> ~/.bashrc
source ~/.bashrc
```

## 🔹 3. Tambahkan SSH Key ke GitHub

### Dapatkan SSH Key Publik

```bash
cat ~/.ssh/id_rsa.pub
```

1. Copy hasilnya.
2. Buka [GitHub SSH Keys](https://github.com/settings/keys).
3. Klik **New SSH Key**, beri nama (misal: "Laptop"), lalu paste.
4. Klik **Add SSH Key**.

## 🔹 4. Konfigurasi Git Global
Set identitas Git agar commit sesuai dengan akun GitHub kamu:

```bash
git config --global user.name "user.name"
git config --global user.email "user.email"
```

Cek apakah sudah benar:

```bash
git config --global --list
```

## 🔹 5. Clone Repository dengan SSH
Jika belum clone repo:

```bash
git clone git@github.com:user.name/repo.git
cd repo
```

Jika sudah clone dengan HTTPS, ubah ke SSH:

```bash
git remote set-url origin git@github.com:user.name/repo.git
```

## 🔹 6. Tes Koneksi ke GitHub
Pastikan SSH key sudah tersambung dengan GitHub:

```bash
ssh -T git@github.com
```

Jika berhasil, akan muncul:

```rust
Hi user.name! You've successfully authenticated...
```

## 🔹 7. Push ke Repository
Masuk ke folder repo:

```bash
cd repo
```

Lalu jalankan:

```bash
git add .
git commit -m "First commit"
git push origin main
```

Jika branch `main` belum ada:

```bash
git branch -M main
git push -u origin main
```

## 🎉 Sekarang Kamu Bisa Push Tanpa Setup Ulang!

Setiap kali ingin push ke repo:

```bash
git add .
git commit -m "Pesan commit"
git push origin main
```

Jika ada kendala, cek kembali langkah-langkah di atas atau buka GitHub Docs. 🚀
