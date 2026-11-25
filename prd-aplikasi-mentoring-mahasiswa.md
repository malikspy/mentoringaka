# PRD — Aplikasi Mentoring Mahasiswa

**Status:** Draft untuk validasi stakeholder  
**Tanggal:** 25 November 2026  
**Pemilik produk:** Tim Program Mentoring  
**Sumber:** `rancangan-aplikasi-mentoring-mahasiswa.md`

## 1. Ringkasan produk

Aplikasi web untuk mengelola program mentoring mahasiswa dari awal sampai akhir: pengaturan periode dan program, pembentukan grup, penugasan mentor, jadwal pertemuan, absensi, laporan kegiatan, persetujuan laporan, evaluasi, dan rekomendasi mentoring lanjutan.

Peserta tidak perlu membuat akun. Data peserta dikelola oleh Admin dan Tim Mentoring, sedangkan Mentor hanya dapat melihat serta mengisi data untuk grup yang ditugaskan kepadanya.

## 2. Masalah yang ingin diselesaikan

Pengelolaan mentoring yang tersebar di spreadsheet dan pesan pribadi membuat tim sulit mengetahui:

- siapa yang aktif di setiap grup dan siapa mentornya;
- pertemuan mana yang sudah berlangsung dan sudah dilaporkan;
- peserta yang tidak hadir atau membutuhkan tindak lanjut;
- laporan yang terlambat atau menunggu persetujuan;
- peserta yang layak mengikuti mentoring lanjutan;
- perubahan data dan riwayat keputusan.

Aplikasi harus menyediakan satu sumber data yang dapat ditelusuri dan dapat dipakai sesuai kewenangan setiap peran.

## 3. Tujuan dan indikator keberhasilan

### Tujuan MVP

1. Memusatkan data peserta, grup, mentor, jadwal, dan laporan dalam satu sistem.
2. Memastikan setiap pertemuan memiliki absensi dan laporan yang dapat ditinjau.
3. Membuat status laporan dan keterlambatan terlihat oleh Tim Mentoring dan Analis.
4. Menyediakan proses rekomendasi mentoring lanjutan yang terdokumentasi.
5. Mengurangi pekerjaan rekap manual melalui dashboard dan ekspor data.

### Indikator keberhasilan

- Minimal 90% laporan pertemuan dikirim melalui aplikasi pada akhir periode pilot.
- Minimal 95% laporan yang dikirim memiliki absensi lengkap.
- Median waktu dari pertemuan selesai sampai laporan dikirim kurang dari 24 jam.
- Tim Mentoring dapat menemukan laporan terlambat dalam waktu kurang dari 2 menit.
- Tidak ada peserta aktif yang terduplikasi dalam grup dan periode yang sama.

## 4. Sasaran pengguna dan hak akses

| Peran | Kebutuhan utama | Hak akses inti |
| --- | --- | --- |
| Admin | Menjaga konfigurasi, akun, dan data master | Akses penuh untuk data yang aktif, pengarsipan, ekspor, dan audit |
| Tim Mentoring | Menjalankan operasional program | Mengelola program, periode, grup, peserta, jadwal, materi, persetujuan laporan, dan rekomendasi |
| Mentor | Melaksanakan sesi dan melaporkan hasil | Melihat grup yang ditugaskan, mengisi absensi/laporan, serta mengajukan rekomendasi |
| Analis | Memantau aktivitas dan kepatuhan | Melihat seluruh grup, status laporan, keterlambatan, kehadiran, dan evaluasi |

Peserta tidak memiliki akun pada MVP. Akses data peserta diberikan melalui peran internal sesuai kebutuhan kerja.

## 5. Ruang lingkup MVP

### Termasuk

1. Login berbasis email dan kata sandi untuk Admin, Tim Mentoring, dan Analis.
2. Login Mentor menggunakan email dan kode akses yang memiliki masa berlaku.
3. Pengelolaan periode, program, grup, akun, peserta, dan master data.
4. Penugasan satu atau beberapa mentor ke grup.
5. Keanggotaan peserta dalam grup dengan tanggal efektif dan riwayat perpindahan.
6. Jadwal pertemuan dan materi.
7. Absensi dan laporan pertemuan.
8. Alur persetujuan laporan: Draft → Dikirim → Perlu revisi → Disetujui.
9. Rekomendasi peserta untuk mentoring lanjutan.
10. Dashboard per peran, notifikasi in-app untuk tugas tertunda, dan ekspor CSV/Excel.
11. Riwayat perubahan data penting dan aktivitas login.

