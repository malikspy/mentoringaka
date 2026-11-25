# Backlog MVP — Aplikasi Mentoring Mahasiswa

**Status:** Siap untuk refinement awal  
**Tanggal:** 25 November 2026  
**Sumber:** [PRD](prd-aplikasi-mentoring-mahasiswa.md), [rancangan aplikasi](rancangan-aplikasi-mentoring-mahasiswa.md), [user flow](userflow-aplikasi-mentoring-mahasiswa.md), [ERD](erd-aplikasi-mentoring-mahasiswa.md)

## Tujuan MVP

Menyediakan satu alur kerja yang dapat dipakai pada pilot: Admin menyiapkan program dan akses, Tim Mentoring menempatkan peserta serta jadwal, Mentor mengisi absensi dan laporan, lalu Tim Mentoring meninjau laporan dan rekomendasi. Analis dapat memantau status, keterlambatan, dan kehadiran.

## Definisi prioritas

- **P0 — wajib:** tanpa item ini alur pilot atau kontrol akses tidak dapat berjalan.
- **P1 — penting:** dibutuhkan agar pilot dapat dipantau, direkap, dan dioperasikan dengan aman.
- **P2 — tunda:** kandidat setelah MVP; tidak dihitung sebagai syarat pilot.

## Backlog terurut

### EPIC FND — Fondasi teknis

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| FND-01 | P0 | Sebagai tim pengembang, saya ingin struktur aplikasi, environment, CI, dan konfigurasi lokal tersedia agar pekerjaan dapat direproduksi. | Aplikasi dapat dijalankan di lokal dan lingkungan uji; konfigurasi rahasia tidak masuk repository; pipeline menjalankan lint dan test. | — |
| FND-02 | P0 | Sebagai tim pengembang, saya ingin migrasi database untuk entitas inti agar data MVP konsisten dengan ERD. | Migrasi membuat tabel user/role, access code, periode, program, grup, peserta, keanggotaan, penugasan, jadwal, laporan, absensi, rekomendasi, notifikasi, audit, dan ekspor; constraint unik dan status utama tersedia. | FND-01 |
| FND-03 | P0 | Sebagai admin, saya ingin seed role dan akun admin awal agar sistem dapat dipakai. | Role Admin, Tim Mentoring, Mentor, dan Analis tersedia; akun awal dibuat melalui mekanisme aman; timestamp disimpan UTC. | FND-02 |

### EPIC AUTH — Autentikasi dan otorisasi

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| AUTH-01 | P0 | Sebagai Admin, Tim Mentoring, atau Analis, saya ingin login dengan email dan kata sandi. | Kredensial valid membuat sesi; kredensial salah menampilkan error umum; akun nonaktif ditolak; password disimpan sebagai hash. | FND-03 |
| AUTH-02 | P0 | Sebagai pengguna, saya hanya ingin melihat halaman dan data sesuai peran. | Endpoint dan halaman di luar hak akses ditolak; Mentor hanya melihat grup yang memiliki penugasan aktif; perubahan peran berlaku pada login berikutnya. | AUTH-01 |
| AUTH-03 | P0 | Sebagai Admin, saya ingin mengelola akun internal dan perannya. | Admin dapat menambah, mengubah, menonaktifkan, mengarsipkan akun, dan mengatur role; akun nonaktif tidak dapat login; perubahan tercatat. | AUTH-01, DATA-02 |
| AUTH-04 | P0 | Sebagai Mentor, saya ingin login dengan email dan kode akses yang aktif. | Kode memiliki hash, tanggal kedaluwarsa default satu tahun, dan status; kode kedaluwarsa/dicabut ditolak; penerbitan ulang menonaktifkan kode lama; kode hanya ditampilkan saat diterbitkan. | AUTH-01, AUTH-03 |
| AUTH-05 | P1 | Sebagai admin, saya ingin melihat aktivitas login agar akses dapat ditelusuri. | Login berhasil dan gagal yang relevan menyimpan pengguna, waktu, dan hasil; data sensitif tidak masuk log. | AUTH-01, AUTH-04, DATA-02 |

