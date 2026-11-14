# Product Planning — Asker FC

- Versi: 2.9 · Tanggal: 14 November 2026
- Status: revisi akses profil, penghapusan peran Kapten dan penghargaan tahunan selesai sampai spesifikasi Mockup SRS v2.8; referensi gambar statis menyertai revisi. Parameter tahunan belum disahkan; bukan implementasi aplikasi.
- Upstream: [Implementation Plan v2.16](./00-implementation-plan.md), pelaksanaan rencana v2.7 atas instruksi pengguna “proses implementation plan tersebut”, 11 November 2026. Parameter rencana menjadi baseline dokumentasi, belum hasil kalibrasi klub. Perluasan loyalitas diotorisasi pengguna 14 November 2026.
- Nama kerja: **Asker FC**. Nama komunitas sudah diberikan; tidak diperlukan kandidat nama produk baru. Tidak ada klaim ketersediaan merek/domain.
- Konsep ditetapkan pengguna: **“Kick & Kumpul” — website komunitas fun football.** Asker FC tetap nama komunitas; Kick & Kumpul menjadi konsep pengalaman dan pesan produk.
- Sumber keputusan: jawaban pengguna, rencana disetujui; fakta eksternal pada bagian sumber. Persona, masalah operasional dan UVP adalah hipotesis kecuali disebut sebagai keputusan pengguna.


> **Keputusan berlaku v2.9 — pembagian tim:** Anggota mendaftar kegiatan secara individu; sistem mengacak peserta dengan mempertimbangkan skill agar tim seimbang. Admin mengatur jumlah/kapasitas tim, memeriksa kelengkapan data, memicu pembagian, meninjau hasil dan mengunci susunan; bukan memilih pemain ke tim secara manual. UI Roster Admin, undangan/pengajuan/penerimaan roster antartim dan mode acak murni tidak digunakan pada alur ini. Ketentuan antartim/tagihan tim pada bagian lama menjadi catatan scope sebelumnya, bukan alur aktif; bila dibutuhkan sebagai format terpisah harus ditentukan kembali. Daftar tim hasil sistem tetap dapat dibaca. Rating tetap dihitung sistem; sumber/skala skill dan toleransi keseimbangan harus ditetapkan sebelum implementasi, tanpa otomatis menyamakan skill dengan rating.

## 1. Ringkasan produk dan nilai

**Tujuan utama yang ditetapkan pengguna: memudahkan anggota tahu jadwal main, daftar, dan bayar, sekaligus membangun rasa kebersamaan tim.** Ini menjadi arah utama konsep Kick & Kumpul dan dasar prioritas pengalaman anggota. Biaya kegiatan hanya untuk kegiatan berbayar; iuran bulanan adalah kewajiban terpisah sesuai periode anggota.

Asker FC adalah **website komunitas fun football berkonsep “Kick & Kumpul”**: tempat anggota mengatur waktu main bersama, berkumpul, dan menjaga kebersamaan melalui sepak bola yang menyenangkan. Fokus pengalaman adalah mudah ikut kegiatan, mengetahui teman satu tim, dan datang bermain bersama. Keanggotaan, pendaftaran, pembagian tim, jadwal, presensi, hasil, klasemen dan statistik membantu kegiatan komunitas berjalan tertib. Web mobile-first digunakan anggota di HP dan pengelola baik di HP maupun desktop. Sasaran MVP satu komunitas dengan 100–500 anggota, bukan 500 pengguna bersamaan.

Makna konsep: **Kick** mewakili bermain bola untuk kesenangan dan aktivitas bersama; **Kumpul** mewakili pertemuan, pertemanan, dan rasa menjadi bagian dari komunitas. Sesi fun football rutin menjadi pengalaman utama; liga dan turnamen menjadi variasi kegiatan komunitas yang tetap mendukung semangat bermain bersama. Skor, klasemen dan statistik berfungsi sebagai informasi pendukung dengan penyajian yang ramah.

Contoh utama: **buka link → lihat jadwal → RSVP → terima pengingat WhatsApp → main → lihat skor dan foto → bayar iuran**. Login mengembalikan anggota ke sesi semula; RSVP individu yang memenuhi syarat cukup satu aksi. Iuran tidak menunggu publikasi hasil/foto dan tetap dapat dibuka langsung. Biaya kegiatan di muka, bila ditetapkan, adalah cabang yang diumumkan sebelum RSVP, bukan alur utama semua sesi. Pada kegiatan antartim, pengelola mengajukan roster dan setiap anggota menyetujui sendiri.

Pengelolaan melalui WhatsApp dan spreadsheet diasumsikan oleh pengguna, belum diamati langsung. Produk tidak menggantikan percakapan sosial; tautan WhatsApp membawa pengguna ke informasi resmi kegiatan.

| ID | Pain point/hipotesis | Dampak yang ingin diatasi | Goal |
|---|---|---|---|
| PAIN-01 | Daftar peserta tersebar dan mudah ganda | Kuota dan roster tidak pasti | GOAL-01 |
| PAIN-02 | Pembagian tim/jadwal berulang secara manual | Beban pengelola dan benturan jadwal | GOAL-02 |
| PAIN-03 | Skor dan klasemen tidak satu sumber | Perselisihan hasil dan informasi terlambat | GOAL-03 |
| PAIN-04 | Presensi dan kontribusi pemain tidak tercatat konsisten | Sulit mengevaluasi keterlibatan | GOAL-04 |
| PAIN-05 | Status transfer dan kewenangan kurang transparan | Verifikasi berulang dan akses berlebihan | GOAL-05 |

