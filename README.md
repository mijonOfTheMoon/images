# 🧑‍💻 Modul Praktik: Git, GitHub, GitHub Actions & GitHub Registry

> **Mentor:** Sesi Pembelajaran Developer  
> **Tingkat:** Pemula hingga Menengah  
> **Prasyarat:** Sudah menginstall Git, memiliki akun GitHub, dan Node.js (opsional)

---

## 📋 Daftar Isi

1. [Setup Awal](#1-setup-awal)
2. [Git — Version Control Lokal](#2-git--version-control-lokal)
3. [GitHub — Remote Repository](#3-github--remote-repository)
4. [GitHub Actions — CI/CD Otomatis](#4-github-actions--cicd-otomatis)
5. [GitHub Container Registry (GHCR)](#5-github-container-registry-ghcr)
6. [Alur Kerja Lengkap (End-to-End)](#6-alur-kerja-lengkap-end-to-end)

---

## 1. Setup Awal

### 1.1 Konfigurasi Identitas Git

Sebelum mulai, konfigurasi nama dan email kamu di Git. Ini akan muncul di setiap commit yang kamu buat.

```bash
git config --global user.name "Nama Kamu"
git config --global user.email "email@kamu.com"

# Cek konfigurasi yang sudah tersimpan
git config --list
```

### 1.2 Fork Playground Repository

> 💡 **Apa itu Fork?**  
> Fork adalah fitur milik **platform hosting** (GitHub, GitLab, Bitbucket, dll.) — **bukan** fitur bawaan Git. Git sendiri tidak mengenal perintah `git fork`.  
>
> Secara teknis, fork bekerja seperti `clone` yang dilakukan di sisi server, dengan tambahan:
> - Menyimpan relasi antara repo hasil fork dan repo aslinya
> - Memudahkan sinkronisasi perubahan dari repo asal (upstream)
> - Memfasilitasi alur Pull Request kembali ke repo aslinya
>
> Fork sangat umum digunakan dalam kontribusi open-source: kamu bebas bereksperimen di fork milikmu tanpa mempengaruhi repository asli.

**Cara Fork Repository:**

1. Buka repository playground yang sudah disediakan mentor
2. Klik tombol **Fork** di pojok kanan atas halaman GitHub
3. Pilih akun/organisasi tujuan, lalu klik **Create fork**
4. Sekarang kamu punya salinan repo tersebut di akunmu sendiri

### 1.3 Clone Repository Hasil Fork

Setelah fork, salin repository ke komputer lokal kamu.

```bash
# Clone via HTTPS
git clone https://github.com/USERNAME-KAMU/nama-repo.git

# Clone via SSH (disarankan)
git clone git@github.com:USERNAME-KAMU/nama-repo.git

# Masuk ke folder project
cd nama-repo
```

### 1.4 Tambahkan Remote Upstream

Hubungkan repo lokal kamu ke repo asli (sumber fork), agar bisa menerima update terbaru.

```bash
# Tambahkan remote ke repo asli (upstream)
git remote add upstream https://github.com/MENTOR/nama-repo.git

# Cek semua remote yang terdaftar
git remote -v
# Output:
# origin    https://github.com/USERNAME-KAMU/nama-repo.git (fetch)
# origin    https://github.com/USERNAME-KAMU/nama-repo.git (push)
# upstream  https://github.com/MENTOR/nama-repo.git (fetch)
# upstream  https://github.com/MENTOR/nama-repo.git (push)
```

---

## 2. Git — Version Control Lokal

### 2.1 Inisialisasi & Status

```bash
# Inisialisasi repo Git baru di folder yang ada
git init

# Cek status file (perubahan apa yang terjadi)
git status

# Cek riwayat commit
git log

# Versi ringkas dari log
git log --oneline --graph --all
```

### 2.2 Staging & Commit

> 💡 Git menggunakan dua tahap sebelum perubahan tersimpan: **staging** (memilih file) lalu **commit** (menyimpan snapshot).

```bash
# Tambahkan satu file ke staging area
git add nama-file.txt

# Tambahkan semua perubahan ke staging area
git add .

# Lihat apa yang sudah di-stage sebelum commit
git diff --staged

# Commit dengan pesan
git commit -m "feat: tambah fitur login"

# Commit dengan tambah semua file yang sudah terlacak sekaligus
git commit -am "fix: perbaiki bug validasi form"
```

> 📌 **Konvensi Pesan Commit (Conventional Commits):**
> - `feat:` — fitur baru
> - `fix:` — perbaikan bug
> - `docs:` — perubahan dokumentasi
> - `chore:` — tugas maintenance
> - `refactor:` — refaktor kode tanpa mengubah fungsionalitas

### 2.3 Branching

> 💡 **Branch** memungkinkan kamu bekerja di fitur/perbaikan yang terisolasi tanpa mengganggu kode utama (`main`).

```bash
# Lihat semua branch
git branch

# Lihat semua branch termasuk remote
git branch -a

# Buat branch baru
git branch nama-fitur

# Pindah ke branch yang ada
git checkout nama-fitur

# Buat sekaligus pindah ke branch baru (shortcut)
git checkout -b nama-fitur

# Cara modern (Git 2.23+)
git switch -c nama-fitur

# Hapus branch yang sudah tidak dipakai
git branch -d nama-fitur
```

### 2.4 Merging & Rebasing

```bash
# Merge branch ke branch aktif saat ini
git checkout main
git merge nama-fitur

# Merge dengan membuat merge commit (lebih rapi di history)
git merge --no-ff nama-fitur

# Rebase — terapkan commit branch di atas branch tujuan (history lebih linear)
git checkout nama-fitur
git rebase main
```

> ⚠️ **Merge vs Rebase:**  
> - `merge` — aman, menjaga history asli, ada merge commit  
> - `rebase` — history lebih bersih/linear, **jangan digunakan di branch publik**

### 2.5 Stash — Simpan Sementara

```bash
# Simpan perubahan yang belum siap di-commit
git stash

# Simpan dengan nama deskriptif
git stash save "WIP: sedang refactor halaman login"

# Lihat daftar stash
git stash list

# Ambil kembali stash terbaru
git stash pop

# Ambil stash tertentu (tanpa menghapusnya)
git stash apply stash@{1}

# Hapus semua stash
git stash clear
```

### 2.6 Undo & Reset

```bash
# Batalkan perubahan di working directory (belum di-stage)
git restore nama-file.txt

# Keluarkan file dari staging (batalkan git add)
git restore --staged nama-file.txt

# Ubah pesan commit terakhir
git commit --amend -m "pesan baru"

# Reset ke commit tertentu — keep perubahan di working directory
git reset --soft HEAD~1

# Reset ke commit tertentu — keep perubahan tidak ter-stage
git reset --mixed HEAD~1

# Reset KERAS — buang semua perubahan (hati-hati!)
git reset --hard HEAD~1

# Buat commit baru yang membatalkan commit tertentu (aman untuk repo publik)
git revert abc1234
```

### 2.7 Tagging

```bash
# Buat tag ringan
git tag v1.0.0

# Buat tag beranotasi (disarankan untuk release)
git tag -a v1.0.0 -m "Release versi 1.0.0"

# Lihat semua tag
git tag

# Push tag ke remote
git push origin v1.0.0

# Push semua tag sekaligus
git push origin --tags
```

---

## 3. GitHub — Remote Repository

### 3.1 Push & Pull

```bash
# Push branch ke remote (pertama kali)
git push -u origin nama-branch

# Push setelah pertama kali
git push

# Pull perubahan terbaru dari remote
git pull

# Pull dari upstream (repo sumber fork)
git pull upstream main

# Fetch — ambil info terbaru dari remote tanpa merge
git fetch origin
git fetch --all
```

### 3.2 Pull Request (PR)

> 💡 **Pull Request** adalah cara mengusulkan perubahan kode ke branch/repo lain di GitHub. PR memungkinkan review, diskusi, dan approval sebelum kode digabung.

**Cara Membuat Pull Request:**

1. Push branch kamu ke fork: `git push origin nama-fitur`
2. Buka repository di GitHub
3. Klik **Compare & pull request** (muncul otomatis) atau masuk ke tab **Pull requests** → **New pull request**
4. Pilih **base repository** (repo tujuan) dan **base branch** (misal: `main`)
5. Pilih **head repository** (fork kamu) dan **compare branch** (nama-fitur)
6. Isi judul, deskripsi, tambahkan reviewer jika perlu
7. Klik **Create pull request**

### 3.3 Issues

> 💡 **Issues** digunakan untuk melacak bug, fitur yang diminta, atau diskusi. Bisa di-link ke PR dan commit.

**Fitur Issues:**
- Label (bug, enhancement, documentation, dll.)
- Assignee — assign ke orang tertentu
- Milestone — kelompokkan ke versi/sprint tertentu
- Link ke PR: tulis `Closes #12` di body PR untuk auto-close issue #12 saat PR di-merge

### 3.4 GitHub Pages

> 💡 **GitHub Pages** memungkinkan hosting static website langsung dari repository, gratis.

**Cara Aktifkan:**
1. Masuk ke **Settings** → **Pages**
2. Pilih **Source**: Deploy from a branch
3. Pilih branch (misal: `main`) dan folder (`/root` atau `/docs`)
4. Klik **Save** — website tersedia di `https://USERNAME.github.io/nama-repo`

### 3.5 Secrets & Environment Variables

> 💡 **Secrets** adalah variabel sensitif (API key, password) yang disimpan terenkripsi di GitHub dan bisa diakses oleh GitHub Actions.

**Cara Menambahkan Secret:**
1. Masuk ke **Settings** → **Secrets and variables** → **Actions**
2. Klik **New repository secret**
3. Isi **Name** (misal: `DOCKER_PASSWORD`) dan **Secret** (nilainya)
4. Klik **Add secret**

Akses di workflow:
```yaml
env:
  MY_SECRET: ${{ secrets.DOCKER_PASSWORD }}
```

---

## 4. GitHub Actions — CI/CD Otomatis

> 💡 **GitHub Actions** adalah platform CI/CD bawaan GitHub. Kamu bisa otomatisasi proses build, test, dan deploy menggunakan file YAML di dalam repository.

### 4.1 Konsep Dasar

| Istilah | Penjelasan |
|---|---|
| **Workflow** | File YAML yang mendefinisikan seluruh proses otomasi |
| **Event** | Pemicu workflow (push, pull_request, schedule, dll.) |
| **Job** | Sekumpulan steps yang berjalan di satu runner |
| **Step** | Satu perintah atau action dalam sebuah job |
| **Action** | Unit kerja yang bisa dipakai ulang (dari Marketplace atau kustom) |
| **Runner** | Server yang menjalankan job (ubuntu, windows, macos) |

### 4.2 Struktur Dasar Workflow

File workflow disimpan di: `.github/workflows/nama-workflow.yml`

```yaml
name: Nama Workflow

on:           # Event pemicu
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  nama-job:
    runs-on: ubuntu-latest   # Runner

    steps:
      - name: Checkout kode
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Jalankan test
        run: npm test
```

### 4.3 Event Pemicu (Triggers)

```yaml
on:
  # Saat ada push ke branch tertentu
  push:
    branches: [ "main", "develop" ]
    paths:
      - 'src/**'           # Hanya trigger jika file di src/ berubah

  # Saat ada pull request
  pull_request:
    branches: [ "main" ]

  # Jadwal (cron syntax)
  schedule:
    - cron: '0 8 * * 1'   # Setiap Senin jam 08:00 UTC

  # Trigger manual dari GitHub UI
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deploy target'
        required: true
        default: 'staging'
        type: choice
        options: [staging, production]

  # Dipanggil oleh workflow lain
  workflow_call:
```

### 4.4 Contoh: CI untuk Node.js

```yaml
# .github/workflows/ci.yml
name: CI — Node.js

on:
  push:
    branches: [ "main" ]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18, 20, 22]   # Test di 3 versi Node sekaligus

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - run: npm ci
      - run: npm run build --if-present
      - run: npm test
```

### 4.5 Contoh: Build & Push Docker Image ke GHCR

```yaml
# .github/workflows/docker-publish.yml
name: Build & Push Docker Image

on:
  push:
    branches: [ "main" ]
    tags: [ 'v*.*.*' ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Login ke GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Ekstrak metadata (tags, labels)
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=semver,pattern={{version}}
            type=semver,pattern={{major}}.{{minor}}
            type=sha

      - name: Build dan Push image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

### 4.6 Contoh: Deploy ke Server via SSH

```yaml
# .github/workflows/deploy.yml
name: Deploy ke Production

on:
  push:
    branches: [ "main" ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production   # Butuh approval jika dikonfigurasi

    steps:
      - uses: actions/checkout@v4

      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd /app
            git pull origin main
            npm install --production
            pm2 restart app
```

### 4.7 Fitur Lanjutan Actions

#### Artifacts — Simpan File Hasil Build

```yaml
- name: Upload build artifact
  uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: ./dist
    retention-days: 7

# Di job lain, download artifact tersebut
- name: Download artifact
  uses: actions/download-artifact@v4
  with:
    name: build-output
    path: ./dist
```

#### Cache — Percepat Build

```yaml
- name: Cache node_modules
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

#### Environment & Deployment Protection

```yaml
jobs:
  deploy-prod:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://myapp.com   # Akan tampil di GitHub UI
```

> 💡 Environment bisa dikonfigurasi dengan **required reviewers** — deployment harus di-approve dulu sebelum berjalan.

#### Conditional Steps

```yaml
steps:
  - name: Hanya jalankan di main branch
    if: github.ref == 'refs/heads/main'
    run: echo "Ini production!"

  - name: Hanya jalankan jika job sebelumnya berhasil
    if: success()
    run: echo "Job sebelumnya sukses"

  - name: Selalu jalankan (termasuk jika ada error)
    if: always()
    run: echo "Cleanup..."
```

#### Reusable Workflow

```yaml
# Memanggil workflow dari repo/file lain
jobs:
  call-reusable:
    uses: org/repo/.github/workflows/reusable.yml@main
    with:
      environment: staging
    secrets: inherit
```

---

## 5. GitHub Container Registry (GHCR)

> 💡 **GHCR** (`ghcr.io`) adalah registry Docker milik GitHub. Kamu bisa menyimpan dan mendistribusikan Docker image langsung dari ekosistem GitHub, terintegrasi dengan repository dan Actions.

### 5.1 Persiapan Docker

Pastikan Docker sudah terinstall:
```bash
docker --version
```

### 5.2 Buat Dockerfile

```dockerfile
# Dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --production

COPY . .

EXPOSE 3000
CMD ["node", "index.js"]
```

### 5.3 Build Image Lokal

```bash
# Build image dengan tag
docker build -t ghcr.io/USERNAME/nama-app:latest .

# Cek image yang sudah dibuild
docker images

# Jalankan container lokal untuk testing
docker run -p 3000:3000 ghcr.io/USERNAME/nama-app:latest
```

### 5.4 Login ke GHCR

```bash
# Login menggunakan Personal Access Token (PAT)
# Buat PAT di: GitHub → Settings → Developer settings → Personal access tokens
# Scope yang diperlukan: write:packages, read:packages, delete:packages

echo "TOKEN_KAMU" | docker login ghcr.io -u USERNAME --password-stdin
```

### 5.5 Push & Pull Image

```bash
# Tag image sebelum push (jika belum)
docker tag nama-app:latest ghcr.io/USERNAME/nama-app:latest
docker tag nama-app:latest ghcr.io/USERNAME/nama-app:v1.0.0

# Push image ke GHCR
docker push ghcr.io/USERNAME/nama-app:latest
docker push ghcr.io/USERNAME/nama-app:v1.0.0

# Pull image dari GHCR
docker pull ghcr.io/USERNAME/nama-app:latest
```

### 5.6 Atur Visibilitas Package

Secara default, package mengikuti visibilitas repository. Untuk mengubahnya:

1. Buka halaman package di GitHub: `github.com/USERNAME?tab=packages`
2. Pilih package → **Package settings**
3. Ubah visibilitas: **Public** atau **Private**
4. Atau link ke repository: **Connect Repository**

### 5.7 Menggunakan Image dari GHCR di docker-compose

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    image: ghcr.io/USERNAME/nama-app:latest
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
```

```bash
# Jalankan dengan docker-compose
docker compose up -d

# Cek status container
docker compose ps

# Lihat log
docker compose logs -f app
```

---

## 6. Alur Kerja Lengkap (End-to-End)

Berikut adalah simulasi alur kerja nyata dari fork hingga deployment otomatis.

```
[Repo Mentor] ──fork──▶ [Repo Kamu di GitHub] ──clone──▶ [Komputer Lokal]
                                                                  │
                                                          buat branch baru
                                                          edit kode
                                                          git add & commit
                                                                  │
                                                          git push origin fitur-x
                                                                  │
                                                    [GitHub Actions terpicu]
                                                    ✅ Run Tests
                                                    ✅ Build Docker Image
                                                    ✅ Push ke GHCR
                                                                  │
                                                          buat Pull Request
                                                          ──▶ review & merge
                                                                  │
                                                    [GitHub Actions terpicu lagi]
                                                    🚀 Deploy ke Server
```

### Latihan Praktik

**Level 1 — Git Dasar:**
- [ ] Fork repository playground dari mentor
- [ ] Clone hasil fork ke lokal
- [ ] Tambahkan remote `upstream`
- [ ] Buat branch baru dengan nama `fitur/nama-kamu`
- [ ] Tambahkan file `nama-kamu.md` berisi biodata singkat
- [ ] Commit dengan pesan konvensional
- [ ] Push ke fork kamu
- [ ] Buat Pull Request ke repo mentor

**Level 2 — GitHub Actions:**
- [ ] Buat file `.github/workflows/ci.yml`
- [ ] Tambahkan step untuk install dependencies & run test
- [ ] Trigger workflow dengan push ke branch kamu
- [ ] Cek hasil workflow di tab **Actions**
- [ ] Coba buat workflow yang gagal, amati hasilnya

**Level 3 — GitHub Registry:**
- [ ] Buat `Dockerfile` sederhana di repo
- [ ] Tambahkan workflow untuk build & push image ke GHCR
- [ ] Tag image dengan versi (`v1.0.0`)
- [ ] Pull image yang sudah di-push ke komputer lokal
- [ ] Jalankan container dari image tersebut

---

## 📚 Referensi

| Topik | Link |
|---|---|
| Git Documentation | https://git-scm.com/doc |
| GitHub Docs | https://docs.github.com |
| GitHub Actions Marketplace | https://github.com/marketplace?type=actions |
| GitHub Container Registry | https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry |
| Conventional Commits | https://www.conventionalcommits.org |
| Docker Documentation | https://docs.docker.com |

---

> 🙋 **Ada pertanyaan?** Jangan ragu untuk bertanya ke mentor atau buka issue di repository playground!
