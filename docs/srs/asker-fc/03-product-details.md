# Product Details — Asker FC

- Versi: 2.5 · Tanggal: 12 November 2026
- Status: mekanika v2.5 menetapkan Spider sebagai satu-satunya jenis grafik pada Event/Profil; rumus dan nominal tetap, konten autentik/validasi produksi masih terbuka.
- Upstream: [PRD v2.5](./02-prd.md), [Product Planning v2.5](./01-product-planning.md), [rencana v2.11](./00-implementation-plan.md), pelaksanaan persetujuan v2.7 tanggal 11 November 2026. ID lama dipertahankan; FLOW-22–26 baru untuk penilaian.
- Batas: perilaku pengguna dan aturan domain; tidak merancang API, skema database atau kode.
- Asumsi: komunitas tunggal, pengguna 18+, satu mode pendaftaran per kegiatan, koneksi internet. Nilai batas formulir di bawah adalah **usulan spesifikasi**, bukan aturan klub yang sudah ada.

## 1. Istilah dan aturan dasar

**Konsep produk: Kick & Kumpul**, website komunitas fun football Asker FC. Alur penemuan sesi, pendaftaran, tim dan jadwal membantu anggota datang bermain serta berkumpul. Liga dan pencatatan hasil tetap mendukung variasi kegiatan komunitas. Penegasan ini mempertahankan semua aturan dan alur yang telah disepakati.

**Tujuan anggota:** mudah tahu jadwal main, daftar dan bayar, sekaligus membangun rasa kebersamaan tim. Hubungan alur: FLOW-03/FLOW-06/FLOW-12 membantu mengetahui kegiatan dan jadwal; FLOW-04 mengonfirmasi keikutsertaan; FLOW-11 menangani transfer manual, bukti serta status pada kegiatan berbayar; FLOW-04/FLOW-05/FLOW-09 mendukung pengenalan rekan dan keterlibatan bermain. Urutan pembayaran terhadap pendaftaran mengikuti kebijakan kegiatan yang sudah ditetapkan, tanpa memaksakan tagihan pada kegiatan gratis.

- **Kegiatan:** liga atau satu sesi rutin, mempunyai zona waktu, slot lapangan, pendaftaran dan roster. Rutin tidak memiliki klasemen liga; pertandingan persahabatan tetap dapat menghasilkan statistik dasar.
- **Anggota aktif:** identitas terverifikasi, profil wajib lengkap, deklarasi 18+ dan akun tidak dinonaktifkan. Deklarasi usia bukan verifikasi identitas penuh.
- **Peserta terdaftar:** individu yang konfirmasinya berhasil; pada mode tim harus ada persetujuan anggota dan penerimaan roster tim. Undangan saja belum mengonsumsi tempat pemain terkonfirmasi.
- **Roster:** pemain untuk satu tim pada satu kegiatan. Satu orang tidak boleh berada di dua roster terkonfirmasi pada kegiatan yang sama.
- **Hasil resmi:** versi hasil yang difinalkan pengelola, bukan draft pencatat. Koreksi membuat versi resmi baru dengan riwayat lama tetap ada.
- **Status UI:** loading, error, empty dan saving tidak mengganti status bisnis.
- **Zona waktu:** usulan default kegiatan Asia/Jakarta; pengelola dapat memilih sebelum publikasi. Semua layar menampilkan label zona, waktu audit dapat ditampilkan sesuai zona kegiatan. Tidak mengikuti zona komputer secara diam-diam.
- **Identitas visual:** [logo Asker FC pengguna](./assets/logo-asker-fc.png) dipakai tanpa modifikasi, dengan UI hitam dan gold dominan sesuai instruksi 10 November 2026, tema gelap default dan tema terang netral, judul Poppins Bold serta copy akrab pada Mockup SRS v2.5. Perubahan warna tidak menyatukan arti status: “diajukan”, “final”, “diterima” dan “belum terkunci” tetap mempunyai label/ikon eksplisit.

## 2. Matriks kewenangan

“Pengelola” di bawah mencakup superadmin atau admin yang ditugaskan pada kegiatan itu. Penugasan kapten/pencatat hanya dapat dibuat oleh pengelola; hanya superadmin yang memberi peran pengelola kegiatan.

| Tindakan | Anggota | Kapten | Pencatat | Pengelola | Superadmin |
|---|---|---|---|---|---|
| Profil sendiri dan pendaftaran pribadi | Ya | Ya | Ya | Ya | Ya |
| Melihat kegiatan terbit/jadwal/hasil internal | Anggota aktif | Sama | Sama | Termasuk draft kegiatan tugasnya | Semua |
| Undang/ubah roster sebelum terkunci | Persetujuan diri | Tim tugasnya | Tidak karena peran pencatat | Kegiatan tugasnya | Semua |
| Konfigurasi/publikasi, pembagian dan jadwal | Tidak | Tidak | Tidak | Kegiatan tugasnya | Semua |
| Kirim skor/partisipasi laga | Tidak | Tidak karena peran kapten | Laga tugasnya | Kegiatan tugasnya | Semua |
| Finalisasi/koreksi hasil dan roster terkunci | Tidak | Tidak | Tidak karena peran pencatat | Kegiatan tugasnya, alasan wajib untuk koreksi | Semua |
| Presensi kegiatan | Lihat diri | Lihat tim | Lihat roster laga | Catat/koreksi kegiatan tugasnya | Semua |
| Bukti transfer | Tagihan pribadi | Tagihan timnya | Tidak karena peran pencatat | Kegiatan tugasnya | Semua |
| Anggota/peran pengelola | Tidak | Tidak | Tidak | Lihat peserta yang diperlukan | Kelola |
| Audit | Riwayat keputusan diri yang relevan | Tim/tagihan sendiri yang relevan | Status kiriman sendiri | Riwayat operasional kegiatan tugasnya | Semua |

Hak baca data kontak tidak otomatis mengikuti hak lihat roster. Tampilan roster menggunakan nama tampilan, bukan email/telepon. Akun dengan dua peran mendapat gabungan hak untuk objek yang ditugaskan, bukan akses global. Bila petugas juga pengelola, ia boleh memfinalkan hasilnya sendiri; sistem mencatat kedua tindakan. Pemisahan dua orang tidak dijanjikan MVP.

Aturan awal pembuatan kegiatan: superadmin membuat atau menyalin draft lalu menugaskan pengelola. Admin hanya mengelola draft/kegiatan yang sudah ditugaskan; tidak memperoleh akses ke kegiatan baru hanya dengan menyalin ID. Istilah pengelola dalam FLOW-03 mencakup superadmin untuk tindakan membuat/menyalin. Konfigurasi dan publikasi draft yang sudah ditugaskan dapat dilakukan admin terkait.

## 3. Siklus status bisnis

| Entitas/ID state | Asal | Pemicu/aktor | Kondisi | Tujuan | Efek pengguna | Jika gagal |
|---|---|---|---|---|---|---|
| Akun / STATE-01 | Belum ada | Daftar email/anggota | Form valid, 18+, kebijakan disetujui | Menunggu verifikasi | Tautan dikirim; belum boleh daftar kegiatan | Form dipertahankan tanpa sandi |
| Akun / STATE-01 | Menunggu verifikasi | Verifikasi/anggota | Tautan valid, profil lengkap | Aktif | Bisa berpartisipasi | Tetap menunggu; minta tautan ulang |
| Akun / STATE-01 | Belum ada | Masuk Google/anggota | Identitas penyedia valid, profil/usia/kebijakan lengkap | Aktif | Akun yang dapat digunakan | Tidak membuat peserta sampai lengkap |
| Akun / STATE-01 | Aktif | Nonaktifkan/superadmin | Bukan superadmin aktif terakhir, alasan | Nonaktif | Akses dan pendaftaran baru ditolak; riwayat tidak dihapus | Tetap aktif bila syarat tidak terpenuhi |
| Kegiatan / STATE-02 | Draft | Publikasikan/pengelola | Konfigurasi dan aturan wajib valid | Terbit | Detail dibuka menurut cakupan publikasi publik/anggota/kegiatan | Tunjukkan field yang belum lengkap |
| Kegiatan / STATE-02 | Terbit | Mulai/pengelola | Daftar ditutup, roster terkunci, jadwal terbit bila ada laga | Berjalan | Pencatatan lapangan aktif | Sebut prasyarat yang kurang |
| Kegiatan / STATE-02 | Berjalan | Selesaikan/pengelola | Semua laga final/dibatalkan, presensi lengkap atau pengecualian beralasan | Selesai | Ringkasan final kegiatan | Daftar pekerjaan tersisa terlihat |
| Kegiatan / STATE-02 | Draft/Terbit/Berjalan | Batalkan/pengelola | Alasan wajib; dampak ditinjau | Dibatalkan | Daftar baru ditutup; data dan tagihan dipertahankan | Tidak berubah bila dibatalkan pada dialog |
| Periode daftar / STATE-03 | Belum dibuka | Buka/pengelola atau waktu buka | Kegiatan terbit dan dalam rentang | Terbuka | CTA daftar aktif | Tetap belum dibuka |
| Periode daftar / STATE-03 | Terbuka | Tenggat/tutup pengelola | Tenggat lewat atau konfirmasi tutup | Ditutup | Cegah konfirmasi peserta baru | UI diperbarui saat submit |
| Periode daftar / STATE-03 | Ditutup | Buka ulang/pengelola | Belum berjalan, roster belum terkunci; alasan jika jadwal terbit | Terbuka | Kuota aktual ditampilkan | Ditolak bila kegiatan berjalan/batal |
| Individu / STATE-04 | Belum daftar | Konfirmasi/anggota | Terbuka, aktif, unik, kuota tersedia | Terdaftar | Slot terisi tepat sekali | Tidak membuat pendaftaran |
| Individu / STATE-04 | Terdaftar | Batal/anggota | Roster belum terkunci | Dibatalkan | Kuota dilepas; tagihan terpisah | Jika terkunci, hubungi pengelola |
| Undangan / STATE-05 | Belum ada | Undang/kapten | Tim draft, anggota aktif | Menunggu | Anggota melihat undangan | Tidak membuat profil baru |
| Undangan / STATE-05 | Menunggu | Terima/anggota | Tim belum diajukan, belum di roster lain | Disetujui | Pemain kandidat roster, belum peserta diterima | Jelaskan konflik terbaru |
| Undangan / STATE-05 | Menunggu | Tolak/anggota | Milik anggota | Ditolak | Tidak menjadi kandidat | Tetap jika operasi belum diketahui |
| Tim / STATE-06 | Draft | Ajukan/kapten | Jumlah kandidat disetujui memenuhi batas, nama unik | Diajukan | Roster menunggu pemeriksaan | Sorot pemain belum menyetujui |
| Tim / STATE-06 | Diajukan | Terima/pengelola | Periode terbuka, kuota tim dan keunikan anggota tersedia | Diterima | Tim dan pemain menjadi terdaftar | Tetap diajukan; alasan kekurangan/konflik |
| Tim / STATE-06 | Diajukan | Kembalikan/pengelola | Alasan wajib | Draft | Kapten dapat memperbaiki | Tidak ada kuota diterima yang dilepas |
| Tim / STATE-06 | Diterima | Tarik tim/kapten atau pengelola | Sebelum roster terkunci; konfirmasi dampak | Ditarik | Kuota tim/pemain dilepas, biaya tidak otomatis kembali | Sesudah terkunci hanya pengelola dan kebijakan withdrawal |
| Roster / STATE-07 | Belum disusun | Simpan pembagian/pengelola | Setiap peserta unik, kapasitas valid | Draft | Roster tersimpan, dapat direvisi | Pratinjau tidak menimpa data |
| Roster / STATE-07 | Draft | Kunci/pengelola | Pendaftaran ditutup, jumlah tim/pemain valid | Terkunci | Bekukan perubahan mandiri | Daftar pelanggaran terlihat |
| Roster / STATE-07 | Terkunci | Koreksi/pengelola | Alasan dan dampak diperiksa | Terkunci, versi baru | Jadwal/partisipasi yang terdampak ditangani eksplisit | Dilarang memindahkan pemain yang sudah tampil untuk mengubah riwayat |
| Jadwal / STATE-08 | Belum ada/Draft | Susun/edit/pengelola | Roster terkunci, slot valid | Draft versi baru | Pratinjau | Versi tersimpan lama tetap ada |
| Jadwal / STATE-08 | Draft | Terbitkan/pengelola | Tidak ada konflik dan pasangan lengkap liga | Terbit | Anggota melihat versi resmi | Tidak menerbitkan sebagian tanpa label |
| Laga / STATE-09 | Terjadwal | Mulai/pencatat/pengelola | Waktu/kegiatan sesuai, belum batal | Berlangsung | Input hasil aktif | Laga final tidak dibuka sebagai baru |
| Laga / STATE-09 | Berlangsung/Terjadwal | Kirim hasil/pencatat | Skor/waktu selesai valid; terjadwal boleh bila mencatat setelah laga | Diajukan | Menunggu validasi | Data form tetap tersedia |
| Laga / STATE-09 | Diajukan | Kembalikan/pengelola | Alasan wajib | Perlu perbaikan | Petugas dapat mengirim revisi | Hasil belum memengaruhi klasemen |
| Laga / STATE-09 | Diajukan | Finalkan/pengelola | Skor valid, partisipasi/rincian diberi label kelengkapan | Final | Perbarui agregasi satu kali | Status sebelumnya dipertahankan |
| Laga / STATE-09 | Final | Koreksi/pengelola | Alasan, versi terkini, pratinjau dampak | Final versi baru | Ganti versi resmi dan hitung ulang | Tolak penimpaan versi usang |
| Laga / STATE-09 | Terjadwal/Berlangsung | Tunda/pengelola | Alasan wajib | Ditunda | Tidak menjadi 0–0; perlu jadwal baru | Tidak menghapus kiriman hasil |
| Laga / STATE-09 | Ditunda | Jadwal ulang/pengelola | Slot valid | Terjadwal | Versi jadwal diperbarui | Tetap ditunda bila konflik |
| Laga / STATE-09 | Nonfinal | Batalkan/pengelola | Alasan dan aturan kegiatan | Dibatalkan | Dikeluarkan dari pertandingan dimainkan | Hasil final harus dikoreksi dengan audit, bukan dibatalkan diam-diam |
| Bayar / STATE-10 | Belum bayar/Ditolak | Unggah/pengirim | Bukti valid, tagihan aktif | Menunggu verifikasi | Riwayat bukti ditambahkan | Tidak menimpa bukti lama |
| Bayar / STATE-10 | Menunggu verifikasi | Terima/tolak pengelola | Versi terbaru, alasan bila tolak | Terverifikasi/Ditolak | Keputusan terlihat pengirim | Konflik harus dimuat ulang |
| Presensi / STATE-11 | Belum dicatat | Tandai/pengelola | Peserta terdaftar | Hadir/Tidak hadir | Masuk cakupan KPI presensi | Tidak mengubah peserta lain |
| Presensi / STATE-11 | Hadir/Tidak hadir | Koreksi/pengelola | Alasan setelah kegiatan selesai | Status terkoreksi | Statistik direvisi | Jika sudah tampil, ubah partisipasi terkait secara eksplisit sebelum tidak hadir |