## 2. Persona dan konteks akses

Persona berikut berbasis peran, belum hasil wawancara. Motivasi yang akan divalidasi adalah menikmati sepak bola santai, bertemu teman, aktif bergerak, dan merasa menjadi bagian dari komunitas. Konsep “Kick & Kumpul” berasal dari keputusan pengguna; rincian motivasi ini tetap hipotesis. Usia MVP 18+ sesuai rekomendasi yang diterima melalui persetujuan rencana; lokasi operasi diasumsikan Indonesia.

| Peran | Kebutuhan dan motivasi | Perilaku/konteks | Kendala utama |
|---|---|---|---|
| Anggota | Mudah tahu jadwal main, daftar dan bayar; merasa menjadi bagian dari tim saat bermain bersama | Membuka tautan kegiatan dari WhatsApp di HP, memeriksa jadwal/biaya dan status daftar/bayar | Informasi jadwal, slot, pembayaran dan teman satu tim belum jelas |
| Pengelola | Memudahkan anggota berkumpul dan memastikan kegiatan ramah serta tertib | Desktop untuk konfigurasi, HP di lapangan | Administrasi berulang, pergantian roster, cuaca dan perubahan jadwal |
| Pencatat skor | Mengirim hasil tanpa salah pertandingan | HP, perhatian terbagi di lapangan | Koneksi tidak stabil, input bersamaan |
| Superadmin | Mengelola anggota dan penugasan secara terkendali | Mengurus akses, audit dan penanganan laporan | Akun berlebihan atau data tidak semestinya terbuka |

Satu akun dapat menjalankan beberapa peran dengan batas objek yang ditugaskan. Sumber kebenaran kewenangan bukan sekadar menu yang terlihat.

Perincian awal: superadmin membuat/menyalin draft dan menugaskan pengelola; admin mengonfigurasi dan memublikasikan hanya kegiatan yang ditugaskan. Ini memperjelas bootstrap cakupan akses, tanpa menambah peran baru.

## 3. Lanskap kompetitor

Riset meja diverifikasi ulang pada **9 November 2026**, memakai dokumentasi resmi. Tidak ada harga yang dipakai sebagai dasar keputusan. “Belum diverifikasi” tidak berarti fitur tidak tersedia. Produk internasional dipakai sebagai pembanding kemampuan, bukan bukti kesesuaian pasar lokal.

| Pembanding/segmen | Pendaftaran dan roster | Jadwal dan hasil | Kekuatan terbukti | Keterbatasan terbukti/hal belum diverifikasi | Implikasi peluang |
|---|---|---|---|---|---|
| Tournify — turnamen/ liga olahraga | Dokumentasi menjelaskan tim/pemain dan formulir pendaftaran | Round robin, penjadwalan lapangan, skor dan klasemen | Luasnya alat operasional turnamen | Pembuatan kegiatan dilakukan melalui admin web, bukan aplikasi mobile; aktivasi aplikasi peserta adalah fitur premium. Alur transfer lokal belum diverifikasi | Hindari mengklaim keunggulan hanya karena ada jadwal/skor; uji kesederhanaan alur anggota komunitas [S1] |
| Challonge — platform turnamen lintas permainan | Mendukung halaman pendaftaran dan pendaftaran tim | Round robin, standings, score reporting, waktu/stasiun pertandingan | Struktur kompetisi dan pelaporan hasil tersedia | Halaman fitur menyebut batas 40 peserta untuk round robin satu tahap. Batas ini peserta kompetisi, bukan anggota komunitas. Kesesuaian pembagian pemain fun football belum diverifikasi | Alur pemain individu → tim sementara layak menjadi fokus eksperimen [S2] |
| TeamSnap ONE — koordinasi tim, pembanding tidak langsung | Web mendukung melihat roster dan RSVP | Jadwal, pengelolaan kegiatan, chat | Koordinasi kegiatan dan ketersediaan tim | Dokumen web mencantumkan pengeditan roster dan unggah event massal sebagai rencana mendatang; jangan digeneralisasi ke seluruh produk TeamSnap. Liga/klasemen pada halaman ini belum diverifikasi | Satukan koordinasi rutin dan kompetisi tanpa mengasumsikan pembanding tidak mampu [S3] |

## 4. Market gap dan UVP

**Inferensi yang belum divalidasi:** anggota Asker FC membutuhkan satu alur ringan yang meliputi daftar individu, tim sementara, jadwal liga kecil, kehadiran dan transparansi hasil dengan biaya transfer manual. Dokumentasi pembanding membuktikan banyak fungsi dasarnya sudah tersedia; belum membuktikan gap pasar luas.

UVP kerja: **“Kick & Kumpul: main bola, ketemu teman, kumpul bareng Asker FC.”** Janji fungsionalnya: anggota mudah tahu jadwal main, daftar, membayar jika diperlukan dan memantau statusnya dari satu tempat. Pengalaman tim membantu anggota mengenal rekan bermain dan merasa menjadi bagian dari komunitas. Diferensiasi yang akan diuji adalah kesesuaian proses komunitas, kemudahan ikut bermain dan membayar, serta pengurangan rekap ulang. Keberhasilan membangun kebersamaan perlu divalidasi melalui pengalaman anggota saat pilot.

Validasi setelah dokumentasi: pengelola menjalankan satu simulasi dengan daftar sintetis, kemudian pilot satu kegiatan nyata. Amati waktu menyiapkan tim/jadwal, anggota yang memerlukan bantuan dan koreksi hasil. Jika penggunaan tetap bergantung pada rekap manual, identifikasi langkah yang tidak cocok sebelum memperluas scope.

