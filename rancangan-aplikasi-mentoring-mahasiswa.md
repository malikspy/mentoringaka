# Rancangan Aplikasi Web Mentoring Mahasiswa

## 1. Tujuan Sistem

Aplikasi digunakan untuk mengelola program mentoring mahasiswa, mulai dari akun pengelola, peserta, grup, jadwal, materi, absensi, laporan mentor, evaluasi, hingga pemantauan oleh analis.

Peserta tidak perlu memiliki akun. Seluruh data peserta dikelola oleh admin, tim mentoring, dan mentor sesuai kewenangan.

## 2. Struktur Akses

### Admin

- Mengelola akun tim mentoring, mentor, dan analis.
- Mengelola data peserta.
- Mengelola periode, program, grup, dan master data.
- Melihat dan mengekspor seluruh laporan.
- Mengaktifkan, menonaktifkan, mengedit, dan mengarsipkan data.
- Menerbitkan, memperpanjang, mencabut, dan membuat ulang kode akses mentor.

### Tim Mentoring

- Membuat dan mengelola grup.
- Menambahkan mentor ke grup.
- Menambahkan, mengedit, dan memindahkan peserta.
- Membuat jadwal pertemuan.
- Mengunggah materi.
- Memeriksa dan menyetujui laporan mentor.
- Melihat rekap absensi dan kegiatan.
- Meninjau rekomendasi peserta untuk mentoring lanjutan.

### Mentor

- Login menggunakan email dan kode akses.
- Melihat grup yang ditugaskan.
- Melihat peserta dalam grup.
- Melihat jadwal dan materi.
- Mengisi laporan setiap pertemuan.
- Mengisi absensi peserta.
- Mengisi keterangan, evaluasi, kendala, dan tindak lanjut.
- Mengajukan peserta yang siap mengikuti mentoring lanjutan.

### Analis

- Memantau seluruh grup dan aktivitas.
- Melihat laporan yang sudah atau belum dikirim.
- Melihat laporan terlambat.
- Memantau kehadiran dan evaluasi.
- Menerima alert untuk grup atau mentor yang belum membuat laporan.

## 3. Data Peserta

Data peserta minimal mencakup:

- Nomor peserta atau NIM.
- Nama lengkap.
- Program atau jurusan.
- Angkatan.
- Nomor telepon bila dibutuhkan.
- Grup mentoring.
- Status aktif atau tidak aktif.
- Catatan khusus.

Peserta dapat dipindahkan antargrup tanpa membuat akun baru. Riwayat absensi, evaluasi, perpindahan grup, dan program tetap tersimpan berdasarkan identitas peserta.

## 4. Absensi dan Laporan Pertemuan

Pada setiap laporan, mentor memilih status kehadiran setiap peserta dan menambahkan catatan bila diperlukan.

Status absensi yang disarankan:

- Hadir.
- Izin.
- Sakit.
- Alpa.

Formulir laporan mencakup:

- Tanggal dan topik pertemuan.
- Ringkasan kegiatan.
- Absensi peserta.
- Catatan peserta.
- Evaluasi umum.
- Kendala.
- Tindak lanjut.
- Lampiran bila diperlukan.

Status laporan mengikuti alur berikut:

`Draft → Dikirim → Perlu revisi → Disetujui`

Sistem menyimpan waktu pengiriman, waktu persetujuan, pengguna yang menyetujui, catatan revisi, dan status keterlambatan.

## 5. Login Mentor dan Kode Akses

- Mentor login menggunakan email dan kode akses.
- Kode akses aktif selama satu tahun sejak tanggal diterbitkan.
- Tidak ada verifikasi email atau OTP.
- Setelah kedaluwarsa, mentor tidak dapat login sampai admin menerbitkan atau memperpanjang kode baru.
- Admin dapat menonaktifkan akun, mencabut kode, atau membuat kode akses baru.
- Sistem mencatat tanggal penerbitan, tanggal kedaluwarsa, status kode, dan waktu login terakhir.
- Kode akses disimpan secara aman dan tidak ditampilkan dalam bentuk terbuka setelah disimpan.