Kegiatan selesai tidak dibuka ulang secara otomatis; koreksi terbatas dilakukan pengelola dengan audit. Kegiatan dibatalkan tidak menerima laga/pendaftaran baru. Roster tidak boleh dibuka oleh kapten setelah terkunci. Pembayaran tidak berubah menjadi refund hanya karena peserta membatalkan.

## 4. Alur fitur

Semua alur memeriksa akun, izin dan status terbaru pada aksi. Tombol batal menutup dialog tanpa menyimpan. Kembali dari form dengan perubahan memperlihatkan konfirmasi buang perubahan; sandi tidak dipertahankan. Data yang sudah tersimpan tetap tersedia setelah masuk ulang.

### FLOW-01 — Akun, profil dan pemulihan

FEAT-01 · US-01 · AC-01–AC-04, AC-87–AC-88. Aktor calon anggota/anggota. Pemicu: tautan masuk atau kegiatan. Hasil: anggota aktif atau penolakan yang jelas.

1. Buka SCR-01; tujuan internal semula dipertahankan tanpa menerima redirect ke situs asing.
2. Daftar email pada SCR-02: isi nama, email, sandi, deklarasi 18+ dan persetujuan kebijakan. Pengguna yang tidak memenuhi usia tidak diproses sebagai peserta.
3. Tautan verifikasi membawa ke SCR-03. Email belum datang: kirim ulang dengan jeda usulan 60 detik; jangan mengungkap keberadaan akun pada pesan publik.
4. Masuk Google memerlukan penyelesaian profil/usia/kebijakan di SCR-02 jika belum lengkap. Persetujuan dibatalkan di penyedia: kembali ke SCR-01, tidak membuat peserta.
5. Jika perlu menautkan akun, pengguna masuk akun lama dan menyetujui penautan; jika tidak bisa membuktikannya, tidak digabung otomatis.
6. Lupa sandi SCR-03: tampil pesan netral; tautan reset usulan berlaku 30 menit dan sekali pakai. Sandi baru tidak masuk riwayat tampilan.
7. Setelah masuk/profil lengkap, kembali ke tujuan internal yang masih berhak, atau SCR-04. Profil sendiri dapat diperbarui di SCR-12: unggah foto sah atau gunakan inisial, isi posisi dan nomor roster terkait, pilih cakupan tayang yang diizinkan. Nomor diperiksa pada tim/kegiatan, bukan unik global; kontak tidak ikut direktori. Data kosong ditandai tanpa foto rekaan. Perubahan email memerlukan verifikasi alamat baru dan tidak langsung mengubah identitas aktif.
8. Bila sesi habis saat mengubah data, masuk ulang dan muat status terbaru sebelum kirim; teks nonrahasia yang masih berada dalam sesi halaman boleh dipulihkan, tidak dijanjikan lintas perangkat.

### FLOW-02 — Anggota, peran dan penugasan

FEAT-02 · US-02 · AC-05–AC-07. Aktor superadmin/pengelola sesuai cakupan. Prasyarat akun aktif; hasil penugasan tercatat.

1. Superadmin membuka SCR-21, mencari anggota dan melihat detail yang diperlukan.
2. Pilih aktif/nonaktif atau peran pengelola dengan kegiatan spesifik; tinjau ringkasan lalu simpan.
3. Pengelola kegiatan menggunakan area penugasan SCR-13 untuk menunjuk kapten tim/pencatat laga dari anggota aktif, tanpa memberi dirinya akses komunitas lain.
4. Sistem menolak cakupan kosong untuk peran yang membutuhkannya dan mencegah hilangnya superadmin terakhir.
5. Perubahan langsung berlaku pada permintaan berikutnya. Form yang sebelumnya terbuka tidak memberi izin lama. Riwayat pelaku/waktu terlihat pada SCR-22 sesuai cakupan.
6. Batal mengembalikan detail anggota tanpa perubahan. Tidak ada email/WhatsApp otomatis karena perubahan peran; pengguna melihat tugasnya setelah memuat data terbaru.

### FLOW-03 — Konfigurasi dan siklus kegiatan

FEAT-03 · US-03 · AC-08–AC-11. Aktor pengelola. Titik masuk SCR-13; hasil draft/terbit/selesai/dibatalkan.

1. Pilih Buat atau Salin; SCR-14 menampilkan jenis liga/rutin, satu mode daftar, kuota, batas roster, waktu, zona, lokasi, slot lapangan, aturan dan biaya opsional.
2. Salin hanya konfigurasi; tanggal dan tenggat harus diperiksa ulang. Anggota, hasil dan bukti bayar kosong.
3. Simpan draft tidak memublikasikan. Publikasi menampilkan ringkasan aturan dan field wajib yang kurang. Kebijakan juara/walkover/withdrawal harus terisi pada liga.
4. Konfigurasi poin dan urutan peringkat dibekukan ketika liga mulai. Mode daftar tidak dapat diubah setelah ada peserta terkonfirmasi. Perubahan kapasitas tidak boleh di bawah jumlah terdaftar.
5. Kegiatan terbit muncul di SCR-04/05; pengelola menutup daftar, mengunci roster dan menerbitkan jadwal sebelum Mulai.
6. Selesaikan menunjukkan hasil/presensi belum beres. Pengecualian presensi diberi alasan dan tetap “belum dicatat”, tidak dipalsukan menjadi tidak hadir.
7. Batalkan memerlukan alasan dan ringkasan peserta/tagihan terdampak. Riwayat dipertahankan; pekerjaan pengingat belum terkirim dibatalkan melalui FLOW-15. Refund tetap manual dan komunikasi tambahan dilakukan petugas sesuai kebutuhan.

### FLOW-04 — Pendaftaran individu dan antartim

FEAT-04 · US-04 · AC-12–AC-15. Aktor anggota/kapten/pengelola. Prasyarat kegiatan terbit dan periode terbuka. Hasil peserta unik atau penolakan.

1. SCR-05 sudah menampilkan mode, kuota, biaya dan aturan. Mode individu yang memenuhi syarat memakai Gabung Main langsung pada SCR-05 dalam satu aksi; status tersimpan terlihat di sana. SCR-06 menjadi detail status/batal/undangan, bukan konfirmasi wajib kedua. Jika belum masuk, kembali ke SCR-05 setelah login sebelum memilih aksi RSVP secara sadar.
2. Mode individu: RSVP sendiri satu klik setelah login dan prasyarat terpenuhi. Kuota diperiksa saat penyimpanan; dua orang memperebutkan slot terakhir hanya satu berhasil. Submit ulang dari orang yang sama menampilkan pendaftaran awal.
3. Mode tim: kapten membuat tim di SCR-07 dan mengundang anggota terdaftar. Teks undangan tampil di SCR-06 anggota; undangan tidak mengirim email/WhatsApp otomatis.
4. Anggota menerima/menolak sendiri. Beberapa undangan boleh menunggu, tetapi hanya satu persetujuan roster aktif pada kegiatan yang sama diperbolehkan. Sebelum tim diterima, anggota dapat menarik persetujuan dan kapten harus memperbaiki roster.
5. Kapten mengajukan roster yang seluruhnya disetujui. Pengelola memeriksa lewat SCR-15 dan menerima jika kuota tim, batas roster dan keunikan masih valid. Penolakan dikembalikan ke draft dengan alasan.
6. Setelah tim diterima, perubahan roster sebelum terkunci harus diajukan ulang; versi lama tetap resmi sampai revisi diterima. Pemain pengganti wajib menyetujui; pemain yang keluar tidak dihapus dari riwayat.
7. Individu membatalkan melalui SCR-06; kapten menarik tim sebelum terkunci melalui SCR-07. Dialog memisahkan akibat kuota dan biaya. Setelah terkunci, tampil kontak pengelola untuk permintaan manual, bukan perubahan otomatis.
8. Kuota penuh atau tenggat lewat: CTA ditolak dengan alasan terkini; tidak membuat waiting list. Kembali ke SCR-05 mempertahankan status peserta yang telah tersimpan.

### FLOW-05 — Pembagian tim dan roster terkunci

FEAT-05 · US-05 · AC-16–AC-18. Aktor pengelola. Prasyarat daftar ditutup; hasil roster terkunci.

1. Buka SCR-16 dari SCR-13/15. Pada mode individu, pilih jumlah tim yang layak berdasarkan peserta, min/maks roster; pada mode tim, gunakan tim diterima tanpa pembagian acak.
2. Pilih mode Acak atau Seimbang berdasarkan skill. Mode acak tidak mengklaim keseimbangan kemampuan; mode skill memakai FLOW-14. Susun pratinjau: setiap peserta terdaftar tepat satu kali, beda jumlah maksimal satu dan tidak melebihi batas. Bila peserta tidak cukup, jelaskan jumlah minimal dan kembali ke konfigurasi/pendaftaran tanpa menyimpan.
3. Pindah pemain menggunakan tombol “Pindahkan ke tim” selain drag jika disediakan. Acak ulang hanya mengubah pratinjau. Batal mengembalikan roster tersimpan.
4. Simpan draft lalu Kunci dengan ringkasan kapasitas dan peserta. Roster terkunci menjadi dasar jadwal.
5. Koreksi setelah terbit memerlukan alasan dan tinjauan jadwal/partisipasi terdampak. Pemain yang sudah tampil tidak dipindahkan secara retroaktif; koreksi data historis hanya lewat alur koreksi hasil/partisipasi.

### FLOW-06 — Jadwal liga dan rutin

FEAT-06 · US-06 · AC-19–AC-21. Aktor pengelola; pembaca anggota. Prasyarat roster terkunci, slot terdefinisi.

1. Buka SCR-17. Liga membentuk pasangan satu kali; n=4 menghasilkan enam laga, n=5 menghasilkan sepuluh laga dengan jeda tim ganjil, bukan tim fiktif.
2. Durasi total slot mencakup waktu pertandingan dan buffer antarlaga. Istirahat minimum dihitung dari akhir pertandingan sebelumnya ke mulai laga berikutnya milik tim itu. Batas hari/slot dan lapangan wajib dihormati.
3. Sesi rutin menambah pasangan persahabatan secara manual; tidak memaksakan semua pasangan dan tidak menghasilkan klasemen liga.
4. Tinjau konflik; jadwal tidak layak menampilkan pasangan yang belum mendapat slot. Sistem tidak menerbitkan jadwal parsial sebagai lengkap. Ubah slot/lapangan/durasi atau susun manual dengan pemeriksaan yang sama.
5. Publikasikan versi valid ke SCR-08. Revisi mempertahankan versi lama sampai pengganti disahkan; hasil final tidak dihapus/direset.
6. Penundaan diberi alasan, slot baru dipilih dan diperiksa. Pekerjaan pengingat yang belum dikirim diperbarui/dibatalkan melalui FLOW-15. Bagikan versi terbaru melalui FLOW-12 bila diperlukan. Anggota melihat waktu pembaruan dan zona kegiatan.

### FLOW-07 — Hasil, pengembalian dan koreksi

FEAT-07 · US-07 · AC-22–AC-25. Aktor pencatat ditugaskan/pengelola. Hasil hasil final berjejak.

1. Pilih laga pada SCR-08 atau tugas SCR-13, lihat SCR-09, lalu SCR-10. Laga lain tidak dapat diedit oleh pencatat tanpa penugasan.
2. Catat skor kedua tim, waktu selesai aktual, pemain tampil dan rincian gol bila tersedia. Usulan skor per tim 0–99, bilangan bulat; waktu aktual tidak boleh setelah waktu saat pencatatan dan tidak sebelum mulai aktual bila dicatat.
3. Gol per pemain hanya untuk roster tim bersangkutan yang dicatat tampil; gol bunuh diri menambah skor lawan tetapi bukan gol pemain lawan. Jumlah rincian gol tidak boleh melebihi skor; jika kurang, tampil “Rincian gol belum lengkap”.
4. Kirim hasil menjadi Diajukan. Petugas boleh menyimpan draft form terlebih dahulu, tetapi draft lokal bukan hasil resmi dan tidak dijanjikan lintas perangkat. Navigasi keluar memperingatkan perubahan belum terkirim.
5. Pengelola meninjau SCR-18 dan memilih Kembalikan dengan alasan atau Finalkan. Finalisasi menampilkan ringkasan skor/kelengkapan/partisipasi. Rincian gol boleh belum lengkap; penampilan yang belum dicatat diberi penanda, bukan diasumsikan semua pemain tampil.
6. Koreksi final: muat versi terkini, isi nilai baru/alasan, tinjau dampak, konfirmasi. Klasemen dan statistik menggunakan versi baru tanpa menambah pertandingan ganda. Bila nilai berubah bersamaan, bandingkan dan muat ulang; tidak otomatis menggabungkan skor.
7. Usulan walkover liga: pengelola mencatat hasil administratif 3–0 menurut kebijakan yang ditetapkan sebelum publikasi; poin dihitung, gol pemain/penampilan tidak diciptakan. Kedua tim tidak hadir: default usulan batal tanpa poin, dengan alasan. Nilai/aturan dapat ditetapkan lain sebelum liga, tidak diubah ad hoc.
8. Usulan withdrawal setelah liga mulai: hasil final lama dipertahankan; laga tersisa diproses satu per satu sesuai kebijakan administratif yang sudah diumumkan. Tidak menghapus tim dan seluruh hasil secara massal.

### FLOW-08 — Klasemen

FEAT-08 · US-08 · AC-26–AC-27. Aktor anggota aktif. Pemicu SCR-05 → SCR-11.

1. Buka liga; tampil jumlah laga final dibanding jadwal dan waktu pembaruan.
2. M/Mn/S/K, GM/GK/SG dan poin berasal hanya dari versi hasil final, termasuk administratif sesuai aturan kegiatan. Satu laga biasa menambah main kedua tim satu kali; poin 3/1/0 adalah baseline diterima.
3. Urut poin, selisih gol, gol memasukkan; sama seluruhnya tampil peringkat bersama (contoh 1,1,3), urutan nama hanya untuk tampilan dan tidak menjadi tiebreak.
4. Usulan default juara bersama bila seluruh kriteria tetap sama. Jika klub memilih aturan lain, harus tertulis sebelum kegiatan; tidak menambah pertandingan playoff dalam MVP secara diam-diam.
5. Pilih tim untuk roster SCR-07 atau hasil SCR-09. Tanpa hasil final tampil “Belum ada hasil final” dan tabel nol berlabel sementara. Kegiatan rutin menyembunyikan tab klasemen, bukan menampilkan klasemen palsu.

### FLOW-09 — Presensi dan partisipasi

