# Panduan Repository Fastpay Terpusat di GitHub dan Penggunaan Multi-Laptop

## 1. Tujuan

Panduan ini menjelaskan cara menggunakan repository Fastpay dari laptop 1, laptop 2, laptop 3, dan perangkat berikutnya dengan GitHub sebagai pusat penyimpanan knowledge.

Panduan mencakup:

- persiapan repository pusat;
- pemasangan Git dan Git LFS;
- pemberian akses pengguna;
- pengambilan project di laptop baru;
- penggunaan bersama Codex;
- sinkronisasi perubahan antar-laptop;
- kerja paralel menggunakan branch;
- pemeriksaan hasil sinkronisasi;
- keamanan data;
- troubleshooting umum.

## 2. Konsep Utama

Gunakan model berikut:

```text
GitHub Private Repository
        |
        +-- Laptop 1: local working copy
        +-- Laptop 2: local working copy
        +-- Laptop 3: local working copy
        +-- github.dev atau Codespaces: optional cloud workspace
```

GitHub menjadi sumber utama atau `source of truth`. Setiap laptop memiliki salinan kerja lokal.

Perubahan dari sebuah laptop baru tersedia di laptop lain setelah perubahan tersebut:

```text
Disimpan
-> git add
-> git commit
-> git push
-> tersedia di GitHub
-> git pull di laptop lain
```

GitHub bukan folder jaringan yang memperbarui semua laptop secara real-time. Sinkronisasi tetap dilakukan melalui `push` dan `pull`.

## 3. Apa yang Tersinkronisasi

Yang dapat tersinkronisasi melalui GitHub:

- file knowledge dan dokumentasi;
- `AGENTS.md` dan `README.md`;
- design-system artifacts;
- spesifikasi produk dan transaksi;
- source code dan test;
- file binary yang didukung Git LFS;
- riwayat commit;
- branch dan tag.

Yang tidak otomatis tersinkronisasi:

- riwayat percakapan atau task Codex;
- perubahan yang belum di-commit dan di-push;
- password, token, dan credential lokal;
- aplikasi yang terpasang di laptop;
- konfigurasi lokal yang masuk `.gitignore`;
- file sementara yang tidak dimasukkan ke repository.

Informasi penting dari percakapan Codex harus dirangkum ke dalam file repository apabila perlu digunakan oleh laptop atau pengguna lain.

## 4. Data yang Harus Diisi Pemilik Repository

Sebelum panduan dibagikan, isi informasi berikut:

```text
Nama repository       : NEED_DISCOVERY
URL repository        : https://github.com/ORGANIZATION/REPOSITORY.git
Pemilik repository    : NEED_DISCOVERY
Branch utama          : main
Status visibility     : NEED_DISCOVERY - wajib dipastikan Private
Penanggung jawab akses: NEED_DISCOVERY
Aturan branch         : NEED_DISCOVERY
Kuota Git LFS         : NEED_DISCOVERY
```

Jangan membagikan repository sebelum statusnya dipastikan `Private` dan aksesnya disetujui oleh pemilik dokumen Fastpay.

## 5. Kebutuhan Laptop

### Wajib

- Windows 10 atau Windows 11;
- koneksi internet;
- ruang kosong minimal 1 GB;
- akun GitHub yang memiliki akses ke repository;
- Git;
- Git LFS;
- browser untuk autentikasi GitHub.

### Opsional

- Codex Desktop untuk pekerjaan berbasis AI;
- Visual Studio Code atau editor lain;
- Microsoft PowerPoint untuk membuka `.pptx`;
- PDF reader untuk membuka `.pdf`;
- GitHub Desktop jika pengguna memilih antarmuka grafis.

Repository saat ini tidak membuktikan adanya kebutuhan instalasi Node.js atau Python untuk sekadar membaca knowledge. Instal runtime tambahan hanya ketika manifest atau kebutuhan implementasi sudah tersedia.

