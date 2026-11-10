# Product Requirements Document — Asker FC

- Versi: 2.1 · Tanggal: 10 November 2026
- Status: kebutuhan diperluas dan diselaraskan; aturan operasional usulan belum disahkan untuk produksi.
- Upstream: [Product Planning v2.1](./01-product-planning.md), [Implementation Plan v2.0](./00-implementation-plan.md). Revisi v2.0 memproses rencana v1.8; ID lama dipertahankan dan kebutuhan baru ditambahkan.
- Asumsi relevan: satu komunitas, peserta 18+, 100–500 anggota, koneksi internet, proses saat ini belum divalidasi lapangan.

## 1. Produk, masalah dan nilai

**Tujuan utama: memudahkan anggota tahu jadwal main, daftar, dan bayar, sekaligus membangun rasa kebersamaan tim.** Jadwal, pendaftaran dan pembayaran membentuk pengalaman inti anggota menuju kegiatan Kick & Kumpul. Alur utama: **buka link → lihat jadwal → RSVP → terima pengingat → main → lihat skor dan foto → bayar iuran**. Kas & Iuran dapat dibuka langsung tanpa menunggu foto/hasil. Biaya kegiatan di muka hanya cabang jika diumumkan sejak awal; kegiatan gratis tidak menimbulkan tagihan kegiatan tetapi iuran bulanan tetap terpisah.

Asker FC adalah **website komunitas fun football berkonsep “Kick & Kumpul”**, yang memudahkan anggota main bola, bertemu teman, dan berkumpul melalui sesi rutin. Anggota memperoleh kepastian slot, teman satu tim dan jadwal; pengelola memperoleh satu catatan operasional. Liga, turnamen, hasil dan klasemen menjadi sarana pendukung kegiatan komunitas. Pencatat hasil dan kapten menjalankan tugas dengan kewenangan terbatas.

Hipotesis masalah awal adalah informasi pendaftaran, jadwal dan skor tersebar di chat dan spreadsheet. Hipotesis ini tidak diperlakukan sebagai hasil wawancara. Keberhasilan produk dinilai dari penyelesaian kegiatan dan pengurangan bantuan/rekap, bukan jumlah fitur yang dibuat.

## 2. Target pengguna dan konteks

| Aktor | Kebutuhan dan perilaku (hipotesis) | Batas kewenangan |
|---|---|---|
| Anggota | Membuka tautan dari grup di HP, mengetahui jadwal main, daftar, membayar bila berbayar dan mengenal teman satu tim | Data sendiri dan informasi kegiatan yang diizinkan |
| Kapten | Memilih roster dan memantau persetujuan anggota | Tim yang ditugaskan, dalam kegiatan terkait |
| Pencatat skor | Mengirim hasil dari lapangan | Pertandingan yang ditugaskan; bukan memfinalkan karena peran pencatat saja |
| Pengelola | Mengatur kegiatan, peserta, jadwal dan validasi | Kegiatan yang ditugaskan |
| Superadmin | Mengelola keanggotaan dan penugasan akses | Seluruh komunitas MVP |

Semua fungsi harus dapat digunakan di HP. Satu akun boleh memiliki beberapa peran; tindakan mengikuti cakupan penugasan yang berlaku. Pengguna tanpa akses tidak memperoleh data hanya dengan mengetahui tautannya.

Identitas visual memakai logo asli Asker FC, **hitam dan gold dominan sesuai logo**, ditetapkan pengguna 10 November 2026, judul tebal, foto aksi asli anggota dengan izin, serta bahasa akrab/humoris, dengan keterbacaan dan kejelasan status tetap mengikuti NFR-03/NFR-04. Revisi visual tidak mengubah fungsi, hak akses atau target KPI.

Superadmin membuat/menyalin draft lalu menunjuk pengelola. Admin dapat mengonfigurasi dan memublikasikan draft yang ditugaskan, bukan membuat akses sendiri ke kegiatan di luar penugasannya.

## 3. Goals dan success metrics

Definisi mengikuti Planning v2.0 dan tidak diubah di PRD. KPI mengukur kelancaran operasional untuk mendukung pengalaman Kick & Kumpul. Observasi pilot memeriksa apakah anggota dapat menemukan jadwal, menyelesaikan pendaftaran dan proses bayar/statusnya; umpan balik kualitatif memeriksa rasa dilibatkan dan kebersamaan tim. Kehadiran atau skor tidak otomatis menjadi bukti kebersamaan. Baseline seluruh KPI **belum diukur**; target **usulan**. Denominator nol berarti tidak berlaku.

| Goal/KPI | Definisi operasional | Target usulan dan periode | Cara ukur |
|---|---|---|---|
| GOAL-01 / KPI-01 | Peserta terdaftar yang menyelesaikan sendiri pendaftaran/konfirmasi ÷ seluruh peserta terdaftar ×100%; berbasis orang pada kedua mode | ≥80% per kegiatan selama empat kegiatan pilot | Pelaku konfirmasi dan peserta unik |
| GOAL-02 / KPI-02 | Menit kerja aktif pengelola dari penutupan peserta sampai publikasi jadwal pertama; jeda nonkerja dikecualikan | Median turun ≥30% terhadap simulasi manual setara, empat kegiatan pilot | Catatan waktu pengelola |
| GOAL-03 / KPI-03 | Laga selesai aktual dengan hasil final ≤15 menit dari selesai aktual ÷ seluruh laga selesai aktual ×100%; hasil belum final tetap denominator | ≥90% per kegiatan pilot | Waktu selesai aktual dan finalisasi |
| GOAL-04 / KPI-04 | Peserta terdaftar dengan hadir/tidak hadir eksplisit ÷ peserta terdaftar saat penutupan presensi ×100% | ≥95% pada akhir tiap kegiatan pilot | Rekap presensi |
| GOAL-05 / KPI-05 | Keputusan verifikasi/penolakan dengan verifikator, waktu dan alasan saat ditolak ÷ seluruh keputusan verifikasi/penolakan ×100%; termasuk iuran bulanan | 100% tiap kegiatan berbayar atau periode bulanan pilot | Riwayat keputusan pembayaran |