FEAT-09 · US-09 · AC-28–AC-30. Aktor pengelola untuk presensi; pencatat ditugaskan untuk partisipasi laga.

1. Pengelola membuka SCR-19, memfilter tim dan menandai hadir/tidak hadir. Default “belum dicatat”. Simpan memperlihatkan jumlah berhasil dan bila ada kegagalan parsial menyebut peserta yang belum berubah.
2. Anggota melihat presensi diri di SCR-12; kapten hanya membaca timnya. Tidak ada self check-in atau QR pada MVP.
3. Pada SCR-10 pilih pemain tampil dari roster yang hadir. Jika belum dicatat/tidak hadir, arahkan pengelola ke SCR-19; pencatat tidak dapat mengubah presensi hanya karena peran pencatat.
4. Partisipasi satu pemain satu kali per laga; tidak mencatat menit bermain/pergantian secara rinci pada MVP. Tautan pencetak gol harus konsisten dengan pemain tampil.
5. Koreksi setelah selesai memerlukan alasan, memperlihatkan dampak dan menjaga riwayat. Jika hendak menandai tidak hadir pemain yang sudah tampil, selesaikan koreksi partisipasi/gol terkait lebih dahulu atau tolak dengan penjelasan.

### FLOW-10 — Rekap faktual dan akses penilaian

FEAT-10 · US-10 · AC-31–AC-32, AC-89–AC-90. Aktor anggota atau pembaca profil sesuai izin; statistik rinci tetap pemilik/petugas berhak.

1. SCR-12 membuka rekap kehadiran, penampilan, Gol/Assist dari hasil sah. Kehadiran per kegiatan, penampilan per laga tampil final nonadministratif; filter tanggal memakai waktu kegiatan, bukan waktu input.
2. Nol terkonfirmasi berbeda dari belum diisi; tampilkan jumlah laga lengkap. Gol bunuh diri tidak dikreditkan pada penyerang. Koreksi hasil/partisipasi memperbarui rekap tanpa penampilan ganda.
3. Pilihan Event menuju FLOW-22 dan Profil menuju FLOW-23. Tiga besaran berlabel Poin mentah event, Nilai event relatif dan Profil akumulasi tidak disatukan dalam satu angka. Filter rekap Gol/top skor tidak menghapus histori dari profil akumulasi.
4. SCR-24 tetap direktori/top skor dan badge voting terbit; Statistik Saya membuka subview privat SCR-12 dalam konteks Tim. Apresiasi ke SCR-25, hasil sumber ke SCR-09; belum ada pemenang tidak dibuat sebagai nol prestasi.

### FLOW-11 — Tagihan, bukti dan verifikasi

FEAT-11 · US-11 · AC-33–AC-36. Aktor peserta individu/kapten, pengelola. Prasyarat tagihan biaya kegiatan atau iuran bulanan aktif dan dimiliki pengirim; kewenangan verifikasi mengikuti jenis/cakupan tagihan.

1. SCR-28 (Kas & Iuran) atau SCR-06/07/05 membawa pengirim ke SCR-20 untuk melihat nominal, tujuan transfer, tenggat dan kebijakan. Basis tagihan per individu/per tim sesuai mode; anggota tim selain kapten tidak memperoleh bukti tim.
2. Transfer dilakukan di luar sistem. Pengirim mengunggah bukti; sistem menampilkan pratinjau/konfirmasi lalu status menunggu verifikasi. Hanya satu bukti aktif menunggu per tagihan; riwayat versi tetap tersimpan.
3. SCR-20 memiliki tampilan pengelola: antrean → detail bukti → terima/tolak. Pengelola wajib memeriksa mutasi bank secara manual; gambar bukti bukan konfirmasi bank.
4. Penolakan wajib alasan dan memungkinkan penggantian berkas. Pengiriman ganda/keputusan bersamaan tidak menghasilkan dua keputusan aktif. Bukti yang sudah terverifikasi tidak dapat diganti pengirim; koreksi keputusan hanya pengelola dengan alasan.
5. Default usulan kebijakan MVP adalah **pengingat saja**, tidak membatalkan otomatis saat tenggat. Bila pengelola menetapkan wajib lunas sebelum roster dikunci, penguncian menolak tagihan belum verifikasi dan menyebut peserta/tim terkait. Kebijakan dipilih saat konfigurasi dan ditampilkan sebelum daftar.
6. Pembatalan kegiatan/peserta memberi label tindak lanjut biaya; uang tidak dipindahkan oleh sistem. Pengelola dapat mencatat refund manual beserta referensi/catatan, bukan mengklaim transaksi bank sukses.
7. Kembali ke SCR-28 atau SCR-06/07 mempertahankan status tagihan. Verifikasi iuran bulanan diteruskan ke FLOW-18 untuk satu pemasukan, bukan dicatat manual kedua kali. Bukti privat tidak dimasukkan ke pesan WhatsApp dan tidak dapat dibuka melalui tautan tanpa otorisasi.

### FLOW-12 — Bagikan tautan resmi

FEAT-12 · US-12 · AC-37–AC-38. Aktor anggota aktif/pengelola; prasyarat kegiatan terbit.

1. Pilih Bagikan di SCR-05/08; pratinjau memuat nama, waktu/zona dan tautan kegiatan internal.
2. Pilih Buka WhatsApp; aplikasi/WhatsApp Web dibuka dengan pesan siap isi. Pengguna memilih penerima dan mengirim sendiri. Batal di WhatsApp tidak mengubah kegiatan.
3. Jika tidak dapat membuka kanal, pilih Salin tautan/pesan. Jika salin gagal, tampil teks yang dapat dipilih manual.
4. Penerima mengikuti tautan, masuk bila perlu dan kembali ke tujuan setelah pemeriksaan izin. Link tidak memberi izin tambahan; konten publik boleh dibaca tanpa masuk sesuai cakupannya. Berbagi manual tidak mempunyai status terkirim/dibaca di sistem; pengingat otomatis memakai status terpisah FLOW-15.

## 5. Validasi formulir

Angka berikut usulan batas UX awal yang dapat dikoreksi sebelum implementasi tanpa mengubah tujuan fitur. Validasi terjadi ketika field ditinggalkan untuk umpan balik dan kembali saat simpan untuk kebenaran kondisi terkini.

| Field | Required/kondisional | Format/batas usulan | Keunikan/lintas field | Waktu cek | Pesan konkret | Pemulihan |
|---|---|---|---|---|---|---|
| Nama tampilan | Wajib profil | 2–80 karakter setelah trim | Tidak dipakai sebagai identitas unik | Isi/simpan | “Isi nama 2–80 karakter.” | Perbaiki nama |
| Email | Wajib email login | Format email, ≤254 karakter | Identitas email terverifikasi unik; tidak menautkan akun hanya dari kemiripan | Simpan/verifikasi | “Periksa alamat email atau gunakan pemulihan akun.” | Koreksi/masuk/pulihkan |
| Sandi | Daftar/reset email | Usulan ≥12 dan ≤128 karakter; izinkan pengelola sandi/paste | Konfirmasi sama; tidak ditampilkan ulang | Simpan | “Gunakan sandi minimal 12 karakter dan cocokkan konfirmasi.” | Isi ulang |
| Usia/kebijakan | Wajib aktivasi | Deklarasi 18+ dan persetujuan terpisah, tidak dicentang awal | Simpan waktu/versi kebijakan; tanggal lahir tidak dikumpulkan default | Aktivasi | “MVP hanya menerima peserta usia 18 tahun ke atas.” | Hubungi pengelola jika ada kesalahan |
| Nama kegiatan/tim | Wajib | Kegiatan 3–120, tim 2–60 karakter | Tim unik dalam kegiatan tanpa membedakan huruf besar | Simpan | “Nama tim sudah digunakan pada kegiatan ini.” | Pilih nama berbeda |
| Waktu pendaftaran | Wajib sebelum publikasi | Buka < tutup ≤ awal kegiatan, zona eksplisit | Koreksi tenggat tidak berlaku mundur menghapus peserta | Publikasi/daftar | “Tutup pendaftaran harus setelah buka dan sebelum kegiatan.” | Perbaiki waktu |
| Tim/roster | Wajib sebelum kunci | Usulan 2–32 tim; pemain minimal 1 dan maksimal 30 per tim, min ≤ maks | Angka maksimal adalah batas dukungan awal yang perlu divalidasi; anggota unik, jumlah aktual dalam batas | Konfirmasi/kunci | “Tim ini memiliki 9 pemain; batas kegiatan 10–12.” | Tambah/kurangi peserta atau ubah konfigurasi sebelum terkunci |
| Durasi/buffer/istirahat | Wajib jadwal | Durasi usulan 1–180 menit; buffer/istirahat 0–180 menit | Seluruh laga harus masuk slot dan memenuhi istirahat | Susun/terbit | “Tim Biru belum mendapat waktu istirahat minimum 10 menit.” | Ubah slot/urutan |
| Lapangan dan slot | Wajib jadwal | Nama 1–80; mulai < selesai, lintas hari diberi tanggal | Lapangan sama tidak boleh overlap; slot berzona | Susun/terbit | “Lapangan A sudah digunakan pukul 08.00–08.20.” | Pilih slot lain |
| Skor/waktu selesai | Wajib kirim hasil | Skor bulat 0–99; waktu selesai ≤ waktu saat ini | Tidak bertentangan dengan mulai aktual bila dicatat | Kirim/final | “Skor harus bilangan bulat 0–99.” | Koreksi skor/waktu |
| Gol dan partisipasi | Kondisional rincian | Pemain roster tampil; jumlah gol ≥0 | Jumlah rincian ≤ skor; gol bunuh diri dikreditkan ke skor lawan | Kirim/final | “Rincian gol melebihi skor tim.” | Sesuaikan skor/rincian |
| Biaya dan tenggat | Jika berbayar | Usulan IDR bulat 1–100.000.000; gratis=0 | Satu tarif/basis; tagihan lama tidak berubah diam-diam ketika tarif direvisi | Publikasi | “Tetapkan nominal, tujuan transfer, tenggat dan kebijakan belum bayar.” | Lengkapi konfigurasi |
| Bukti transfer | Jika tagihan berbayar | Usulan JPG/PNG/PDF ≤5 MB, satu berkas aktif | Periksa isi/jenis aktual, bukan ekstensi saja; nama file tidak menentukan akses | Unggah | “Gunakan JPG, PNG atau PDF maksimal 5 MB.” | Pilih berkas lain |
| Alasan koreksi/tolak/batal | Kondisional aksi | Usulan 10–500 karakter | Tidak menerima alasan hanya spasi | Konfirmasi | “Jelaskan alasan minimal 10 karakter.” | Tulis alasan yang dapat ditelusuri |

Rentang tim, roster, skor dan biaya merupakan batas kapasitas awal yang perlu disahkan terhadap kegiatan nyata, bukan pembatasan domain sepak bola universal. Bila kegiatan nyata melampaui rentang, catat perubahan kapasitas dan uji dampak sebelum dipakai.

## 6. Error, edge case dan pemulihan

| Kondisi | Respons terlihat | Data/status | Pesan | Pemulihan | AC |
|---|---|---|---|---|---|
| Form salah | Field terkait disorot dengan teks | Tidak menyimpan | Pesan pada tabel validasi | Fokus field pertama salah | AC-01,08,22,34 |
| Daftar kosong | Empty state dan CTA sesuai izin | Tidak membuat data contoh menjadi nyata | “Belum ada kegiatan terbuka.” | Kembali/ubah filter | AC-08,32 |
| Koneksi hilang sebelum kirim | Penanda offline; CTA simpan dicegah bila diketahui | Form lokal dipertahankan selama halaman terbuka | “Koneksi terputus. Periksa koneksi sebelum menyimpan.” | Sambungkan lalu periksa status | AC-25, NFR-08 |
| Timeout setelah kirim | Status tidak diketahui, bukan gagal pasti | Jangan membatalkan efek yang mungkin sudah terjadi | “Status penyimpanan belum diketahui. Periksa status.” | Muat status operasi/objek; hanya kirim ulang bila belum tersimpan | AC-12,25,35 |
| Klik dua kali | Tombol sibuk lalu tampil hasil tunggal | Tidak menggandakan kuota/hasil/bukti aktif | “Sedang menyimpan…” | Tahan sampai status diketahui | AC-12,14,25 |
| Sesi habis | Minta masuk ulang, simpan tujuan internal | Tidak menyatakan perubahan sudah tersimpan | “Sesi berakhir. Masuk kembali untuk melanjutkan.” | Muat data terbaru, rekonsiliasi form | AC-06,25,38 |
| Akses dicabut | Hentikan aksi, sembunyikan data terbatas | Tidak mengubah objek | “Anda tidak memiliki akses untuk tindakan ini.” | Kembali ke SCR-04/hubungi pengelola | AC-06,07,34 |
| Dua pengguna ambil slot terakhir | Satu berhasil, lainnya melihat penuh | Kapasitas tidak terlampaui | “Kuota baru saja penuh. Pendaftaran Anda belum terkonfirmasi.” | Pilih kegiatan lain | AC-14 |
| Data diedit bersamaan | Tampilkan ringkasan versi terbaru | Tolak penimpaan versi usang | “Data berubah sejak Anda membukanya. Muat versi terbaru.” | Bandingkan dan isi ulang; tanpa merge otomatis skor | AC-25,35 |
| Email/Google gagal | Pesan kanal bermasalah, tindakan lain tetap tersedia | Akun/peserta tidak dianggap aktif sebelum prasyarat | “Belum dapat melanjutkan. Coba lagi atau gunakan metode masuk lain.” | Kirim ulang/reset/metode terhubung yang sah | AC-02,04 |
| Berkas ditolak/unggah gagal | Tunjukkan format/ukuran atau kegagalan | Bukti lama tetap aktif sampai pengganti sukses | “Bukti belum tersimpan. Pilih ulang atau coba lagi.” | Cek status unggah, ulang jika belum tersimpan | AC-34,35 |
| Laga ditunda/cuaca | Status ditunda dan alasan | Tidak memengaruhi klasemen sebagai hasil | “Laga ditunda; jadwal pengganti belum ditetapkan.” | Pengelola menjadwal ulang | AC-20,26 |
| Gol/partisipasi belum lengkap | Badge kelengkapan pada hasil/statistik | Skor dapat final, statistik pemain hanya dari yang tercatat | “Rincian pemain belum lengkap.” | Petugas/pengelola melengkapi melalui revisi berjejak | AC-22,31,32 |
| Simpan presensi sebagian gagal | Daftar berhasil/gagal terpisah | Hanya peserta yang terkonfirmasi berhasil berubah | “3 perubahan tersimpan, 1 perlu diperiksa.” | Periksa peserta gagal tanpa mengulang semua | AC-28,30 |

## 7. Notifikasi dan umpan balik