## 6. Persiapan oleh Pemilik Repository

Lakukan sekali pada GitHub:

1. Pastikan repository sudah dibuat.
2. Pastikan visibility adalah `Private`.
3. Pastikan branch utama adalah `main`.
4. Pastikan file repository terlihat sebagai folder dan file, bukan hanya satu ZIP.
5. Pastikan file PowerPoint besar menggunakan Git LFS.
6. Pastikan file ZIP cadangan tidak masuk repository.
7. Tambahkan anggota yang membutuhkan akses.

Jika akun GitHub laptop lain berbeda:

1. Buka repository di GitHub.
2. Buka `Settings`.
3. Buka pengaturan akses atau collaborators.
4. Undang akun GitHub yang diizinkan.
5. Minta pengguna menerima undangan.
6. Jika organisasi memakai SSO, selesaikan otorisasi organisasi.

Berikan akses minimum sesuai kebutuhan. Jangan memberikan akses admin jika pengguna hanya perlu membaca atau mengubah file.

## 7. Instalasi Git di Laptop Baru

Buka PowerShell, kemudian jalankan:

```powershell
winget install --id Git.Git -e
```

Tutup dan buka kembali PowerShell. Periksa instalasi:

```powershell
git --version
```

Jika `winget` tidak tersedia, instal Git for Windows dari sumber resmi yang disetujui organisasi.

## 8. Konfigurasi Identitas Git

Setiap commit menyimpan nama dan email pembuat perubahan.

Untuk konfigurasi seluruh repository di laptop:

```powershell
git config --global user.name "NAMA PENGGUNA"
git config --global user.email "EMAIL PERUSAHAAN"
```

Periksa konfigurasi:

```powershell
git config --global user.name
git config --global user.email
```

Gunakan email perusahaan atau email GitHub `noreply` yang disetujui. Jangan memasukkan password atau token sebagai nilai email.

## 9. Instalasi dan Aktivasi Git LFS

Repository memiliki file PowerPoint besar, sehingga Git LFS diperlukan.

Periksa apakah Git LFS tersedia:

```powershell
git lfs version
```

Jika tersedia:

```powershell
git lfs install
```

Jika belum tersedia:

```powershell
winget install --id GitHub.GitLFS -e
```

Setelah instalasi, buka kembali PowerShell dan jalankan:

```powershell
git lfs install
git lfs version
```

Pengaturan jenis file yang dilacak LFS seharusnya sudah tersimpan di `.gitattributes` pada repository pusat. Laptop baru tidak perlu membuat aturan LFS baru jika aturan tersebut sudah tersedia.

## 10. Mengambil Project di Laptop Baru

Pilih lokasi kerja:

```powershell
cd "$env:USERPROFILE\Documents"
```

Clone repository:

```powershell
git clone https://github.com/ORGANIZATION/REPOSITORY.git
```

Masuk ke folder repository:

```powershell
cd REPOSITORY
```

Ambil seluruh file Git LFS:

```powershell
git lfs pull
```

Ganti `ORGANIZATION/REPOSITORY` dengan URL resmi.

Untuk repository ini, utamakan `git clone` daripada `Download ZIP`. Clone mempertahankan riwayat, remote, branch, dan integrasi Git LFS.

## 11. Autentikasi GitHub

Saat clone atau push pertama, Git dapat membuka browser untuk login.

Gunakan akun GitHub yang sudah diberikan akses. Jangan memasukkan password GitHub biasa ke prompt password Git. Gunakan alur browser atau Git Credential Manager.

Jika muncul `Repository not found`, periksa:

- URL repository;
- akun GitHub yang sedang login;
- status undangan collaborator;
- otorisasi organisasi atau SSO;
- visibility repository.

## 12. Verifikasi Setelah Clone

Jalankan:

```powershell
git status
git remote -v
git branch --show-current
git log -1 --oneline
git lfs ls-files
```

Status yang diharapkan:

```text
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

Pastikan:

- `origin` mengarah ke repository yang benar;
- branch aktif adalah `main`;
- commit terbaru sesuai dengan GitHub;
- file `.pptx` besar muncul pada `git lfs ls-files`;
- `AGENTS.md` dan `README.md` tersedia;
- folder knowledge dapat dibuka;
- file binary tidak hanya berupa pointer LFS berukuran kecil.

Jika file LFS belum terunduh:

```powershell
git lfs pull
```

## 13. Membuka Project di Codex

1. Instal dan login ke Codex Desktop.
2. Pilih `Open Folder`.
3. Pilih folder hasil clone.
4. Pastikan folder yang dibuka adalah root repository.
5. Pastikan `AGENTS.md` berada pada root.
6. Minta Codex melakukan inspeksi sebelum mengubah file.

Prompt awal yang disarankan:

```text
Inspect this repository without changing any files.

1. Confirm the repository root.
2. List the instruction files you loaded.
3. Summarize the Fastpay product context and mandatory payment-safety rules.
4. Review the current folder structure.
5. Identify source conflicts or NEED_DISCOVERY items.
6. Do not install dependencies or scaffold an application.
```

Codex membaca knowledge dari file yang tersedia pada salinan lokal. Jalankan `git pull` sebelum membuka atau melanjutkan pekerjaan agar knowledge lokal tidak tertinggal.

## 14. Workflow Harian Satu Laptop Aktif

### Sebelum bekerja

```powershell
cd "$env:USERPROFILE\Documents\REPOSITORY"
git status
git pull
git lfs pull
```

Pastikan tidak ada perubahan lokal yang belum selesai sebelum melakukan `pull`.

### Setelah melakukan perubahan

Periksa perubahan:

```powershell
git status
git diff
```

Simpan ke Git:

```powershell
git add .
git commit -m "docs: update Fastpay knowledge"
git push
```

Periksa kembali:

```powershell
git status
```

Status akhir harus `working tree clean` dan branch harus sesuai dengan `origin`.

## 15. Berpindah dari Laptop 1 ke Laptop 2

### Di laptop 1 sebelum berpindah

```powershell
git status
git add .
git commit -m "docs: save work before switching laptop"
git push
```

Jangan berpindah laptop jika perubahan penting masih hanya tersimpan lokal.

### Di laptop 2 sebelum melanjutkan

```powershell
git status
git pull
git lfs pull
```

Setelah itu, buka folder tersebut di Codex atau editor.

### Aturan sederhana

```text
Sebelum meninggalkan laptop: commit dan push.
Sebelum memakai laptop lain: pull dan lfs pull.
```

## 16. Workflow Beberapa Pengguna atau Laptop Secara Bersamaan

Jika dua laptop bekerja pada bagian berbeda secara bersamaan, gunakan branch.

Buat branch baru:

```powershell
git switch main
git pull
git switch -c docs/nama-perubahan
```

Lakukan perubahan, kemudian:

```powershell
git add .
git commit -m "docs: describe the change"
git push -u origin docs/nama-perubahan
```

Buat Pull Request di GitHub dan minta review sebelum digabungkan ke `main`.

Contoh branch:

```text
docs/update-operational-profile
docs/ppob-state-mapping
feat/transfer-bank-flow
fix/transfer-pending-recovery
test/transfer-duplicate-protection
```

Jangan mengedit file `.pptx`, `.pdf`, atau binary yang sama secara bersamaan dari dua laptop. Git tidak dapat menggabungkan isi binary secara aman seperti file teks.

## 17. Mengedit Langsung di GitHub

Untuk perubahan ringan pada `.md`, `.json`, atau file teks:

1. Buka repository di browser.
2. Buka file dan pilih edit; atau
3. Tekan tombol `.` untuk membuka `github.dev`.
4. Lakukan perubahan.
5. Commit perubahan ke branch yang sesuai.

Setelah perubahan online dibuat, laptop lokal harus menjalankan:

```powershell
git pull
```

`github.dev` cocok untuk edit teks ringan. Gunakan clone lokal atau Codespaces jika membutuhkan terminal, script, build, atau pemeriksaan yang lebih lengkap.

## 18. GitHub Codespaces sebagai Workspace Cloud

Codespaces dapat digunakan jika tim ingin bekerja melalui browser tanpa menyiapkan seluruh lingkungan lokal.

Alur umum:

1. Buka repository GitHub.
2. Pilih `Code`.
3. Pilih tab `Codespaces`.
4. Buat Codespace pada branch yang sesuai.
5. Lakukan perubahan dan validasi.
6. Commit dan push.
7. Hentikan Codespace setelah selesai untuk mengelola penggunaan kuota.

Codespaces tidak menghilangkan kebutuhan commit dan push. Repository GitHub tetap menjadi sumber utama.

Penggunaan Codespaces, billing, kuota, dan konfigurasi keamanan harus disetujui pemilik repository atau organisasi.

## 19. Menarik Perubahan Terbaru dengan Aman

Sebelum `pull`, jalankan:

```powershell
git status
```

Jika bersih:

```powershell
git pull
git lfs pull
```

Jika ada perubahan lokal, selesaikan dan commit terlebih dahulu:

```powershell
git add .
git commit -m "wip: save local work before sync"
git pull
```

Gunakan commit `wip` hanya untuk pekerjaan sementara yang memang perlu disimpan. Rapikan riwayat sesuai aturan tim sebelum merge ke `main`.

Jangan menggunakan `git reset --hard`, force push, atau menghapus riwayat untuk mengatasi masalah sinkronisasi tanpa persetujuan pemilik repository.

## 20. Menangani Konflik File Teks

Konflik terjadi ketika dua perubahan menyentuh bagian file yang sama.

Langkah aman:

1. Hentikan perubahan tambahan.
2. Jalankan `git status`.
3. Identifikasi file yang konflik.
4. Buka file dan bandingkan kedua perubahan.
5. Pertahankan isi yang benar berdasarkan source of truth.
6. Jangan memilih salah satu sisi secara otomatis untuk aturan finansial, transaksi, keamanan, atau compliance.
7. Minta review owner jika terdapat konflik keputusan.
8. Setelah selesai:

```powershell
git add NAMA_FILE
git commit -m "docs: resolve merge conflict"
git push
```

Jika konflik menyangkut business rule, money state, fee, reversal, refund, settlement, PIN, OTP, KYC, atau production truth, labeli sebagai `NEED_DISCOVERY` dan minta keputusan owner. Jangan menyelesaikannya secara diam-diam.

## 21. Menangani Konflik File Binary

File PowerPoint, PDF, gambar, dan ZIP tidak dapat digabungkan baris demi baris.

Jika terjadi konflik binary:

1. Tentukan siapa pemilik versi final.
2. Bandingkan waktu, sumber, dan status approval.
3. Simpan kedua versi dengan nama berbeda jika kebenarannya belum ditentukan.
4. Jangan menimpa versi approved tanpa persetujuan.
5. Catat keputusan pada commit atau decision log.

Untuk file Office yang sering diedit bersama, pertimbangkan SharePoint atau OneDrive perusahaan sebagai tempat kolaborasi aktif. GitHub dapat menyimpan versi final atau referensi yang sudah disetujui.

## 22. Pemeriksaan Repository Pusat

Gunakan pemeriksaan berikut secara berkala:

```powershell
git status
git remote -v
git fetch origin
git branch -vv
git log --oneline --decorate -10
git lfs ls-files
```

Di halaman GitHub, periksa:

- visibility adalah `Private`;
- branch utama adalah `main`;
- commit terbaru tersedia;
- folder repository tampil normal;
- file ZIP lokal tidak ikut ter-upload;
- file besar tercatat sebagai Git LFS;
- daftar collaborator masih sesuai;
- tidak ada secret atau data sensitif;
- branch protection sesuai kebijakan tim.

## 23. Uji dari Laptop Baru

Pengujian paling kuat adalah clone bersih:

```powershell
cd "$env:USERPROFILE\Documents"
git clone https://github.com/ORGANIZATION/REPOSITORY.git fastpay-clone-test
cd fastpay-clone-test
git lfs pull
git status
git lfs ls-files
```

Validasi:

- clone berhasil tanpa error;
- autentikasi berhasil;
- branch `main` tersedia;
- status bersih;
- knowledge dapat dibaca;
- file LFS dapat dibuka;
- `AGENTS.md` tersedia;
- Codex dapat membuka root repository.

## 24. Troubleshooting

### `fatal: not a git repository`

Terminal sedang berada di folder yang salah.

```powershell
cd "$env:USERPROFILE\Documents\REPOSITORY"
git status
```

### `Repository not found`

Periksa URL dan hak akses:

```powershell
git remote -v
```

Pastikan undangan collaborator sudah diterima dan akun yang aktif benar.

### `remote origin already exists`

Periksa remote yang ada:

```powershell
git remote -v
```

Jika URL salah, ganti secara eksplisit:

```powershell
git remote set-url origin https://github.com/ORGANIZATION/REPOSITORY.git
```

### `Your branch is behind origin/main`

```powershell
git pull
git lfs pull
```

### `Your branch is ahead of origin/main`

Periksa commit lalu push:

```powershell
git log --oneline origin/main..main
git push
```

### `Your branch and origin/main have diverged`

Jangan force push. Jalankan:

```powershell
git status
git log --oneline --graph --decorate --all -20
```

Minta bantuan pemilik repository untuk menentukan integrasi riwayat yang aman.

### Git meminta identitas

```powershell
git config --global user.name "NAMA PENGGUNA"
git config --global user.email "EMAIL PERUSAHAAN"
```

### File PowerPoint hanya berisi pointer LFS

```powershell
git lfs install
git lfs pull
```

### Git LFS gagal karena kuota

Hentikan upload berulang. Hubungi pemilik repository untuk memeriksa kuota storage dan bandwidth Git LFS.

### Push ditolak karena file lebih dari 100 MB

File besar belum dikelola dengan Git LFS atau sudah masuk riwayat Git biasa. Jangan membuat commit tambahan berulang-ulang. Minta bantuan maintainer untuk migrasi file ke Git LFS dan pembersihan riwayat secara terkontrol.

### Push ditolak karena remote memiliki perubahan baru

```powershell
git pull
```

Selesaikan konflik bila muncul, jalankan validasi, kemudian push kembali.

## 25. Aturan Keamanan dan Privasi

Repository Fastpay harus diperlakukan sebagai workspace internal.

Jangan commit atau upload:

- password;
- Personal Access Token;
- private key;
- PIN atau OTP;
- data KYC;
- data pelanggan nyata;
- nomor rekening atau nomor telepon lengkap;
- credential produksi;
- endpoint produksi yang tidak disetujui;
- export database;
- screenshot yang mengandung data sensitif;
- file konfigurasi rahasia;
- ZIP cadangan project.

Aturan wajib:

- gunakan repository `Private`;
- gunakan data mock yang sudah disanitasi;
- tinjau `git status` sebelum commit;
- tinjau `git diff --staged` sebelum push;
- batasi collaborator;
- cabut akses pengguna yang sudah tidak membutuhkan;
- jangan mengirim repository ke layanan eksternal tanpa persetujuan;
- jangan menonaktifkan kontrol keamanan untuk mempercepat pekerjaan.

Sebelum commit:

```powershell
git status
git diff --staged
```

## 26. Aturan untuk File ZIP

File `fastpay-ux-ui-engineering-local.zip` adalah cadangan atau media pemindahan lokal. File tersebut tidak boleh menjadi isi utama repository dan tidak perlu di-commit.

Repository GitHub harus menampilkan struktur file yang dapat dibaca, bukan hanya satu arsip ZIP.

Jika file ZIP sudah muncul sebagai untracked file:

```powershell
git status
```

Jangan menjalankan `git add .` sebelum memastikan ZIP tersebut dikecualikan melalui `.gitignore` atau dipindahkan ke lokasi cadangan yang disetujui.

## 27. Checklist Laptop Baru

```text
[ ] Akun GitHub memiliki akses
[ ] Undangan repository sudah diterima
[ ] Repository dipastikan Private
[ ] Git terpasang
[ ] Git LFS terpasang
[ ] Nama dan email Git dikonfigurasi
[ ] Repository berhasil di-clone
[ ] git lfs pull berhasil
[ ] Branch aktif adalah main
[ ] Remote origin benar
[ ] Working tree clean
[ ] AGENTS.md tersedia
[ ] README.md tersedia
[ ] File knowledge dapat dibaca
[ ] File PowerPoint besar dapat dibuka
[ ] Codex membuka root repository yang benar
[ ] Pengguna memahami pull sebelum bekerja
[ ] Pengguna memahami commit dan push setelah bekerja
[ ] Pengguna memahami larangan data sensitif
```

## 28. Checklist Sebelum Berpindah Laptop

```text
[ ] git status sudah diperiksa
[ ] perubahan sudah direview
[ ] tidak ada data sensitif
[ ] perubahan sudah di-commit
[ ] perubahan sudah di-push
[ ] GitHub menampilkan commit terbaru
[ ] laptop berikutnya menjalankan git pull
[ ] laptop berikutnya menjalankan git lfs pull
```

## 29. Ringkasan Perintah

### Setup pertama

```powershell
git --version
git lfs install
git clone https://github.com/ORGANIZATION/REPOSITORY.git
cd REPOSITORY
git lfs pull
git status
```

### Mulai bekerja

```powershell
git status
git pull
git lfs pull
```

### Selesai bekerja

```powershell
git status
git diff
git add .
git diff --staged
git commit -m "docs: describe the change"
git push
git status
```

### Periksa repository

```powershell
git remote -v
git branch --show-current
git log -1 --oneline
git lfs ls-files
```

## 30. Rekomendasi Operasional

Gunakan GitHub Private sebagai pusat knowledge Fastpay dengan aturan berikut:

1. `main` berisi versi yang sudah direview.
2. Perubahan signifikan dibuat melalui branch dan Pull Request.
3. Setiap laptop melakukan `pull` sebelum bekerja.
4. Setiap laptop melakukan `commit` dan `push` setelah selesai.
5. File besar disimpan melalui Git LFS.
6. File Office yang aktif diedit bersama dapat ditempatkan di SharePoint atau OneDrive perusahaan, lalu direferensikan dari repository.
7. Keputusan penting dari task Codex dirangkum ke dokumen repository.
8. Akses, branch protection, backup, dan kuota LFS ditinjau berkala.
9. Tidak ada production secret atau data pelanggan di repository.
10. Konflik aturan finansial, transaksi, security, atau compliance harus diputuskan oleh owner yang berwenang.

## 31. NEED_DISCOVERY Sebelum Panduan Dinyatakan Final

- URL repository resmi;
- konfirmasi repository sudah `Private`;
- organization atau pemilik repository;
- daftar collaborator yang disetujui;
- branch protection untuk `main`;
- aturan Pull Request dan reviewer;
- kuota dan billing Git LFS;
- lokasi resmi untuk kolaborasi file Office;
- aturan backup repository;
- prosedur pencabutan akses;
- dukungan Codespaces dan kebijakan biayanya.

Panduan ini dapat digunakan sebagai panduan operasional awal. Isi `NEED_DISCOVERY` harus dikonfirmasi oleh pemilik repository dan pihak yang berwenang sebelum dijadikan kebijakan final organisasi.