### EPIC MASTER — Periode, program, grup, dan penugasan

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| MASTER-01 | P0 | Sebagai Admin, saya ingin membuat dan mengarsipkan periode. | Periode memiliki nama, tanggal aktif, batas waktu laporan, dan status; tanggal tidak valid ditolak; data berhistori tidak dihapus permanen. | AUTH-02 |
| MASTER-02 | P0 | Sebagai Admin, saya ingin mengelola program dalam periode. | Program wajib terkait periode; override batas waktu boleh kosong; program arsip tidak dapat dipilih untuk data baru. | MASTER-01 |
| MASTER-03 | P0 | Sebagai Tim Mentoring, saya ingin membuat dan mengarsipkan grup. | Grup wajib terkait program/periode; grup arsip tidak muncul pada jadwal atau keanggotaan baru; riwayat tetap dapat dibaca. | MASTER-02 |
| MASTER-04 | P0 | Sebagai Tim Mentoring, saya ingin menetapkan satu atau beberapa mentor ke grup. | Hanya pengguna ber-role Mentor yang dapat ditugaskan; rentang tanggal efektif tervalidasi; mentor dapat memiliki beberapa grup; penugasan lama dapat diakhiri. | AUTH-03, MASTER-03 |

### EPIC PARTICIPANT — Peserta dan keanggotaan

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| PART-01 | P0 | Sebagai Admin atau Tim Mentoring, saya ingin menambah dan mengubah data peserta. | Nomor peserta/NIM dan nama wajib; status, jurusan, angkatan, telepon opsional, dan catatan tersimpan; data aktif dapat dinonaktifkan atau diarsipkan. | AUTH-02 |
| PART-02 | P0 | Sebagai Admin atau Tim Mentoring, saya ingin mengimpor peserta dari CSV dengan validasi. | Template tersedia; baris invalid dan duplikat dilaporkan sebelum commit; hasil impor menampilkan jumlah berhasil/gagal dan alasan error; impor dapat diulang tanpa membuat duplikat. | PART-01 |
| PART-03 | P0 | Sebagai Tim Mentoring, saya ingin menambahkan peserta ke grup dengan tanggal efektif. | Peserta hanya dapat ditambahkan ke grup aktif; satu peserta tidak dapat memiliki dua keanggotaan aktif pada periode yang sama; tanggal efektif dan status tersimpan. | MASTER-03, PART-01 |
| PART-04 | P0 | Sebagai Tim Mentoring, saya ingin memindahkan peserta antargrup tanpa kehilangan riwayat. | Keanggotaan lama ditutup, keanggotaan baru dibuat, tanggal efektif tercatat, dan histori asal/tujuan dapat dilihat; absensi/laporan lama tetap merujuk ke peserta yang sama. | PART-03, DATA-02 |

### EPIC OPS — Jadwal dan materi

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| OPS-01 | P0 | Sebagai Tim Mentoring, saya ingin membuat dan mengubah jadwal pertemuan. | Jadwal wajib memiliki grup, waktu mulai/selesai, topik, lokasi atau tautan, dan `report_due_at` yang dihitung dari batas waktu efektif; jadwal tanpa grup ditolak. | MASTER-03, MASTER-04 |
| OPS-02 | P1 | Sebagai Tim Mentoring, saya ingin mengunggah dan mengarsipkan materi grup. | Materi memiliki judul, file, tipe, ukuran, pengunggah, dan status; file melebihi batas ditolak; materi arsip tidak tampil pada daftar aktif; Mentor hanya melihat materi grupnya. | AUTH-02, MASTER-03 |

### EPIC REPORT — Absensi dan laporan pertemuan

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| REPORT-01 | P0 | Sebagai Mentor, saya ingin membuat laporan dari jadwal yang sudah berlangsung. | Hanya jadwal grup yang ditugaskan dan belum memiliki laporan aktif yang dapat dipilih; laporan awal berstatus Draft; Mentor tidak dapat membuat laporan ad-hoc. | AUTH-02, OPS-01, PART-03 |
| REPORT-02 | P0 | Sebagai Mentor, saya ingin mengisi absensi setiap peserta. | Peserta aktif pada jadwal ditampilkan; status hanya Hadir/Izin/Sakit/Alpa; satu baris per peserta; catatan opsional tersimpan. | REPORT-01 |
| REPORT-03 | P0 | Sebagai Mentor, saya ingin menyimpan isi laporan dan melanjutkannya nanti. | Form memuat tanggal/topik, ringkasan, evaluasi umum, kendala, tindak lanjut, dan lampiran opsional; Draft dapat diubah oleh pengirim; lampiran disimpan sebagai metadata object storage. | REPORT-01, REPORT-02 |
| REPORT-04 | P0 | Sebagai Mentor, saya ingin mengirim laporan setelah semua data lengkap. | Pengiriman ditahan jika ada absensi kosong; sistem menghitung `is_late` dari `report_due_at`; status berubah Draft menjadi Dikirim; waktu kirim dan histori status tersimpan. | REPORT-02, REPORT-03, DATA-02 |
| REPORT-05 | P0 | Sebagai Mentor, saya ingin memperbaiki laporan yang dikembalikan. | Laporan Perlu revisi menampilkan catatan wajib dari peninjau; Mentor dapat mengubah dan mengirim ulang; laporan Disetujui menjadi read-only. | REVIEW-02, REVIEW-03 |