Pengingat jadwal WhatsApp otomatis masuk MVP melalui FLOW-15. Tidak ada notification center, push, SMS atau email jadwal otomatis MVP. “In-app” di sini adalah pesan pada layar terkait dan badge status ketika data dimuat, bukan kanal kiriman baru.

| Pemicu | Penerima | Kanal | Waktu/kondisi | Contoh isi | Tujuan/aksi | Preferensi | Gagal/duplikat |
|---|---|---|---|---|---|---|---|
| Daftar email | Pemilik alamat | Email transaksional | Setelah form diterima, usulan tautan 24 jam | “Verifikasi alamat email untuk mengaktifkan akun.” | SCR-03 | Diperlukan untuk login email | Kirim ulang usulan jeda 60 detik; akun tetap menunggu |
| Lupa sandi | Pemilik alamat terdaftar | Email transaksional | Setelah permintaan, tautan usulan 30 menit | “Atur ulang sandi akun Anda.” | SCR-03 | Keamanan akun | UI selalu netral; tautan sekali pakai, token lama tidak dipakai kembali |
| Daftar/konfirmasi/undangan | Anggota/kapten | Pesan pada SCR-06/07 | Setelah hasil simpan diketahui | “Keikutsertaan dikonfirmasi.” | Detail status peserta | Tidak perlu preferensi | Tidak mengirim kanal eksternal; muat ulang status jika ragu |
| Hasil diajukan/dikembalikan/final | Pencatat/pengelola/anggota berhak | Badge SCR-09/18 dan klasemen | Saat memuat hasil terbaru | “Perlu perbaikan: skor belum sesuai.” | SCR-10/18 | Tidak perlu preferensi | Finalisasi tetap sah tanpa pengguna sedang membuka layar |
| Bukti diterima/ditolak | Pengirim | Status SCR-20 | Saat keputusan tersimpan/dimuat | “Bukti ditolak: tanggal transfer tidak terbaca.” | Unggah bukti pengganti | Tidak perlu preferensi | Tidak menganggap pengirim sudah membaca |
| Bagikan kegiatan/jadwal | Penerima pilihan pengirim | Tautan WhatsApp manual | Hanya setelah pengirim memilih | “Asker FC — Liga Sabtu, 14 Nov 2026 07.00 WIB. Info: [tautan].” | SCR-05/08 setelah masuk | Pengguna memilih kirim/batal | Salin tautan bila gagal, tanpa status terkirim |

Pengiriman email gagal tidak membuat verifikasi berhasil; pendaftaran akun dapat tetap menunggu dan pengguna mencoba ulang. Kanal berbagi tidak menentukan keberhasilan simpan kegiatan/jadwal.

## 8. Privasi, audit dan keputusan sebelum produksi

Data contoh dalam mockup sepenuhnya sintetis. Kontak dan bukti tidak muncul di roster/klasemen. Bukti dibatasi pada pengirim/pengelola; pemindahan peran kapten memerlukan pengelola meninjau hak tagihan tim dan tidak membagikan riwayat privat sembarangan.

Usulan minimisasi usia: simpan deklarasi 18+, waktu, versi kebijakan dan status penanganan laporan; tidak mengumpulkan tanggal lahir/identitas rutin. Jika usia diragukan, pengelola menangguhkan partisipasi dan menangani melalui kontak operasional; penetapan metode verifikasi memerlukan keputusan pemilik komunitas sebelum produksi.

Retensi final belum ditetapkan. Sebelum produksi, pemilik komunitas menetapkan masa simpan akun, hasil, audit dan bukti pembayaran beserta dasar pemrosesan. Permintaan koreksi/penghapusan melalui kontak pengelola: verifikasi pemohon, inventaris data, putuskan penghapusan/pseudonimisasi atau retensi yang beralasan, catat keputusan dan tindak lanjut. Tidak menjanjikan penghapusan seluruh hasil tim bila bertentangan dengan catatan sah yang perlu dipertahankan.

Default usulan yang harus disahkan untuk kegiatan nyata: zona waktu, jumlah tim/roster, kebijakan administratif 3–0, juara bersama, withdrawal, tenggat dan kebijakan biaya. Dokumen dapat menjadi acuan pembangunan; belum menjadi pengesahan kebijakan komunitas atau hasil uji produksi.

## 9. Batas fitur lanjutan

FEAT-13, FEAT-14, FEAT-17 dan FEAT-18 belum dijadwalkan untuk detail alur/layar. FEAT-15/16 tetap MVP; FEAT-28 dipromosikan ke MVP penilaian sederhana pada bagian 13. Analitik yang memerlukan data tambahan tetap di luar MVP. Tidak ada flow MVP yang mengaktifkan tenant baru, gateway, kanal otomatis tambahan, AI, format tambahan atau akun anak. FEAT-27 opsional belum diaktifkan. Persiapan batas komunitas tidak sama dengan fitur multi-komunitas operasional.

## 10. Keputusan operasional usulan v2.0

Bagian ini memberikan default yang konkret agar desain/skenario sintetis dapat diuji. Ini **rekomendasi spesifikasi, belum kebijakan klub yang disahkan**. Pemilik komunitas perlu mengesahkan atau mengubahnya sebelum kegiatan nyata; perubahan tetap mengikuti tujuan PRD.

| Area | Default usulan dan batas |
|---|---|
| Skill | Skala ordinal 1–5 untuk kemampuan keseluruhan; penilai adalah pengelola kegiatan atau delegasi superadmin. Pemain membaca nilai sendiri, penilai membaca peserta yang diperlukan. Nilai tidak diumumkan di direktori. Posisi terpisah dari nilai dan tidak dijanjikan seimbang otomatis |
| Ukuran keseimbangan | Tampilkan rerata nilai per tim dan selisih rerata tertinggi–terendah; jumlah pemain tetap beda maksimal satu. Tujuan memperkecil selisih ini, tanpa janji optimum global atau ambang kelulusan yang tidak diberikan pengguna |
| Voting MVP laga | Hanya laga final nonadministratif; kandidat adalah pemain yang tercatat tampil. Pemilih adalah peserta laga yang tampil dan aktif saat voting. Satu suara aktif per akun, tidak boleh memilih diri; boleh mengganti sebelum tutup. Buka setelah finalisasi, default tutup 24 jam setelah dibuka dalam zona kegiatan |
| Voting bulanan | Bulan kalender dalam zona komunitas, usulan Asia/Jakarta. Kandidat anggota aktif yang memiliki setidaknya satu penampilan final pada bulan itu; pemilih anggota aktif dengan setidaknya satu kehadiran pada bulan itu. Usulan buka tanggal 1 pukul 09.00, tutup tanggal 4 pukul 21.00 bulan berikutnya; hanya setelah petugas memeriksa data. Ini voting terpisah dari MVP laga |
| Hasil voting | Hasil tidak menampilkan hitungan langsung selama voting. Setelah tutup, petugas menerbitkan jumlah suara sah dan pemenang. Seri = pemenang bersama; nol suara = tidak ada pemenang. Tak ada pengundian/pemenang manual tersembunyi. Pilihan per orang tidak publik |
| Pengingat | Satu pengingat per peserta per sesi, usulan H−24 jam; waktu dapat dikonfigurasi sebelum publikasi. RSVP setelah waktu pengingat tidak memicu backlog mendadak; status Dilewati ditampilkan. Pengingat bukan kanal wajib untuk dapat bermain |
| Retry pengingat | Usulan maksimal 3 percobaan terkonfirmasi gagal dengan jeda 5 lalu 15 menit, hanya sebelum sesi dimulai. Status belum diketahui wajib rekonsiliasi dahulu; bila provider tidak mendukung deduplikasi/cek status, jangan otomatis mengirim ulang dan arahkan ke pemeriksaan petugas. Tidak mengklaim exactly-once lintas provider |
| Iuran | Petugas kas menerbitkan manual per bulan. Tarif, tanggal jatuh tempo, daftar anggota yang dikenai dan saldo awal wajib diisi dari keputusan klub, tanpa angka default yang disangka tarif resmi. Tidak otomatis menagih tamu bulanan |
| Publikasi | Draft default. Publik atau anggota/kegiatan dipilih eksplisit untuk sesi/hasil/album; foto profil mengikuti pilihan cakupan yang diizinkan. Iuran, bukti, suara dan nilai tidak dapat dijadikan konten publik |
| Tamu | Akun 18+ dengan konfirmasi sendiri, ditandai tamu untuk kegiatan yang diizinkan. Tidak menjadi anggota aktif klub atau penerima tagihan bulanan otomatis. Kapten tamu hanya mengelola roster timnya |

Pemilihan yang sedang terbuka memakai snapshot kandidat/pemilih yang disahkan saat buka; kandidat dan aturan tidak diubah diam-diam. Akun yang dinonaktifkan tidak dapat mengirim suara baru. Bila kelayakan atau hasil laga dikoreksi dan memengaruhi voting, hentikan/tandai perlu tinjau dan ikuti pembatalan/revisi beralasan, bukan menghitung ulang pemenang diam-diam.

## 11. Alur perluasan MVP

### FLOW-13 — Buka link, Beranda, kalender dan lokasi

FEAT-19, FEAT-20 · US-19, US-20 · AC-45–AC-50. Aktor publik/anggota. Prasyarat konten diterbitkan untuk audiens yang sesuai; hasil sesi ditemukan dan konteks RSVP dipertahankan.

1. Tautan komunitas membuka SCR-04, tautan sesi langsung membuka SCR-05. Periksa izin baca sebelum menampilkan isi; sesi tidak ada/tidak boleh dibaca memberikan penjelasan netral dan kembali ke SCR-23.
2. SCR-04 menampilkan logo, hero foto aksi asli/placeholder berlabel, CTA Gabung Main, sesi terdekat yang belum batal dan angka ringkasan. Anggota aktif tidak memasukkan akun tamu; pertandingan/gol berasal hasil final nonbatal, gol bermain mengecualikan hasil administratif. Cakupan ringkasan usulan sepanjang waktu diberi label.
3. SCR-23 menampilkan kalender desktop atau agenda HP; filter tanggal/status mempertahankan konteks. Pilih sesi membuka SCR-05 dengan alamat, pin/peta jika tersedia, petunjuk arah, kuota/biaya dan zona eksplisit. Provider peta belum ditetapkan; tanpa peta alamat/tautan arah tetap tersedia.
4. Pilih Gabung Main: bila perlu login, SCR-01 lalu kembali SCR-05. Jika memenuhi prasyarat, satu aksi membuat RSVP dan menampilkan status di SCR-05; tidak memaksa konfirmasi kedua di SCR-06. Mode antartim ke SCR-07 melalui aturan FLOW-04.
5. Setelah terdaftar, tunjukkan jadwal/tim dan pilihan pengingat SCR-33. Jangan menganggap RSVP sebagai opt-in. Setelah main, detail sesi menautkan SCR-09/SCR-11, album SCR-27 dan Kas & Iuran SCR-28. Foto/hasil belum tersedia tidak mengunci kas.
6. Tidak ada sesi: “Belum ada laga. Saatnya ajak teman main!”; filter kosong menawarkan reset; error muat mempertahankan filter dan Coba lagi. Publik tidak melihat daftar peserta privat walau sesi publik.

### FLOW-14 — Nilai skill dan pembagian seimbang

FEAT-16, FEAT-05 · US-16, US-05 · AC-16–AC-18, AC-42, AC-79–AC-81. Aktor penilai/pengelola sesuai cakupan. Prasyarat peserta terkonfirmasi; hasil snapshot nilai dan pratinjau yang dapat ditinjau.

1. Pengelola membuka SCR-32 dari SCR-16; pilih peserta, periksa nilai dan penilai/waktu, lalu simpan skala 1–5 dengan keterangan perubahan. Pemain tidak dapat memberi nilai sah pada dirinya hanya karena menjadi anggota.
2. SCR-16 memilih mode Acak atau Skill. Acak tetap menjaga jumlah/kapasitas. Skill memerlukan semua peserta punya nilai sah; tampil nama data kurang, aksi Lengkapi nilai SCR-32 atau Ganti ke Acak dengan pilihan eksplisit. Jangan menganggap nilai kosong = nol atau rata-rata.
3. Default usulan pembagian skill: urut nilai menurun, acak urutan untuk nilai sama, alokasikan berikutnya ke tim dengan jumlah paling sedikit lalu rerata terendah yang masih berkapasitas; perbaikan pertukaran diperbolehkan selama tidak menambah selisih jumlah/kapasitas. Tampilkan rerata dan selisih tertinggi–terendah sebagai ringkasan, bukan label “pasti adil”.
4. Pindah manual melalui pemilih tim memperbarui ringkasan; tampil peringatan bila sebaran memburuk. Simpan hanya jika peserta unik/kapasitas valid; Batal mempertahankan versi lama. Snapshot nilai/mode/penilai terkait dicatat saat simpan. Nilai berubah sebelum simpan meminta muat data terbaru/pratinjau ulang.
5. Kunci mengikuti FLOW-05. Mengubah penilaian kemudian tidak mengacak ulang roster terkunci. Mode antartim hanya membaca roster diterima; jangan mengganti pemain antartim menggunakan pembagian otomatis.
6. Akses nilai tidak berhak ditolak; data kosong mempertahankan mode yang dipilih sambil menunggu tindakan. Notifikasi eksternal tidak diperlukan untuk edit nilai; status simpan dan jejak tampil pada SCR-32/SCR-22.

### FLOW-15 — Preferensi dan pengiriman pengingat WhatsApp

FEAT-15 · US-15 · AC-41, AC-82–AC-86. Aktor peserta, pengelola dan proses terjadwal. Prasyarat provider/akun/template siap sebelum pengiriman nyata; hasil pengingat terjadwal dan status yang dapat ditelusuri.