## 5. Model bisnis dan biaya

MVP adalah alat internal komunitas, bukan SaaS komersial. Pihak yang diperkirakan menanggung biaya sistem adalah komunitas; penanggung jawab dan anggarannya belum ditetapkan. Biaya kegiatan opsional adalah pemasukan penyelenggaraan kegiatan, bukan otomatis pendapatan produk atau laba.

Satu kegiatan memiliki satu tarif dan basis tagihan: per orang pada mode individu, per tim pada mode antartim. Transfer dan refund berlangsung di luar sistem; sistem mencatat bukti serta keputusan verifikasi. Biaya kegiatan nol berarti tidak ada tagihan kegiatan; iuran bulanan tetap terpisah.

Biaya utama yang perlu dianggarkan: hosting, penyimpanan dan backup privat, domain, email transaksional, pemeliharaan, dukungan pengelola, dan biaya pembuatan visual jika ada. Tidak tersedia dasar untuk proyeksi pendapatan, harga langganan atau ROI numerik. Eksperimen awal adalah kesediaan komunitas membiayai operasional setelah pilot, bukan memungut biaya aplikasi baru.

## 6. Katalog fitur dan prioritas

Seluruh MVP masuk scope yang disetujui. Urutan di bawah menunjukkan dependensi pengerjaan, bukan izin menghapus fitur. Fase lanjutan hanya roadmap.

| ID | Fitur/fungsi | Pain → goal | Nilai dan alasan fase | Dependensi |
|---|---|---|---|---|
| FEAT-01 | Akun email/Google dan profil anggota 18+ | PAIN-01 → GOAL-01 | MVP; identitas peserta yang konsisten | Kebijakan usia, email |
| FEAT-02 | Hak akses dan penugasan per kegiatan/tim/laga | PAIN-05 → GOAL-05 | MVP; membatasi perubahan dan akses data | FEAT-01 |
| FEAT-03 | Kegiatan liga/rutin dan salin konfigurasi | PAIN-02 → GOAL-02 | MVP; konfigurasi menjadi sumber aturan | FEAT-02 |
| FEAT-04 | Daftar individu/antartim, konfirmasi roster, pembatalan | PAIN-01 → GOAL-01 | MVP; mengendalikan kuota dan identitas | FEAT-01,03 |
| FEAT-05 | Pembagian tim acak/skill dengan kapasitas terjaga dan roster terkunci | PAIN-02 → GOAL-02 | MVP; mengurangi rekap tim | FEAT-04 |
| FEAT-06 | Jadwal liga satu putaran dan laga rutin | PAIN-02 → GOAL-02 | MVP; menghindari benturan | FEAT-03,05 |
| FEAT-07 | Skor, Gol/Assist, kartu, metadata hasil, finalisasi poin dan koreksi beralasan | PAIN-03 → GOAL-03 | MVP; satu sumber hasil sah | FEAT-02,06 |
| FEAT-08 | Klasemen dari hasil final | PAIN-03 → GOAL-03 | MVP; transparansi peringkat | FEAT-07 |
| FEAT-09 | Presensi, partisipasi laga, insiden dan hak koreksi penalti | PAIN-04 → GOAL-04 | MVP; dasar statistik yang jujur | FEAT-04,06 |
| FEAT-10 | Rekap faktual, poin mentah event, nilai relatif event dan akumulasi profil | PAIN-04 → GOAL-04 | MVP; keterlibatan dan kontribusi terlihat | FEAT-07,09 |
| FEAT-11 | Biaya opsional dan bukti transfer privat | PAIN-05 → GOAL-05 | MVP; menghindari pemeriksaan berulang | FEAT-03,04 |
| FEAT-12 | Berbagi tautan kegiatan lewat WhatsApp | PAIN-03 → GOAL-03 | MVP; mengarahkan komunikasi ke informasi resmi | FEAT-03,06 |
| FEAT-13 | Multi-komunitas operasional | PAIN-05 → GOAL-05 | Lanjutan; kebutuhan MVP hanya satu komunitas | Uji isolasi lintas komunitas |
| FEAT-14 | Payment gateway dan penerbitan tagihan berkala otomatis | PAIN-05 → GOAL-05 | Lanjutan; hindari dependensi finansial tambahan pada pilot | FEAT-11, provider |
| FEAT-15 | Pengingat jadwal WhatsApp otomatis | PAIN-03 → GOAL-03 | MVP; fitur pembeda yang diminta | FEAT-04,06; provider, persetujuan penerima |
| FEAT-16 | Penilaian skill dan dasar pembagian seimbang | PAIN-02 → GOAL-02 | MVP; mendukung mode skill FEAT-05 | Penilai berwenang, skala dan kualitas data |
| FEAT-17 | Format tambahan dan liga beberapa putaran | PAIN-02 → GOAL-02 | Lanjutan; satu putaran membatasi kompleksitas | FEAT-06,08 |
| FEAT-18 | Peserta di bawah umur dengan perlindungan wali | PAIN-01 → GOAL-01 | Lanjutan bersyarat; memerlukan alur operasional tambahan | Keputusan komunitas dan kajian privasi |