## 4. Skenario utama

1. **Sesi individu:** pengelola menyiapkan kegiatan → anggota mendaftar → kuota dikonfirmasi → tim dibagi dan dikunci → jadwal dipublikasikan → presensi dicatat → hasil dikirim dan difinalkan → anggota melihat klasemen/statistik.
2. **Liga antartim:** kapten mengajukan tim → anggota mengonfirmasi undangan → roster memenuhi batas dan diajukan → pengelola menerima tim → jadwal liga disusun → pertandingan berjalan.
3. **Koreksi hasil:** pengelola membuka hasil final yang salah → mencatat alasan dan hasil revisi → memeriksa dampak → menyimpan koreksi → peringkat dan statistik diperbarui dengan riwayat tetap terlihat.
4. **Kegiatan berbayar:** peserta atau kapten melihat tagihan → mengunggah bukti → pengelola menerima/menolak → pengirim melihat keputusan. Status bayar tidak disamakan dengan status peserta.
5. **Sesi rutin:** pengelola menyalin konfigurasi kegiatan lama → memilih tanggal baru → mengosongkan peserta/hasil lama → menjalankan laga persahabatan tanpa klasemen liga.

## 5. Scope dan kebutuhan fungsional MVP

Setiap US mengacu fitur Planning. Setiap AC memuat kondisi, aksi dan hasil yang dapat diamati; detail mekanika terdapat dalam Product Details.

### FEAT-01 — Akun dan anggota

**US-01 → FEAT-01:** As a member, I want to sign in with email or Google and maintain my profile so that I can participate with one verified identity.

- **AC-01 → US-01:** Given email belum terdaftar dan pengguna menyatakan usia ≥18, when formulir valid dikirim dan email diverifikasi, then akun dapat mengakses kegiatan yang diizinkan dan persetujuan kebijakan tercatat.
- **AC-02 → US-01:** Given pengguna memilih Google, when verifikasi penyedia berhasil dan profil wajib dilengkapi, then pengguna dapat masuk; bila ada identitas lama yang perlu ditautkan, then pengguna harus membuktikan kendali akun lama sebelum penyatuan.
- **AC-03 → US-01:** Given usia tidak memenuhi syarat, email belum diverifikasi atau akun dinonaktifkan, when pengguna mencoba mendaftar kegiatan, then akses ditolak dengan alasan yang sesuai tanpa membuat peserta.
- **AC-04 → US-01:** Given pengguna lupa sandi, when permintaan pemulihan diajukan, then tampil pesan yang sama untuk email terdaftar/tidak terdaftar; tautan pemulihan valid hanya sekali dan tautan kedaluwarsa menawarkan permintaan ulang.

### FEAT-02 — Hak akses dan penugasan

**US-02 → FEAT-02:** As a superadmin, I want to assign scoped roles so that each operator can manage only their responsibilities.

- **AC-05 → US-02:** Given superadmin memilih anggota aktif dan objek penugasan yang valid, when penugasan disimpan, then anggota memperoleh hanya tindakan untuk cakupan itu dan riwayat perubahan tercatat.
- **AC-06 → US-02:** Given anggota tidak ditugaskan pada kegiatan/tim/laga, when membuka tautan atau mencoba mengubah data terbatas, then data/tindakan ditolak dan data tetap sama.
- **AC-07 → US-02:** Given hak akses dicabut atau akun dinonaktifkan, when permintaan berikutnya dilakukan, then kewenangan lama tidak berlaku; sistem mencegah pencabutan superadmin aktif terakhir.

### FEAT-03 — Kegiatan

**US-03 → FEAT-03:** As an organizer, I want to configure and publish an event so that members can join under clear rules.

- **AC-08 → US-03:** Given kegiatan masih draft, when pengelola mengisi jenis liga/rutin, satu mode daftar, kapasitas, tanggal, lokasi, aturan dan biaya bila ada, then kegiatan dapat dipublikasikan jika seluruh kondisi wajib valid.
- **AC-09 → US-03:** Given ada pendaftaran terkonfirmasi, when pengelola mencoba mengganti mode daftar, then perubahan ditolak; perubahan konfigurasi lain yang memengaruhi peserta/jadwal memperlihatkan dampak dan memerlukan konfirmasi.
- **AC-10 → US-03:** Given kegiatan lama dipilih untuk disalin, when salinan dibuat, then hanya konfigurasi disalin menjadi draft; peserta, bukti bayar, skor dan presensi tidak ikut.
- **AC-11 → US-03:** Given kegiatan dipublikasikan, when pengelola membatalkannya dengan alasan, then status pembatalan dan alasan terlihat bagi peserta, pendaftaran baru ditutup, dan catatan yang sudah ada dipertahankan.

### FEAT-04 — Pendaftaran dan roster antartim

**US-04 → FEAT-04:** As a member or captain, I want to confirm participation with a valid roster so that my event slot is unambiguous.