1. SCR-33 mode anggota: masukkan nomor milik sendiri, baca cakupan/pola pengingat, lalu beri persetujuan eksplisit yang awalnya tidak dicentang. Simpan nomor format internasional, waktu dan versi persetujuan. Mematikan preferensi tidak membatalkan RSVP.
2. SCR-14 menetapkan waktu pengingat untuk sesi. Sesudah RSVP sah dan kanal siap, jadwalkan satu pekerjaan per sesi/penerima/jenis pengingat pada versi jadwal terkait. Pekerjaan bertahan setelah restart. Nomor pribadi hanya terlihat kepada subjek dan petugas pengiriman berhak, bukan roster.
3. Saat jatuh tempo, periksa lagi peserta aktif/terkonfirmasi, izin, nomor, sesi belum batal/belum mulai dan versi waktu terbaru. Yang tidak memenuhi dilewati/dibatalkan dengan alasan. RSVP terlambat mengikuti default tanpa backlog pada bagian 10.
4. Proses mengirim menggunakan format/template yang disetujui provider bila disyaratkan; pesan hanya nama sesi, waktu/zona, lokasi dan tautan detail. Contoh: “Sepatu siap? Sabtu Santai mulai 14 Nov 2026, 07.00 WIB di Lapangan Komunitas. Cek sesi: [tautan]. Atur pengingat melalui profil.” Tidak menyertakan tagihan, bukti, skill atau pilihan voting.
5. Pisahkan status internal Terjadwal/Diproses/Dibatalkan/Dilewati/Gagal/Perlu pemeriksaan dari status provider Diterima provider/Terkirim/Tersampaikan/Dibaca bila memang ada bukti. Diterima provider belum berarti tersampaikan. Tidak adanya bukti baca bukan berarti belum dibaca secara pasti.
6. Revisi waktu/lokasi sebelum kirim membatalkan pekerjaan usang dan memperbarui yang baru; deduplikasi pengingat tetap per sesi/penerima/jenis, sehingga pengingat yang sudah terkirim tidak otomatis dikirim kedua kali hanya karena versi berubah. Jika sudah terkirim, SCR-33 memberi tanda “Perubahan setelah pengingat”; petugas membagikan pembaruan manual melalui FLOW-12. Pesan terkirim tidak dapat ditarik kembali oleh situs.
7. Gagal pasti: retry terbatas sesuai bagian 10 dan syarat terbaru. Timeout/hasil tidak diketahui: Perlu pemeriksaan, rekonsiliasi referensi provider sebelum retry; jangan menjanjikan nol duplikasi jika provider tidak mendukung kontrol yang diperlukan. Status berulang/usang tidak menurunkan bukti yang lebih baru.
8. Petugas membuka SCR-33 mode pengelola untuk filter sesi/status, lihat ringkasan nomor dimask, kegagalan dan tindak lanjut. Retry beralasan hanya untuk gagal terkonfirmasi; tidak ada tombol kirim massal bebas atau akses grup WhatsApp. Kegagalan kanal tidak membatalkan RSVP; detail sesi tetap sumber jadwal.
9. Provider belum dipilih/konfigurasi belum siap: tampil “Pengingat belum aktif” dan jangan mengklaim terjadwal untuk terkirim. Uji menggunakan penerima uji yang menyetujui; tugas dokumentasi ini tidak mengirim pesan.

### FLOW-16 — Voting MVP dan Pemain Terbaik Bulan Ini

FEAT-21 · US-21 · AC-51–AC-56. Aktor petugas voting kegiatan/komunitas dan pemilih berhak. Titik masuk SCR-24/SCR-09; hasil suara tersimpan atau hasil publikasi.

1. Petugas membuka SCR-34, memilih jenis Laga/Bulanan, konteks/periode, zona, waktu, daftar kandidat/pemilih dan aturan pada bagian 10. Validasi konteks unik dan tidak ada pemilihan aktif lain untuk konteks yang sama. Bulanan dikelola superadmin/delegasi komunitas, bukan setiap pengelola kegiatan.
2. Periksa data penampilan/kehadiran; data belum lengkap memberi peringatan dan memerlukan penyelesaian sebelum membuka. Buat snapshot eligibility; tampil aturan kepada anggota pada SCR-25 sebelum voting. Pemilih tidak berhak membaca hasil yang boleh dilihat, tanpa tombol kirim aktif.
3. Pilih satu kandidat dan Konfirmasi suara; cek kelayakan, kandidat bukan diri, periode masih terbuka dan versi terbaru. Sukses menunjukkan “Suara tersimpan”. Selama terbuka Ganti pilihan mengganti satu suara aktif, bukan menambah suara.
4. Menutup halaman sebelum simpan tidak memberi suara. Double-click atau request bersamaan mengembalikan satu pilihan aktif; hasil tidak diketahui memakai Cek status sebelum kirim ulang. Tenggat di sisi layanan menentukan penerimaan, bukan jam HP.
5. Pada tutup, suara baru/perubahan ditolak. Petugas meninjau hasil agregat lalu Terbitkan; seri menghasilkan pemenang bersama, nol suara “Belum ada pemenang — tidak ada suara sah”. Hasil laga dan bulanan tidak dicampur atau dihitung dari gol.
6. Koreksi final pertandingan/eligibility memicu Perlu tinjau pada pemilihan terkait; petugas membatalkan dengan alasan atau merevisi hasil dari suara sah yang dapat diaudit, lalu menerbitkan versi baru. Jika kandidat/pemilih/aturan perlu diganti, batalkan pemilihan lama dan buka pemilihan pengganti; jangan mengubah snapshot sambil voting berjalan. Riwayat lama tetap terlabel dibatalkan.
7. Hasil publikasi tampil di SCR-25 dan apresiasi SCR-24/SCR-12 sesuai audiens; pilihan per individu tidak tampil publik atau di audit umum. Petugas penanganan berhak dapat menyelidiki sengketa dengan audit akses. Tidak ada kiriman eksternal otomatis untuk voting.

### FLOW-17 — Album, foto aksi dan highlight

FEAT-22 · US-22 · AC-57–AC-61. Aktor pengelola pertandingan/petugas media; pembaca sesuai audiens. Prasyarat pertandingan sah, media dan izin; hasil album diterbitkan atau ditarik.

1. Dari SCR-13 ke SCR-30, pilih pertandingan, buat album draft, unggah foto atau tambahkan tautan highlight dari layanan yang disetujui. Usulan JPG/PNG/WebP ≤10 MB per foto, ≤20 item per unggah; video berupa tautan HTTPS tanpa unggah/transcoding sendiri.
2. Per item isi caption/teks alternatif yang menjelaskan aksi dan catat izin tayang. Hapus metadata lokasi sensitif pada publikasi bila ada. Unggah gagal mempertahankan item berhasil sebagai draft dan menampilkan item gagal; retry tidak menggandakan media yang sudah diterima.
3. Tinjau audiens Publik/Anggota/Kegiatan terkait, hak tayang dan pratinjau mobile. Terbitkan eksplisit; hanya item sah ikut publikasi. Mengetahui URL media tidak mengabaikan izin pada akses yang dilayani sistem; jangan menyimpan media privat sebagai URL publik tak terlindungi.
4. Pembaca SCR-26 memilih album SCR-27 atau dari hasil SCR-09; grid responsif, lihat foto besar dengan caption/alt, tautan video membuka player/layanan lain setelah aksi pengguna. Tautan gagal memiliki Buka sumber; jangan mengaku video tersimpan di sistem.
5. Foto belum tersedia: placeholder “Foto aksi anggota • menunggu unggahan”, bukan foto generatif yang diklaim asli. Detail hasil tetap menuju SCR-28 untuk bayar iuran walau skor/foto belum terbit.
6. Tarik publikasi dengan alasan: hilangkan dari daftar dan hentikan akses berikutnya melalui sistem; cache/salinan yang sudah diunduh pihak lain tidak dijamin hilang. Revisi hanya petugas berhak dengan riwayat. Sponsor/anggota pada contoh tidak boleh direkayasa sebagai hubungan nyata. Tidak ada notifikasi eksternal album pada MVP.

### FLOW-18 — Iuran bulanan dan buku kas

FEAT-23, FEAT-11 · US-23, US-11 · AC-62–AC-67, AC-33–AC-36. Aktor anggota/petugas kas yang didelegasikan. Hasil tagihan dan saldo sah yang tidak berlipat.

1. Petugas kas membuka SCR-31, menetapkan saldo awal sekali per buku kas beserta tanggal/referensi; tidak menambah saldo awal setiap periode bulanan. Setelah transaksi aktif ada, koreksi saldo awal memakai penyesuaian beralasan.
2. Pilih bulan YYYY-MM, anggota yang dikenai, tarif dan jatuh tempo yang diputuskan klub. Pratinjau jumlah tagihan dan nominal; terbitkan manual. Satu tagihan aktif per anggota/periode; pemanggilan ulang mengembalikan tagihan yang sama. Tamu dikecualikan kecuali dijadikan anggota lewat proses sah terpisah.
3. Anggota membuka SCR-28 langsung atau setelah hasil/foto. Tampilkan ringkasan kas periode, tagihan sendiri bulanan/kegiatan dipisahkan, nominal/status/jatuh tempo. Tidak ada tagihan berarti “Tidak ada tagihan aktif”, bukan membuat tagihan per pertandingan.
4. Bayar melalui SCR-20 mengikuti FLOW-11: transfer di luar sistem, bukti valid → Menunggu verifikasi, ditolak → perbaiki bukti, diterima → Lunas. Petugas verifikasi bulanan adalah petugas kas komunitas; pengelola kegiatan hanya biaya kegiatan tugasnya.
5. Verifikasi sah mencatat pemasukan terkait pembayaran tepat sekali. Bila pemasukan biaya kegiatan ikut buku kas komunitas, gunakan referensi pembayaran yang sama dan konsolidasi sekali; jangan menulis ulang sebagai pemasukan manual. Menolak bukti tidak menaikkan saldo.
6. Petugas mencatat pemasukan lain atau pengeluaran sewa lapangan: tanggal, nominal IDR positif, jenis, kategori, uraian dan referensi. Draft tidak masuk saldo; konfirmasi mencatat transaksi sah. Saldo akhir = saldo awal + pemasukan sah − pengeluaran sah sampai akhir periode. Saldo boleh negatif dan diberi label, bukan dibulatkan nol.
7. Koreksi transaksi/keputusan bayar memakai alasan dan pratinjau efek. Pembalikan verifikasi membalik pemasukan terkait, status tagihan diperbarui dan uang tidak dipindahkan otomatis. Menyetujui ulang tidak menambah pemasukan neto dua kali. Periode lama tidak ditimpa tanpa jejak; tidak ada tombol hapus audit.
8. Timeout/transaksi bersamaan memerlukan Cek status/muat versi terbaru. Hak kas dicabut langsung menolak aksi berikutnya. Anggota tidak melihat identitas pembayar/status tunggakan orang lain; laporan anggota memuat kategori/total dan rincian transaksi yang telah diringkas tanpa bukti atau kontak pribadi.
9. Batal pada pratinjau tidak menerbitkan tagihan/transaksi. Kembali ke SCR-28 menampilkan status yang tersimpan. Biaya di muka hanya cabang sesuai konfigurasi sesi; iuran tidak menjadi prasyarat melihat skor/foto. Tidak ada notifikasi iuran WhatsApp otomatis tambahan pada MVP.

### FLOW-19 — Kontak dan sponsor

FEAT-24 · US-24 · AC-68–AC-70. Aktor publik dan petugas publikasi komunitas. Titik masuk Lainnya → SCR-29; pengelolaan SCR-35.

1. Superadmin/delegasi mengisi nomor WhatsApp admin publik, URL sosial, logo sponsor asli, nama/alt, urutan dan URL opsional. Validasi nomor internasional, URL HTTPS dan berkas foto/logo; skema skrip/URL berbahaya ditolak.
2. Simpan draft lalu pratinjau; Terbitkan memperlihatkan hanya field aktif. Nomor admin ini berbeda dari nomor pengingat privat peserta. Tanpa kontak/sponsor tampil informasi belum tersedia; tidak membuat sponsor rekaan.
3. Klik WhatsApp membuka pesan siap isi untuk dikirim pengguna; fallback tampil nomor publik/salin. Klik media sosial/sponsor membuka tujuan luar dengan label; tidak otomatis mengirim pesan atau memberi akses data anggota.
4. Revisi/Nonaktifkan memperbarui tampilan berikutnya dan audit; akses petugas tanpa delegasi ditolak. Tidak perlu notifikasi eksternal untuk perubahan konten.

### FLOW-20 — Preferensi tema dan navigasi HP

FEAT-25 · US-25 · AC-71–AC-73. Aktor semua pembaca. Hasil tema terbaca dan pilihan tersimpan pada perangkat.

1. Muat tema tersimpan; bila belum memilih gunakan tema gelap hitam–gold. Bila pilihan tersimpan Sesuai perangkat, ikuti preferensi perangkat. Kontrol berlabel Terang/Gelap/Sesuai perangkat di header/profil, bukan ikon tanpa nama.
2. Pilih tema menerapkan token ke halaman, modal, tabel dan semua status tanpa mengubah data formulir. Simpan di perangkat yang sama; penyimpanan tidak tersedia tetap menerapkan tema sesi dan menampilkan batas persistensi bila relevan. Sinkron lintas perangkat tidak dijanjikan.
3. Navigasi bawah Beranda/Jadwal/Tim/Lainnya; Lainnya membuka Hasil, Galeri, Kas, Kontak dan Toko hanya bila aktif. Drawer/panel memiliki tutup dan pengembalian fokus; konten tidak tertutup nav bawah.
4. Kembali/muat ulang mempertahankan pilihan tema dan status data yang benar. Pengelolaan memakai drawer di HP tanpa menghilangkan aksi desktop. Tidak ada notifikasi eksternal untuk pilihan tema.

### FLOW-21 — Tim tamu antar-komunitas

FEAT-26 · US-26 · AC-74–AC-76. Aktor kapten tamu/pemain/pengelola. Prasyarat sesi antartim menerima tamu, akun aktif 18+ dan persetujuan roster.

1. Tautan sesi publik → SCR-05, kapten masuk/melengkapi akun kemudian SCR-07. Cantumkan nama dan asal tim, status tamu; tidak membuat tenant baru atau memberi role superadmin komunitas lain.
2. Pemain tamu membuat/masuk akun sendiri dan menyetujui undangan melalui SCR-06. Pengelola meninjau SCR-15; kuota/keunikan sama dengan tim internal. Persetujuan kapten tidak menggantikan persetujuan pemain.
3. Tim diterima melihat roster/jadwal/hasil dan album yang memang dibagikan untuk kegiatan itu. Tagihan tim kegiatan dikelola kapten dengan batas bukti FLOW-11; kas komunitas, direktori internal di luar sesi dan voting bulanan anggota tidak otomatis terbuka.
4. Penugasan pencatat tamu hanya bila pengelola menunjuk laga tertentu. Selesai/ditarik mencabut tindakan aktif sesuai status tanpa menghapus hasil/presensi sah; hak baca riwayat mengikuti cakupan yang diumumkan. Permintaan tanpa izin ditolak, tidak menambah anggota aktif atau iuran bulanan.

## 12. Status, validasi dan pemulihan tambahan