| ID | Fitur/fungsi | Pain → goal | Nilai dan alasan fase | Dependensi |
|---|---|---|---|---|
| FEAT-19 | Beranda, hero foto aksi dan ringkasan komunitas | PAIN-03 → GOAL-03 | MVP; pintu masuk Kick & Kumpul | Publikasi konten, FEAT-03,07 |
| FEAT-20 | Kalender sesi, lokasi/peta dan RSVP satu klik | PAIN-01,03 → GOAL-01,03 | MVP; anggota mudah menemukan dan mengikuti sesi | FEAT-03,04; provider peta |
| FEAT-21 | Voting MVP pertandingan dan Pemain Terbaik Bulan Ini | PAIN-04 → GOAL-04 | MVP; apresiasi dan kebersamaan | FEAT-01,07,09; aturan voting |
| FEAT-22 | Album foto dan tautan highlight per pertandingan | PAIN-04 → GOAL-04 | MVP; kenangan bersama dan dokumentasi | Media asli/izin; penyimpanan |
| FEAT-23 | Kas, iuran bulanan dan biaya lapangan | PAIN-05 → GOAL-05 | MVP; kewajiban dan kas transparan sesuai izin | FEAT-11; petugas kas |
| FEAT-24 | Kontak admin, media sosial dan sponsor | PAIN-03 → GOAL-03 | MVP; komunikasi dan dukungan komunitas | Kontak/logo asli, publikasi |
| FEAT-25 | Mode gelap dan preferensi tampilan mobile-first | PAIN-01 → GOAL-01 | MVP; pemakaian mayoritas dari HP | Token dua tema dan aksesibilitas |
| FEAT-26 | Tim peserta antar-komunitas | PAIN-01 → GOAL-01 | MVP; tim tamu dalam kegiatan Asker FC | FEAT-01,02,04; akun tamu terbatas |
| FEAT-27 | Toko/Merchandise opsional | PAIN-04 → GOAL-04 | Opsional; belum aktif, katalog/pesan WhatsApp sebagai usulan batas awal | Keputusan aktivasi, produk asli |
| FEAT-28 | Penilaian sembilan kategori, achievement dan keterlacakan sumber poin | PAIN-04 → GOAL-04 | MVP; apresiasi lintas posisi dengan input sederhana sesuai instruksi terbaru | FEAT-03,05,07,08,09,10; aturan penilaian berversi |