### Tidak termasuk dalam MVP

- Akun atau portal mandiri untuk peserta.
- Video conference dan chat real-time.
- OTP atau verifikasi email.
- Penilaian otomatis berbasis AI.
- Integrasi sistem akademik eksternal sebelum kebutuhan dan sumber datanya disepakati.

## 6. Kebutuhan fungsional

### FR-01 — Autentikasi dan otorisasi

- Pengguna login sesuai metode yang ditetapkan untuk perannya.
- Sistem menolak akses ke halaman dan data di luar kewenangan pengguna.
- Admin dapat mengaktifkan, menonaktifkan, mengarsipkan, dan mengubah peran akun internal.
- Setiap login menyimpan waktu dan pengguna yang melakukan login.

**Kriteria penerimaan:** pengguna Mentor hanya melihat grup yang ditugaskan; pengguna yang dinonaktifkan tidak dapat login; perubahan hak akses berlaku pada login berikutnya.

### FR-02 — Kode akses Mentor

- Admin menerbitkan kode akses untuk Mentor dengan tanggal kedaluwarsa default satu tahun.
- Status kode: Aktif, Akan berakhir, Kedaluwarsa, atau Dicabut.
- Admin dapat memperpanjang, mencabut, atau membuat ulang kode.
- Kode disimpan dalam bentuk aman dan tidak ditampilkan kembali setelah proses penerbitan selesai.

**Kriteria penerimaan:** login gagal untuk kode yang kedaluwarsa atau dicabut; penerbitan ulang menonaktifkan kode lama; sistem menampilkan tanggal kedaluwarsa sebelum kode dibagikan.

### FR-03 — Periode, program, dan grup

- Admin membuat periode dan program beserta tanggal aktif, batas waktu laporan, dan status.
- Tim Mentoring membuat grup, menetapkan program/periode, dan menambahkan satu atau beberapa mentor.
- Data yang sudah memiliki riwayat tidak dihapus permanen; data dinonaktifkan atau diarsipkan.

**Kriteria penerimaan:** grup harus memiliki program dan periode; grup yang diarsipkan tidak muncul sebagai pilihan untuk jadwal baru.

### FR-04 — Data dan keanggotaan peserta

- Data minimal: nomor peserta/NIM, nama, program/jurusan, angkatan, nomor telepon opsional, status aktif, dan catatan.
- Tim Mentoring dapat menambah, mengubah, memindahkan, dan menonaktifkan peserta.
- Perpindahan memiliki tanggal efektif dan menyimpan grup asal serta grup tujuan.
- Sistem menolak peserta yang sama terdaftar dua kali pada grup dan periode yang sama.

**Kriteria penerimaan:** riwayat absensi dan laporan tetap merujuk pada identitas peserta setelah perpindahan grup.

### FR-05 — Jadwal dan materi

- Tim Mentoring membuat jadwal yang terhubung ke grup, tanggal, waktu, topik, lokasi/tautan, dan batas waktu laporan.
- Mentor melihat jadwal grupnya dan materi yang tersedia.
- Tim Mentoring dapat mengunggah, mengganti, dan mengarsipkan materi.

**Kriteria penerimaan:** jadwal yang sudah lewat dapat dipakai sebagai dasar pembuatan laporan; jadwal tanpa grup tidak dapat disimpan.

### FR-06 — Absensi dan laporan pertemuan

- Mentor membuat laporan dari jadwal yang tersedia.
- Setiap peserta memiliki status Hadir, Izin, Sakit, atau Alpa.
- Form laporan memuat tanggal/topik, ringkasan kegiatan, catatan peserta, evaluasi umum, kendala, tindak lanjut, dan lampiran opsional.
- Laporan dapat disimpan sebagai Draft dan dilanjutkan kemudian.
- Sistem menyimpan waktu kirim, waktu persetujuan, pengirim, penyetuju, catatan revisi, dan status keterlambatan.

**Kriteria penerimaan:** laporan tidak dapat dikirim jika absensi belum lengkap; laporan yang dikembalikan memuat alasan revisi; Mentor hanya dapat mengubah laporan Draft atau Perlu revisi.

### FR-07 — Persetujuan laporan