| Entitas/ID | Asal → tujuan | Aktor/pemicu dan syarat | Efek dan bila gagal |
|---|---|---|---|
| Pengingat / STATE-12 | Belum dijadwal → Terjadwal → Diproses | Proses; RSVP, opt-in, konfigurasi dan waktu valid | Gagal prasyarat = Dilewati dengan alasan; tidak mengubah RSVP |
| Pengingat / STATE-12 | Terjadwal → Dibatalkan / Terjadwal versi baru | Proses; batal/mundur/opt-out atau revisi sebelum kirim | Pekerjaan lama tidak lagi dapat dikirim; perlombaan saat kirim diperiksa ulang |
| Pengingat / STATE-12 | Diproses → Diterima provider / Gagal / Perlu pemeriksaan | Bukti dari provider atau timeout | Retry hanya gagal pasti sesuai batas; status provider tersampaikan/dibaca hanya jika ada bukti |
| Voting / STATE-13 | Draft → Terbuka → Ditutup → Diterbitkan | Petugas berhak; konfigurasi sah, waktu tutup, hasil sah | Suara hanya pada Terbuka; gagal publikasi tetap Ditutup |
| Voting / STATE-13 | Terbuka/Ditutup/Diterbitkan → Perlu tinjau → Dibatalkan atau Diterbitkan versi baru | Koreksi relevan dan alasan | Bekukan suara/hasil yang dipersoalkan; jejak lama tetap ada |
| Media / STATE-14 | Draft → Terbit → Ditarik | Petugas media; izin/audiens sah, alasan penarikan | Berkas gagal tetap draft; konten ditarik tidak boleh tetap di daftar publik |
| Tagihan bulanan / STATE-15 | Draft → Terbit → Dibatalkan atau aktif sampai lunas | Petugas kas; periode/anggota unik dan alasan bila batal | Status bayar mengikuti STATE-10; pembatalan tidak memindahkan uang |
| Transaksi kas / STATE-16 | Draft → Tercatat → Dibalik/dikoreksi berjejak | Petugas kas, data/referensi valid | Draft tidak memengaruhi saldo; transaksi lama tidak dihapus |
| Konten kontak / STATE-17 | Draft → Terbit → Nonaktif | Petugas publikasi; field sah | Hanya Terbit tampil publik; kegagalan tidak menghapus versi lama |

Transisi langsung Menunggu verifikasi → Lunas oleh pengirim, Draft media → Publik tanpa peninjauan, Voting Ditutup → suara baru dan Gagal pengingat → Tersampaikan tanpa bukti tidak diizinkan. STATE-10 Terverifikasi ditampilkan sebagai “Lunas” pada tagihan; dua istilah merujuk satu keputusan, bukan dua pembayaran.

| Field | Kondisi / format dan batas usulan | Keunikan/lintas field, waktu cek | Pesan dan pemulihan |
|---|---|---|---|
| Posisi/nomor/foto profil | Opsional; posisi daftar/label, nomor 1–99, foto JPG/PNG/WebP ≤5 MB | Nomor pada roster satu tim/kegiatan; cek saat simpan roster/profil terkait | “Nomor 7 sudah dipakai di tim ini.” Pilih nomor lain; tidak melarang nomor sama di tim lain |
| Skill | Wajib mode skill, bilangan bulat 1–5 dan penilai berhak | Cek simpan nilai dan pembagian; nilai kosong tidak dianggap nol | “Lengkapi nilai 3 peserta atau pilih mode Acak.” |
| Voting | Jenis, konteks/periode, kandidat/pemilih dan buka < tutup wajib | Satu konteks aktif, satu suara aktif per pemilih; cek buka/kirim/tutup | “Voting sudah ditutup. Suara belum berubah.” Lihat hasil/status |
| Nomor/persetujuan pengingat | Kondisional opt-in; format internasional, usulan + dan 8–15 digit | Nomor milik pengguna; izin terbaru dicek saat jadwal dan kirim | “Periksa nomor WhatsApp dan persetujuan pengingat.” Koreksi atau nonaktifkan |
| Waktu pengingat | Jeda usulan 1–168 jam sebelum sesi, zona sesi | Waktu kirim < mulai; provider siap; cek jadwal/kirim | “Pengingat tidak dijadwalkan karena sesi sudah terlalu dekat.” Jadwal tetap tersedia |
| Foto album/logo | JPG/PNG/WebP, foto ≤10 MB, logo ≤5 MB, alt/caption dan izin tayang | Periksa isi sebenarnya dan hak tayang sebelum terbit | “Foto belum dapat diterbitkan. Lengkapi izin dan keterangan.” Simpan draft/perbaiki |
| URL peta/video/sosial/sponsor | HTTPS, usulan ≤2048 karakter | Tautan disimpan sebagai tujuan, tidak mengeksekusi konten input; cek simpan/buka | “Gunakan tautan HTTPS yang valid.” Perbaiki; alamat lapangan tetap tampil |
| Iuran/transaksi | Periode YYYY-MM, nominal IDR bulat positif, tanggal, kategori/referensi; saldo awal dapat nol/negatif | Satu tagihan aktif per anggota/periode, satu pemasukan neto per pembayaran; cek terbit/verifikasi | “Tagihan periode ini sudah tersedia.” Buka yang ada; koreksi melalui jejak |

| Kondisi | Respons/state dan pemulihan | AC |
|---|---|---|
| Skill berubah saat pratinjau | Roster tersimpan tidak ditimpa; muat snapshot dan susun ulang | AC-79–AC-81 |
| Suara ganda atau kirim dekat tenggat | Satu suara aktif; jika hasil belum diketahui Cek status, jangan mengirim suara tambahan | AC-52, AC-53 |
| Seri / tanpa suara | Pemenang bersama / tidak ada pemenang, bukan fallback gol/penunjukan diam-diam | AC-54, AC-55 |
| Provider belum siap/timeout | Belum aktif / Perlu pemeriksaan; pengelola lihat SCR-33, RSVP tidak dibatalkan | AC-82, AC-84 |
| Sesi berubah setelah kirim | Label perubahan setelah pengingat, bagikan pembaruan manual; tidak mengulang pengingat otomatis | AC-41, AC-83 |
| Foto/hasil belum terbit | State jelas dan Kas & Iuran tetap aktif | AC-59, AC-67 |
| Kas terbit/verifikasi ulang | Tampilkan objek lama, tanpa tagihan/pemasukan ganda; konflik muat versi terbaru | AC-62, AC-64, AC-66 |
| Tamu akses kas/anggota internal | Ditolak tanpa data privat; kembali detail sesi | AC-75 |

## 13. Matriks akses dan notifikasi tambahan

| Area | Publik | Anggota aktif | Tamu kegiatan | Pengelola sesi | Superadmin/delegasi komunitas |
|---|---|---|---|---|---|
| Beranda/kontak/sponsor | Hanya terbit | Sama | Sama | Baca; edit bukan otomatis | Publikasi sesuai delegasi |
| Jadwal/hasil/album | Hanya cakupan publik | Sesuai izin | Hanya sesi berhak | Kelola sesi/album tugasnya | Sesuai delegasi |
| Direktori/profil | Hanya profil yang diizinkan publik | Cakupan anggota | Roster sesi saja | Data peserta yang diperlukan | Sesuai delegasi |
| Skill | Tidak | Nilai diri sendiri | Nilai diri bila dinilai untuk sesi | Penilai untuk peserta tugasnya | Delegasi penilai |
| Voting | Hasil yang diizinkan | Sesuai eligibility | MVP laga bila memenuhi; bukan bulanan otomatis | Kelola voting laga tugasnya | Kelola bulanan bila berhak |
| Kas/iuran | Tidak | Ringkasan/utang sendiri | Tagihan kegiatan miliknya saja | Biaya kegiatan tugasnya | Kas komunitas hanya petugas kas |
| Preferensi pengingat | Tidak | Nomor/pilihan sendiri | Nomor/pilihan sendiri | Status kirim peserta tugasnya | Konfigurasi provider hanya berwenang |

| Pemicu | Penerima/kanal | Waktu dan contoh isi | Tujuan/preferensi | Gagal/duplikat |
|---|---|---|---|---|
| Jelang sesi | Peserta opt-in / WhatsApp otomatis | Usulan H−24; nama sesi, waktu/zona, lokasi, tautan | SCR-05; opt-in/opt-out SCR-33 | FLOW-15; retry terbatas, rekonsiliasi bila tidak diketahui |
| RSVP/ubah suara/unggah bukti | Pelaku / pesan layar | Sesudah simpan diketahui: “Suara tersimpan”, “Menunggu verifikasi” | SCR-05, SCR-25, SCR-20 | Tidak mengklaim terkirim ke kanal eksternal |
| Hasil/foto belum tersedia | Pembaca / state halaman | Saat buka: “Foto aksi belum diunggah” | SCR-27 dan tetap ke SCR-28 | Coba lagi; tidak mengunci iuran |
| Tagihan/keputusan iuran | Pemilik/petugas / status halaman | Saat buka status terbaru | SCR-28/SCR-20; tidak ada WhatsApp iuran otomatis | Satu objek dan satu pemasukan sah |

### Perjalanan utuh untuk pilot

Buka tautan SCR-04/SCR-05 → pilih sesi SCR-23/SCR-05 → RSVP FLOW-04 → preferensi/pengingat FLOW-15 → main/presensi FLOW-09 → hasil SCR-09/SCR-11 dan foto SCR-27 → Kas & Iuran SCR-28 → bukti/verifikasi SCR-20/FLOW-18. Skenario mengecek konten belum tersedia, gagal kirim, tanpa tagihan dan pembayaran ditolak. Pilot belum dijalankan; ini spesifikasi yang diturunkan dari instruksi pengguna.
## 13. Penilaian pemain dan sumber poin v2.2

Bagian ini menjadi acuan aktif penilaian; FLOW-03/05/06/07/08/09/10/14/16 tetap berlaku dengan perluasan berikut. Instruksi pengguna memproses rencana v2.7 menerima rekomendasi eksplisitnya sebagai baseline dokumentasi: profil mulai 50 sekali dan cap per event, rasio maksimum atas pemain tampil, bobot posisi, kebobolan −2, aturan tumpukan/streak dan metadata khusus. Itu bukan bukti kalibrasi keadilan atau pengesahan kebijakan klub untuk kegiatan nyata. Dua rincian penalti yang sebelumnya belum dipilih kini ditetapkan melalui klarifikasi pengguna 12 November 2026, bukan diasumsikan dari persetujuan lama.

### 13.1. Input, istilah dan rumus aktif

Preset penilaian: empat tim, enam laga, tiga laga per tim. Daftar pasangan A–B/A–C/A–D/B–C/B–D/C–D; penjadwalan tetap memeriksa waktu, lapangan dan istirahat. Mode rutin lain tetap dapat digunakan; penghargaan peringkat memerlukan event berklasemen yang dikonfigurasi eksplisit, bukan mengubah semua sesi rutin menjadi liga diam-diam.

Setelah pembagian tim disimpan, setiap pemain/posisi event mempunyai satu poin awal **50 pada kesembilan kategori**. Antartim memakai penerimaan/pengesahan susunan tim sebelum main sebagai titik pembentukan; tidak melakukan pengacakan lintas tim tamu. Pratinjau tidak membuat saldo; pengacakan ulang tidak memberi 50 lagi. Pemain pengganti hanya memperoleh satu baseline setelah penerimaan roster beralasan dan hanya poin laga yang benar-benar diikuti; sumber historis tidak dipindahkan kepadanya.

Individu hanya mengisi **Gol dan Assist**, bilangan bulat nonnegatif per laga. Tampil/tidak tampil, kartu, presensi dan metadata khusus merupakan catatan terpisah. Kebobolan otomatis = skor lawan. Rincian terkonfirmasi memenuhi: Gol pemain tim + gol bunuh diri lawan = skor tim; Assist tim ≤ Gol pemain tim; Assist pemain ≤ Gol rekan setim selain dirinya. Pemberi assist dan pencetak harus berbeda, tampil dan satu tim. Karena input agregat, hubungan gol-assist per kejadian tidak diklaim tersedia. Nol adalah konfirmasi; kosong belum dicatat. Skor dapat final saat rincian belum lengkap, tetapi poin terkait tetap sementara.

Gunakan delta per kategori dari sumber unik: bonus individu/tim/streak B, potongan hasil tim T (hanya Teamwork 0/3/6/9), pelanggaran P dan kebobolan K. `delta = B − T − P − K`; `mentah = max(0, 50 + delta)`. Mentah tidak dibatasi 100, delta bertanda tetap tersimpan. Kebobolan Defender/Goalkeeper yang tampil: K_Defending = 2 × skor lawan per laga; Striker/Midfielder tidak dipotong otomatis. Clean Sheet +5 pada nol kebobolan; Resilience +2 pada satu kebobolan memberi bersih 0 setelah K=2; dua kebobolan memberi −4. Ini proksi hasil tim, bukan pengukuran tackle/save/menit. Posisi penilaian satu per event, ditetapkan sebelum main; rotasi menit tidak diukur.

Q_e = pemain terkonfirmasi tampil minimal satu laga sah. `maks_C = max(mentah_C pada Q_e)`; jika >0, `relatif_C = 100 × mentah_C / maks_C`. Perbandingan lintas empat tim dan posisi. Tertinggi/seri tertinggi 100; semua sama positif semuanya 100. Pemain tidak tampil mempunyai poin mentah/penalti profil tetapi nilai relatif — dan bukan pembanding. Q kosong: “Belum ada pemain tampil terkonfirmasi”. Maksimum 0: “Tidak dapat dinormalisasi — seluruh poin 0”; overall/peringkat yang membutuhkan kategori itu belum diterbitkan. Sebelum main, tampilkan poin awal 50, nilai relatif belum final. Jika semua terkena potongan sama, 100 relatif tetap mungkin; selalu tampilkan sumber/mentah agar tidak berarti bebas pelanggaran. Penentuan seri dan overall menggunakan presisi penuh, tampilan dua desimal. Overall event hanya dirangking untuk pemain tampil tiga laga dan event lengkap; pemain tampil sebagian tetap dapat membaca nilai relatif dengan jumlah sampel jelas.

Profil tidak memakai normalisasi: baseline 50 satu kali pada sembilan kategori. Event final diurut menurut waktu mulai lalu ID stabil; `profil_baru_C = clamp(profil_lama_C + delta_event_C, 0, 100)`. Gabung seluruh sumber satu event sebelum cap; simpan nominal dan perubahan efektif. Tidak ada cadangan di luar batas: 50 +70 =100, event berikutnya −3 =97; 50 −70 =0, berikutnya +3 =3. +10/+3/0 menjadi 63, bukan rerata. Event baru tetap mentah 50. Event tanpa delta tidak menaikkan profil atau memulihkan Discipline; katalog belum mempunyai bonus pemulihan Discipline.

Kartu kuning/merah dan potongan kategori diproses sekali melalui delta; tidak ada pengurangan kedua dari profil. Profil mencakup seluruh event sah, termasuk outlist/absen yang memiliki kewajiban roster dan keputusan sah. Batal sebelum pembagian tanpa kewajiban tidak mempunyai posting; pembatalan event tidak memberi bonus bermain. Penyelamatan hasil laga sah dari event terpotong memerlukan keputusan admin beralasan; bonus event lengkap tetap ditahan, tidak otomatis menjadi kemenangan/clean sheet administratif.

| Posisi event | Finishing | Playmaking | Defending | Teamwork | Discipline | Commitment | Consistency | Impact | Achievement |
|---|---|---|---|---|---|---|---|---|---|
| Striker | 45% | 20% | 5% | 5% | 5% | 5% | 5% | 5% | 5% |
| Midfielder | 20% | 45% | 5% | 5% | 5% | 5% | 5% | 5% | 5% |
| Defender | 5% | 5% | 60% | 5% | 5% | 5% | 5% | 5% | 5% |
| Goalkeeper | 0% | 0% | 70% | 5% | 5% | 5% | 5% | 5% | 5% |


