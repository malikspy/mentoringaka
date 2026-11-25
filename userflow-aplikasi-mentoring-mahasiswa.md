# User Flow — Aplikasi Mentoring Mahasiswa

Dokumen ini memetakan perjalanan utama pengguna berdasarkan PRD aplikasi mentoring mahasiswa.

## 1. Peta navigasi umum

```mermaid
flowchart TD
    A[Halaman login] --> B{Pilih / kenali peran}
    B -->|Admin| C[Dashboard Admin]
    B -->|Tim Mentoring| D[Dashboard Tim Mentoring]
    B -->|Mentor| E[Login email + kode akses]
    B -->|Analis| F[Dashboard Analis]
    E --> G{Kode akses valid?}
    G -->|Tidak| H[Tampilkan alasan gagal login]
    H --> E
    G -->|Ya| I[Dashboard Mentor]

    C --> J[Menu administrasi]
    D --> K[Menu operasional program]
    I --> L[Menu grup dan laporan]
    F --> M[Menu monitoring]
```

## 2. Alur akses berdasarkan peran

### 2.1 Admin, Tim Mentoring, dan Analis

```mermaid
flowchart TD
    A[Buka aplikasi] --> B[Masukkan email dan kata sandi]
    B --> C{Kredensial valid?}
    C -->|Tidak| D[Tampilkan pesan kesalahan]
    D --> B
    C -->|Ya| E{Akun aktif?}
    E -->|Tidak| F[Tampilkan akun tidak aktif]
    E -->|Ya| G[Catat waktu login]
    G --> H[Arahkan ke dashboard sesuai peran]
```

### 2.2 Mentor

```mermaid
flowchart TD
    A[Buka aplikasi] --> B[Masukkan email dan kode akses]
    B --> C{Email dan kode cocok?}
    C -->|Tidak| D[Tampilkan login gagal]
    D --> B
    C -->|Ya| E{Kode masih aktif?}
    E -->|Kedaluwarsa / dicabut| F[Tampilkan status kode]
    F --> G[Hubungi Admin untuk kode baru]
    E -->|Aktif| H[Catat waktu login]
    H --> I[Dashboard Mentor]
```

## 3. User flow Admin

```mermaid
flowchart TD
    A[Dashboard Admin] --> B{Pilih pekerjaan}
    B --> C[Kelola akun]
    B --> D[Kelola periode dan program]
    B --> E[Kelola master peserta]
    B --> F[Kelola kode akses Mentor]
    B --> G[Lihat laporan dan ekspor]
    B --> H[Lihat audit log]

    C --> C1[Tambah / ubah / nonaktifkan akun]
    C1 --> A

    D --> D1[Buat periode]
    D1 --> D2[Buat program]
    D2 --> D3[Atur batas waktu laporan]
    D3 --> A

    E --> E1[Tambah / impor peserta]
    E1 --> E2[Validasi data dan duplikasi]
    E2 --> E3[Simpan peserta aktif]
    E3 --> A

    F --> F1[Pilih Mentor]
    F1 --> F2{Tindakan kode}
    F2 --> F3[Terbitkan kode]
    F2 --> F4[Perpanjang kode]
    F2 --> F5[Cabut kode]
    F2 --> F6[Buat ulang kode]
    F3 --> F7[Tampilkan kode satu kali]
    F4 --> A
    F5 --> A
    F6 --> F7
    F7 --> A
```

## 4. User flow Tim Mentoring

### 4.1 Menyiapkan program dan grup

```mermaid
flowchart TD
    A[Dashboard Tim Mentoring] --> B[Pilih periode dan program]
    B --> C[Buat atau buka grup]
    C --> D[Tambahkan satu atau beberapa Mentor]
    D --> E[Tambahkan peserta]
    E --> F{Peserta sudah aktif di grup dan periode yang sama?}
    F -->|Ya| G[Tolak duplikasi dan tampilkan detail]
    F -->|Tidak| H[Simpan keanggotaan dengan tanggal efektif]
    H --> I[Grup siap digunakan]
    I --> J[Atur jadwal pertemuan]
    J --> K[Unggah materi opsional]
```

### 4.2 Memindahkan peserta