- **AC-12 → US-04:** Given mode individu terbuka dan kuota tersedia, when anggota aktif mengonfirmasi, then satu pendaftaran terdaftar dibuat; permintaan ganda mengembalikan pendaftaran yang sama, bukan tambahan kuota.
- **AC-13 → US-04:** Given mode antartim, when kapten mengundang anggota berakun dan mereka menyetujui, then roster yang memenuhi batas dapat diajukan dan diterima pengelola; anggota yang belum menyetujui tidak dihitung sebagai pemain terkonfirmasi.
- **AC-14 → US-04:** Given kuota penuh, periode tertutup, anggota sudah masuk roster kegiatan, atau roster tidak memenuhi syarat, when pendaftaran/konfirmasi dicoba termasuk secara bersamaan, then pelanggaran ditolak tanpa melebihi kuota atau menduplikasi peserta.
- **AC-15 → US-04:** Given roster belum terkunci, when anggota membatalkan dengan konfirmasi, then status menjadi dibatalkan dan kuota dilepas; jika roster sudah terkunci, then pengunduran diarahkan ke pengelola dan tidak mengubah hasil lama.

### FEAT-05 — Pembagian dan penguncian tim

**US-05 → FEAT-05:** As an organizer, I want to preview and adjust automatically allocated teams so that confirmed individuals have a usable roster.

- **AC-16 → US-05:** Given peserta individu terkonfirmasi dan konfigurasi kapasitas layak, when pembagian diminta, then setiap peserta muncul tepat satu kali dan selisih jumlah pemain antartim maksimal satu, pada mode acak. Mode skill menggunakan penilaian sah, memperlihatkan dasar keseimbangan serta kekurangannya, dan tidak menjanjikan keseimbangan sempurna.
- **AC-17 → US-05:** Given pratinjau atau edit roster belum terkunci, when pengelola memindahkan pemain atau mengacak ulang, then pemeriksaan duplikasi/kapasitas dijalankan; pembatalan pratinjau mempertahankan versi tersimpan.
- **AC-18 → US-05:** Given roster memenuhi batas, when pengelola menguncinya, then kapten/anggota tidak dapat mengubah roster; koreksi oleh pengelola memerlukan alasan dan pemeriksaan dampak pada jadwal/partisipasi.

### FEAT-06 — Jadwal

**US-06 → FEAT-06:** As an organizer, I want to publish a feasible match schedule so that teams know when and where to play.

- **AC-19 → US-06:** Given liga satu putaran dengan n tim terkunci, when jadwal dibuat, then terdapat n(n−1)/2 pasangan unik; tim ganjil mendapat jeda tanpa pertandingan palsu. Laga rutin dapat disusun sebagai persahabatan tanpa klasemen.
- **AC-20 → US-06:** Given slot, durasi, lapangan, jeda dan istirahat minimum ditentukan, when jadwal dibuat/diubah, then benturan tim/lapangan dan pelanggaran slot ditolak; bila tidak layak, kendala ditampilkan dan jadwal sebelumnya tidak ditimpa.
- **AC-21 → US-06:** Given pratinjau valid, when dipublikasikan, then peserta melihat versi terbaru dan waktunya; pertandingan yang sudah final tidak dapat dijadwal ulang dengan menghapus hasilnya.

### FEAT-07 — Hasil dan koreksi

**US-07 → FEAT-07:** As a scorekeeper or organizer, I want to submit and validate match results so that official results are accountable.

- **AC-22 → US-07:** Given petugas ditugaskan pada laga, when skor bilangan bulat nonnegatif dan waktu selesai aktual yang valid dikirim, then hasil berstatus diajukan; klasemen belum berubah. Rincian pencetak gol boleh belum lengkap dan ditandai demikian.
- **AC-23 → US-07:** Given hasil diajukan, when pengelola memfinalkan setelah pemeriksaan, then hasil final memperbarui klasemen/statistik satu kali. Pencatat tanpa peran pengelola tidak dapat memfinalkan.
- **AC-24 → US-07:** Given hasil final, when pengelola mengonfirmasi koreksi beserta alasan, then versi baru menjadi resmi, dampak dihitung ulang dan versi lama tetap dapat ditelusuri.
- **AC-25 → US-07:** Given data sudah berubah oleh petugas lain atau hasil penyimpanan belum diketahui karena koneksi, when pengguna mencoba menyimpan ulang, then sistem memeriksa status terbaru dan mencegah penimpaan diam-diam maupun hasil ganda.

### FEAT-08 — Klasemen

**US-08 → FEAT-08:** As a member, I want to view standings based on official results so that I can understand the league position.

- **AC-26 → US-08:** Given hasil final liga tersedia, when klasemen dibuka atau hasil dikoreksi, then main/menang/seri/kalah/gol/selisih/poin konsisten dengan hasil final dan aturan kegiatan; hasil diajukan, ditunda dan dibatalkan tidak dihitung sebagai 0–0.
- **AC-27 → US-08:** Given poin, selisih dan gol memasukkan sama, when peringkat ditampilkan, then tim mendapat peringkat bersama; aturan penentuan juara yang ditetapkan sebelum liga ditampilkan, tanpa tiebreak ad hoc.

### FEAT-09 — Presensi dan partisipasi

**US-09 → FEAT-09:** As an organizer or assigned scorekeeper, I want to record attendance and match participation separately so that involvement is accurate.