Bobot adalah baseline dokumentasi dari rencana yang diterima, perlu kalibrasi pilot. Overall event = bobot posisi × sembilan nilai relatif; tidak dinormalisasi lagi agar pemenang overall selalu 100. Overall profil baseline 50 sekali; perubahan overall per event = jumlah bobot posisi/versi event × perubahan **efektif** kategori profil, lalu cap overall 0–100. Bonus yang tertahan batas kategori tidak menaikkan overall. Posisi baru tidak mengubah bobot histori. Untuk posisi tetap tanpa cap overall, overall setara bobot atas kategori profil; perubahan posisi dapat berbeda dan rincian pengaruh per event wajib tersedia.

### 13.2. Katalog achievement, penyesuaian dan penalti

#### Achievement individu — 12 jenis

| Achievement | Syarat | Bonus kategori | Frekuensi/sumber |
|---|---|---|---|
| First Blood | Mencetak gol pertama pertandingan, bukan gol pertama setiap tim | Finishing +2; Impact +1 | Maksimal satu pemain per laga; metadata gol pertama terkonfirmasi |
| Goal Scorer | Mencetak ≥1 gol | Finishing +3 | Sekali per pemain/laga, bukan +3 untuk setiap gol |
| Brace | Mencetak tepat 2 gol dalam satu pertandingan | Finishing +3 tambahan | Sekali per pemain/laga; diganti Hat-Trick jika mencapai ≥3 gol |
| Hat-Trick | Mencetak ≥3 gol dalam satu pertandingan | Finishing +5 tambahan | Sekali per pemain/laga, tanpa Brace |
| Assist Maker | Membuat ≥1 assist | Playmaking +3 | Sekali per pemain/laga, bukan per assist |
| Playmaker | Membuat ≥2 assist dalam satu pertandingan | Playmaking +4 tambahan | Sekali per pemain/laga, dapat ditambah Assist Maker |
| Goal Contribution | Minimal satu gol dan satu assist dalam laga yang sama | Impact +3 | Sekali per pemain/laga |
| Clean Sheet | Tim tidak kebobolan; pemain tampil sebagai Defender/Goalkeeper menurut posisi penilaian event | Defending +5 | Sekali per pemain/laga; bukan untuk pemain di bangku cadangan yang tidak tampil |
| Defensive Resilience | Defender/Goalkeeper tampil dan tim kebobolan maksimal 1 gol | Defending +2 | Sekali per pemain/laga; bila 0 kebobolan, usulan hanya Clean Sheet yang memberi bonus |
| Match Winner | Mencetak gol penentu kemenangan menurut definisi 13.5.4 | Impact +4 | Maksimal satu pemain pada tim pemenang per laga; tidak ada pada hasil seri |
| Perfect Attendance | Hadir dan bermain pada seluruh tiga laga tim dalam event | Commitment +4 | Sekali per pemain/event; terlambat atau kartu tidak menghapus badge ini, tetapi penalti terpisah tetap berlaku |
| Consistent Performer | Berkontribusi dalam tiga event berturut-turut menurut 13.5.3 | Consistency +4 | Sekali pada event yang menyelesaikan satu blok streak tiga event |

#### Achievement dan penyesuaian tim — 9 jenis

| Achievement/penyesuaian | Kondisi tim | Bonus/potongan untuk pemain yang memenuhi partisipasi | Waktu/sumber |
|---|---|---|---|
| Champion | Peringkat 1 event | Achievement +6 | Sekali setelah klasemen event final |
| Runner-Up | Peringkat 2 event | Achievement +3 | Sekali setelah klasemen event final |
| Unbeaten Team | Tidak kalah pada ketiga pertandingan tim | Teamwork +4 | Sekali setelah event selesai; usulan diganti Perfect Victory jika seluruhnya menang |
| Perfect Victory | Menang seluruh tiga pertandingan | Teamwork +6 | Sekali setelah event selesai, tidak ditambah Unbeaten Team |
| Best Defense | Kebobolan total paling sedikit dari empat tim | Defending +3, hanya Defender/Goalkeeper | Sekali setelah event selesai; tim dengan jumlah minimum yang sama mendapat penghargaan bersama |
| Best Attack | Total gol tim terbanyak dari empat tim | Impact +2 | Sekali setelah event selesai; tim dengan jumlah maksimum yang sama mendapat penghargaan bersama |
| Comeback Victory | Menang setelah pernah tertinggal pada laga tersebut | Teamwork +3 | Sekali per laga yang memenuhi syarat; hanya pemain tim pemenang yang tampil pada laga itu |
| Last Place | Peringkat 4 event | Teamwork −6 | Sekali setelah klasemen event final |
| Winless Team | Tidak menang satu pun dari ketiga pertandingan | Teamwork −3 | Sekali setelah event selesai |


Aturan tumpukan: Goal Scorer sekali per laga (≥1 Gol) ditambah tepat satu Brace untuk 2 atau Hat-Trick untuk ≥3, total +3/+6/+8 sebelum First Blood. Assist Maker dan Playmaker dapat ditambah, total +7 untuk ≥2 Assist. First Blood, Goal Contribution dan Match Winner boleh bersamaan jika fakta sah. Clean Sheet menggantikan Resilience; Perfect Victory menggantikan Unbeaten. Tidak ada cap 100 pada mentah; hanya normalisasi/profil yang 0–100.

Last Place −6 dan Winless −3 dapat bersamaan, hanya Teamwork −9. Tiga seri memberi Unbeaten +4 dan Winless −3 (bersih +1). Penerima penghargaan/penyesuaian event tim adalah roster yang tampil minimal satu laga; Perfect Attendance mensyaratkan ketiga laga. Comeback hanya pemain pemenang yang tampil pada laga terkait. Pemain batal sebelum pembagian atau tidak tampil tidak menerima bonus/potongan tim. Pemain yang tampil lalu mundur tetap menerima hasil tim dan penalti outlist sah yang merupakan sumber terpisah.

Champion/Runner-Up/Last Place menunggu peringkat resmi setelah semua enam laga bermain sah; jika seri belum menentukan posisi 1/2/4 menurut aturan pra-event, tahan penyesuaian posisi sebagai “Menunggu penetapan peringkat”. Best Defense/Attack dengan min/max sama mendapat penghargaan bersama. Semua data wajib diperiksa; tidak memberi penghargaan event lengkap pada walkover atau event yang terpotong.

Consistent Performer: event lengkap dengan minimal satu penampilan dan ≥1 Gol atau Assist; jalur defensif alternatif bagi Defender/Goalkeeper adalah tiga penampilan dengan rerata kebobolan ≤1. Urut event komitmen menurut waktu mulai/ID; blok tiga event berkontribusi tidak bertumpuk, enam menghasilkan award event ketiga/keenam. Event tanpa kontribusi atau absen/outlist tanpa alasan memutus; tidak ikut tanpa kewajiban, batal atau alasan mendesak disetujui dilewati. Sumber belum lengkap menahan evaluasi, bukan dilewati. Koreksi lama mereplay streak setelahnya. Katalog hanya memakai data sederhana dan belum membuktikan tingkat kesulitan setara per posisi.

Metadata khusus di SCR-10: Gol pertama memilih pemain tampil dengan Gol≥1 atau opsi gol bunuh diri tanpa penerima First Blood; hasil 0–0 tidak berlaku. Gol penentu memilih pemain tim pemenang yang mencetak gol ke-(gol akhir lawan+1), misalnya gol kedua pada 3–1; petugas memastikan urutan, bukan menebak dari total, gol bunuh diri tidak memberi Match Winner. Comeback: Ya/Tidak/Belum diketahui, Ya memerlukan tim menang dan lawan Gol≥1. Menang 1–0 tidak bisa comeback. Total agregat tidak dapat membuktikan urutan; pengelola mengesahkan catatan kejadian. Status metadata belum diketahui menahan badge saja; admin boleh menutup “Tidak dapat diverifikasi” dengan alasan dan label kekurangan bukti, bukan mengaku kejadian tidak terjadi. Mekanisme itu tidak boleh menggantikan data wajib Gol/Assist/partisipasi/presensi.

MVP laga/bulanan tetap hasil voting dan badge tanpa tambahan poin. Tidak ada bonus universal atau konversi ×10. Penghargaan Champion memberi mentah Achievement 56, bukan mulai 0; Perfect Attendance memberi Commitment 54.

| Pelanggaran | Potongan kategori | Lingkup/status |
|---|---|---|
| Kartu kuning | Discipline −3 | Per kartu sah, per pertandingan |
| Kartu merah | Discipline −10; Teamwork −3 | Per kartu sah, per pertandingan |
| Outlist setelah tim diacak tanpa alasan mendesak | Discipline −12; Commitment −8 | Sekali per kejadian event; pengacakan sudah disimpan dan diumumkan |
| Terlambat 1–10 menit | Discipline −2 | Satu tingkat keterlambatan per event |
| Terlambat 11–20 menit | Discipline −4; Commitment −2 | Satu tingkat keterlambatan per event; angka ditetapkan pengguna 12 November 2026 |
| Terlambat lebih dari 20 menit | Discipline −6; Commitment −3 | Satu tingkat keterlambatan per event; angka ditetapkan pengguna 12 November 2026 |
| Tidak hadir tanpa alasan mendesak | Discipline −15; Commitment −12; Teamwork −5 | Sekali per kejadian event setelah verifikasi admin; bukan Commitment −17 |
| Tidak mengikuti pertandingan tanpa alasan yang disetujui | Commitment −4 | Per laga yang seharusnya diikuti, setelah admin memverifikasi tanggung jawab dan alasan |


**Klarifikasi final pengguna 12 November 2026:** terlambat 11–20 Commitment −2, >20 Commitment −3; absen Discipline −15/Commitment −12/Teamwork −5. Tidak ada Commitment −17 atau potongan positif. Ketiga baris kini boleh dievaluasi dengan versi aturan lengkap setelah status, tanggung jawab dan alasan diverifikasi admin; bukan otomatis menghukum karena nominal sudah tersedia.

Keterlambatan diukur dari batas hadir yang diumumkan ke waktu aktual: tepat waktu ≤0, pertama >0–10, kedua >10–20, ketiga >20 menit. Hanya satu tingkat; tidak ada toleransi bebas penalti 10 menit. Alasan mendesak yang disetujui membebaskan sesuai aturan. Kartu per kejadian laga bersifat kumulatif: satu kuning lalu merah −13 Discipline/−3 Teamwork; dua kuning lalu merah −16/−3 jika ketiganya sah. Pengecualian penggantian kartu memerlukan peraturan komunitas berversi; entri kartu sama tidak boleh ganda.

Outlist sebelum pembagian bebas penalti. Setelah pengacakan disimpan/diumumkan, verifikasi alasan; persetujuan keluar roster tidak otomatis alasan mendesak, tetapi tidak ditambah absen. Tidak hadir seluruh event tidak ditambah tiga penalti melewatkan laga; tidak tiba berarti absen, bukan terlambat. Laga terlewat akibat keterlambatan sama tidak ditambah −4; penolakan laga berbeda yang memang wajib diikuti dapat −4. Dicadangkan, cedera disetujui, larangan akibat merah atau laga batal tidak otomatis penolakan. Gol/Assist nol bukan bukti tidak hadir.

### 13.3. Status, validasi dan notifikasi penilaian

| Entitas / state | Asal | Pemicu/aktor | Kondisi | Tujuan dan efek | Gagal/pemulihan |
|---|---|---|---|---|---|
| Penilaian event / STATE-18 | Belum dibentuk | Simpan pembagian/pengelola | Roster sah, versi aturan tersedia | Sementara; mentah 50 sekali | Pratinjau/batal tidak membuat saldo |
| Penilaian event / STATE-18 | Sementara | Periksa/pengelola | Skor/data wajib lengkap; metadata terdisposisi; insiden diputuskan | Siap difinalkan | Tampilkan sumber kurang, bukan nol |
| Penilaian event / STATE-18 | Siap difinalkan | Finalkan/pengelola | Versi sumber terbaru dan semua prasyarat | Final; snapshot mentah/maksimum/relatif dan posting profil konsisten | Konflik mempertahankan versi sebelumnya; muat ulang |
| Profil / STATE-19 | Versi berlaku | Finalisasi/koreksi/proses terkait | Sumber aktif unik dan urutan event diketahui | Versi baru setelah replay; histori lama tetap | Saat menghitung ulang tampil status, jangan campur angka versi |
| Penalti / STATE-22 | Dicatat | Ajukan pencatat | Pemain/laga/waktu/jenis dan alasan tersedia | Menunggu verifikasi | Input dipertahankan dan field salah ditandai |
| Penalti / STATE-22 | Menunggu verifikasi | Putuskan/admin cakupan | Bukti tanggung jawab/status/alasan cukup dan versi aturan terkonfirmasi | Disahkan memberi delta; Ditolak tidak memberi delta | Status/alasan belum diverifikasi tetap menunggu |
| Koreksi penalti / STATE-20 | Belum ada | Ajukan/pemain terkait | Penalti miliknya, alasan/bukti | Diajukan → Ditinjau; potongan aktif belum otomatis hilang | Cek status sebelum retry, tidak membuat permohonan ganda |
| Koreksi penalti / STATE-20 | Ditinjau | Putuskan/admin lain bila konflik diri | Alasan keputusan wajib | Diterima merevisi sumber/replay; Ditolak mempertahankan dengan alasan | Tidak boleh diputus akun tanpa kewenangan |
| Metadata / STATE-21 | Belum diketahui | Konfirmasi/petugas lalu pengelola | Sesuai hasil dan roster | Terverifikasi ada/tidak ada atau Tidak berlaku | Data bertentangan ditolak; tetap belum diketahui |
| Metadata / STATE-21 | Belum diketahui | Tutup/admin | Alasan tidak dapat membuktikan kejadian | Tidak dapat diverifikasi, bonus ditahan dan kekurangan bukti berlabel | Tidak berlaku untuk data statistik wajib |

Skor final STATE-09 tidak otomatis berarti STATE-18 final. Insiden/perubahan sumber setelah final membuat revisi yang memerlukan pengesahan ulang; snapshot final lama tetap berlabel versi sampai versi baru siap. Koreksi tidak boleh menghapus audit.