```mermaid
flowchart TD
    A[Daftar peserta] --> B[Pilih peserta]
    B --> C[Pilih Pindahkan grup]
    C --> D[Pilih grup tujuan dan tanggal efektif]
    D --> E{Peserta sudah aktif di grup tujuan pada periode yang sama?}
    E -->|Ya| F[Tolak perpindahan]
    E -->|Tidak| G[Tutup keanggotaan grup lama]
    G --> H[Simpan keanggotaan grup baru]
    H --> I[Simpan riwayat perpindahan]
```

### 4.3 Meninjau laporan

```mermaid
flowchart TD
    A[Dashboard Tim Mentoring] --> B[Buka antrean laporan]
    B --> C[Filter periode, program, grup, Mentor, atau status]
    C --> D[Pilih laporan Dikirim]
    D --> E[Periksa topik, ringkasan, absensi, evaluasi, dan lampiran]
    E --> F{Laporan lengkap dan sesuai?}
    F -->|Ya| G[Setujui laporan]
    G --> H[Status Disetujui dan read-only]
    F -->|Tidak| I[Isi catatan revisi]
    I --> J[Kembalikan dengan status Perlu revisi]
    J --> K[Monitor pengiriman ulang]
```

### 4.4 Meninjau rekomendasi mentoring lanjutan

```mermaid
flowchart TD
    A[Dashboard Tim Mentoring] --> B[Buka rekomendasi Diajukan]
    B --> C[Periksa alasan, evaluasi, kompetensi, dan kebutuhan]
    C --> D{Keputusan}
    D -->|Setujui| E[Pilih program / grup tujuan]
    E --> F[Simpan Disetujui]
    D -->|Tolak| G[Isi catatan keputusan]
    G --> H[Simpan Ditolak]
    D -->|Perlu evaluasi| I[Isi permintaan evaluasi]
    I --> J[Simpan Perlu evaluasi]
    F --> K[Catat peninjau dan tanggal]
    H --> K
    J --> K
```

## 5. User flow Mentor

### 5.1 Melihat tugas

```mermaid
flowchart TD
    A[Dashboard Mentor] --> B[Lihat ringkasan tugas]
    B --> C{Pilih area}
    C --> D[Grup saya]
    C --> E[Jadwal mendatang]
    C --> F[Draft laporan]
    C --> G[Laporan perlu revisi]
    C --> H[Rekomendasi peserta]

    D --> D1[Lihat peserta dan riwayat grup]
    E --> E1[Lihat detail jadwal dan materi]
    F --> F1[Lanjutkan draft]
    G --> G1[Buka catatan revisi]
    H --> H1[Lihat status pengajuan]
```

### 5.2 Mengisi dan mengirim laporan pertemuan

```mermaid
flowchart TD
    A[Dashboard Mentor] --> B[Pilih jadwal yang sudah berlangsung]
    B --> C[Pilih Buat laporan]
    C --> D[Isi tanggal dan topik]
    D --> E[Isi absensi semua peserta]
    E --> F[Tambahkan catatan peserta bila perlu]
    F --> G[Isi ringkasan, evaluasi, kendala, dan tindak lanjut]
    G --> H[Tambahkan lampiran opsional]
    H --> I{Pilih tindakan}
    I -->|Simpan draft| J[Status Draft]
    J --> K[Lanjutkan atau ubah nanti]
    I -->|Kirim laporan| L{Absensi lengkap?}
    L -->|Tidak| M[Tampilkan peserta yang belum diisi]
    M --> E
    L -->|Ya| N{Melewati batas waktu?}
    N -->|Ya| O[Simpan sebagai terlambat]
    N -->|Tidak| P[Simpan sebagai tepat waktu]
    O --> Q[Status Dikirim]
    P --> Q
    Q --> R[Menunggu peninjauan Tim Mentoring]
    R --> S{Hasil peninjauan}
    S -->|Disetujui| T[Status Disetujui]
    S -->|Perlu revisi| U[Buka catatan revisi]
    U --> V[Perbaiki laporan]
    V --> Q
```

### 5.3 Mengajukan rekomendasi mentoring lanjutan

```mermaid
flowchart TD
    A[Dashboard Mentor] --> B[Buka grup yang ditugaskan]
    B --> C[Pilih peserta]
    C --> D[Pilih Rekomendasikan mentoring lanjutan]
    D --> E[Isi alasan rekomendasi]
    E --> F[Isi evaluasi dan kompetensi]
    F --> G[Isi kebutuhan mentoring lanjutan]
    G --> H[Tambahkan catatan opsional]
    H --> I[Simpan pengajuan]
    I --> J[Status Diajukan]
    J --> K{Sudah ditinjau Tim Mentoring?}
    K -->|Belum| L[Masih dapat diubah Mentor]
    L --> J
    K -->|Ya| M[Tampilkan keputusan dan catatan]
```