- Tim Mentoring melihat antrean laporan Dikirim.
- Peninjau dapat menyetujui atau mengembalikan laporan untuk revisi dengan catatan wajib.
- Laporan Disetujui menjadi read-only, kecuali dibuka kembali oleh pengguna berwenang.

**Kriteria penerimaan:** setiap perubahan status tercatat beserta pengguna dan waktunya; Mentor menerima indikator bahwa laporan perlu direvisi.

### FR-08 — Rekomendasi mentoring lanjutan

- Mentor memilih peserta dari grup yang ditugaskan dan mengisi alasan, evaluasi, kompetensi, kebutuhan, serta catatan tambahan.
- Pengajuan memiliki status Diajukan, Disetujui, Ditolak, atau Perlu evaluasi.
- Tim Mentoring meninjau pengajuan dan dapat mengisi program/grup tujuan serta catatan keputusan.
- Riwayat pengajuan tidak dihapus.

**Kriteria penerimaan:** Mentor tidak dapat memilih peserta dari grup lain; pengajuan yang sudah ditinjau tidak dapat diubah Mentor; keputusan menyimpan tanggal dan peninjau.

### FR-09 — Dashboard dan alert

- Dashboard menampilkan metrik sesuai peran.
- Sistem menandai laporan belum dikirim, terlambat, perlu revisi, dan menunggu persetujuan.
- Analis dapat memfilter berdasarkan periode, program, grup, mentor, status laporan, dan rentang tanggal.
- Alert in-app tersedia untuk pekerjaan yang membutuhkan tindakan.

**Kriteria penerimaan:** filter memperbarui jumlah dan daftar data secara konsisten; laporan terlambat ditentukan dari batas waktu yang berlaku untuk program/periode.

### FR-10 — Ekspor dan audit

- Admin dan Tim Mentoring dapat mengekspor data sesuai hak akses ke CSV atau Excel.
- Ekspor menyertakan filter yang digunakan dan waktu ekspor.
- Sistem menyimpan riwayat perubahan data penting: akun, peserta, keanggotaan, jadwal, laporan, persetujuan, dan rekomendasi.

**Kriteria penerimaan:** pengguna tidak dapat mengekspor data di luar kewenangannya; audit log memuat objek, tindakan, pengguna, waktu, dan perubahan utama.

## 7. Alur utama

### 7.1 Mentor mengirim laporan

1. Mentor login dengan email dan kode akses aktif.
2. Mentor memilih grup dan jadwal yang sudah berlangsung.
3. Mentor mengisi status absensi seluruh peserta.
4. Mentor mengisi ringkasan, evaluasi, kendala, dan tindak lanjut.
5. Mentor menyimpan Draft atau mengirim laporan.
6. Sistem memvalidasi kelengkapan, menghitung keterlambatan, dan mengubah status menjadi Dikirim.
7. Tim Mentoring meninjau dan menyetujui atau mengembalikan laporan.

### 7.2 Tim Mentoring menempatkan peserta

1. Tim Mentoring memilih periode, program, dan grup.
2. Tim Mentoring menambahkan peserta yang belum memiliki keanggotaan aktif pada periode tersebut.
3. Sistem mencatat tanggal efektif dan mencegah duplikasi.
4. Jika peserta dipindahkan, sistem menutup keanggotaan lama dan menyimpan riwayatnya.

### 7.3 Rekomendasi mentoring lanjutan

1. Mentor memilih peserta dari grupnya.
2. Mentor mengisi alasan dan evaluasi singkat.
3. Sistem menyimpan pengajuan sebagai Diajukan.
4. Tim Mentoring meninjau dan memilih keputusan.
5. Sistem menyimpan keputusan, catatan, peninjau, tanggal, dan grup/program tujuan jika disetujui.

## 8. Aturan bisnis

- Satu Mentor dapat ditugaskan ke beberapa grup.
- Satu grup dapat memiliki beberapa Mentor sesuai kebijakan program.
- Peserta tidak boleh memiliki dua keanggotaan aktif pada grup yang sama dan periode yang sama.
- Perpindahan peserta menggunakan tanggal efektif; histori lama tetap dapat dilihat.
- Laporan dibuat dari jadwal yang terdaftar.
- Batas waktu laporan dapat dikonfigurasi per program atau periode.
- Status terlambat dihitung ketika laporan belum Dikirim melewati batas waktu.
- Data yang memiliki riwayat dinonaktifkan atau diarsipkan, bukan dihapus permanen.
- Semua waktu disimpan dalam UTC dan ditampilkan dalam zona waktu operasional yang dikonfigurasi.

