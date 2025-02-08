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


# 🔹 Setup Workflow Permissions (Read and Write)

## 1️⃣ Mengatur Workflow Permissions di Repository
1. Buka **GitHub Repository** yang ingin kamu atur.
2. Pergi ke **Settings** > **Actions** > **General**.
3. Scroll ke bagian **Workflow permissions**.
4. Pilih **Read and write permissions**.
5. Jika perlu, aktifkan **Allow GitHub Actions to create and approve pull requests**.
6. Klik **Save**.

---

## 2️⃣ Mengatur Permissions dalam Workflow YAML
Tambahkan bagian `permissions` di dalam file workflow (`.github/workflows/your-workflow.yml`):

```yaml
name: Example Workflow

on:
  push:
    branches:
      - main

jobs:
  example-job:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      issues: write
      pull-requests: write  # Opsional, jika ingin mengelola PR

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Run a Script
        run: echo "Workflow running with write permissions!"
```

---

## 🎉 Workflow Sekarang Punya **Read & Write Permissions**!
Setelah langkah-langkah di atas, GitHub Actions dalam repository ini bisa **membaca dan menulis** ke repository, termasuk mengubah konten, membuat branch, atau mengelola issue dan PR.

Jika ada kendala, cek kembali langkah-langkah di atas atau buka GitHub Docs. 🚀