FEAT-29 (MVP diperluas), PAIN-06/GOAL-06 dan KPI-06 melengkapi katalog ini; rincian program loyalitas pada [bagian 16](#16-loyalitas-anggota--feat-29).

## 7. Goals, OKR dan definisi KPI

Objective produk: **memudahkan anggota tahu jadwal main, daftar, dan bayar, sekaligus membangun rasa kebersamaan tim**, sesuai konsep Kick & Kumpul. Objective operasional pilot: kegiatan pertama dapat dijalankan dari buka link sampai verifikasi iuran setelah hasil/foto dengan satu sumber informasi. KPI di bawah mengukur dukungan operasional terhadap pengalaman tersebut; manfaat yang dirasakan anggota diperiksa melalui observasi dan umpan balik pilot berikut. Seluruh baseline **belum diukur**. Target berikut **usulan**, bukan fakta atau komitmen. Pilot satu kegiatan hanya menguji kelayakan; evaluasi stabilitas dilakukan selama empat kegiatan pertama. Jika denominator nol, tampilkan “tidak berlaku”, bukan 0%.

| Tujuan anggota | Dukungan fitur yang sudah ada | Validasi pengalaman saat pilot |
|---|---|---|
| Tahu jadwal main | FEAT-03, FEAT-06, FEAT-12 | Anggota dapat menemukan waktu, lokasi/lapangan dan versi jadwal terbaru; amati kebutuhan bantuan |
| Daftar | FEAT-01, FEAT-04 | Anggota memahami kuota, menyelesaikan pendaftaran/konfirmasi dan mengenali statusnya; KPI-01 tetap digunakan |
| Bayar | FEAT-11, FEAT-23 | Pada kegiatan berbayar, pemilik tagihan memahami nominal, tujuan transfer, tenggat, unggah bukti dan hasil verifikasi; catat hambatan dan bantuan yang diperlukan |
| Kebersamaan tim | FEAT-04, FEAT-05, FEAT-09 | Tanyakan apakah anggota mengetahui rekan satu tim dan merasa dilibatkan saat bermain bersama; kumpulkan umpan balik kualitatif, tanpa menganggap kehadiran sebagai bukti rasa kebersamaan |

Validasi di atas belum dilakukan. Tujuan kebersamaan merupakan keputusan produk; keberhasilan mencapainya belum menjadi fakta riset. Tidak ada baseline atau target numerik baru yang diasumsikan.

| ID | Tujuan/outcome | Definisi atau rumus | Sumber/cara ukur | Baseline | Target | Periode | Status bukti |
|---|---|---|---|---|---|---|---|
| GOAL-01 / KPI-01 | Pendaftaran mandiri | Peserta berstatus terdaftar yang menyelesaikan sendiri pendaftaran/konfirmasi ÷ seluruh peserta terdaftar ×100%; hitung orang, termasuk mode tim | Catatan pelaku konfirmasi dan peserta unik | Belum diukur | Usulan ≥80%, untuk menguji beban bantuan | Tiap kegiatan; empat kegiatan pilot | Hipotesis |
| GOAL-02 / KPI-02 | Persiapan tim/jadwal lebih ringkas | Menit kerja aktif pengelola sejak peserta ditutup sampai jadwal pertama dipublikasikan; jeda nonkerja dikecualikan | Catatan waktu pengelola dengan definisi sama pada simulasi manual dan sistem | Belum diukur | Usulan turun ≥30% dari simulasi pembanding setara | Median empat kegiatan pilot | Hipotesis; memerlukan baseline |
| GOAL-03 / KPI-03 | Hasil tersedia tepat waktu | Laga selesai aktual dengan hasil final ≤15 menit setelah selesai aktual ÷ seluruh laga selesai aktual ×100%; hasil belum final tetap denominator | Waktu selesai aktual, waktu finalisasi, laporan pengelola | Belum diukur | Usulan ≥90%, menilai kebutuhan petugas | Tiap kegiatan pilot | Hipotesis |
| GOAL-04 / KPI-04 | Cakupan presensi jelas | Peserta terdaftar dengan status hadir/tidak hadir eksplisit ÷ peserta terdaftar saat penutupan presensi ×100% | Rekap presensi; “belum dicatat” bukan “tidak hadir” | Belum diukur | Usulan ≥95% | Akhir tiap kegiatan pilot | Hipotesis |
| GOAL-05 / KPI-05 | Keputusan transfer dapat ditelusuri | Keputusan verifikasi/penolakan yang memiliki verifikator, waktu dan alasan saat ditolak ÷ seluruh keputusan verifikasi/penolakan ×100% | Audit pembayaran kegiatan/iuran; sesi gratis tanpa tagihan tidak masuk denominator | Belum diukur | Usulan 100%, menguji kelengkapan pencatatan | Tiap kegiatan berbayar atau periode iuran bulanan pilot | Target kualitas |

Guardrail: tidak ada perubahan skor atau bukti transfer yang dapat diakses di luar kewenangan pada skenario uji; tidak ada anggota ganda dalam satu kegiatan. Ini kriteria kualitas, bukan bukti keamanan produksi. Kelengkapan rincian gol dicatat terpisah agar KPI hasil final tidak menyamarkan statistik yang belum lengkap.

## 8. Risiko dan mitigasi

| Risiko | Dampak/indikator dini | Penilaian | Mitigasi/validasi |
|---|---|---|---|
| Anggota tetap menggunakan daftar di chat | Kuota sistem berbeda dari lapangan | Kemungkinan belum terukur | Satu tautan resmi, pengelola menghentikan daftar paralel pada pilot |
| Koneksi lapangan buruk | Hasil tertunda dan submit berulang | Masuk akal dari konteks, belum diukur | Uji koneksi, tampilkan status penyimpanan dan pemulihan tanpa duplikasi |
| Scope melebihi kapasitas tim | Banyak fungsi belum teruji mendekati akhir | Risiko menengah secara kualitatif karena luas scope | Reestimasi setelah Details v2.0; baseline 10–12 minggu tidak lagi menjadi jadwal rilis untuk perluasan scope |
| Identitas/transfer terbuka | Kerugian privasi dan hilangnya kepercayaan | Dampak tinggi; probabilitas belum diukur | Batas akses, minimisasi data dan uji negatif |
| Klaim tim “seimbang” disalahartikan | Keberatan terhadap pembagian | Asumsi perlu diuji | Label mode acak/skill, ukuran sebaran nilai, keterbatasan data, pratinjau dan penyesuaian manual |
| Pencatat tidak tersedia | KPI hasil gagal walau aplikasi berfungsi | Ketergantungan operasional | Tetapkan petugas dan pengganti sebelum kegiatan |

## 9. Roadmap dan batas teknologi

React + TypeScript, ASP.NET Core Web API, PostgreSQL dan Docker tetap terkunci. Rekomendasi yang diterima adalah modular monolith, autentikasi email/Google, pencarian/agregasi melalui PostgreSQL, logging dan health checks. Tidak ada kebutuhan AI, broker terdistribusi, NoSQL atau micro frontend pada MVP. Pengingat membutuhkan pekerjaan terjadwal persisten, pemulihan setelah restart dan kontrol retry/duplikasi; pilihan scheduler tetap terbuka. UI library, provider hosting/email/berkas belum dipilih.

Urutan: dokumentasi → fondasi dan staging → anggota/kegiatan/roster → jadwal/hasil → operasional → pilot → evaluasi KPI. Baseline 10–12 minggu bersifat historis; estimasi indikatif setelah perincian tersedia pada bagian 9 rencana v1.9 (154–220 hari-developer termasuk cadangan usulan); kapasitas dan kalender tetap perlu validasi tim. Urutan baru menambahkan publikasi/media, penilaian skill, voting, pengingat otomatis, kas bulanan dan dua tema sebelum pilot utuh. Prinsip multi-komunitas disiapkan pada batas data dan akses; tidak menambahkan onboarding tenant sekarang.

Pemicu evaluasi skalabilitas adalah hasil pengukuran latensi, beban puncak dan waktu agregasi yang gagal memenuhi target, bukan sekadar tercapainya 500 anggota. Optimalisasi dilakukan berdasarkan bottleneck terukur sebelum menambah layanan baru.

## 10. Peluncuran dan adopsi

Segmen awal hanya anggota Asker FC. Pengelola mengumumkan satu tautan melalui grup WhatsApp, pengelola membantu konfirmasi anggota, dan satu petugas mendampingi pada pilot. Pesan: **“Kick & Kumpul: yuk main bola dan kumpul bareng Asker FC. Pilih sesi, daftar, lalu ketemu di lapangan.”** Informasi jadwal dan hasil tetap tersedia sebagai pendukung. Tidak diperlukan iklan berbayar atau akuisisi komunitas lain.

Eksperimen aktivasi: amati jumlah anggota yang menyelesaikan konfirmasi tanpa bantuan (KPI-01), alasan gagal, dan waktu bantuan. Jika rendah, sederhanakan instruksi/formulir dalam scope sebelum menambah kanal. Evaluasi empat kegiatan pertama untuk melihat pola adopsi; jangan menyimpulkan market fit dari satu acara.

## 11. Keputusan, asumsi dan pekerjaan terbuka

- Baseline diterima v1.1: 18+, liga satu putaran, dua mode terpisah, poin 3/1/0, biaya manual dan dua belas gambar. Instruksi v1.5–v1.8 menggantikan batas publikasi, skill, notifikasi dan alur bayar; instruksi terbaru pengguna mengotorisasi pengerjaan paket revisi.
- Asumsi: operasi Indonesia, koneksi tersedia, developer penuh waktu, peserta anggota atau tamu berakun dengan cakupan kegiatan, pengelola bersedia menjadi pemilik proses.
- Identitas visual: [logo asli pengguna](./assets/logo-asker-fc.png) dipertahankan. UI memakai **hitam dan gold dominan sesuai logo**, ditetapkan pengguna 10 November 2026; tema gelap default dan tema terang netral tetap tersedia; judul Poppins Bold, foto aksi asli dengan izin, copy akrab/humoris dan mode gelap. Pilihan dalam scope dirinci pada Mockup SRS v2.1.
- Terbuka nonpenghambat dokumentasi: hosting, UI library, penyedia email/berkas dan anggaran.
- Sebelum produksi: pemilik operasional, dasar dan retensi pemrosesan data, mekanisme laporan usia, kebijakan walkover/withdrawal dan pengembalian dana harus disahkan komunitas. Dokumen turunan menandai default usulan yang dapat diuji, bukan menyatakan aturan klub sudah ada.
- Validasi lapangan belum dilakukan; wawancara dan penggunaan data anggota asli tidak dijalankan dalam pekerjaan ini.

## 12. Sumber

S1–S3 diverifikasi ulang 9 November 2026 melalui sumber resmi. S4 merupakan sumber baseline 8 November 2026; tidak menjadi dasar klaim pengiriman otomatis. Harga, kecocokan lokal dan ketiadaan fitur tidak diasumsikan.

- S1: [Tournify Getting started](https://help.tournifyapp.com/en/articles/15069302-getting-started-with-tournify), pembaruan 14 September 2026: jenis kompetisi, lapangan, pendaftaran dan batas aplikasi mobile.
- S2: [Challonge tournament features](https://docs.challonge.com/sv/features/tournaments): round robin, batas peserta, pendaftaran dan pelaporan hasil.
- S3: [TeamSnap ONE web tools](https://teams-help.teamsnap.com/article/3036-what-you-can-do-on-teamsnap-one-on-the-web), pembaruan 2 September 2026: roster, jadwal, RSVP dan batas web yang disebutkan.
- S4: [WhatsApp click to chat](https://faq.whatsapp.com/5913398998672934): tautan dengan pesan siap isi memerlukan tindakan pengguna; bukan jaminan pengiriman.
- Dasar kajian privasi dan teknologi mengikuti sumber dalam rencana v1.1. Review kepatuhan dan versi layanan sebelum produksi tetap terbuka; sumber tersebut bukan pembenaran untuk menambah scope.

## 13. Perluasan strategi dan keputusan revisi v2.0

Halaman aktif: Beranda; Jadwal & Daftar; Tim & Pemain; Hasil & Klasemen; Galeri; Kas & Iuran; Kontak & Sponsor. Toko/Merchandise hanya muncul bila kelak diaktifkan. Beranda/jadwal/hasil/media dapat publik hanya jika diterbitkan untuk publik; daftar anggota/foto tidak otomatis publik. Anggota melihat tagihannya sendiri dan ringkasan kas, bukan bukti atau kewajiban orang lain. Penilaian skill dibatasi pada subjek dan penilai berwenang. Pengelola kegiatan tidak otomatis menjadi petugas kas komunitas.

Persona tambahan (hipotesis): pengunjung dari tautan ingin melihat jadwal tanpa kehilangan tujuan saat masuk; pemain tamu ingin bergabung ke satu kegiatan tanpa memperoleh hak anggota komunitas/kas; petugas kas yang didelegasikan ingin menerbitkan iuran dan mencatat sewa lapangan tanpa hitung ganda. Delegasi adalah cakupan izin, bukan role global baru.

UVP dan empat fitur pembeda: pembagian acak atau skill, dua voting apresiasi, pengingat WhatsApp otomatis, serta mode gelap/mobile-first dalam alur komunitas. Ini keputusan scope, bukan bukti keunggulan pasar atau kemampuan yang tidak dimiliki pembanding. Album, sponsor dan pendaftaran juga sudah ditemukan pada pembanding Tournify; peluang tetap berupa kesesuaian alur lokal yang harus diuji.

Biaya tambahan: pengiriman WhatsApp menurut provider/negara/kategori saat dipilih, penyimpanan foto/backup serta layanan video/peta bila digunakan. Iuran bulanan mendanai operasional klub dan berbeda dari tagihan per kegiatan maupun pendapatan aplikasi. Buku kas sederhana tidak sama dengan pembukuan lengkap. Belum ada angka anggaran/ROI yang disahkan.

Guardrail pilot tambahan: kuota dan suara tidak ganda; foto draft dan bukti privat tidak terbuka; retry pengingat tidak membuat kiriman ulang yang tidak disengaja; perubahan saldo hanya dari transaksi sah sekali. KPI-01–KPI-04 tetap dengan definisi sebelumnya. KPI-05 diperluas pada seluruh keputusan pembayaran manual, termasuk iuran bulanan; periode pelaporan bulanan atau kegiatan terkait dengan denominator keputusan unik yang sama. Target usulan 100%, baseline belum diukur. Umpan balik voting, keadilan tim dan rasa kebersamaan bersifat kualitatif; jumlah suara bukan bukti kepuasan.

Riset integrasi 9 November 2026: dokumentasi resmi Twilio menjelaskan jendela layanan 24 jam dan penggunaan template disetujui di luarnya. Karena pengingat dapat dimulai bisnis, rencana harus memuat template, persetujuan penerima dan opt-out. Ini kajian kelayakan, bukan pemilihan Twilio; akun/provider, biaya dan persetujuan template masih terbuka. Rujukan Meta langsung belum berhasil dibaca pada sesi ini, sehingga ketentuan akhir wajib dicek saat provider dipilih.

- S5: [Twilio WhatsApp key concepts](https://www.twilio.com/docs/whatsapp/key-concepts), diakses 9 November 2026; jendela layanan dan template.
- S6: [Twilio WhatsApp overview](https://www.twilio.com/docs/whatsapp/api), diakses 9 November 2026; persetujuan penerima dan kebutuhan template.
- S7: [Twilio notification templates](https://www.twilio.com/docs/whatsapp/tutorial/send-whatsapp-notification-messages-templates), diakses 9 November 2026; pengiriman proaktif dan template yang dapat ditolak/dijeda.

Keputusan default operasional di Details v2.0 adalah usulan yang dapat diuji dengan data sintetis. Harus disahkan pemilik komunitas sebelum kegiatan nyata: nilai skill, eligibility voting, waktu pengingat, tarif/periode kas, publikasi media dan akses tamu. Foto aksi asli/kontak/sponsor belum diberikan; contoh visual diberi label. Provider dan validasi lapangan tidak ditutup secara fiktif oleh kelengkapan dokumen.
## 14. Penilaian pemain v2.2 — scope yang diproses

Instruksi pengguna memproses implementation plan v2.7 mengotorisasi perincian paket SRS dan parameter rekomendasinya untuk dokumentasi. Penilaian sembilan kategori masuk MVP melalui perluasan FEAT-07/09/10 dan promosi FEAT-28, tanpa mengubah stack, konsep Kick & Kumpul, toko opsional atau status privasi. Analitik yang memerlukan menit, tackle, passing atau save tetap di luar MVP input sederhana; bukan penundaan seluruh FEAT-28.

| Besaran | Nilai bagi pemain | Batas dan sumber |
|---|---|---|
| Poin mentah event | Mulai 50 di sembilan kategori saat pembagian tim disimpan; berubah dari bonus/potongan | Minimal 0, tidak dicap 100 sebelum perbandingan; bukan skill manual |
| Nilai event relatif | Tertinggi tiap kategori = 100; seri tertinggi bersama | Perbandingan pemain tampil lintas empat tim; dipengaruhi peserta lain dan bukan kemampuan absolut |
| Profil akumulasi | Seluruh perubahan poin permainan dipertahankan lintas event | Baseline spesifikasi 50 sekali, cap 0–100 setelah setiap event; bukan rerata dan bukan penjumlahan nilai relatif |

Preset event berisi 4 tim, 6 pasangan unik dan 3 laga per tim. Individu hanya diinput Gol/Assist; kebobolan berasal skor lawan. Partisipasi, kartu, presensi dan tiga konfirmasi kejadian khusus dipisahkan sebagai metadata. Skill manual 1–5 tetap privat dan tidak otomatis diganti oleh sembilan kategori.

Manfaat yang akan diuji: pemain bertahan dapat memperoleh penghargaan tanpa Gol/Assist, pemain memahami asal perubahan nilainya, dan koreksi tidak menggandakan poin. Katalog terdiri dari 12 achievement individu, 7 achievement tim dan 2 penyesuaian negatif tim. Voting MVP/bulanan tetap apresiasi terpisah tanpa bonus statistik pada katalog ini. Nama, syarat, nominal dan penumpukan dirinci di [Product Details](./03-product-details.md#13-penilaian-pemain-dan-sumber-poin-v22).

Risiko: nilai relatif 100 bisa timbul saat semua sama atau sama-sama terkena penalti; pemain tidak tampil harus dikecualikan dari pembanding. Bobot posisi dan kebobolan −2 merupakan baseline simulasi yang perlu kalibrasi, bukan bukti adil. Discipline belum mempunyai bonus pemulihan dan sejumlah kategori belum punya potongan; jangan menambahkan aturan fiktif. Klarifikasi pengguna 12 November 2026 menutup tiga baris penalti: terlambat 11–20 Commitment −2, >20 Commitment −3, absen Discipline −15/Commitment −12/Teamwork −5. Pengesahan tetap memerlukan verifikasi insiden, bukan hambatan nominal yang belum jelas.

Pilot tetap empat kegiatan menurut KPI lama; tambahkan pengamatan beban input Gol/Assist, kelengkapan sumber, koreksi penalti dan distribusi peringkat/achievement per posisi. Belum ada target numerik atau baseline baru. Riset pembanding dan provider pada bagian 3/12/13 tetap catatan bertanggal 9 November; tidak diklaim diverifikasi ulang pada revisi domain ini. Estimasi pembangunan perlu dihitung ulang dari perluasan Details, bukan memakai kalender historis.

## 15. Penyajian statistik v2.5

Spider chart/radar menjadi satu-satunya jenis grafik pada Event dan Profil; tidak ada pemilih jenis grafik. Tujuannya membantu anggota melihat bentuk kontribusi sembilan kategori tanpa mengubah nilai atau menyamakan makna nilai event relatif dan akumulasi profil. Data numerik tetap tersedia untuk aksesibilitas/ketelitian. Jenis grafik tidak menambah input statistik atau hak baca; detail implementasi mengikuti Details/Mockup. Ini perluasan presentasi FEAT-10/FEAT-28 yang sudah diminta pengguna, bukan penambahan metrik atau klaim hasil uji kegunaan.

## 16. Loyalitas anggota — FEAT-29

Sumber: instruksi pengguna 14 November 2026 dan rencana v2.12 bagian 14. PAIN-06 adalah hipotesis kebutuhan apresiasi frekuensi main yang belum dipenuhi statistik. GOAL-06: mendukung partisipasi berulang melalui reward yang dapat diklaim; dampak retensi belum terbukti.

| ID | Fitur/fungsi | Pain → goal | Nilai/fase | Dependensi |
|---|---|---|---|---|
| FEAT-29 | Poin loyalitas, saldo/riwayat, katalog, klaim, pemenuhan dan koreksi | PAIN-06 → GOAL-06 | MVP diperluas; apresiasi anggota yang sering main | FEAT-01/02/03/09; verifikasi kehadiran/partisipasi, anggaran dan petugas reward |

| ID | Definisi outcome | Sumber | Baseline/target | Periode |
|---|---|---|---|---|
| GOAL-06 / KPI-06 | Anggota penerima kredit sah bulan M yang bermain pada >=1 event sah bulan M+1 / seluruh anggota penerima kredit sah bulan M x100% | Presensi/partisipasi dan riwayat kredit aktif setelah koreksi | Belum diukur / belum ditetapkan | Bulanan, zona program; tunggu M+1 selesai; denominator nol = tidak berlaku |

Hitung anggota unik, catat versi koreksi. KPI mengukur partisipasi berulang, bukan membuktikan reward menyebabkan kenaikan. Pilot mengumpulkan alasan hadir/tidak hadir dan beban pemenuhan hadiah; tidak mengarang target.

Usulan baseline memberi nominal tetap sekali per event selesai dengan hadir dan minimal satu laga terverifikasi, terlepas dari performa/posisi. Saldo awal 0, tanpa cap 100 atau konversi statistik. Ketentuan lengkap dan keputusan aktivasi mengikuti rencana bagian 14. Tamu dan event sebelum aktivasi tidak memperoleh poin otomatis.

Risiko: presensi palsu ditangani verifikasi sumber; biaya/stok hadiah ditangani kuota dan pemilik anggaran; klaim ganda ditangani reservasi; koreksi setelah hadiah diterima ditangani riwayat selisih poin dan penahanan klaim. Saldo negatif bukan utang uang. Program bukan penjualan poin; reward terpisah dari toko opsional FEAT-27. Pemenuhan manual tidak menambahkan checkout/pengiriman atau kanal notifikasi baru. Gratis main harus direkonsiliasi petugas kas melalui prosedur koreksi berwenang dengan rujukan klaim, bukan otomatis dianggap pembayaran.

## 17. Profil terkelola dan penghargaan tahunan — v2.8

Otorisasi: instruksi pengguna melanjutkan rencana v2.15 hingga Mockup SRS. Admin/pengelola memelihara profil klub dan data resmi pemain; Member mengustomisasi profil sendiri dalam batas field. Peran Kapten dihapus, termasuk pada tim tamu. Persetujuan roster tetap dilakukan setiap pemain. Penanggung jawab tagihan tim adalah relasi kepemilikan tagihan, bukan peran baru dengan hak roster.

| ID | Kebutuhan | Nilai dan dependensi |
|---|---|---|
| FEAT-01/02 | Batas field profil, pengelolaan klub dan pemain, penolakan akses lintas anggota | Satu sumber data resmi; penugasan komunitas/event, audit dan verifikasi foto |
| FEAT-30 | Peringkat dan penghargaan tahunan bagi poin tertinggi | Apresiasi tahunan di luar klaim reward; bergantung pada sumber poin sah, aturan periode dan finalisasi |
| PAIN-07 → GOAL-07 | Apresiasi keaktifan belum memiliki pengakuan tahunan | Penghargaan berstatus dan bertahun pada profil; dampak motivasi masih hipotesis |

Baseline rancangan menggunakan poin loyalitas yang diperoleh dalam tahun, bersih dari koreksi sumber; penukaran reward tidak mengurangi poin tahunan. Ini usulan, bukan keputusan jenis poin pengguna. Nama kerja Pemain Terbaik Tahunan Asker FC terinspirasi status seperti Ballon d'Or; bukan klaim afiliasi. Hadiah material tambahan belum ditetapkan. Rating statistik tetap dihitung sistem dan tidak dikonversi menjadi poin loyalitas.

KPI-07 usulan: proporsi anggota aktif yang membuka peringkat tahunan pada periode publikasi; baseline/target belum tersedia, pengukuran memerlukan keputusan instrumentasi dan privasi. Pilot memeriksa pemahaman perbedaan saldo, poin tahunan dan rating, kejelasan status sementara/final serta proses koreksi. Tidak ada klaim peningkatan retensi sebelum pengukuran.

### Penyelarasan pembagian tim v2.9

UI Roster Admin dihapus dari gambar aktif. Alur berlaku: RSVP individu → pengacakan otomatis berbasis skill → pratinjau keseimbangan → simpan/kunci → lihat susunan. Referensi antartim, undangan tim, acak murni dan penempatan manual dalam baseline lama tidak berlaku untuk alur ini. ID lama dipertahankan untuk ketertelusuran; bagian yang digantikan bukan persyaratan aktif.