## 6. User flow Analis

```mermaid
flowchart TD
    A[Dashboard Analis] --> B[Lihat indikator monitoring]
    B --> C[Grup tanpa aktivitas]
    B --> D[Laporan belum dikirim]
    B --> E[Laporan terlambat]
    B --> F[Pengajuan rekomendasi]
    B --> G[Tren kehadiran dan evaluasi]

    C --> H[Pilih grup]
    D --> I[Pilih daftar laporan]
    E --> J[Pilih laporan terlambat]
    F --> K[Pilih rekomendasi]
    G --> L[Pilih rentang waktu]

    H --> M[Lihat detail dan aktivitas terakhir]
    I --> N[Lihat jadwal dan Mentor terkait]
    J --> O[Lihat usia keterlambatan]
    K --> P[Lihat status proses]
    L --> Q[Lihat hasil filter / tren]
```

## 7. Alur status laporan

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Dikirim: Mentor kirim
    Dikirim --> Disetujui: Tim Mentoring setujui
    Dikirim --> PerluRevisi: Tim Mentoring kembalikan
    PerluRevisi --> Dikirim: Mentor kirim ulang
    Disetujui --> [*]
```

Catatan: status terlambat adalah penanda waktu pada laporan, bukan status pengganti. Laporan terlambat tetap dapat berada pada status Draft, Dikirim, Perlu revisi, atau Disetujui.

## 8. Alur status kode akses Mentor

```mermaid
stateDiagram-v2
    [*] --> Aktif: Admin terbitkan
    Aktif --> AkanBerakhir: Mendekati tanggal kedaluwarsa
    AkanBerakhir --> Aktif: Admin perpanjang
    Aktif --> Kedaluwarsa: Melewati tanggal kedaluwarsa
    AkanBerakhir --> Kedaluwarsa: Melewati tanggal kedaluwarsa
    Aktif --> Dicabut: Admin cabut
    AkanBerakhir --> Dicabut: Admin cabut
    Kedaluwarsa --> Aktif: Admin terbitkan kode baru
    Dicabut --> Aktif: Admin terbitkan kode baru
```

## 9. Alur pengecualian yang harus terlihat pengguna

| Situasi | Respons sistem |
| --- | --- |
| Login dengan akun nonaktif | Tampilkan status akun dan arahkan ke Admin |
| Login dengan kode Mentor kedaluwarsa | Tampilkan tanggal kedaluwarsa dan instruksi menghubungi Admin |
| Peserta duplikat dalam grup/periode | Tolak penyimpanan dan tampilkan data yang bentrok |
| Laporan tanpa absensi lengkap | Tahan pengiriman dan tandai peserta yang belum diisi |
| Laporan dikembalikan | Tampilkan catatan revisi pada halaman laporan dan dashboard Mentor |
| Mentor membuka peserta dari grup lain | Tolak akses dan arahkan kembali ke grup yang ditugaskan |
| Grup atau program diarsipkan | Hilangkan dari pilihan data baru, tetapi pertahankan riwayat |
| Ekspor tanpa hak akses | Tolak permintaan dan catat percobaan akses bila diperlukan |

## 10. Jalur terpendek untuk tugas inti

| Tugas | Jalur utama |
| --- | --- |
| Mentor mengirim laporan | Login → Dashboard → Jadwal → Buat laporan → Absensi → Isi laporan → Kirim |
| Tim Mentoring menyetujui laporan | Login → Dashboard → Antrean laporan → Filter → Buka laporan → Setujui |
| Admin menerbitkan kode Mentor | Login → Dashboard → Akun Mentor → Pilih Mentor → Terbitkan kode |
| Tim Mentoring menempatkan peserta | Login → Dashboard → Program → Grup → Tambah peserta → Simpan |
| Mentor merekomendasikan peserta | Login → Dashboard → Grup → Peserta → Rekomendasikan → Kirim |
| Analis menemukan laporan terlambat | Login → Dashboard → Laporan terlambat → Filter → Buka detail |