- **AC-28 → US-09:** Given pengelola membuka daftar peserta kegiatan, when hadir/tidak hadir disimpan, then pencatat dan waktu tercatat; peserta yang belum dicatat tetap “belum dicatat”, bukan otomatis tidak hadir.
- **AC-29 → US-09:** Given pencatat mengisi partisipasi laga, when pemain roster dipilih sebagai tampil, then penampilan terkait laga tersebut; presensi kegiatan tidak otomatis membuat semua anggota tampil di setiap laga. Pemain tidak hadir memerlukan koreksi presensi sebelum dicatat tampil.
- **AC-30 → US-09:** Given kegiatan/hasil telah selesai, when koreksi presensi/partisipasi dilakukan oleh pengelola dengan alasan, then statistik diperbarui tanpa menduplikasi penampilan.

### FEAT-10 — Statistik dasar

**US-10 → FEAT-10:** As a member, I want to see attendance, appearances and goals with completeness labels so that I can interpret my contribution correctly.

- **AC-31 → US-10:** Given data sah tersedia dan filter periode/kegiatan dipilih, when statistik ditampilkan, then kehadiran dihitung per kegiatan, penampilan per laga final dan gol dari rincian gol hasil final; gol bunuh diri tidak dikreditkan pada pemain lawan.
- **AC-32 → US-10:** Given rincian pencetak gol belum lengkap atau belum ada laga final, when statistik dibuka, then kelengkapan/keadaan kosong ditampilkan dan tidak mengarang angka pemain; koreksi hasil tercermin pada ringkasan.

### FEAT-11 — Pembayaran manual

**US-11 → FEAT-11:** As a payer or organizer, I want to submit and verify transfer evidence so that event fees are traceable.

- **AC-33 → US-11:** Given kegiatan berbayar, when peserta individu atau kapten tim membuka tagihan, then nominal, tujuan transfer, tenggat dan dampak belum bayar terlihat; kegiatan gratis tidak meminta bukti.
- **AC-34 → US-11:** Given pengirim berhak dan berkas memenuhi batas, when bukti diunggah, then status menjadi menunggu verifikasi dan hanya pengirim serta pengelola berwenang yang dapat mengakses bukti.
- **AC-35 → US-11:** Given bukti menunggu verifikasi, when pengelola menerima/menolak, then keputusan, pelaku dan waktu tercatat; penolakan wajib beralasan dan pengirim dapat mengirim bukti pengganti dengan riwayat terjaga.
- **AC-36 → US-11:** Given tagihan belum terverifikasi, when tenggat terlewati, then konsekuensi yang sudah ditetapkan ditampilkan; perubahan peserta tidak terjadi diam-diam. Pembatalan/refund diproses manual dan tidak otomatis memindahkan uang.

### FEAT-12 — Berbagi WhatsApp

**US-12 → FEAT-12:** As a member or organizer, I want to share the official event link through WhatsApp so that others can find current information.

- **AC-37 → US-12:** Given kegiatan telah dipublikasikan, when Bagikan dipilih, then pesan berisi nama dan tautan kegiatan dapat dibuka di WhatsApp untuk dikirim pengguna; bukti bayar, telepon dan email tidak ikut.
- **AC-38 → US-12:** Given WhatsApp tidak dapat dibuka atau penerima belum masuk, when berbagi/tautan digunakan, then tersedia salin tautan atau alur masuk kembali ke tujuan; sistem tidak mengklaim pesan terkirim/dibaca dan tidak melewati izin kegiatan.

## 6. Perubahan fase fitur lama dan roadmap lanjutan

FEAT-15 dan FEAT-16 dipromosikan ke MVP sesuai instruksi pengguna; ID lama tetap dipakai. Fitur lainnya pada tabel tetap konseptual lanjutan. Iuran bulanan manual berada di FEAT-23, bukan tertunda bersama penerbitan tagihan otomatis FEAT-14.

| Fitur | User story | Acceptance criteria konseptual |
|---|---|---|
| FEAT-13 | **US-13:** As a superadmin, I want to operate separate communities so that each community controls its own data. | **AC-39 → US-13:** Given dua komunitas, when pengguna komunitas A mencoba data B tanpa penugasan, then akses ditolak termasuk pada pencarian, laporan dan berkas |
| FEAT-14 | **US-14:** As a payer, I want integrated payments and automatically issued recurring dues so that payment status does not require repeated manual checks. | **AC-40 → US-14:** Given penyedia dan aturan tagihan telah dipilih, when konfirmasi transaksi yang sama diterima lebih dari sekali, then pembayaran tercatat satu kali; gagal/refund mengikuti status yang dapat ditelusuri |
| FEAT-15 / MVP | **US-15:** As a member, I want configurable automatic reminders so that I can act on important event updates. | **AC-41 → US-15:** Given kanal, preferensi dan jadwal terbaru disepakati, when waktu pengingat tiba, then penerima berhak mendapat pengingat jadwal terbaru sesuai preferensi tanpa pengingat ganda untuk sesi yang sama; revisi setelah pesan terkirim ditandai untuk tindak lanjut petugas |
| FEAT-16 / MVP | **US-16:** As an organizer, I want skill assessment and ability-aware allocation so that team balance can be assessed with agreed data. | **AC-42 → US-16:** Given data kemampuan dan definisi keseimbangan divalidasi, when pembagian dibuat, then dasar pembagian dan keterbatasan terlihat serta pengelola dapat mengoreksinya |
| FEAT-17 | **US-17:** As an organizer, I want additional competition formats so that event rules can match future needs. | **AC-43 → US-17:** Given format tambahan disepakati, when jadwal/hasil dihitung, then jumlah pertemuan, kelolosan dan peringkat mengikuti format itu dan tidak mengubah riwayat kompetisi lama |
| FEAT-18 | **US-18:** As a guardian, I want controlled participation for a minor so that the minor's data and permissions are handled appropriately. | **AC-44 → US-18:** Given kebijakan usia/wali telah ditetapkan, when anak didaftarkan tanpa persetujuan/verifikasi yang diwajibkan, then partisipasi tidak diaktifkan; penarikan persetujuan memiliki tindak lanjut yang dapat ditelusuri |