### EPIC REVIEW — Persetujuan laporan

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| REVIEW-01 | P0 | Sebagai Tim Mentoring, saya ingin melihat antrean laporan Dikirim dan memfilternya. | Antrean dapat difilter menurut periode, program, grup, mentor, status, dan tanggal; jumlah dan daftar hasil konsisten. | REPORT-04 |
| REVIEW-02 | P0 | Sebagai Tim Mentoring, saya ingin menyetujui laporan yang lengkap. | Peninjau berwenang dapat menyetujui; status menjadi Disetujui; waktu dan pengguna penyetuju tercatat; laporan tidak dapat diedit Mentor. | REVIEW-01, DATA-02 |
| REVIEW-03 | P0 | Sebagai Tim Mentoring, saya ingin mengembalikan laporan untuk revisi. | Catatan revisi wajib; status menjadi Perlu revisi; histori tersimpan; Mentor menerima notifikasi in-app. | REVIEW-01, DATA-02 |

### EPIC REC — Rekomendasi mentoring lanjutan

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| REC-01 | P1 | Sebagai Mentor, saya ingin mengajukan rekomendasi untuk peserta di grup saya. | Hanya peserta dengan keanggotaan aktif pada grup Mentor yang dapat dipilih; alasan, evaluasi, kompetensi, kebutuhan, dan catatan tersimpan; status awal Diajukan; draft pengajuan dapat diubah sebelum ditinjau. | AUTH-02, PART-03 |
| REC-02 | P1 | Sebagai Tim Mentoring, saya ingin meninjau dan memutuskan rekomendasi. | Keputusan hanya Disetujui/Ditolak/Perlu evaluasi; catatan, peninjau, tanggal, dan histori tersimpan; status yang sudah ditinjau tidak dapat diubah Mentor; grup tujuan harus berada pada program tujuan. | REC-01, MASTER-02, MASTER-03, DATA-02 |

### EPIC MON — Dashboard dan notifikasi

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| MON-01 | P0 | Sebagai pengguna, saya ingin melihat dashboard sesuai peran. | Admin melihat akun/peserta/grup/laporan; Tim Mentoring melihat grup, jadwal, antrean laporan, dan rekomendasi; Mentor melihat grup, jadwal, draft/revisi; Analis melihat aktivitas, keterlambatan, kehadiran, dan evaluasi. | AUTH-02, MASTER-03, REPORT-04, REVIEW-01 |
| MON-02 | P1 | Sebagai pengguna, saya ingin menerima alert in-app untuk pekerjaan tertunda. | Alert dibuat untuk laporan belum dikirim/terlambat, laporan perlu revisi, dan laporan menunggu persetujuan; penerima hanya pengguna berwenang; alert dapat ditandai sudah dibaca. | REPORT-04, REVIEW-01 |
| MON-03 | P1 | Sebagai Analis, saya ingin memfilter monitoring lintas grup. | Filter periode, program, grup, mentor, status laporan, dan rentang tanggal tersedia; daftar laporan terlambat dihitung dari batas waktu efektif; hasil dapat dibuka ke detail. | MON-01, REPORT-04 |

### EPIC DATA — Ekspor, audit, dan pemulihan

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| DATA-01 | P1 | Sebagai Admin atau Tim Mentoring, saya ingin mengekspor data yang boleh saya akses. | CSV dan XLSX tersedia untuk peserta, laporan, absensi, dan rekomendasi; filter dan waktu ekspor dicatat; permintaan di luar kewenangan ditolak; data ekspor sesuai filter. | AUTH-02, MON-03 |
| DATA-02 | P0 | Sebagai Admin, saya ingin melihat audit log perubahan penting. | Perubahan akun, peserta, keanggotaan, jadwal, laporan, persetujuan, rekomendasi, dan login mencatat objek, tindakan, pengguna, waktu, serta perubahan utama; log tidak dapat diubah pengguna biasa; rahasia tidak dicatat. | FND-02 |
| DATA-03 | P1 | Sebagai operator, saya ingin backup dan memulihkan database sebelum pilot. | Backup terjadwal berhasil; restore diuji pada lingkungan uji; hasil restore diverifikasi dengan data contoh; prosedur dan waktu pemulihan terdokumentasi. | FND-02 |

### EPIC NFR — Kesiapan pilot