Status kode akses:

- Aktif.
- Akan berakhir.
- Kedaluwarsa.
- Dicabut.

## 6. Rekomendasi Mentoring Lanjutan

Mentor dapat mengajukan peserta yang dinilai siap mengikuti mentoring lanjutan.

Alurnya:

1. Mentor memilih peserta dari grup yang ditugaskan.
2. Mentor mengisi alasan rekomendasi.
3. Mentor menambahkan evaluasi singkat dan kompetensi yang sudah dikuasai.
4. Mentor menuliskan kebutuhan mentoring lanjutan dan catatan tambahan.
5. Sistem menyimpan pengajuan dengan status `Diajukan`.
6. Tim mentoring meninjau pengajuan.
7. Tim mentoring menetapkan status `Disetujui`, `Ditolak`, atau `Perlu evaluasi`.
8. Peserta yang disetujui dapat ditempatkan ke program atau grup mentoring lanjutan.

Mentor hanya dapat merekomendasikan peserta dari grupnya. Mentor dapat mengubah pengajuan selama belum ditinjau. Riwayat rekomendasi tetap tersimpan.

Data rekomendasi mencakup peserta, mentor pengusul, grup asal, program lanjutan, tanggal pengajuan, alasan, evaluasi, status, catatan peninjau, grup tujuan, dan tanggal persetujuan.

## 7. Entitas Data Utama

- Pengguna.
- Peran.
- Peserta.
- Periode.
- Program.
- Grup.
- Keanggotaan peserta dalam grup.
- Penugasan mentor.
- Jadwal pertemuan.
- Materi.
- Laporan pertemuan.
- Absensi peserta.
- Evaluasi.
- Rekomendasi mentoring lanjutan.
- Notifikasi atau alert.
- Riwayat perubahan data.

## 8. Dashboard

### Admin

- Jumlah akun aktif.
- Peserta aktif.
- Grup dan mentor.
- Laporan berdasarkan status.
- Rekap periode dan program.

### Tim Mentoring

- Grup yang dikelola.
- Jadwal mendatang.
- Laporan menunggu persetujuan.
- Rekap kehadiran.
- Rekomendasi mentoring lanjutan.

### Mentor

- Grup yang ditugaskan.
- Jadwal terdekat.
- Laporan draft.
- Laporan yang perlu direvisi.
- Peserta yang dapat direkomendasikan untuk mentoring lanjutan.

### Analis

- Grup tanpa aktivitas.
- Laporan belum dikirim.
- Laporan terlambat.
- Mentor dengan laporan tertunda.
- Tren kehadiran dan evaluasi.

## 9. Aturan Bisnis

- Mentor dapat menangani beberapa grup.
- Satu grup dapat memiliki satu atau beberapa mentor sesuai kebijakan program.
- Peserta tidak boleh terdaftar ganda pada grup yang sama dalam periode yang sama.
- Perpindahan peserta menggunakan tanggal efektif.
- Laporan dibuat berdasarkan jadwal yang terdaftar.
- Batas waktu laporan dapat dikonfigurasi per program atau periode.
- Alert dikirim berdasarkan tenggat yang ditentukan.
- Data yang memiliki riwayat sebaiknya dinonaktifkan atau diarsipkan agar riwayat tetap tersedia.

## 10. Prioritas MVP

1. Login dan hak akses.
2. Pengelolaan periode, program, grup, mentor, dan peserta.
3. Penugasan mentor dan peserta.
4. Jadwal pertemuan.
5. Form laporan dan absensi.
6. Persetujuan laporan.
7. Rekomendasi mentoring lanjutan.
8. Dashboard monitoring.
9. Ekspor laporan ke Excel atau CSV.