## 7. Kebutuhan kualitas dan operasional

Target numerik di bawah adalah **usulan perancangan**, perlu diuji pada lingkungan representatif sebelum disahkan untuk produksi; bukan SLA yang dijanjikan.

| ID | Outcome dan kriteria penerimaan |
|---|---|
| NFR-01 | Pada seluruh tindakan baca/tulis terbatas, uji akun di luar cakupan harus menolak akses tanpa mengubah data; penonaktifan/penarikan peran berlaku pada permintaan berikutnya |
| NFR-02 | Email, telepon, bukti transfer dan penilaian usia hanya terlihat oleh subjek/petugas yang memerlukan. Log operasional tidak memuat sandi, token pemulihan atau isi bukti transfer |
| NFR-03 | Pada viewport 360 px hingga desktop, tugas utama tidak membutuhkan scroll horizontal halaman; tabel dapat digulir dalam area berlabel. Semua aksi dapat dilakukan tanpa drag dan dengan keyboard |
| NFR-04 | Usulan target kontras teks normal ≥4,5:1, fokus terlihat, label input programatis dan pesan kesalahan tidak hanya warna; audit dilakukan pada layar dan modal utama |
| NFR-05 | Usulan skenario uji: 500 akun tersimpan dan 50 pengguna bersamaan menjalankan baca/daftar/input hasil selama 15 menit; persentil 95 respons baca ≤2 detik dan simpan ≤3 detik, tidak termasuk penyedia eksternal/unggah berkas. Nol kuota/hasil ganda pada skenario tersebut |
| NFR-06 | Usulan backup harian, toleransi kehilangan data ≤24 jam dan pemulihan ≤4 jam setelah insiden dinyatakan; satu latihan pemulihan dilakukan sebelum pilot produksi. Target perlu disesuaikan dengan hosting dan biaya |
| NFR-07 | Perubahan peran, roster terkunci, jadwal terbit, hasil final, presensi final dan keputusan pembayaran mencatat pelaku, waktu, alasan bila koreksi, serta nilai sebelum/sesudah yang relevan; hanya petugas berhak dapat membaca riwayat |
| NFR-08 | Jika hasil simpan belum diketahui, antarmuka menunjukkan “Status penyimpanan belum diketahui” dan menawarkan cek status sebelum kirim ulang; setelah diketahui berhasil, aksi yang sama tidak menggandakan efek |
| NFR-09 | Tanggal/jam kegiatan memuat zona waktu kegiatan; usulan default Asia/Jakarta yang dapat diatur pengelola sebelum publikasi. Zona sesi komputer tidak boleh menggeser jadwal tanpa label |
| NFR-10 | Penanggung jawab retensi, penghapusan, laporan usia/privasi, dukungan dan insiden ditunjuk sebelum peluncuran; mekanisme permintaan tersedia melalui kontak pengelola, tanpa menambahkan portal layanan baru pada MVP |

## 8. Pain → solusi → manfaat → indikator

| Pain | Solusi | Manfaat | Indikator |
|---|---|---|---|
| PAIN-01 | FEAT-01,04 | Peserta unik, konfirmasi jelas | KPI-01; uji kuota/duplikasi |
| PAIN-02 | FEAT-03,05,06 | Persiapan lebih ringkas dan jadwal layak | KPI-02; nol benturan pada jadwal terbit |
| PAIN-03 | FEAT-07,08,12 | Sumber hasil resmi mudah ditemukan | KPI-03; uji konsistensi koreksi |
| PAIN-04 | FEAT-09,10 | Keterlibatan tidak disimpulkan dari roster saja | KPI-04; label rincian statistik |
| PAIN-05 | FEAT-02,11 | Akses dan verifikasi dapat ditelusuri | KPI-05; uji akses negatif |

## 9. Asumsi, batas dan keputusan terbuka

- Satu komunitas penyelenggara, peserta anggota/tamu berakun 18+, satu mode daftar per kegiatan; koneksi dibutuhkan. Mode acak atau seimbang berdasarkan skill menjaga kapasitas dan pratinjau roster.
- Jenis liga satu putaran; kegiatan rutin adalah sesi terpisah dengan laga persahabatan, bukan pengulangan kalender otomatis.
- Tidak ada pemain tamu tanpa akun, waiting list otomatis, sinkronisasi offline, refund otomatis, streaming sendiri, marketplace atau pembukuan penuh. Halaman publik/foto masuk MVP dengan izin publikasi; video berupa tautan/preview.
- Multi-komunitas operasional, format tambahan, kanal notifikasi selain pengingat jadwal WhatsApp, gateway/penerbitan tagihan otomatis, statistik lanjutan dan peserta anak merupakan roadmap. Toko opsional tidak aktif otomatis.
- Kebijakan walkover, pengunduran tim, penentuan juara ketika seri, tenggat bayar, retensi dan kontak laporan harus jelas sebelum kegiatan relevan dipublikasikan/produksi. Details menyediakan default usulan yang ditandai.
- Negara operasional Indonesia masih asumsi. Kebijakan 18+ membatasi scope, bukan bukti kepatuhan hukum.

## 10. Risiko adopsi dan validasi