| ID | Prioritas | User story / pekerjaan | Kriteria penerimaan ringkas | Dependensi |
| --- | --- | --- | --- | --- |
| NFR-01 | P0 | Sebagai pengguna, saya ingin alur inti dapat digunakan dengan keyboard dan pada ponsel. | Semua form memiliki label dan pesan error; fokus keyboard terlihat; status tidak hanya memakai warna; login Mentor, isi absensi, dan kirim laporan dapat dipakai pada layar ponsel. | AUTH-01, REPORT-04 |
| NFR-02 | P0 | Sebagai pemilik produk, saya ingin akses dan data sensitif terlindungi. | Password/kode hanya disimpan sebagai hash; sesi berakhir sesuai konfigurasi; query mengikuti hak akses; file lampiran tidak dapat diakses tanpa otorisasi. | AUTH-02, REPORT-03 |
| NFR-03 | P1 | Sebagai pemilik produk, saya ingin performa cukup untuk pilot. | Dashboard dan daftar utama menampilkan hasil awal dalam maksimal 3 detik pada beban pilot yang disepakati; query utama memiliki indeks yang diperlukan. | MON-01, MON-03 |
| NFR-04 | P0 | Sebagai pemilik produk, saya ingin kesiapan pilot dapat diverifikasi. | Data contoh berhasil diimpor; hak akses empat peran diuji; semua kriteria penerimaan FR-01 sampai FR-10 punya bukti uji; ekspor dan restore berhasil; panduan singkat pengguna tersedia. | Semua P0/P1 |

## Urutan rilis yang disarankan

### Sprint 0 — Fondasi dan keputusan

`FND-01` → `FND-02` → `FND-03` → `AUTH-01` → `AUTH-02` → `DATA-02`

Validasi keputusan produk sebelum implementasi penuh: penyedia identitas, satu laporan per jadwal, zona waktu operasional, lama retensi data/lampiran, jumlah peninjau, format ekspor pilot, dan kebutuhan email.

### Sprint 1 — Setup program dan data

`AUTH-03` → `AUTH-04` → `MASTER-01` → `MASTER-02` → `MASTER-03` → `MASTER-04` → `PART-01` → `PART-02` → `PART-03` → `PART-04`

### Sprint 2 — Operasional Mentor

`OPS-01` → `OPS-02` → `REPORT-01` → `REPORT-02` → `REPORT-03` → `REPORT-04`

### Sprint 3 — Pengawasan dan keputusan

`REVIEW-01` → `REVIEW-02` → `REVIEW-03` → `MON-01` → `MON-02` → `MON-03` → `REC-01` → `REC-02`

### Sprint 4 — Data keluar dan pilot

`DATA-01` → `DATA-03` → `AUTH-05` → `NFR-01` → `NFR-02` → `NFR-03` → `NFR-04`

Urutan sprint dapat diubah setelah kapasitas tim dan keputusan produk divalidasi; dependensi pada setiap item tetap berlaku.

## Traceability ke kebutuhan fungsional

| Kebutuhan PRD | Item backlog |
| --- | --- |
| FR-01 Autentikasi dan otorisasi | AUTH-01 sampai AUTH-05 |
| FR-02 Kode akses Mentor | AUTH-04 |
| FR-03 Periode, program, dan grup | MASTER-01 sampai MASTER-04 |
| FR-04 Data dan keanggotaan peserta | PART-01 sampai PART-04 |
| FR-05 Jadwal dan materi | OPS-01 sampai OPS-02 |
| FR-06 Absensi dan laporan pertemuan | REPORT-01 sampai REPORT-05 |
| FR-07 Persetujuan laporan | REVIEW-01 sampai REVIEW-03 |
| FR-08 Rekomendasi mentoring lanjutan | REC-01 sampai REC-02 |
| FR-09 Dashboard dan alert | MON-01 sampai MON-03 |
| FR-10 Ekspor dan audit | DATA-01 sampai DATA-03 |
| Persyaratan nonfungsional dan definisi selesai | NFR-01 sampai NFR-04 |

## Definition of Ready

Sebuah item siap masuk sprint jika:

- tujuan dan aktor jelas;
- kriteria penerimaan dapat diuji;
- dependensi teknis dan keputusan produk diketahui;
- data contoh atau fixture tersedia bila diperlukan;
- pemilik produk menerima perilaku yang diharapkan.

## Definition of Done MVP

Item dianggap selesai jika kode dan migrasi sudah ditinjau, validasi hak akses diterapkan di server, kriteria penerimaan lulus, audit/status history dibuat bila diwajibkan, error dan empty state tersedia, dan bukti uji tersimpan. MVP siap pilot setelah `NFR-04` selesai.

## Kandidat setelah MVP (P2)

- Portal atau akun mandiri peserta.
- OTP dan verifikasi email.
- Chat atau video conference.
- Penilaian otomatis berbasis AI.
- Integrasi sistem akademik eksternal.
- Notifikasi email atau WhatsApp.
- Laporan ad-hoc yang tidak berasal dari jadwal.