## 9. Entitas data utama

`User`, `Role`, `AccessCode`, `Period`, `Program`, `Group`, `Participant`, `GroupMembership`, `MentorAssignment`, `MeetingSchedule`, `Material`, `MeetingReport`, `Attendance`, `Evaluation`, `Recommendation`, `Notification`, dan `AuditLog`.

Relasi penting:

- `Period` memiliki banyak `Program` dan `Group`.
- `Group` memiliki banyak `Participant` melalui `GroupMembership`.
- `Group` memiliki satu atau beberapa `Mentor` melalui `MentorAssignment`.
- `MeetingSchedule` memiliki satu `MeetingReport` aktif.
- `MeetingReport` memiliki banyak `Attendance` dan satu riwayat status.
- `Recommendation` merujuk pada `Participant`, `Mentor`, dan grup asal.

## 10. Persyaratan nonfungsional

- **Keamanan:** kata sandi dan kode akses tidak disimpan dalam bentuk terbuka; akses berbasis peran; sesi berakhir setelah periode tidak aktif yang dikonfigurasi.
- **Privasi:** data peserta hanya tampil untuk pengguna berwenang; ekspor mengikuti hak akses; audit log tidak dapat diedit pengguna biasa.
- **Kinerja:** halaman dashboard dan daftar utama menampilkan hasil awal dalam ≤3 detik pada beban pilot yang disepakati.
- **Ketersediaan:** target uptime MVP minimal 99% per bulan, di luar jadwal pemeliharaan.
- **Aksesibilitas:** form dapat digunakan dengan keyboard, label input jelas, dan status tidak hanya dibedakan dengan warna.
- **Responsif:** alur inti Mentor dapat digunakan pada layar laptop dan ponsel.
- **Pemulihan:** backup database berkala dan prosedur pemulihan diuji sebelum pilot.

## 11. Rencana rilis

### Fase 1 — Fondasi

Autentikasi, peran, kode akses Mentor, periode, program, grup, dan akun.

### Fase 2 — Operasional mentoring

Peserta, keanggotaan, penugasan Mentor, jadwal, materi, absensi, dan laporan.

### Fase 3 — Pengawasan

Persetujuan, dashboard Analis, alert, audit log, rekomendasi lanjutan, dan ekspor.

### Fase 4 — Pilot dan perbaikan

Pilot pada satu periode/program, pelatihan pengguna, pengumpulan metrik, dan perbaikan sebelum perluasan.

## 12. Risiko dan mitigasi

| Risiko | Dampak | Mitigasi |
| --- | --- | --- |
| Data awal peserta tidak konsisten | Duplikasi dan laporan salah | Validasi impor, template standar, dan laporan error sebelum impor |
| Mentor terlambat mengirim laporan | Monitoring tidak akurat | Batas waktu per program, indikator terlambat, dan alert in-app |
| Kode akses dibagikan ke orang lain | Akses tidak sah | Kode sekali terbit, pencabutan cepat, log login, dan kebijakan rotasi |
| Perubahan struktur program setelah laporan berjalan | Riwayat sulit ditelusuri | Arsipkan data dan simpan tanggal efektif, jangan menghapus permanen |
| Adopsi rendah | Data tetap tersebar | Pilot terbatas, panduan singkat, dan pantau rasio laporan digital |

## 13. Pertanyaan yang perlu divalidasi

1. Apakah akun Admin, Tim Mentoring, dan Analis memakai login kata sandi yang sama atau penyedia identitas organisasi?
2. Apakah satu laporan dibuat per jadwal, atau Mentor boleh membuat laporan ad-hoc?
3. Berapa lama data dan lampiran harus disimpan?
4. Batas waktu laporan dihitung berdasarkan zona waktu mana?
5. Apakah persetujuan laporan memerlukan satu peninjau atau lebih dari satu?
6. Format ekspor apa yang wajib tersedia pada pilot?
7. Apakah notifikasi email diperlukan setelah MVP?

## 14. Definisi selesai MVP

MVP siap dipilotkan jika seluruh FR-01 sampai FR-10 berjalan pada lingkungan uji, setiap kriteria penerimaan telah diverifikasi, data contoh berhasil diimpor, hak akses diuji untuk empat peran, ekspor menghasilkan data yang benar, dan prosedur backup/pemulihan telah diuji.