Risiko utama: daftar paralel di chat, kesulitan masuk, petugas skor tidak tersedia dan koneksi lapangan. Mitigasi: satu tautan resmi, bantuan saat pilot, petugas cadangan, pemulihan submit dan pengukuran KPI yang sama dengan Planning. Validasi berupa simulasi serta satu kegiatan pilot diikuti evaluasi empat kegiatan pertama; belum dilakukan pada tahap dokumentasi.

Tidak ada klaim riset pengguna, performa atau keamanan yang telah lulus. Keputusan terbuka operasional tidak menghalangi kelengkapan paket dokumen, tetapi menghalangi kesiapan produksi pada area terkait.

## 11. Kebutuhan perluasan v2.0

Pemetaan fase ini mengikuti Planning v2.0; US-19–US-26 adalah MVP, US-27 opsional dan US-28 lanjutan. Rincian default di Details berlabel usulan untuk diuji, bukan aturan klub yang sudah disahkan.

### FEAT-19 — Beranda komunitas

**US-19 → FEAT-19:** As a visitor, I want to discover the next session and the community spirit so that I can decide to join.

- **AC-45 → US-19:** Given halaman publik dibuka, when Beranda dimuat, then logo, pesan Kick & Kumpul, jadwal terdekat yang diterbitkan dan CTA Gabung Main terlihat; konten draft tidak ditampilkan.
- **AC-46 → US-19:** Given statistik sah tersedia, when ringkasan ditampilkan, then anggota aktif, laga final nonbatal dan gol bermain dihitung dengan label cakupan; hasil administratif tidak menambah gol bermain.
- **AC-47 → US-19:** Given foto aksi asli belum tersedia atau belum diizinkan, when hero dibuka, then area foto berlabel belum tersedia tanpa mengaku menampilkan dokumentasi anggota; tidak ada sesi membuka state kosong yang jelas.

### FEAT-20 — Kalender, lokasi dan RSVP

**US-20 → FEAT-20:** As a member, I want to find a session on a calendar and join in one action so that I can arrange my game day.

- **AC-48 → US-20:** Given sesi terbit yang boleh dibaca tersedia, when kalender/agenda difilter, then tanggal, zona, lokasi, kuota dan status sesuai sesi; kalender desktop dan agenda HP membuka detail sesi yang sama.
- **AC-49 → US-20:** Given lokasi memiliki alamat dan pin, when detail dibuka, then alamat dan aksi petunjuk arah tersedia; kegagalan peta tidak menghilangkan alamat atau menghalangi RSVP.
- **AC-50 → US-20:** Given anggota aktif memenuhi syarat pada sesi individu terbuka, when Gabung Main dipilih, then RSVP dan status dikonfirmasi dalam satu aksi tanpa formulir panjang tambahan; pengunjung masuk dahulu lalu kembali ke sesi yang sama; kuota/duplikasi tetap diperiksa.

### FEAT-21 — Dua voting apresiasi

**US-21 → FEAT-21:** As an eligible member, I want to vote for match MVP and Player of the Month so that we can appreciate fellow players.

- **AC-51 → US-21:** Given petugas berhak menetapkan pemilihan, when kandidat, pemilih, waktu buka/tutup, zona dan aturan seri valid, then pemilihan per laga atau per bulan dibuka dengan konteks dan aturan yang terlihat; keduanya tidak mencampur suara.
- **AC-52 → US-21:** Given pemilih dan kandidat memenuhi aturan pada periode terbuka, when pilihan dikonfirmasi, then tepat satu suara aktif tercatat per akun per pemilihan; klik ganda atau permintaan bersamaan tidak menambah suara.
- **AC-53 → US-21:** Given pemilih tidak berhak, kandidat tidak sah, memilih diri dilarang, atau tenggat lewat, when suara dikirim, then ditolak dengan alasan dan hasil tidak berubah.
- **AC-54 → US-21:** Given pemilihan ditutup, when hasil diterbitkan, then hitungan suara sah dan pemenang mengikuti aturan; seri dan tanpa suara diberi status eksplisit, bukan pemenang rekaan.
- **AC-55 → US-21:** Given hasil bulanan dibuka, when periode dipilih, then nama periode, hasil dan riwayat revisi terlihat; pemenang tidak otomatis dihitung dari gol atau akumulasi MVP pertandingan.
- **AC-56 → US-21:** Given pemilihan dikoreksi/dibatalkan petugas berhak, when alasan dan dampak dikonfirmasi, then perubahan tercatat dan pemilih melihat status terbaru; pilihan pribadi tidak dibuka ke anggota lain.

### FEAT-22 — Galeri dan highlight

**US-22 → FEAT-22:** As a member, I want to browse match albums so that I can relive our time together.

- **AC-57 → US-22:** Given petugas mengelola pertandingan tugasnya, when foto valid atau tautan highlight disimpan, then album draft terkait pertandingan tersedia untuk ditinjau, belum publik.
- **AC-58 → US-22:** Given izin tayang dan cakupan audiens ditetapkan, when album/media diterbitkan, then hanya audiens yang diizinkan dapat melihat foto/video; pencabutan publikasi menghentikan akses berikutnya.
- **AC-59 → US-22:** Given anggota melihat hasil, when Lihat foto dipilih, then album pertandingan yang sesuai dibuka; jika belum ada foto, state kosong dan akses Kas & Iuran tetap tersedia.
- **AC-60 → US-22:** Given berkas tidak sah, gagal unggah, atau video tidak dapat diputar, when media digunakan, then status per item dan aksi pemulihan terlihat tanpa publikasi parsial yang disangka lengkap; tautan video dapat dibuka sebagai alternatif.
- **AC-61 → US-22:** Given petugas menarik media dengan alasan, when dikonfirmasi, then media tidak lagi muncul pada halaman/tautan terbatas yang dilayani sistem dan riwayat disimpan; sistem tidak menjanjikan menarik salinan yang telah diunduh orang lain.

