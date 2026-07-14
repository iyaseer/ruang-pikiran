# Iyas Mawardi — Hugo + GitHub Pages + Sitepins

Starter website personal minimalis berbahasa Indonesia untuk artikel, cerita, dan pojok buletin. Proyek ini tidak memakai tema eksternal, Node.js, atau database.

## Struktur penting

- `content/artikel` — artikel panjang dan opini
- `content/cerita` — cerita atau catatan perjalanan
- `content/buletin` — buletin berkala
- `static/images/uploads` — gambar yang diunggah melalui Sitepins
- `hugo.toml` — nama situs, menu, deskripsi, dan pengaturan utama
- `.github/workflows/hugo.yaml` — publikasi otomatis ke GitHub Pages

## Instalasi alat di Windows

Buka PowerShell, kemudian jalankan:

```powershell
winget install --id Git.Git -e --source winget
winget install Hugo.Hugo.Extended
winget install Microsoft.VisualStudioCode
```

Tutup dan buka kembali PowerShell, lalu periksa:

```powershell
git --version
hugo version
code --version
```

## Menjalankan website secara lokal

Masuk ke folder proyek melalui PowerShell:

```powershell
cd C:\lokasi\folder\ruang-pikiran-hugo
hugo server -D
```

Buka alamat yang muncul, biasanya `http://localhost:1313/`.

## Mengubah identitas website

Edit `hugo.toml`:

- `title` — nama website
- `params.author` — nama penulis
- `params.tagline` — kalimat utama di beranda
- `params.intro` — deskripsi pendek

Nilai `baseURL` belum perlu diubah ketika GitHub Actions digunakan. Workflow akan memakai alamat GitHub Pages secara otomatis.

## Mengunggah ke GitHub

1. Buat repository baru di GitHub, misalnya `ruang-pikiran`.
2. Jangan centang pembuatan README karena proyek ini sudah memilikinya.
3. Di PowerShell pada folder proyek, jalankan:

```powershell
git init
git branch -M main
git add .
git commit -m "Membuat website Hugo pertama"
git remote add origin https://github.com/USERNAME/ruang-pikiran.git
git push -u origin main
```

Ganti `USERNAME` dengan username GitHub Anda.

4. Di repository GitHub, buka **Settings → Pages**.
5. Pada **Build and deployment → Source**, pilih **GitHub Actions**.
6. Buka tab **Actions** dan tunggu proses berwarna hijau.
7. Alamat website akan muncul pada hasil deployment dan halaman Settings → Pages.

## Menghubungkan Sitepins

Buat akun di Sitepins dan hubungkan repository GitHub tersebut.

Gunakan pengaturan berikut:

- Content Folder: `content`
- Media Root: `static/images/uploads`
- Media Public: `static`
- Code Folder: kosongkan
- Site Config: pilih `hugo.toml` bila ingin mengedit judul/deskripsi dari CMS
- Custom Commit Message: nonaktif agar proses menyimpan lebih sederhana

Setelah Sitepins selesai memindai konten, buka folder `artikel` dan pilih file `mengapa-saya-membuat-ruang-ini.md`. Ubah isinya, simpan, lalu Sitepins akan membuat commit ke GitHub. Commit tersebut otomatis memicu GitHub Actions dan memperbarui website.

## Membuat konten baru di Sitepins

Pada masing-masing folder (`artikel`, `cerita`, atau `buletin`), buat schema dari file contoh yang sudah tersedia. Sitepins akan membaca front matter dan membuat formulir untuk:

- title
- date
- description
- author
- tags
- kategori
- cover
- featured
- draft

Untuk memublikasikan, pastikan `draft` bernilai `false`.

## Memasang gambar

Unggah gambar melalui Media Library Sitepins. Gambar akan masuk ke `static/images/uploads`.

Pada kolom `cover`, isi path relatif seperti:

```text
images/uploads/nama-gambar.jpg
```

Di isi artikel, gunakan shortcode berikut agar gambar tetap benar pada alamat GitHub Pages maupun custom domain:

```markdown
{{< image src="images/uploads/nama-gambar.jpg" alt="Teks alternatif" caption="Keterangan gambar" >}}
```

## Custom domain

Lakukan tahap ini setelah alamat bawaan GitHub Pages sudah berfungsi.

1. Beli domain dari registrar pilihan Anda.
2. Masukkan domain pada **GitHub repository → Settings → Pages → Custom domain** terlebih dahulu.
3. Untuk domain utama tanpa `www` (contoh `ruangiyas.my.id`), buat empat record DNS berikut:

| Type | Name/Host | Value |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |

4. Untuk subdomain `www`, buat record:

| Type | Name/Host | Value |
|---|---|---|
| CNAME | `www` | `USERNAME.github.io` |

Gunakan username GitHub, tanpa nama repository. Hapus record `A`, `AAAA`, atau `CNAME` lain yang bertabrakan pada host yang sama.

5. Setelah pemeriksaan DNS GitHub berhasil, aktifkan **Enforce HTTPS**.
6. Perbarui `baseURL` di `hugo.toml`, misalnya:

```toml
baseURL = "https://ruangiyas.my.id/"
```

7. Commit perubahan tersebut. Karena proyek memakai GitHub Actions, file `CNAME` tidak diperlukan.

Perubahan DNS tidak selalu langsung terlihat; pemeriksaan domain dan sertifikat HTTPS dapat memerlukan waktu hingga sekitar 24 jam.

## Membuat tulisan lewat terminal (opsional)

```powershell
hugo new content artikel/judul-artikel.md
hugo new content cerita/judul-cerita.md
hugo new content buletin/edisi-002.md
```

Buka file yang dibuat dan ubah `draft: true` menjadi `draft: false` ketika siap diterbitkan.