| Field/aksi | Pemeriksaan dan pesan | Pemulihan |
|---|---|---|
| Gol/Assist | Nonnegatif, integer, pemain tampil, satu tim, total konsisten; “Rincian Gol/Assist belum cocok dengan skor.” | Periksa roster/angka/gol bunuh diri; data tetap tersimpan sebagai draft |
| Posisi event | Salah satu empat posisi, snapshot pra-main; perubahan pascahasil beralasan | Tinjau dampak badge/bobot, hitung ulang, bukan mengejar nilai terbaik |
| Gol pertama/penentu | Pemain punya Gol dan memenuhi tim/hasil; “Kejadian belum sesuai hasil pertandingan.” | Perbaiki atau tandai belum diketahui |
| Pengesahan penalti | Status, waktu, kewajiban laga dan alasan admin; “Status atau alasan kejadian belum diverifikasi.” | Lengkapi verifikasi dan tinjau potongan kategori yang sudah ditetapkan |
| Q/maksimum | Kosong atau maksimum 0 bukan error data angka nol | Tampilkan state khusus dan jangan menerbitkan overall palsu |
| Simpan bersamaan/tidak diketahui | Bandingkan versi/status terbaru; “Data telah berubah. Tinjau versi terbaru.” | Cek status lalu muat ulang dan pratinjau dampak sebelum simpan ulang |
| Finalisasi/replay gagal | Jangan menampilkan separuh profil baru dan separuh nilai lama | Pertahankan versi final lama berlabel, proses ulang sumber unik |

Notifikasi penilaian hanya konfirmasi/status dalam situs pada SCR-12/18/19/22, tidak menambah kanal WhatsApp/email yang tidak diminta. Pengajuan koreksi memperlihatkan nomor/konteks dan status; admin melihat antrean di SCR-19. Keputusan beralasan terlihat kepada pemohon di SCR-12/panel SCR-19 sesuai izin. Kegagalan memuat umpan balik tidak menggandakan permohonan atau membatalkan hasil tersimpan. Tidak ada pengiriman pesan nyata dalam tugas dokumentasi.

### 13.4. Alur baru dan perluasan layar lama

### FLOW-22 — Poin mentah dan nilai relatif event

FEAT-03/05/07/09/10/28 · US-29 · AC-91–AC-98, AC-131, AC-133. Aktor pemain terkait/pengelola.

1. SCR-14 memilih preset penilaian empat tim dan versi aturan; SCR-16 menyimpan pembagian setelah posisi dikonfirmasi. Poin awal 50 terbentuk sekali; antartim menggunakan susunan diterima, tidak diacak lintas tim.
2. SCR-10 mencatat Gol/Assist pada dua tim, pemain tampil, kartu dan panel kejadian khusus. Kirim hasil bukan finalisasi bagi pencatat. FLOW-07 tetap memisahkan pengiriman/finalisasi skor.
3. SCR-12/24 subview Statistik Saya memilih Event. Tampilkan identitas/konteks, jumlah laga, poin mentah pada rincian dan nilai relatif sebagai spider chart dengan label jelas; angka sumber tetap tersedia sesuai 13.7. Awal event belum final, bukan prestasi 100 sebelum main.
4. Jika pemain tampil dan data tersedia, kategori dinormalisasi atas Q_e; tidak tampil menampilkan — dan alasan. Angka 100 dapat berarti setara seluruh pembanding, bukan bebas kartu.
5. Lihat rincian membuka panel ledger SCR-22 terbatas pada sumber miliknya; hasil sumber ke SCR-09. Loading/error mempertahankan filter, Coba lagi hanya memuat, bukan mem-posting poin.

### FLOW-23 — Profil akumulasi dan replay histori

FEAT-10 · US-30 · AC-99–AC-105, AC-132. Aktor pemain/petugas berhak; dependensi sumber final pada FLOW-26.

1. Pilih Profil pada SCR-12/Statistik Saya; jelaskan “Akumulasi seluruh permainan”, baseline 50 satu kali, jumlah event/penampilan. Gunakan Spider sebagai satu-satunya jenis grafik, tanpa normalisasi ulang angka profil. Belum ada riwayat tidak membuat penampilan palsu.
2. Telusuri event menurut waktu main/ID. Setiap event menambah delta bersih lalu cap 0–100; ledger menampilkan nominal/efektif dan versi. Filter rekap Gol tidak menjadi filter perhitungan karier.
3. Overall mengikuti perubahan efektif dengan posisi/bobot historis. Memilih event lain tidak membawa nilai relatif ke profil atau mengubah modal event berikutnya.
4. Koreksi lama mereplay dari event paling awal yang berubah sampai terbaru, termasuk streak/overall. Sampai konsisten, tampilkan versi final sebelumnya dan status perhitungan ulang; retry tidak membuat delta kedua.
5. Koreksi pembanding lain hanya mengubah relatif event, bukan profil pemain yang sumbernya sama. Pengguna tanpa hak yang membuka tautan profil rinci ditolak tanpa nilai privat; Kembali ke direktori tetap tersedia.

### FLOW-24 — Achievement individu/tim dan streak

FEAT-07/08/09/28 · US-28/US-31 · AC-78, AC-106–AC-114. Aktor petugas/pengelola, pemain membaca hasil diri.

1. Setelah laga sah, evaluasi sumber katalog; Gol/Assist, tampil dan posisi membatasi kandidat. Kejadian khusus belum diketahui menahan award terkait.
2. Cocokkan aturan tumpukan dan kunci unik pemain/kode/laga; sumber event memakai pemain/kode/event. Jangan memberi Brace dan Hat-Trick bersamaan.
3. Setelah event lengkap/klasemen final, evaluasi Champion/Runner-Up, Best Defense/Attack, Unbeaten/Perfect Victory dan Last Place/Winless sekali. Tampilkan penerima dan potongan terarah; seri posisi belum sah menahan award posisi saja sampai pengesahan.
4. Urutan event komitmen mengevaluasi streak; event penutup tiga/enam memberi +4 sekali per blok. Pembatalan/koreksi mengoreksi sumber award dan replay profil.
5. Pemain melihat badge, syarat, angka dan sumber pada SCR-12, rincian di SCR-22. Voting SCR-25/34 tetap badge tanpa bonus statistik; peringkat statistik tidak menentukan pemenang voting.

### FLOW-25 — Insiden, verifikasi dan pengajuan koreksi

FEAT-09/02 · US-32 · AC-115–AC-123. Aktor pencatat, admin kegiatan, pemain terkait.

1. Pencatat memasukkan kartu di SCR-10; admin memasukkan status hadir/waktu/outlist/laga terlewat di SCR-19 dengan alasan. Sumber dicatat belum otomatis sah.
2. Admin membuka panel verifikasi SCR-19; periksa snapshot pengacakan, waktu, kewajiban laga dan alasan. Bukti, status atau alasan yang belum lengkap mempertahankan Menunggu verifikasi tanpa potongan final; nominal terlambat/absen sudah ditetapkan.
3. Putusan sah menghasilkan delta kategori; Tolak insiden tidak menghasilkan potongan. Outlist/absen/terlambat yang berasal kejadian sama direkonsiliasi dahulu; kartu terpisah tetap kumulatif.
4. Pemain membuka detail penalti dari SCR-12, memilih Ajukan koreksi, mengisi alasan/bukti dan mengirim. Batal tidak mengirim; timeout Cek status sebelum retry. Dalam peninjauan tidak otomatis mencabut keputusan aktif.
5. Admin yang tidak berkepentingan pada kasus dirinya meninjau dan memutuskan dengan alasan. Diterima merevisi sumber; Ditolak mempertahankan keputusan dengan penjelasan. Status tersedia hanya bagi pemilik/petugas, bukti tidak dibuka ke direktori.
6. Putusan dan hasil replay ke SCR-12/22; jika proses gagal, pertahankan versi berlaku dan opsi pemulihan. Tidak ada pemberitahuan WhatsApp/email baru.

### FLOW-26 — Pengesahan poin berversi dan koreksi konsisten

FEAT-07/10/28 · US-33 · AC-124–AC-130. Aktor pengelola berhak.

1. SCR-18 mode Penilaian event memuat versi sumber: skor, Gol/Assist, partisipasi, presensi, metadata, keputusan insiden dan klasemen. Tampilkan kelengkapan per sumber dan aturan pembekuan saat pembagian.
2. Pengelola menyelesaikan data wajib/disposisi metadata. Pastikan versi aturan memuat nominal terkonfirmasi pengguna. Insiden dengan verifikasi belum lengkap menghalangi poin final sampai admin memutuskan; tidak ada lagi hambatan tanda Commitment pada tabel aktif.
3. Pratinjau memuat mentah, Q/maksimum, relatif, badge/penalti, delta profil/overall dan event selanjutnya yang terdampak. Bila versi berubah sebelum simpan, muat ulang dan tinjau dampak baru.
4. Konfirmasi menerbitkan snapshot poin dan mengganti kontribusi profil tepat sekali. Versi sama yang diulang hanya mengembalikan hasil. Jangan menggandakan baseline/award atau memublikasikan snapshot campur.
5. Koreksi final memuat alasan dan versi lama/baru. Replay seluruh event selanjutnya, bukan menambah selisih pada profil terkini. Pengubahan maksimum A memperbarui relatif B tanpa mengubah profil B.
6. Pembatalan sumber menarik poin dengan audit dan replay; batal event tidak otomatis menghapus histori. Event terpotong mempertahankan sumber sah dengan status yang jelas dan tidak memberi badge event lengkap secara palsu.

### 13.5. Contoh hitung acuan dan keputusan terbuka

Contoh enam pertandingan/16 pemain, sumber Gol/Assist, urutan kejadian, sembilan maksimum dan overall ada pada [rencana, simulasi v2.7](./00-implementation-plan.md#1310-hasil-review-dan-simulasi-sintetis-v27). Keempat contoh Tim A mempunyai mentah: Striker F66/P50/D50/TW54/Disc50/Com54/Cons50/I62/Ach56; Midfielder F50/P63/D50/TW54/Disc50/Com54/Cons50/I52/Ach56; Defender dan Goalkeeper F50/P50/D63/TW54/Disc50/Com54/Cons50/I52/Ach56. Maksimum 66/63/63/54/50/54/50/62/56. Overall relatif 94,84/93,31/96,95/99,19; profil event pertama sesuai poin mentah, bukan nilai relatif.

Kasus cap: +70 lalu −3 memberikan 100 lalu 97; koreksi event pertama menjadi +40 mereplay menjadi 90 lalu 87, bukan menambahkan selisih −30 ke saldo 97 (hasil salah 67). Semua sama Discipline 47 memberi relatif 100 bersama, tetapi profil terkena −3; angka relatif bukan pembebasan pelanggaran. Baseline profil/bobot/kebobolan/tumpukan kini baseline dokumentasi dari persetujuan rencana, tetap perlu ditinjau klub sebelum produksi dan dipilotkan; bukti aritmetis tidak membuktikan adil secara statistik. Nominal Commitment/absen telah ditetapkan pengguna 12 November; konten asli/provider dan kebijakan operasional lainnya tetap terbuka.

### 13.6. Contoh verifikasi penalti terkonfirmasi v2.3

| Kejadian sah tanpa perubahan lain | Discipline mentah/profil pertama | Commitment mentah/profil pertama | Teamwork mentah/profil pertama | Overall profil pertama |
|---|---|---|---|---|
| Terlambat 11–20 menit | 46 | 48 | 50 | 49,70 |
| Terlambat >20 menit | 44 | 47 | 50 | 49,55 |
| Tidak hadir tanpa alasan mendesak | 35 | 38 | 45 | 48,40 |

Semua contoh mulai 50, memakai bobot 5% pada tiga kategori tersebut, mengasumsikan verifikasi admin selesai dan tanpa award/potongan lain. Nilai relatif berbeda: bila maksimum pembanding 50, keterlambatan 11–20 menghasilkan Discipline 92/Commitment 96; >20 menghasilkan 88/94. Pemain absen tanpa penampilan tidak memperoleh nilai relatif/peringkat, tetapi potongan profil tetap diproses. Jika terlambat tetap tampil ketiga laga, Perfect Attendance +4 dapat mengubah Commitment; hitung kedua sumber yang berbeda, bukan menghapus fakta terlambat.

Versi aturan yang memuat klarifikasi dipasang sebelum pengesahan insiden tertunda. Koreksi event yang sudah final mengikuti persetujuan admin, alasan, snapshot sumber dan replay, tidak mengubah histori tanpa audit. Angka lama yang ambigu tidak pernah menjadi nilai final dan tidak boleh dipakai sebagai potongan kedua. AC-117/AC-118 mempertahankan ID dan kini menguji nominal eksplisit, bukan menunggu keputusan yang sudah diterima.

### 13.7. Penyajian statistik hanya Spider

FEAT-10/28 · US-29/US-30 · AC-131–AC-133 · FLOW-22/FLOW-23 · SCR-12/SCR-24. Spider/radar adalah satu-satunya jenis grafik. Tidak ada kontrol atau preferensi jenis grafik. Tab Event/Profil menentukan seri aktif; pemilih event menentukan rincian event, bukan modal atau perhitungan karier. Event terpilih tetap dipertahankan saat kembali dari Profil. Pergantian konteks hanya membaca sumber yang sesuai izin; tidak mem-posting atau menghitung ulang poin.

Seri Event = nilai relatif sembilan kategori untuk event terpilih, dengan helper “Tertinggi tiap kategori = 100”; seri Profil = akumulasi sembilan kategori personal, helper “Akumulasi seluruh permainan”. Jangan tumpuk kedua seri yang berbeda makna pada satu spider atau mengganti angka profil dengan normalisasi event. Overall tetap kartu angka terpisah. Poin mentah >100 tetap di ledger, tidak dicap untuk membuat bentuk grafik.

Sumbu selalu urutan tetap searah jarum jam mulai Finishing di atas: Finishing, Playmaking, Defending, Teamwork, Discipline, Commitment, Consistency, Impact, Achievement. Spider memiliki tepat sembilan sumbu, pusat 0, cincin 25/50/75/100 dan maksimum 100; tidak menyesuaikan skala otomatis agar profil tampak penuh. Tidak ada sumbu untuk overall atau metrik tambahan.

Spider menyediakan label/nilai tanpa hover wajib, judul konteks dan tabel/daftar numerik aksesibel dengan nama kategori lengkap. Tab Event/Profil dan pemilih event dapat dioperasikan keyboard dengan status terpilih yang terbaca; pergantian konteks tidak memindahkan fokus tanpa alasan. Warna gold, outline/titik dan label teks membedakan seri dari grid, bukan warna saja. Jika label pendek diperlukan di HP, legenda numerik memakai nama lengkap; label/angka tidak boleh terpotong. Area chart responsif tidak menyebabkan scroll horizontal halaman.

Loading, kosong, tidak tampil, Q_e kosong/maksimum 0 dan sumber belum lengkap mengikuti status statistik semula. Nilai belum tersedia ditampilkan —/penjelasan, bukan 0; jangan menutup poligon sembilan nilai dengan mengarang titik kosong. Jika seri belum lengkap, tampilkan state “Grafik belum lengkap” dan daftar nilai sah; nol yang memang terkonfirmasi tetap titik 0. Gagal menggambar tidak menghilangkan tabel, filter atau aksi rincian.