### FEAT-23 — Kas dan iuran bulanan

**US-23 → FEAT-23:** As a member or authorized treasurer, I want clear monthly dues and a simple cash ledger so that obligations and field costs are transparent.

- **AC-62 → US-23:** Given petugas kas berwenang memilih anggota, periode, nominal dan tenggat, when tagihan bulanan diterbitkan manual, then satu tagihan aktif per anggota/periode tercatat; penerbitan ulang tidak menggandakan kewajiban dan tagihan kegiatan tetap terpisah.
- **AC-63 → US-23:** Given anggota membuka Kas & Iuran, when data dimuat, then ia melihat ringkasan saldo, pemasukan/pengeluaran dan kewajiban/status miliknya; bukti serta status individu lain tidak terbuka.
- **AC-64 → US-23:** Given bukti iuran diverifikasi, when pembayaran dinyatakan lunas, then satu pemasukan ditautkan ke pembayaran; keputusan ulang tidak menambah saldo kedua kali. Unggah bukti saja tetap menunggu verifikasi.
- **AC-65 → US-23:** Given petugas kas mencatat saldo awal atau biaya sewa lapangan dengan tanggal, nominal, kategori dan keterangan, when disimpan, then saldo = saldo awal + pemasukan sah − pengeluaran sah; draft tidak ikut saldo dan transaksi terkait kegiatan tidak dicatat ganda.
- **AC-66 → US-23:** Given transaksi/tagihan sudah tercatat, when koreksi atau pembatalan beralasan dilakukan oleh petugas berhak, then versi/penyesuaian berjejak mengoreksi saldo tanpa menghapus jejak lama; pihak tanpa delegasi ditolak.
- **AC-67 → US-23:** Given alur selesai bermain dan melihat skor/foto, when Bayar iuran dipilih, then kewajiban yang sama dapat dibuka dan dibayar manual; belum ada foto/hasil atau tidak ada tagihan tidak menghalangi navigasi dan tidak membuat tagihan baru.

### FEAT-24 — Kontak dan sponsor

**US-24 → FEAT-24:** As a visitor, I want official contact and sponsor information so that I can reach the club and recognize its supporters.

- **AC-68 → US-24:** Given kontak diterbitkan, when halaman dibuka, then WhatsApp admin publik dan media sosial yang diaktifkan terlihat; kontak pribadi anggota tidak ikut.
- **AC-69 → US-24:** Given petugas publikasi berwenang menyimpan logo sponsor asli dan tautan yang valid, when dipublikasikan, then logo/urutan sesuai konfigurasi; tanpa sponsor tampil state kosong tanpa logo rekaan.
- **AC-70 → US-24:** Given tautan kontak/sponsor tidak lengkap atau tidak valid, when disimpan/dibuka, then alasan terlihat dan tidak mengarahkan ke tujuan yang tidak disetujui; draft atau kontak dinonaktifkan tidak tampil publik.

### FEAT-25 — Tema dan mobile-first

**US-25 → FEAT-25:** As a mobile member, I want a readable light or dark theme so that I can use the site comfortably on my phone.

- **AC-71 → US-25:** Given belum memilih tema, when situs dibuka, then tema gelap hitam–gold menjadi awal; pilihan eksplisit Terang/Gelap/Sesuai perangkat tersimpan setelah muat ulang pada perangkat yang sama, dan pilihan Sesuai perangkat mengikuti preferensi perangkat.
- **AC-72 → US-25:** Given kedua tema dan lebar 360/390/430 px atau desktop, when anggota menjalankan RSVP, voting, unggah bukti atau pengelola membagi tim, then kontrol/state terbaca dan dapat dipakai tanpa luapan horizontal halaman atau drag wajib.
- **AC-73 → US-25:** Given gaya terbaru diterapkan, when layar ditinjau, then hitam dan gold dominan sesuai logo, tema gelap default, tema terang netral dengan header hitam dan gold, judul tebal dan copy akrab/humoris konsisten; pesan pembayaran/error menyebut status dan tindakan jelas, foto contoh tidak diklaim asli.

### FEAT-26 — Peserta antar-komunitas

**US-26 → FEAT-26:** As a visiting captain, I want to register my team's consenting players for an Asker FC event so that we can play together without joining a separate tenant.

- **AC-74 → US-26:** Given kegiatan menerima tim tamu, when kapten berakun mengajukan nama/asal tim dan roster pemain berakun yang menyetujui, then pengelola meninjau dengan batas kuota/keunikan yang sama.
- **AC-75 → US-26:** Given akun tamu diterima pada satu kegiatan, when ia membuka direktori internal, iuran bulanan atau kegiatan lain yang terbatas, then akses ditolak; ia hanya mendapat informasi/tindakan sesuai tugas dan partisipasinya.
- **AC-76 → US-26:** Given tamu selesai atau ditarik dari kegiatan, when akses ditinjau, then riwayat hasilnya tetap sah dan kewenangan aktif mengikuti cakupan terbaru; tidak membuat komunitas penyelenggara baru otomatis.

### FEAT-27 — Toko opsional

**US-27 → FEAT-27:** As a member, I want to view optional club merchandise so that I can express my team identity.

