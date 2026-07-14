# Panduan cepat sampai konten pertama terbit

## Tahap 1 — Pasang alat di Windows

Buka PowerShell dan jalankan:

```powershell
winget install --id Git.Git -e --source winget
winget install Hugo.Hugo.Extended
winget install Microsoft.VisualStudioCode
```

Tutup lalu buka kembali PowerShell. Pastikan instalasi berhasil:

```powershell
git --version
hugo version
code --version
```

## Tahap 2 — Jalankan website di komputer

1. Ekstrak ZIP proyek ini.
2. Masuk ke folder proyek, misalnya:

```powershell
cd "$HOME\Downloads\ruang-pikiran-hugo"
hugo server -D
```

3. Buka `http://localhost:1313/`.
4. Biarkan PowerShell tetap terbuka selama melihat pratinjau lokal. Tekan `Ctrl+C` untuk menghentikan server.

## Tahap 3 — Ubah identitas situs

Buka folder dengan VS Code:

```powershell
code .
```

Kemudian edit `hugo.toml`:

- `title = "Iyas Mawardi"`
- `author = "Iyas Mawardi"`
- `tagline`
- `intro`

Simpan file. Browser lokal akan diperbarui otomatis.

## Tahap 4 — Buat repository dan unggah ke GitHub

1. Buat repository **publik** baru bernama `ruang-pikiran`.
2. Jangan tambahkan README, `.gitignore`, atau lisensi dari GitHub karena semuanya sudah ada di proyek.
3. Jalankan dari folder proyek:

```powershell
git init
git branch -M main
git add .
git commit -m "Membuat website Hugo pertama"
git remote add origin https://github.com/USERNAME/ruang-pikiran.git
git push -u origin main
```

Ganti `USERNAME` dengan username GitHub Anda. Bila Git meminta identitas, jalankan:

```powershell
git config --global user.name "Nama Anda"
git config --global user.email "email-github-anda@example.com"
```

Lalu ulangi perintah `git commit` dan `git push`.

## Tahap 5 — Aktifkan GitHub Pages

1. Buka repository GitHub.
2. Masuk ke **Settings → Pages**.
3. Pada **Build and deployment → Source**, pilih **GitHub Actions**.
4. Buka tab **Actions** dan pastikan workflow **Build and deploy Hugo site** selesai dengan tanda hijau.
5. Buka kembali **Settings → Pages** untuk melihat alamat website.

Workflow sudah tersedia di `.github/workflows/hugo.yaml`, sehingga setiap commit ke branch `main` akan menerbitkan situs secara otomatis.

## Tahap 6 — Hubungkan Sitepins

1. Masuk ke Sitepins menggunakan GitHub.
2. Pilih **Add New Site** dan repository `ruang-pikiran`.
3. Isi konfigurasi:

```text
Content Folder : content
Media Root     : static/images/uploads
Media Public   : static
Code Folder    : kosong
Site Config    : hugo.toml (opsional)
```

4. Simpan konfigurasi dan izinkan Sitepins membuat folder `.sitepins` bila diminta.
5. Pada folder `artikel`, buat schema dari file contoh `mengapa-saya-membuat-ruang-ini.md`. Ulangi nanti untuk folder `cerita` dan `buletin`.

## Tahap 7 — Terbitkan konten pertama

1. Di Sitepins, buka `artikel/mengapa-saya-membuat-ruang-ini.md`.
2. Ubah judul, ringkasan, tag, dan isi tulisan.
3. Pastikan `draft` bernilai `false`.
4. Klik **Save/Commit**.
5. Sitepins akan menyimpan perubahan sebagai commit GitHub. Commit itu memicu deployment otomatis.
6. Setelah workflow GitHub Actions berwarna hijau, muat ulang website Anda.

Konten pertama sekarang sudah terbit.

## Tahap 8 — Tambahkan domain setelah situs bawaan aktif

Pada **Settings → Pages**, masukkan custom domain. Atur DNS domain utama dengan empat record `A` berikut:

```text
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Buat juga `CNAME` untuk `www` menuju `USERNAME.github.io`. Setelah DNS terverifikasi, aktifkan **Enforce HTTPS**, lalu ubah `baseURL` di `hugo.toml` menjadi alamat domain baru.