- **AC-77 → US-27:** Given modul belum diaktifkan, when navigasi dibuka, then toko disembunyikan; bila kelak katalog/pemesanan WhatsApp disahkan dan diaktifkan, produk nyata serta cara memesan terlihat tanpa mengasumsikan stok, checkout atau pengiriman terintegrasi.

### FEAT-28 — Statistik lanjutan

**US-28 → FEAT-28:** As an organizer, I want future performance metrics with agreed definitions so that deeper analysis can be based on reliable data.

- **AC-78 → US-28:** Given metrik dan kualitas data disepakati pada fase lanjutan, when laporan dibuat, then definisi, cakupan dan data yang belum lengkap terlihat; angka tidak direkayasa dari data yang tidak dicatat.

### Perincian AC pada fitur lama yang dipromosikan/diperluas

- **AC-79 → US-16:** Given penilai diberi kewenangan, when nilai skill pada skala yang disepakati disimpan, then pemain, penilai, nilai dan waktu/versi tercatat; pihak tanpa kewenangan tidak dapat mengubah atau membaca nilai privat.
- **AC-80 → US-16:** Given mode skill dipilih tetapi nilai peserta belum lengkap, when pembagian diminta, then daftar data kurang terlihat; pengguna melengkapi nilai atau memilih mode acak secara eksplisit, tanpa pengisian nilai rekaan.
- **AC-81 → US-16:** Given pratinjau skill dibuat, when perubahan manual dilakukan, then ukuran sebaran nilai diperbarui, kapasitas/keunikan diperiksa dan keterbatasan hasil ditampilkan.
- **AC-82 → US-15:** Given peserta terkonfirmasi memiliki nomor dan persetujuan yang berlaku, when waktu pengingat sebelum sesi tiba, then satu pengingat berisi sesi, waktu/zona, lokasi dan tautan diproses; RSVP bukan persetujuan kanal secara otomatis.
- **AC-83 → US-15:** Given sesi batal, peserta mundur, preferensi dimatikan atau jadwal direvisi, when pengingat belum dikirim dievaluasi, then pekerjaan lama dibatalkan/diperbarui; tidak mengirim jadwal usang.
- **AC-84 → US-15:** Given pengiriman gagal atau hasil belum diketahui, when petugas meninjau status, then alasan/status tersedia, retry hanya dilakukan sesuai batas dan setelah memeriksa kiriman yang mungkin berhasil; kegagalan tidak membatalkan RSVP.
- **AC-85 → US-15:** Given layanan pulih atau status pengiriman diterima berulang, when pekerjaan dilanjutkan, then tidak membuat pengingat tambahan untuk konteks yang sama; status diproses tanpa mundur dari bukti yang lebih baru.
- **AC-86 → US-15:** Given anggota menolak atau mencabut persetujuan pengingat, when preferensi disimpan, then pengingat berikutnya dihentikan dan jadwal tetap dapat dilihat di situs; publik tidak dapat membaca nomor peserta.
- **AC-87 → US-01:** Given pemilik profil mengisi foto/posisi/nomor punggung dan cakupan tayang yang diizinkan, when disimpan, then profil diperbarui; foto tidak sah ditolak dan nomor diperiksa dalam cakupan tim terkait, bukan unik seluruh klub.
- **AC-88 → US-01:** Given profil belum lengkap atau foto tidak diizinkan, when direktori dibuka, then hanya informasi yang boleh dilihat dan placeholder berlabel tampil; email/nomor WhatsApp pribadi tidak ikut.
- **AC-89 → US-10:** Given daftar top skor dibuka, when periode dipilih, then urutan memakai gol sah dari hasil final tanpa gol bunuh diri dan cakupan data/seri terlihat.
- **AC-90 → US-10:** Given apresiasi pemain ditampilkan pada direktori/profil, when periode atau laga dipilih, then hasil voting yang sudah diterbitkan ditautkan dengan konteksnya; belum ada pemenang tidak ditampilkan sebagai nol prestasi yang pasti.

## 12. Akses, kualitas dan pengujian perluasan

Publik hanya membaca Beranda, kontak/sponsor, jadwal/hasil/media yang secara eksplisit dipublikasikan. Anggota aktif membaca informasi internal yang diizinkan, RSVP dan tagihannya sendiri. Tamu terbatas pada kegiatan yang diterima. Pengelola mengelola sesi/album/voting tugasnya; kas komunitas dan publikasi kontak/sponsor memerlukan delegasi eksplisit dari superadmin. Nilai skill: pemain terkait dan penilai berhak; suara individual: pemilik dan petugas penanganan berwenang, tidak untuk pengumuman publik. Audit tidak boleh membocorkan pilihan pribadi melalui halaman umum.

NFR-02 diperluas ke foto, pilihan suara, nilai skill dan nomor pengingat. NFR-07 mencakup perubahan nilai, voting, publikasi dan kas beserta koreksi. NFR-08 berlaku juga pada suara, pengingat dan transaksi kas. NFR-03/NFR-04 berlaku pada kedua tema. Tidak ada notification center baru atau kanal tambahan selain yang disepakati.

Skenario penerimaan utama mengikuti tujuh tahap alur; uji juga foto/hasil belum tersedia, kuota terakhir, skill kosong, suara ganda/seri/tanpa suara, jadwal berubah sebelum kirim, berhenti menerima, tagihan terbit ulang, keputusan verifikasi ganda, akses tamu ke kas dan preferensi tema setelah muat ulang. Ini kebutuhan pengujian produk, belum hasil eksekusi.