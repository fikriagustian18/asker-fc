# Implementation Plan Proyek — Asker Football Club

- Versi: 2.13
- Tanggal: 14 November 2026
- Status: dokumentasi loyalitas selesai sampai mockup SRS; 14 referensi aktif termasuk SCR-37/SCR-38. Parameter program dan validasi produksi tetap terbuka.
- Bahasa: Indonesia; user story mengikuti format Inggris pada referensi skill.
- Tujuan file: rencana penyusunan paket SRS dan gambaran pembangunan produk; bukan otorisasi implementasi aplikasi.
- Upstream: jawaban pengguna butir 1–8 pada percakapan ini (tanpa nomor versi), ide awal, lampiran Untitled-1.txt, dan skill srs-documentation beserta referensi intake-and-plan. Perluasan loyalitas diotorisasi pengguna 14 November 2026.
- Prioritas sumber: jawaban terbaru pengguna mengungguli lampiran awal yang masih menyebut “pengelola ruang”.

## 1. Ringkasan dan dasar keputusan

Website komunitas fun football Asker Football Club berkonsep **“Kick & Kumpul”**, berupa web responsif mobile-first. Pengalaman utama memudahkan anggota ikut bermain, bertemu teman dan berkumpul lewat sesi rutin. Mendukung pendaftaran individu yang kemudian dibagi ke tim secara otomatis, pendaftaran antartim, serta pertandingan rutin. Liga/turnamen menjadi variasi kegiatan komunitas; format kompetisi MVP adalah liga dengan jumlah tim, pemain, durasi pertandingan dan lapangan yang dapat dikonfigurasi.

Masalah awal adalah pendaftaran, pembagian tim, jadwal, skor, dan transparansi informasi yang terpisah. Penggunaan WhatsApp dan spreadsheet merupakan **asumsi pengguna**, belum hasil observasi atau wawancara. Validasi proses saat ini menjadi bagian riset setelah persetujuan.

**Tujuan utama yang ditetapkan pengguna: memudahkan anggota tahu jadwal main, daftar, dan bayar, sekaligus membangun rasa kebersamaan tim.** Administrasi kegiatan, pembagian tim, presensi dan hasil membantu tujuan tersebut. Pembayaran memakai transfer/verifikasi manual pada MVP; kegiatan gratis tanpa tagihan kegiatan, sedangkan iuran bulanan dikelola terpisah sesuai kewajiban anggota. Baseline dan angka target KPI belum tersedia; tidak akan direkayasa.

### Keputusan pengguna

- Satu komunitas pada MVP, dengan desain yang memungkinkan multi-komunitas kemudian.
- Kapasitas perencanaan 100–500 anggota; bukan angka pengguna aktif bersamaan.
- Target pengembangan 8–12 minggu, tim 2–3 developer; asumsi perencanaan, bukan komitmen durasi.
- Stack inti terkunci: React, TypeScript, ASP.NET Core Web API, PostgreSQL, Docker.
- Fitur utama: keanggotaan, kegiatan, pembagian tim, jadwal, hasil, klasemen, presensi, statistik dasar pemain.
- Struktur halaman terbaru dari pengguna: Beranda; Jadwal & Daftar; Tim & Pemain; Hasil & Klasemen; Galeri; Kas & Iuran; Toko/Merchandise (opsional); Kontak & Sponsor. Tujuh halaman nonopsional masuk scope aktif, sementara toko dicatat sebagai modul opsional.

### Struktur halaman utama — keputusan pengguna terbaru

Konsep tetap **Kick & Kumpul**. Perjalanan utama anggota yang ditetapkan pengguna: **buka link → lihat jadwal → RSVP → terima pengingat → main → lihat skor dan foto → bayar iuran**. Informasi rekan setim mendukung persiapan sebelum main; iuran ditempatkan pada tahap setelah bermain dan melihat dokumentasi. Daftar berikut menjadi acuan navigasi dan memperluas baseline v1.4. Bagian ini mencatat kebutuhan, bukan klaim halaman telah diimplementasikan atau divisualisasikan ulang.

| No. | Halaman | Isi dan kemampuan yang diminta | Hasil yang dituju | Fase |
|---|---|---|---|---|
| 1 | Beranda | Hero foto tim, pesan Kick & Kumpul, jadwal main terdekat, CTA **Gabung Main**, statistik singkat jumlah anggota, pertandingan dan gol | Anggota segera tahu kegiatan terdekat dan merasa menjadi bagian komunitas | MVP diperluas |
| 2 | Jadwal & Daftar | Kalender sesi main, waktu/zona, lokasi lapangan dengan peta, kuota pemain, RSVP satu klik dan status keikutsertaan | Tahu jadwal dan daftar tanpa rekap lewat chat | MVP diperluas |
| 3 | Tim & Pemain | Daftar tim/anggota, profil berfoto, posisi bermain, nomor punggung, top skor dan MVP bulanan | Mengenal rekan bermain dan mengapresiasi kontribusi | MVP diperluas |
| 4 | Hasil & Klasemen | Skor pertandingan dan klasemen internal atau turnamen antar-komunitas | Hasil kegiatan mudah diikuti dengan konteks kompetisi yang jelas | MVP diperluas |
| 5 | Galeri | Album per pertandingan berisi foto dan video highlight | Mengabadikan pengalaman bersama dan memperkuat kebersamaan | MVP diperluas |
| 6 | Kas & Iuran | Ringkasan kas, transparansi iuran bulanan, biaya sewa lapangan dan status pembayaran | Anggota memahami kewajiban dan penggunaan dana komunitas | MVP diperluas |
| 7 | Toko/Merchandise | Jersey dan atribut tim | Mendukung identitas tim melalui merchandise | Opsional; belum otomatis masuk pembangunan MVP |
| 8 | Kontak & Sponsor | WhatsApp admin, tautan media sosial, area logo sponsor | Memudahkan komunikasi dan menampilkan pendukung komunitas | MVP diperluas |

Menu mobile harus tetap memudahkan akses delapan area tersebut: rekomendasi navigasi ringkas Beranda, Jadwal, Tim, Lainnya; area Lainnya memuat Hasil, Galeri, Kas, Kontak dan toko bila diaktifkan. Ini usulan pola navigasi untuk dirinci pada Mockup SRS, bukan delapan tab yang dipaksakan muat sekaligus. Akses pengelolaan tetap mengikuti peran, terpisah dari navigasi utama anggota/pengunjung.

### Alur utama anggota — keputusan pengguna v1.8

**Buka link → lihat jadwal → RSVP → terima pengingat → main → lihat skor dan foto → bayar iuran.** Urutan ini menjadi acuan perjalanan anggota, navigasi antarhalaman dan skenario pilot.

| Tahap | Perilaku dan transisi yang perlu diturunkan ke SRS |
|---|---|
| Buka link | Tautan komunitas membuka Beranda atau langsung ke detail sesi yang dibagikan. Sesi publik dapat dilihat sebelum masuk; konten terbatas meminta login sesuai izin. Tautan tidak valid atau sesi tidak tersedia diberi penjelasan dan akses kembali ke jadwal |
| Lihat jadwal | Anggota melihat waktu/zona, lokasi, kuota dan ketentuan biaya yang sudah diketahui, lalu memilih Gabung Main. Tautan langsung mempertahankan konteks sesi |
| RSVP | Anggota yang memenuhi syarat mengonfirmasi keikutsertaan. Jika perlu login, kembali ke sesi yang sama setelah login. Tampilkan status berhasil, penuh atau pendaftaran ditutup; RSVP berulang tidak menggandakan peserta |
| Terima pengingat | Peserta yang memenuhi kebijakan penerima mendapat pengingat WhatsApp sebelum main, berisi waktu, lokasi dan tautan sesi. Rekomendasi tautan tetap dapat dibuka dari HP. Kegagalan pengiriman tidak membatalkan RSVP; jadwal tetap dapat diakses melalui situs dan status pengiriman mengikuti bukti provider |
| Main | Anggota datang bermain dan berkumpul; susunan tim serta jadwal tersedia dan petugas mencatat presensi/hasil sesuai kewenangan. Kehadiran tidak otomatis disimpulkan dari RSVP atau pengiriman pengingat |
| Lihat skor dan foto | Setelah sesi, anggota membuka hasil dan album terkait dari detail sesi. Hasil yang belum final serta foto yang belum dipublikasikan diberi state yang jelas. Sediakan akses ke Kas & Iuran meskipun skor/foto belum tersedia |
| Bayar iuran | Anggota melihat tagihan miliknya, jenis/periode, nominal, tenggat dan instruksi transfer, mengunggah bukti lalu menunggu verifikasi manual. Tampilkan status menunggu verifikasi, perlu perbaikan/ditolak dengan alasan, atau lunas sesuai hasil verifikasi; unggah bukti tidak langsung berarti lunas. Jika tidak ada tagihan, tampilkan state tersebut tanpa membuat kewajiban baru |

Posisi bayar iuran setelah skor/foto adalah alur utama yang diminta pengguna; akses Kas & Iuran tetap tersedia langsung dan tidak menunggu publikasi media. Iuran bulanan tetap terpisah dari biaya kegiatan, sehingga kegiatan gratis tidak otomatis menghapus kewajiban iuran bulanan yang sah. Urutan ini tidak berarti tagihan bulanan dibuat setiap selesai pertandingan. Jika suatu kegiatan mensyaratkan biaya di muka, tampilkan sebagai cabang yang jelas sebelum RSVP, dengan ketentuan diumumkan sejak awal; jangan menjadikan pembayaran di muka sebagai alur utama semua sesi. Tarif, jatuh tempo dan aturan biaya tetap perlu dirinci.

### Batas perincian dan keputusan untuk halaman baru

| Area | Rekomendasi perencanaan | Keputusan yang masih terbuka |
|---|---|---|
| Hero dan statistik | Gunakan foto tim asli yang diberikan komunitas; jangan mengklaim foto sintetis sebagai dokumentasi nyata. Jumlah anggota = anggota aktif komunitas; pertandingan = laga nonbatal dengan hasil final; gol = skor tim pada hasil final, dengan hasil administratif ditandai/dikecualikan dari ringkasan gol bermain | Sumber foto, izin tayang, serta rentang ringkasan sepanjang waktu/periode perlu ditetapkan sebelum finalisasi konten |
| RSVP satu klik | Anggota aktif yang sudah masuk dapat memilih Ikut Main pada sesi terbuka dan mendapat konfirmasi/status dalam aksi yang sama; cek kuota dan keunikan saat simpan, cegah submit ganda. Pengunjung masuk dahulu lalu kembali ke sesi. Biaya/kebijakan terlihat sebelum aksi; RSVP tidak memindahkan uang | Ketentuan keanggotaan dan informasi wajib tambahan tidak boleh berubah menjadi formulir panjang tersembunyi pada klaim satu klik |
| Kalender dan peta | Kalender desktop dan agenda/list yang mudah dipakai di HP; setiap sesi memuat alamat, pin peta dan aksi buka petunjuk arah. Jika peta gagal, alamat dan tautan arah tetap tersedia | Provider peta, pengelolaan koordinat dan biaya integrasi belum dipilih |
| Profil pemain | Foto, posisi dan nomor punggung dapat dikelola melalui profil/roster. Rekomendasi nomor unik dalam satu tim/kegiatan, bukan unik seluruh komunitas; posisi dan nomor boleh belum diisi | Visibilitas profil/foto dan aturan nomor pada tim tetap versus tim sementara perlu ditetapkan |
| Top skor dan MVP bulanan | Top skor memakai rincian gol hasil final, mengecualikan gol bunuh diri dan menandai data belum lengkap. MVP pertandingan dan Pemain Terbaik Bulan Ini dipilih melalui voting terpisah; periode, publikasi dan revisi tercatat | Voting sudah ditetapkan pengguna; kelayakan pemilih/kandidat, periode, privasi suara dan aturan seri perlu dirinci |
| Antar-komunitas | Asker FC tetap penyelenggara satu komunitas; kegiatan dapat diikuti tim yang mewakili komunitas lain. Liga satu putaran tetap baseline format. Ini perluasan peserta kegiatan, dengan penamaan/asal tim yang jelas | Aturan akun/pendaftaran peserta tamu dan cakupan kapten/pencatat eksternal harus dirinci. Dukungan ini tidak otomatis mengaktifkan onboarding tenant atau format gugur |
| Galeri | Pengelola menautkan album ke pertandingan dan mengelola publikasi foto serta highlight. Rekomendasi MVP mengunggah foto dan mencatat tautan video/preview dari layanan yang disepakati; tanpa sistem transcoding/streaming sendiri | Sumber media, batas unggah, layanan video, izin publikasi, moderasi dan akses album belum ditetapkan |
| Kas dan iuran bulanan | Tambahkan tagihan per anggota/periode, verifikasi manual, saldo awal, pemasukan terverifikasi, biaya sewa lapangan dan ringkasan kas. Pisahkan jenis tagihan bulanan dan biaya kegiatan; catat transaksi sekali saja, beserta pelaku dan alasan koreksi | Tarif, jatuh tempo, penanggung jawab kas, kewenangan pencatatan pengeluaran dan aturan koreksi/saldo awal perlu ditetapkan. Rekomendasi penerbitan tagihan bulanan dilakukan manual per periode dengan pencegahan duplikasi |
| Kontak dan sponsor | Admin yang diberi kewenangan mengelola nomor WhatsApp publik, tautan media sosial dan logo sponsor beserta tautan opsional. Gunakan logo yang disediakan, bukan sponsor rekaan | Kontak resmi, akun sosial, aset sponsor dan urutan penempatan belum diberikan |
| Toko opsional | Rekomendasi fase lanjutan dimulai dari katalog jersey/atribut dan pemesanan lewat WhatsApp; menu disembunyikan bila tidak aktif | Aktivasi toko dan apakah hanya katalog atau mencakup stok, pesanan, checkout dan pengiriman belum diputuskan; fitur transaksi tidak dimasukkan diam-diam |

Rekomendasi di atas belum diperlakukan sebagai keputusan pengguna. Rincian yang belum dipilih ditutup saat penyelarasan PRD/Details, sebelum implementasi area terkait.

### Usulan akses informasi

- Publik: Beranda, kontak/media sosial/sponsor, serta jadwal, hasil dan media yang secara eksplisit dipublikasikan pengelola.
- Anggota yang masuk: RSVP, informasi tim/pemain sesuai izin, ringkasan kas dan rincian kewajiban/status pembayaran sendiri. Status pembayaran anggota lain dan bukti transfer tetap terbatas pada petugas berwenang.
- Profil foto/pemain dan album tidak otomatis menjadi publik karena halaman navigasinya tersedia. Tentukan jangkauan tayang untuk setiap jenis konten sebelum publikasi.
- Rekomendasi superadmin mengelola kas/iuran tingkat komunitas dan publikasi sponsor; pengelola kegiatan mengelola album, hasil, peserta dan biaya kegiatan yang ditugaskan. Delegasi kas/publikasi komunitas harus eksplisit.

Akses publik/anggota ini masih rekomendasi untuk review, bukan pengesahan bahwa semua data boleh dibuka. Batasan lama “halaman publik dan publikasi foto di luar MVP” digantikan oleh scope halaman baru dengan aturan publikasi yang perlu dirinci.

### Peran dan batas akses awal

| Peran | Cakupan |
|---|---|
| Superadmin | Seluruh komunitas, pengaturan, anggota, penugasan pengelola dan akses |
| Admin/pengelola | Kegiatan yang ditugaskan, peserta, tim, jadwal, validasi hasil dan biaya |
| Kapten | Roster timnya pada kegiatan terkait, tunduk pada tenggat dan persetujuan pengelola |
| Pencatat skor | Hasil pertandingan yang ditugaskan; koreksi mengikuti aturan audit |
| Anggota | Profil sendiri, pendaftaran, informasi kegiatan, jadwal dan hasil yang diizinkan |

Rekomendasi: satu akun boleh memiliki beberapa peran, dengan otorisasi berdasarkan komunitas, kegiatan, tim, atau pertandingan. Detail konflik kewenangan akan diturunkan pada PRD dan Product Details.

## 2. Matriks stack

Status “perlu keputusan” tidak mengubah stack inti. Persetujuan rencana menerima pilihan yang secara eksplisit direkomendasikan; pilihan yang masih “belum dipilih” tetap terbuka dan tidak dianggap otomatis diputuskan. Versi paket ditentukan menjelang pembangunan berdasarkan dukungan saat itu.

| Kategori | Kebutuhan produk | Teknologi pilihan | Status | Sumber keputusan | Implikasi/batasan |
|---|---|---|---|---|---|
| Frontend | Web mobile-first | React + TypeScript | ditetapkan | Pengguna | Satu aplikasi web responsif |
| Backend | API dan aturan domain | ASP.NET Core Web API | ditetapkan | Pengguna | Rekomendasi modular monolith untuk tim kecil |
| SQL | Transaksi dan relasi | PostgreSQL | ditetapkan | Pengguna | Konsistensi roster, hasil dan klasemen |
| Container | Lingkungan deployment | Docker | ditetapkan | Pengguna | Hosting, backup dan pengelolaan secret tetap perlu dipilih |
| NoSQL | Belum ada kebutuhan khusus | — | tidak diperlukan | Rekomendasi | Tidak menambah datastore MVP |
| Multi-platform | HP dan desktop | Web responsif | ditetapkan | Pengguna | Native mobile di luar MVP |
| Scalable search | Pencarian anggota/kegiatan | Query PostgreSQL | perlu keputusan | Rekomendasi | Search engine terpisah tidak diperlukan pada scope awal |
| Data analytics | Statistik dasar dan laporan | Agregasi PostgreSQL melalui API | perlu keputusan | Rekomendasi | Tanpa data warehouse MVP |
| Message brokers | Pekerjaan asinkron | — | tidak diperlukan | Rekomendasi | Pengingat memerlukan pekerjaan terjadwal persisten dan retry; broker terdistribusi belum diperlukan |
| AI framework | Pembagian tim | — | tidak diperlukan | Rekomendasi | Pembagian acak/skill berbasis aturan; tidak memerlukan AI |
| Monitoring tools | Diagnosis gangguan | Logging terstruktur dan health checks ASP.NET Core | perlu keputusan | Rekomendasi | Layanan agregasi log/alert mengikuti hosting |
| UI component library | Konsistensi dan aksesibilitas | Belum dipilih | perlu keputusan | Terbuka | Tidak menghambat dokumen; pilih sebelum implementasi UI |
| JavaScript animation library | Interaksi dasar | — | tidak diperlukan | Rekomendasi | Transisi CSS secukupnya |
| Micro frontend | Aplikasi satu tim kecil | — | tidak diperlukan | Rekomendasi | Satu frontend |
| Autentikasi | Email dan Google | Rekomendasi ASP.NET Core Identity + Google OAuth | perlu keputusan | Permintaan rekomendasi pengguna | Email/password terverifikasi, reset password, konfigurasi OAuth dan layanan email |
| Penyimpanan berkas | Bukti transfer privat; foto profil, foto tim/galeri dan logo sponsor dengan akses publikasi terpisah | Penyimpanan persisten, provider belum dipilih | perlu keputusan | Perluasan halaman pengguna | Pisahkan berkas privat dan media terpublikasi; batas ukuran/jenis, retensi dan backup harus dirinci |
| Peta/lokasi | Menampilkan lokasi lapangan dan petunjuk arah | Provider peta belum dipilih | perlu keputusan | Halaman Jadwal & Daftar | Koordinat/alamat wajib dapat dibaca meski peta gagal; biaya/ketentuan provider belum diverifikasi |
| Pengingat WhatsApp | Pengiriman otomatis jadwal | Provider belum dipilih; rekomendasi pekerjaan terjadwal persisten di backend | perlu keputusan | Fitur pembeda pengguna v1.6 | Biaya/ketentuan, persetujuan penerima, status provider, retry dan pencegahan duplikasi perlu diverifikasi |
| Tema UI | Mode gelap dan mobile-first | Token tema dan preferensi tersimpan pada React | ditetapkan | Fitur pembeda pengguna v1.6 | UI library tetap terbuka; periksa kontras dan state di HP |
| Video highlight | Media per pertandingan | Rekomendasi tautan/preview video, provider belum dipilih | perlu keputusan | Halaman Galeri | Scope MVP tidak memerlukan transcoding atau live streaming sendiri |

Desain multi-komunitas awal: identitas komunitas pada data dan batas akses domain, tanpa UI tenant, onboarding komunitas, billing SaaS, atau klaim isolasi multi-tenant yang belum diuji.

## 3. Scope fase dan rekomendasi domain

### MVP

1. Akun, profil anggota, penugasan peran, serta login email dan Google. Penyatuan akun harus diverifikasi, bukan semata-mata mencocokkan email.
2. Kegiatan liga dan fun football rutin; konfigurasi kapasitas, waktu, lapangan, serta buka/tutup pendaftaran.
3. Dua mode pendaftaran per kegiatan: individu atau antartim. Rekomendasi tidak mencampur kedua mode dalam satu kegiatan pada MVP untuk menghindari konflik roster.
4. Pembagian otomatis peserta individu dengan pilihan mode acak atau seimbang berdasarkan skill, kapasitas tim terjaga, pratinjau, perubahan manual dan penguncian roster. Mode skill menggunakan penilaian yang didefinisikan dan dapat ditinjau; aturan penilaian serta penanganan data belum lengkap harus dirinci sebelum implementasi.
5. Rekomendasi liga satu putaran; pembentukan jadwal memperhitungkan durasi, lapangan, dan larangan tim bermain bersamaan. Jadwal dapat direvisi oleh pengelola dengan pemeriksaan benturan.
6. Input hasil, validasi/finalisasi, koreksi beralasan dan jejak perubahan; klasemen dihitung dari hasil yang sah.
7. Rekomendasi poin menang/seri/kalah 3/1/0; urutan peringkat poin, selisih gol, lalu gol memasukkan. Jika tetap sama, tampilkan peringkat bersama; penentuan juara mengikuti aturan tambahan yang ditetapkan sebelum kegiatan dimulai, bukan keputusan ad hoc setelah hasil diketahui. Nilai dan urutan ini merupakan usulan untuk review, bukan aturan resmi klub.
8. Presensi kegiatan dan statistik dasar: jumlah kehadiran, penampilan pertandingan berdasarkan partisipasi yang dicatat, serta gol dari pencatatan pencetak gol. Tidak menyamakan hadir kegiatan dengan tampil di semua pertandingan.
9. Biaya opsional per kegiatan, unggah bukti transfer dan verifikasi manual. Bukti, penolakan dan verifikator dicatat; tanpa pencocokan bank otomatis. Pengembalian dana diselesaikan di luar sistem dan dapat diberi catatan.
10. Pengingat jadwal otomatis lewat WhatsApp menjadi kebutuhan MVP; tautan berbagi manual tetap pelengkap. Provider, biaya, persetujuan penerima dan konfigurasi pengiriman perlu ditetapkan. Status proses internal dibedakan dari status pengiriman yang benar-benar didukung provider; jangan mengklaim pesan pasti dibaca.
11. Beranda dengan hero foto tim, jadwal terdekat, Gabung Main dan ringkasan anggota/pertandingan/gol; cakupan tayang mengikuti kebijakan publikasi yang ditetapkan.
12. Kalender/agenda sesi, peta lapangan, kuota dan RSVP satu klik bagi anggota yang memenuhi prasyarat.
13. Profil berfoto, posisi, nomor punggung, top skor, voting MVP pertandingan dan voting “Pemain Terbaik Bulan Ini” per periode bulanan. Kedua pemilihan mempunyai kandidat, pemilih, periode dan hasil tersendiri; aturan kelayakan dan seri perlu dirinci.
14. Hasil/klasemen internal maupun kegiatan antar-komunitas yang dikelola Asker FC; kebijakan peserta tamu dirinci tanpa mengaktifkan multi-tenant operasional.
15. Galeri foto dan highlight video per pertandingan, beserta pengelolaan konten/publikasi.
16. Kas sederhana, tagihan iuran bulanan, biaya sewa lapangan dan transparansi status pembayaran sesuai izin, selain biaya kegiatan yang sudah ada.
17. Kontak admin, media sosial dan area sponsor yang dapat dikelola petugas berwenang.
18. Mode gelap dan tampilan mobile-first pada seluruh alur anggota dan pengelola, termasuk formulir, tabel, dialog, status dan pesan kesalahan; pilihan tema disimpan dan kontras tetap terbaca.

### Empat fitur pembeda — keputusan pengguna 9 November 2026

Instruksi terbaru pengguna menetapkan keempat fitur berikut dalam scope MVP dan menggantikan pembatasan baseline yang bertentangan. Detail di bawah merupakan rekomendasi perencanaan untuk diturunkan ke PRD/Details, bukan klaim implementasi atau aturan klub yang telah disahkan.

| Fitur | Perincian yang wajib direncanakan | Bukti penerimaan yang harus tersedia pada SRS |
|---|---|---|
| Pembagian tim acak atau seimbang berdasarkan skill | Pengelola memilih mode; tetapkan skala skill, pemberi nilai, akses nilai dan ukuran keseimbangan. Bila nilai belum lengkap, tampilkan kekurangan dan pilihan melengkapi atau beralih ke acak secara eksplisit. Pembagian tidak menjanjikan optimasi sempurna; perubahan manual dan pengacakan ulang tercatat | Semua peserta terdaftar dialokasikan sekali tanpa melampaui kapasitas; mode acak dan skill dapat diuji dengan data contoh; kualitas mode skill diperiksa menurut ukuran yang disepakati; roster terkunci terlindungi |
| Voting MVP dan Pemain Terbaik Bulan Ini | Pisahkan pemilihan per pertandingan dan per bulan; tetapkan kandidat/pemilih yang memenuhi syarat, voting diri sendiri, batas waktu/zona, perubahan suara, privasi suara, hasil seri, pembatalan dan publikasi hasil. Rekomendasi satu suara aktif per akun per pemilihan, dengan pencegahan duplikasi saat permintaan bersamaan | Pemilih berhak dapat memberikan suara pada periode terbuka; suara ganda/tidak berhak/terlambat ditolak; hasil dihitung dari suara sah; seri dan tanpa suara ditampilkan sesuai aturan; apresiasi bulanan tidak otomatis berasal dari jumlah gol atau jumlah MVP laga |
| Pengingat jadwal lewat WhatsApp | Pengiriman otomatis terjadwal kepada peserta yang memenuhi kebijakan penerima; tetapkan waktu pengingat, zona waktu, persetujuan/berhenti menerima, provider dan biaya. Simpan pekerjaan secara persisten, cegah pengiriman ganda, batasi retry dan sediakan pemantauan kegagalan. Revisi/pembatalan sesi memperbarui atau membatalkan pekerjaan terkait | Sesi yang layak menghasilkan pengingat sesuai waktu; sesi batal atau penerima yang berhenti menerima tidak dikirimi; restart/retry tidak menggandakan pengingat; kegagalan provider terlihat dan dapat ditangani petugas. Pengujian memakai penerima uji sebelum pengiriman nyata |
| Mode gelap dan mobile-first | Pilihan tema tersimpan; default awal hitam–gold gelap, Sesuai perangkat tersedia sebagai pilihan. Tetapkan hitam dan gold dominan sesuai logo; tema gelap menjadi default, tema terang memakai permukaan netral, header hitam dan gold, kontras teks/kontrol, fokus dan target sentuh; status tidak hanya dibedakan lewat warna | RSVP, pembayaran, voting dan pembagian tim dapat dipakai pada HP; pemeriksaan pada lebar 360/390/430 px dan desktop tanpa luapan halaman; tema tetap setelah muat ulang dan komponen/state utama terbaca dalam mode gelap |

Provider WhatsApp serta detail skala skill, kelayakan voting dan waktu pengingat tetap keputusan terbuka. Kebutuhan fiturnya sudah ditetapkan pengguna dan tidak lagi diposisikan sebagai opsi atau fase lanjutan. Tidak ada pengiriman pesan dalam tugas review dokumentasi ini.

### Batas operasional yang perlu diturunkan ke SRS

Ketentuan berikut merupakan rekomendasi untuk menghilangkan ambiguitas scope, bukan spesifikasi fitur lengkap.

| Area | Dasar perencanaan MVP |
|---|---|
| Fun football rutin | Satu kegiatan untuk setiap sesi/tanggal, dapat dibuat dengan menyalin konfigurasi kegiatan sebelumnya; tanpa mesin pengulangan kalender. Laga persahabatan dapat dicatat tanpa klasemen liga |
| Pendaftaran antartim | Kapten memilih peserta berakun dan masing-masing mengonfirmasi; tim dapat mewakili komunitas lain dalam kegiatan Asker FC. Alur tamu perlu dirinci; pemain tanpa akun tidak otomatis diizinkan dan kapten tidak membuat identitas pribadi tanpa persetujuan |
| Keunikan peserta | Satu anggota hanya masuk satu roster dalam kegiatan yang sama. Kapasitas diperiksa kembali ketika pendaftaran dikonfirmasi, termasuk permintaan bersamaan |
| Kuota dan pembatalan | Pendaftaran ditolak saat penuh dengan alasan jelas; waiting list otomatis di luar MVP. Pengunduran diri sebelum roster terkunci membebaskan kuota; setelah terkunci diproses pengelola dengan jejak perubahan |
| Pembagian tim | Dilakukan sebelum roster terkunci; pengacakan ulang ditinjau sebelum disimpan. Perubahan setelah jadwal terbit harus memperlihatkan dampaknya dan tidak menghapus hasil yang sudah dicatat |
| Jadwal | Konfigurasi mencakup slot tersedia, jeda antarlaga dan waktu istirahat minimum per tim. Larangan benturan berlaku pada tim dan lapangan; jika jadwal tidak layak, jelaskan kendalanya tanpa menjanjikan optimasi global |
| Hasil dan statistik | Pencatat mengirim hasil; pengelola memfinalkan. Hanya hasil final masuk klasemen. Koreksi menghitung ulang klasemen dan statistik secara konsisten, serta menyimpan pelaku dan alasan |
| Kelengkapan pencetak gol | Skor akhir tetap dapat difinalkan bila rincian pencetak gol belum lengkap; statistik gol diberi penanda belum lengkap. Gol bunuh diri tidak dikreditkan sebagai gol pemain penyerang |
| Pertandingan bermasalah | Penundaan dan pembatalan tidak otomatis menjadi skor 0–0. Aturan walkover dan pengunduran tim harus ditetapkan sebelum liga dimulai dan dijelaskan dalam Details |
| Biaya kegiatan | Rekomendasi satu tarif dan basis tagihan per kegiatan: per individu untuk mode individu, per tim untuk mode antartim. Status pembayaran dipisahkan dari status pendaftaran; tenggat dan dampak belum bayar ditetapkan pengelola sebelum pendaftaran dibuka |
| Iuran dan kas | Tagihan bulanan per anggota/periode terpisah dari tagihan kegiatan; pemasukan dari pembayaran terverifikasi ditautkan agar tidak dihitung dua kali. Saldo awal dan biaya lapangan dicatat dengan otorisasi/riwayat; laporan kas bukan pembukuan penuh |
| Publikasi media/profil | Konten draft tidak tampil publik; petugas berwenang menentukan cakupan tayang dan dapat menarik publikasi. Bukti transfer dan kontak pribadi tidak dipakai sebagai materi galeri/profil publik |
| Koneksi lapangan | MVP memerlukan koneksi; kegagalan penyimpanan diberi pesan dan opsi coba lagi tanpa menggandakan hasil. Sinkronisasi offline di luar MVP |


### Fase berikutnya

Multi-komunitas operasional beserta uji isolasi; payment gateway dan rekonsiliasi otomatis; penagihan berkala otomatis di luar penerbitan tagihan bulanan manual; notifikasi otomatis selain pengingat jadwal WhatsApp yang kini masuk MVP; statistik lanjutan di luar top skor/MVP bulanan yang diminta; liga beberapa putaran atau format kompetisi tambahan; dukungan peserta anak apabila diputuskan diperlukan. Toko/Merchandise tetap modul opsional dengan aktivasi dan kedalaman yang perlu diputuskan.

### Di luar scope paket ini

Implementasi aplikasi dan deployment produksi dalam tugas dokumentasi ini; aplikasi native, live streaming/transcoding sendiri, integrasi perangkat lapangan, payroll, pembukuan penuh, marketplace multi-penjual, serta klaim sertifikasi SRS atau kepatuhan hukum. Halaman toko opsional tidak disamakan dengan marketplace; checkout, stok dan pengiriman tetap memerlukan keputusan scope tersendiri.

## 4. Rekomendasi kebijakan usia dan privasi

**Usulan untuk review: MVP membatasi akun dan peserta pada usia 18 tahun ke atas**, dengan deklarasi usia, informasi batas usia yang jelas, penanganan laporan akun tidak sesuai, dan pemeriksaan terbatas oleh pengelola bila diperlukan. Hindari pengumpulan salinan identitas secara rutin. Pembatasan ini adalah pilihan scope yang diusulkan, bukan keputusan pengguna atau jaminan kepatuhan.

Jika klub memang memiliki peserta di bawah 18 tahun, scope harus diperbarui sebelum PRD difinalkan: mekanisme usia dan persetujuan orang tua/wali, verifikasi wali, batas akses profil, penarikan persetujuan, retensi/penghapusan, dan prosedur operasional. Jangan sekadar mengizinkan kapten mendaftarkan anak tanpa proses tersebut.

Asumsi yurisdiksi Indonesia perlu dikonfirmasi sebelum peluncuran. UU 27/2022 Pasal 25 mengatur persetujuan orang tua/wali untuk pemrosesan data anak. PP 17/2025 mendefinisikan anak sebagai seseorang di bawah 18 tahun serta memuat perlindungan dalam layanan elektronik. Penerapan lengkap, aturan pelaksana terkini dan mekanisme verifikasi perlu ditinjau berdasarkan layanan aktual; rekomendasi ini bukan kesimpulan legal final.

Data MVP dibatasi pada kebutuhan keanggotaan dan kegiatan. Nomor telepon, email, bukti transfer, dan data penilaian usia tidak ditampilkan di klasemen. Retensi berkas dan permintaan penghapusan akan dirumuskan tanpa mengarang masa simpan wajib.

## 5. Deliverable dan lokasi

Lokasi: `docs/srs/asker-fc/`. Format Markdown UTF-8, tautan relatif, gambar PNG. Kedalaman penuh untuk MVP diperluas pada v1.5; toko opsional/fase berikutnya berupa roadmap dan batas scope. Berkas yang sudah tersedia di bawah masih baseline v1.3 dan perlu direvisi; angka lama bukan jumlah final kebutuhan baru.

| File | Isi yang akan dibuat setelah persetujuan |
|---|---|
| [01-product-planning.md](./01-product-planning.md) | Masalah, asumsi, segmen, riset tiga pembanding, prioritas, strategi dan metrik |
| [02-prd.md](./02-prd.md) | Revisi v2.0: 28 user story/90 AC; halaman, voting, pengingat, skill, kas dan peserta tamu diselaraskan |
| [03-product-details.md](./03-product-details.md) | Revisi v2.0: 21 alur; RSVP satu aksi, skill, pengingat, voting, media, kas, tema dan peserta tamu dirinci |
| [04-mockup-srs.md](./04-mockup-srs.md) | Baseline 22 layar; inventaris dan 12 gambar terpilih harus diselaraskan dengan navigasi/halaman baru |
| [05-traceability.md](./05-traceability.md) | Pemetaan GOAL → PAIN/FEAT → US/AC → FLOW → SCR dan pemeriksaan aktual |
| [mockups/](./mockups/) | 12 PNG final: delapan mobile dan empat desktop |

Setiap dokumen mencatat versi, status, upstream beserta versi dan asumsi. ID stabil digunakan tanpa memakai ulang ID yang dihapus. Rencana ini diperbarui dengan keputusan dan progres.

Saat penyelarasan, pertahankan ID FEAT/US/AC/FLOW/SCR yang sudah ada untuk kebutuhan yang sama; tambahkan ID baru untuk fungsi baru. Bagian iuran bulanan manual dipindahkan dari roadmap ke MVP secara eksplisit; gateway dan penagihan otomatis tetap lanjutan. Perubahan fase dan pemisahan kebutuhan harus tercermin pada katalog fitur dan traceability, bukan sekadar mengganti versi header.

## 6. Urutan penyusunan

1. Rencana v1.5 mencatat delapan halaman dari instruksi pengguna sebagai perubahan scope yang diotorisasi; rekomendasi akses, MVP bulanan dan kedalaman toko tetap terbuka. Revisi rencana ini tidak meminta persetujuan ulang untuk halaman yang sudah diperintahkan.
2. Product Planning: sesuaikan katalog fitur/fase, nilai kebersamaan, kas dan publikasi; gunakan riset baseline sebagai sumber bertanggal, tambah verifikasi hanya pada klaim baru yang diperlukan.
3. PRD: turunkan setiap halaman ke story/AC, akses publik/anggota, RSVP satu klik, peserta antar-komunitas, media/profil, MVP bulanan dan kas/iuran. Tutup atau tandai keputusan domain yang belum final.
4. Product Details: definisikan status publikasi, unggah/penarikan media, penggantian foto, tagihan bulanan unik, pencatatan pengeluaran, koreksi kas, pemilihan MVP dan akses peserta tamu. Pertahankan aturan kuota, hasil final dan audit baseline.
5. Mockup SRS: revisi navigasi serta inventaris layar anggota/publik/admin, lalu perbarui 12 gambar representatif dalam anggaran yang sama. Foto tim/pemain dan materi sponsor membutuhkan sumber konten yang sah dan jelas.
6. Traceability: periksa semua delapan halaman terwakili atau ditandai opsional, hapus konflik batas scope lama, dan audit ulang versi sumber, tautan, akses, statistik serta hasil visual.

Tidak ada gate persetujuan per dokumen setelah rencana disetujui. Perubahan material yang diusulkan di luar scope dikembalikan untuk review.

Setiap tahap memiliki pemeriksaan internal sebelum lanjut: Planning membedakan bukti dan asumsi; PRD memberi AC yang dapat diuji untuk seluruh fitur MVP; Details menutup aturan status dan hak akses; Mockup menggunakan inventaris dari Details. Traceability diperbarui sejak PRD, bukan baru disusun di akhir. Pemeriksaan internal ini tidak meminta persetujuan tambahan pengguna.

## 7. Riset dan sumber

Riset baseline tersedia pada Product Planning v1.3: Tournify, Challonge dan TeamSnap ONE dari dokumentasi resmi dengan tanggal akses 8 November 2026. Riset tersebut belum memvalidasi perluasan galeri, MVP bulanan, kas/iuran, sponsor atau toko. Saat penyelarasan, verifikasi fakta baru dari sumber resmi yang relevan; jangan menyimpulkan harga, ketiadaan fitur atau dukungan integrasi tanpa bukti. Wawancara/pilot anggota tetap belum dilakukan.

Referensi awal yang diperiksa pada 8 November 2026:

- [Microsoft: Google external login setup](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/social/google-logins): konfigurasi OAuth memerlukan kredensial dan redirect URI; rahasia perlu disimpan aman. Mendukung kelayakan opsi login, bukan desain final autentikasi SPA.
- [UU 27/2022, JDIH Komdigi](https://jdih.komdigi.go.id/produk_hukum/view/id/832/t/crc32/): dasar kajian data pribadi dan data anak.
- [PP 17/2025, BPK](https://peraturan.bpk.go.id/Download/377994/PP%20Nomor%2017%20Tahun%202025.pdf): dasar kajian perlindungan anak dalam sistem elektronik.

Riset berikutnya memverifikasi integrasi WhatsApp, pilihan operasional email/hosting, dukungan stack dan kebutuhan privasi yang relevan. Wawancara anggota/pengelola memerlukan masukan pengguna; riset meja tidak dianggap sebagai validasi pengguna.

## 8. Scope visual

- Baseline memiliki 22 layar; revisi v1.5 diperkirakan **28–36 layar/varian utama** untuk tujuh halaman inti, detail dan pengelolaan. Ini perkiraan perencanaan; inventaris final mengikuti Details. Toko opsional belum dihitung sebagai alur transaksi.
- Tambahan visual v1.6: mode/penilaian skill, voting laga/bulanan, preferensi dan pemantauan pengingat, serta tema. Inventaris 28–36 layar tetap indikatif dan perlu ditinjau; anggaran tetap 12 gambar, termasuk contoh mode gelap. Semua state tetap dispesifikasikan tertulis.
- Kelompok layar: delapan area navigasi yang diminta (toko kondisional); autentikasi/profil; detail sesi/RSVP/peta; roster/posisi/nomor; top skor/MVP; album/detail media; kas/tagihan bulanan/pengeluaran; kontak/sponsor; serta konfigurasi, presensi, pembagian tim, skor dan administrasi yang sudah ada.
- Semua layar MVP mendapat spesifikasi tertulis, termasuk loading, kosong, error, tanpa izin dan sukses yang relevan.
- Anggaran tetap **12 gambar terpilih**: 8 mobile target 390×844 dan 4 desktop target 1440×900. Dua belas gambar lama menjadi baseline arsip saat penggantinya tersedia; tidak dianggap sudah mencakup hero, kalender, galeri dan kas baru. Penambahan jumlah di atas 12 tidak diotorisasi melalui asumsi agen.
- Rekomendasi distribusi mobile: Beranda, Jadwal & Daftar, detail sesi/RSVP, Tim & Pemain, Hasil & Klasemen, Galeri, Kas & Iuran, Kontak & Sponsor. Desktop: dashboard pengelola, konfigurasi sesi, pengelolaan galeri dan pengelolaan kas/verifikasi. Pemilihan final mengikuti Details dan tetap berjumlah 12.
- Login, pembagian tim, input skor, MVP bulanan serta toko opsional tetap mendapat spesifikasi teks sesuai scope; tidak semua memerlukan gambar terpisah. Semua fungsi pengelola tetap dapat digunakan di HP. Gambar hanya subset representatif dari inventaris final.
- Gambar berfokus pada state utama; state tambahan ditulis dalam spesifikasi. Gambar bukan prototipe interaktif.
- Identitas visual mengikuti instruksi pengguna 10 November 2026: **hitam dan gold dominan sesuai logo**. Arahan ini menggantikan hijau/putih/kuning v1.7–v1.9 sebagai acuan aktif. Gunakan [logo asli](./assets/logo-asker-fc.png) tanpa modifikasi. Konsep Kick & Kumpul, Poppins Bold, foto asli berizin dan copy akrab tetap berlaku.
- Pelaksanaan v1.9: 12 PNG pengganti dibuat dengan built-in imagegen dan diinspeksi; SCR-28 dikoreksi terarah. Foto aksi/profil, kontak resmi dan logo sponsor asli belum tersedia, sehingga konten terkait memakai state menunggu unggahan yang berlabel. Gambar referensi UI selesai, konten autentik tetap tertunda. Prompt, dimensi aktual dan batas raster tercatat di [Mockup SRS v2.1](./04-mockup-srs.md) dan [manifest gambar](./mockups/archive/v2.0/manifest-v2.json).
- Anggaran biaya uang untuk generasi gambar belum ditentukan; angka 12 adalah batas jumlah deliverable awal. Penambahan material perlu review.

### Gaya visual dan bahasa — keputusan pengguna v1.7

- **Suasana dan warna:** santai, energik dan akrab dengan kehidupan lapangan. Hijau lapangan menjadi warna utama, putih menjadi dasar tema terang, dengan oranye atau kuning sebagai aksen CTA, penekanan dan apresiasi. Pilihan aksen final dan nilai warna ditetapkan saat perincian token UI; keduanya merupakan alternatif yang diizinkan pengguna.
- **Tipografi:** judul tampil tebal dan mudah dipindai. Bebas Neue atau Poppins Bold adalah contoh dari pengguna, bukan pilihan font final yang sudah terkunci. Teks isi, jadwal, nominal dan formulir memakai font yang terbaca di HP; keluarga font, bobot, fallback dan ukuran ditetapkan pada Mockup SRS. Hindari penggunaan gaya judul yang mengurangi keterbacaan teks panjang.
- **Fotografi:** utamakan foto aksi asli anggota saat bermain dan berkumpul pada hero, profil yang relevan dan galeri. Perhatikan izin tayang, potongan foto di layar HP dan keterbacaan teks di atas foto. Bila aset belum tersedia, gunakan placeholder berlabel hingga foto asli tersedia; gambar sintetis tidak dianggap sebagai dokumentasi anggota.
- **Tone bahasa:** akrab, ringan dan humoris sesuai semangat fun football serta konsep Kick & Kumpul. Contoh arahan copy: “Yuk, turun ke lapangan!”, “Sepatu siap, tinggal kamu!” dan “Belum ada laga. Saatnya ajak teman main!” Humor bersifat ramah dan tidak mengejek kemampuan, hasil voting atau kondisi anggota. Label tindakan, jadwal, biaya, pembayaran dan pesan gagal tetap jelas; humor tidak mengaburkan status atau tindakan pemulihan.
- **Mode gelap dan mobile-first:** kebutuhan v1.6 tetap berlaku. Gunakan permukaan gelap dengan teks terang, identitas hijau serta aksen terpilih; putih tidak harus menjadi latar mode gelap. Periksa kontras CTA, teks di atas foto, status, fokus, dialog dan formulir pada kedua tema serta viewport HP.
- **Bukti penerimaan dokumentasi:** spesifikasi visual memuat token dua tema, hierarki font, penggunaan foto, contoh copy untuk CTA/state kosong/sukses/gagal, serta pemeriksaan kontras dan pemotongan foto pada HP. Subset 12 gambar tetap mencakup mode gelap dan mengikuti arahan visual ini; tidak menambah jumlah gambar secara otomatis.

Arahan gaya ini sudah diotorisasi pengguna. Nilai warna, pilihan font/aksen final dan aset foto masih perlu dirinci, tanpa mengubah scope fitur atau stack. Pembaruan dokumen turunan v2.0 dan 12 gambar referensi telah dilakukan pada pelaksanaan v1.9; konten asli masih perlu disediakan komunitas.

## 9. Gambaran pembangunan produk

Target awal pengguna adalah 8–12 minggu; baseline 10–12 minggu dibuat untuk scope v1.4. **Estimasi tersebut belum berlaku untuk scope v1.6 yang diperluas.** Kalender/peta, foto/profil, MVP bulanan, album/video, kas/iuran dan publikasi menambah pekerjaan. Estimasi baru perlu dihitung dari PRD/Details dan kapasitas 2–3 developer penuh waktu; tidak ada durasi baru yang dijanjikan tanpa dasar. Waktu penyusunan paket SRS tetap terpisah dari pembangunan aplikasi.

| Tahap baseline v1.4 | Alokasi historis, belum direestimasi | Hasil dasar yang tetap dibutuhkan |
|---|---|---|
| Fondasi | Minggu 1–2 | Struktur React/API, PostgreSQL, Docker, CI, autentikasi, batas akses, model komunitas dan lingkungan staging |
| Keanggotaan/kegiatan | Minggu 3–4 | Anggota, kegiatan, dua mode daftar, roster, kapten dan pembagian tim |
| Kompetisi | Minggu 5–6 | Jadwal liga, pemeriksaan konflik, skor, koreksi dan klasemen |
| Operasional | Minggu 7–8 | Presensi, statistik, pembayaran manual, tautan WhatsApp dan alur admin |
| Penguatan/pilot | Minggu 9–10 | Regresi lintas fitur, latihan backup/restore, UAT dan pilot satu kegiatan dari pendaftaran hingga hasil final |
| Cadangan | Minggu 11–12 | Perbaikan pilot, dokumentasi operasional dan persiapan peluncuran |

Tambahkan paket kerja v1.5: (a) halaman komunitas dan akses publik/anggota, (b) kalender/peta/RSVP, (c) profil/top skor/MVP, (d) galeri dan publikasi, (e) tagihan bulanan/buku kas/biaya lapangan, dan (f) kontak/sponsor. Toko memiliki estimasi terpisah jika diaktifkan. Penjadwalan baru harus memuat integrasi, migrasi kebutuhan, pengujian izin/media/kas dan UAT; tabel historis di atas bukan jadwal rilis baru. Bila target waktu tetap tidak berubah, prioritas perlu diputuskan bersama pengguna dan tidak memangkas halaman wajib diam-diam.

Pilot scope terbaru mengikuti tujuh tahap alur anggota v1.8 sampai verifikasi iuran, dengan pengiriman pengingat ke penerima uji. Akses hasil, foto dan kas diperiksa pada HP dalam kedua tema. Ini rencana pengujian, belum hasil uji aplikasi.

Pengujian berjalan pada setiap tahap, khususnya aturan liga, benturan jadwal, kuota, otorisasi lintas kegiatan/tim, pembayaran manual dan koreksi hasil bersamaan. Tahap pilot memeriksa integrasi keseluruhan, bukan menjadi awal pengujian.

Pembagian kerja indikatif: developer pertama memimpin API/data, developer kedua UI/integrasi, dan developer ketiga (bila tersedia) mendukung otomatisasi pengujian, operasional dan fitur lintas lapisan. Dengan dua developer, tanggung jawab pengujian/operasional dibagi keduanya; kapasitas ini harus masuk estimasi. Penilaian ulang dilakukan setelah Details dan satu alur utuh login → daftar kegiatan berhasil di staging.

Kesiapan peluncuran produk dibedakan dari kelengkapan paket SRS: pilot selesai, tidak ada cacat kritis terbuka pada akses/hasil/pembayaran, pemulihan backup diuji, pemilik operasional ditunjuk, serta keputusan hosting, email, retensi dan kebijakan usia ditutup. Angka performa dan beban uji ditetapkan dalam PRD dengan asumsi eksplisit.

Paket kerja tambahan v1.6 mencakup penilaian skill dan dua mode pembagian, dua siklus voting, pekerjaan pengingat persisten/integrasi WhatsApp, preferensi tema dan pengujian HP. Estimasi perlu dihitung ulang; tabel historis tidak menjanjikan tanggal rilis. Verifikasi dokumentasi resmi provider WhatsApp diperlukan untuk persetujuan penerima, template, biaya, batas layanan dan status pengiriman.

### Estimasi indikatif setelah perincian v2.0

Perhitungan ini **usulan awal oleh agen**, belum validasi tim, komitmen rilis atau hasil pengukuran pembangunan. Asumsi: developer berpengalaman bekerja penuh waktu 5 hari/minggu, satu komunitas, scope MVP v2.0, tanpa toko aktif/gateway/native. Aset, keputusan klub dan akun/provider siap saat dibutuhkan; waktu tunggu persetujuan eksternal tidak termasuk. Pengujian tetap berjalan per paket; penguatan mencakup integrasi/UAT/operasional tambahan.

| Paket kerja | Hari-developer usulan | Dasar cakupan |
|---|---|---|
| Fondasi, akun, akses dan lingkungan | 18–25 | Login, delegasi, data/berkas, staging/CI |
| Kegiatan, roster, jadwal dan hasil | 25–35 | Dua mode, kuota, liga/rutin, koreksi |
| Halaman komunitas, kalender/peta/profil | 18–25 | Beranda, RSVP, direktori/top skor, dua tema/mobile |
| Skill dan dua voting | 12–18 | Nilai, eligibility, suara unik, periode/seri |
| Media, kontak dan sponsor | 12–18 | Publikasi/audiens, album, tautan highlight |
| Pengingat WhatsApp | 8–12 | Preferensi, penjadwalan persisten, retry/rekonsiliasi |
| Kas/iuran dan verifikasi | 15–22 | Tagihan periode, pemasukan sekali, saldo/audit |
| Penguatan, pilot dan operasional | 20–28 | Regresi lintas fitur/tema, izin, restore/UAT |
| Total sebelum cadangan | **128–183** | Tidak menghitung waktu tunggu eksternal |
| Total dengan cadangan usulan 20% | **154–220** | Dibulatkan ke atas; bukan anggaran disahkan |

Kapasitas ideal 2 developer (10 hari-developer/minggu) memberi perkiraan aritmetis **16–22 minggu**; 3 developer (15 hari-developer/minggu) sekitar **11–15 minggu**. Ketergantungan urutan/onboarding/waktu tunggu dapat memperpanjang kalender; tidak semua paket dapat berjalan serentak. Target awal 8–12 minggu perlu review kapasitas/prioritas, tanpa menghapus fitur wajib diam-diam. Validasi ulang setelah alur utuh tersedia di staging.

## 10. Risiko dan keputusan terbuka

| Topik | Dampak | Status/penghambat | Penyelesaian |
|---|---|---|---|
| Proses manual belum divalidasi | Prioritas bisa meleset | Nonpenghambat rencana | Validasi contoh kegiatan dan masalah saat Planning |
| Usia dan negara operasional | Mengubah alur keanggotaan/privasi | Rekomendasi untuk review; penghambat finalisasi kebijakan | Default usulan 18+, Indonesia; revisi bila klub menerima anak |
| Aturan liga dan mode daftar | Mengubah jadwal/klasemen | Rekomendasi untuk review | Satu putaran, satu mode per kegiatan, 3/1/0 dan tiebreak sebagaimana di atas |
| Makna “seimbang” | Harapan kualitas tim tidak sama | Nonpenghambat | MVP mendukung mode acak dan seimbang berdasarkan skill; tetapkan skala, sumber nilai, ukuran keseimbangan dan penanganan data kosong |
| Jadwal padat atau tidak layak | Generator tidak bisa menempatkan semua laga | Nonpenghambat | Nyatakan ketidaklayakan dan kebutuhan penyesuaian slot/lapangan |
| Koreksi skor bersamaan | Statistik/klasemen tidak konsisten | Nonpenghambat | Finalisasi, audit dan pengendalian pembaruan bersamaan |
| Login, email dan berkas | Ketergantungan kredensial/provider | Nonpenghambat dokumen; penghambat peluncuran | Pilih hosting/provider dan uji pengiriman, OAuth, backup serta akses privat |
| Kapasitas 100–500 anggota | Belum menjelaskan beban puncak | Nonpenghambat | Tetapkan skenario uji dan target kinerja saat PRD, tandai sebagai target usulan |
| Target 8–12 minggu | Scope v1.6 lebih luas dari dasar estimasi | Penghambat komitmen tanggal rilis | Estimasi indikatif v2.0 tersedia pada bagian 9; validasi kapasitas tim sebelum komitmen rilis, baseline 10–12 minggu historis |
| Retensi dan kebijakan privasi | Data tersimpan berlebihan | Penghambat kebijakan produksi | Tetapkan bersama pemilik komunitas sebelum peluncuran |
| Anggaran/UI library | Memengaruhi operasional dan tampilan | Nonpenghambat | Logo dan palet telah diarahkan pengguna; anggaran dan UI library tetap terbuka |
| Akses publik/profil/galeri/kas | Salah cakupan tayang dapat membuka data yang tidak ditujukan publik | Penghambat finalisasi izin | Tetapkan matriks akses per halaman/aksi; ringkasan kas anggota dipisahkan dari bukti dan pembayaran orang lain |
| Foto tim/anggota dan logo sponsor | Hero dan konten autentik belum tersedia | Penghambat finalisasi konten visual terkait | Minta/siapkan aset asli saat tahap konten; gunakan label contoh bila belum final, tanpa mengklaim data nyata |
| Iuran bulanan dan kas | Tagihan atau pemasukan berpotensi terhitung ganda | Penghambat aturan kas final | Tetapkan periode, tarif, saldo awal, otorisasi dan hubungan pembayaran dengan ledger |
| MVP bulanan | Kriteria pemenang belum ditetapkan | Penghambat aturan apresiasi final | Voting MVP pertandingan dan Pemain Terbaik Bulan Ini sudah ditetapkan; rincikan kelayakan, periode, seri, privasi dan audit |
| Peserta antar-komunitas | Batas anggota klub dan peserta tamu belum terdefinisi | Penghambat alur tamu final | Tentukan akun, roster, izin dan penugasan petugas tanpa mengasumsikan multi-tenant |
| Video/peta/toko | Provider, biaya dan kedalaman belum dipilih | Nonpenghambat rencana, memengaruhi implementasi | Pilih provider/batas layanan saat perincian; toko tetap opsional |

## 11. Kriteria selesai paket

- Alur buka link → lihat jadwal → RSVP → terima pengingat → main → lihat skor dan foto → bayar iuran ditelusuri ke story/AC, alur dan layar. Pilot menguji alur utuh termasuk kembali ke sesi setelah login, pengingat gagal, skor/foto belum tersedia, tidak ada tagihan dan verifikasi pembayaran. Bukti transfer saja tidak boleh mengubah status menjadi lunas.

- Gaya santai dan energik tercermin pada hitam dan gold sesuai logo, judul tebal, foto aksi asli anggota dan copy akrab/humoris. Token dua tema, contoh copy dan aturan foto konsisten pada spesifikasi serta subset gambar; palet UI baseline lama tidak lagi menjadi acuan aktif.

- Empat fitur pembeda dipetakan ke story/AC, alur dan layar: pembagian acak/skill, voting MVP pertandingan dan Pemain Terbaik Bulan Ini, pengingat WhatsApp otomatis, serta mode gelap/mobile-first. Uji mencakup data skill kosong, suara ganda/seri/tenggat, revisi/batal/retry pengingat dan persistensi tema pada HP.

- Empat dokumen inti, traceability dan 12 gambar tersedia; bukan file placeholder.
- Tujuh halaman inti terwakili dalam kebutuhan/alur/layar; toko diberi status opsional dan batas yang eksplisit. Hero, kalender/peta/RSVP, profil, top skor/MVP, galeri, kas/iuran serta sponsor tidak hanya menjadi judul menu tanpa perilaku pendukung.
- Tagihan bulanan, saldo/pengeluaran kas, akses publik/media dan peserta antar-komunitas memiliki aturan serta acceptance criteria; konflik dengan pengecualian baseline lama telah diselesaikan.
- Kebutuhan MVP dapat ditelusuri ke tujuan, alur, acceptance criteria dan layar.
- Peran, biaya opsional, mode pendaftaran, roster, jadwal, hasil dan koreksi memiliki aturan yang konsisten.
- Kebutuhan nonfungsional membahas otorisasi, privasi, audit, aksesibilitas, backup/restore dan target performa dengan asumsi yang jelas.
- Riset memakai sumber dan tanggal; asumsi, rekomendasi dan keputusan pengguna dibedakan.
- Seluruh gambar dilihat dan diperiksa untuk keterbacaan, data contoh, konsistensi layar dan viewport.
- Pemeriksaan tautan, ID dan versi upstream selesai; kekurangan dicatat secara eksplisit.
- Paket disebut selesai hanya bila seluruh deliverable yang disepakati terpenuhi; selain itu status sebagian-tertunda dengan tindak lanjut.

## 12. Persetujuan dan progres

**Status pelaksanaan v1.9 (9 November 2026):** Dokumen Planning/PRD/Details/Mockup/Traceability v2.0 serta 12 gambar pengganti selesai diselaraskan, diinspeksi dan diaudit. Semua 84 AC MVP terhubung ke alur/layar; seluruh 90 AC tercakup pada traceability. Pemeriksaan ID, tautan, tabel, encoding, dimensi dan batas PRD lulus. Konten foto asli, kontak dan sponsor tetap tertunda.

**Riwayat status revisi v1.8 (9 November 2026):** Instruksi pengguna menetapkan alur buka link sampai bayar iuran setelah melihat skor/foto. Review rencana selesai; sinkronisasi dokumen turunan, navigasi mockup dan traceability alur masih tertunda. Scope fitur, gaya visual dan anggaran 12 gambar tetap berlaku.

**Riwayat status revisi v1.7 (9 November 2026):** Instruksi gaya visual terbaru mengotorisasi revisi palet UI, tipografi, fotografi dan tone bahasa. Review rencana selesai; sinkronisasi Planning/PRD/Details/Mockup/Traceability dan 12 gambar masih tertunda. Logo asli, empat fitur pembeda dan mobile-first tetap berlaku.

**Riwayat status revisi v1.6 (9 November 2026):** Empat fitur pembeda ditetapkan pengguna; rincian aturan dan provider tetap terbuka. Review implementation plan selesai. Planning/PRD/Details/Mockup/Traceability dan gambar masih perlu sinkronisasi fitur, fase, story/AC, alur, layar dan versi upstream; paket tetap sebagian-tertunda.

**Riwayat status revisi v1.5 (9 November 2026):** daftar delapan halaman dari pengguna menjadi otorisasi perubahan scope rencana ini. Pernyataan baseline selesai di log lama hanya berlaku untuk scope sebelum v1.5. Revisi ini menyelesaikan pembaruan implementation plan yang dirujuk pengguna; belum menjalankan penulisan ulang seluruh SRS atau generasi mockup baru. Pilihan yang masih berupa rekomendasi (akses, rincian aturan voting, provider, peserta tamu dan kedalaman toko) belum otomatis disahkan.

**Versi terbaru diotorisasi: 1.8 → diproses sebagai 1.9.** Instruksi pengguna 9 November 2026: “proses implementation plan yang sebagin masih tertunda”. Ini otorisasi untuk menyelaraskan paket dokumentasi dan 12 mockup dalam scope terbaru, bukan implementasi aplikasi.

**Persetujuan baseline historis: 1.1.** Bukti instruksi pengguna pada 8 November 2026: “proses implementation plan tersebut”, dengan lampiran rencana ini. Instruksi diberikan setelah rencana v1.1 disajikan untuk persetujuan; diterima sebagai persetujuan scope dan rekomendasi eksplisit. Status disetujui → dikerjakan. Pilihan provider yang belum ditentukan tetap terbuka.

Persetujuan versi 1.1 menerima scope dokumentasi dan rekomendasi eksplisit sebagai dasar kerja. Keputusan tanpa pilihan konkret (misalnya provider hosting dan UI library) tetap terbuka. Pengguna dapat meminta revisi, khususnya usia minimum, aturan liga, mode daftar, batas operasional dan jumlah gambar. “Lanjut” sesudah review dapat menjadi persetujuan sesuai workflow skill. Permintaan “review implementation plan dan optimize jika diperlukan” mengotorisasi revisi rencana ini, bukan pembuatan dokumen turunannya.

| Deliverable/tahap | Status |
|---|---|
| Intake | Selesai; rekomendasi tersisa termuat di rencana |
| Implementation Plan v1.9 | Rencana v1.8 diotorisasi pengguna dan diproses; progres/keputusan terbuka dicatat |
| Product Planning v2.0 | Tersusun: 28 fitur, perubahan fase, persona tamu/petugas kas, biaya dan riset ulang sumber resmi |
| PRD v2.0 | Tersusun: 28 story/90 AC, tujuh halaman aktif, toko opsional dan izin perluasan |
| Product Details v2.0 | Tersusun: 21 alur, state/validasi/gagal/pemulihan, default operasional berlabel usulan |
| Mockup SRS v2.0 dan gambar | 36 spesifikasi layar (35 MVP + toko kondisional); 12 PNG pengganti diinspeksi, prompt/dimensi/temuan tersimpan pada manifest. Konten asli foto/kontak/sponsor masih tertunda |
| Traceability v2.0 | Pemetaan 28 fitur/28 story/90 AC/21 alur/36 layar dan NFR selesai; audit struktural serta cakupan 84 AC MVP lulus |

### Riwayat perubahan dan hasil review

Revisi pelaksanaan v1.9 — 9 November 2026: pengguna mengotorisasi pemrosesan pekerjaan tertunda pada rencana v1.8. Paket v2.0 mempertahankan ID baseline, mempromosikan FEAT-15/FEAT-16 ke MVP, memisahkan statistik lanjutan FEAT-28, menambahkan story/AC, sembilan alur dan 14 spesifikasi layar (satu toko kondisional). Gambar hijau–putih–kuning dan dua tema mengganti subset lama dengan anggaran tetap 12. Foto/kontak/sponsor asli tidak direkayasa; keputusan provider dan pengesahan aturan produksi tetap terbuka.

Revisi v1.8 — 9 November 2026: pengguna menetapkan alur buka link → lihat jadwal → RSVP → terima pengingat → main → lihat skor dan foto → bayar iuran. Perjalanan sebelumnya yang menempatkan pembayaran sebelum main diganti sebagai alur utama. Rencana merinci transisi, state pengingat/hasil/media/tagihan, pemisahan iuran bulanan dari biaya kegiatan dan verifikasi manual. Kriteria selesai dan pilot diperluas hingga tahap iuran; dokumen turunan dan gambar belum disinkronkan.

Revisi v1.7 — 9 November 2026: pengguna menetapkan nuansa santai/energik, hijau lapangan dan putih dengan aksen oranye atau kuning, judul tebal (contoh Bebas Neue atau Poppins Bold), foto aksi asli anggota serta bahasa akrab/humoris. Palet UI hitam–emas–perak baseline diganti secara eksplisit; logo asli dipertahankan. Mode gelap diadaptasi ke arah warna baru. Nilai warna/font/aksen final dan foto tetap perlu perincian; status paket turunan serta gambar belum sinkron. Anggaran tetap 12 gambar.

Revisi v1.6 — 9 November 2026: instruksi pengguna mengotorisasi empat fitur pembeda dalam MVP. Review memperbaiki penundaan skill/notifikasi, asumsi MVP manual dan ketiadaan mode gelap. Baseline turunan belum diselaraskan; tidak ada implementasi, riset integrasi terbaru atau generasi gambar dalam review ini.

Revisi v1.5 — 9 November 2026: instruksi pengguna merinci Beranda, Jadwal & Daftar, Tim & Pemain, Hasil & Klasemen, Galeri, Kas & Iuran, Toko/Merchandise opsional, serta Kontak & Sponsor. Rencana memperluas MVP pada tujuh halaman inti, mencatat toko sebagai opsional, dan membedakan kebutuhan yang diperintahkan dari rekomendasi yang belum diputuskan. Pengecualian publikasi foto dan penundaan iuran bulanan manual pada baseline lama diganti secara eksplisit. Estimasi 10–12 minggu menjadi historis; inventaris baru diperkirakan 28–36 layar dan anggaran gambar tetap 12 terpilih. Paket turunannya ditandai perlu sinkronisasi, bukan diklaim selesai untuk scope baru.

Revisi v1.4: instruksi pengguna “makesure Tujuan memudahkan anggota tahu jadwal main, daftar, dan bayar, sekaligus membangun rasa kebersamaan tim.” Tujuan tersebut ditetapkan pada ringkasan, persona, janji produk dan evaluasi pilot, lalu dipetakan ke fitur/alur/layar yang sudah ada. Ini klarifikasi tujuan tanpa perubahan stack, lingkup fitur, definisi KPI atau gambar. Dokumen turunan menjadi v1.3.

| Versi | Perubahan | Dasar |
|---|---|---|
| 1.0 | Rencana awal dari jawaban pengguna dan referensi awal | Intake pengguna |
| 1.1 | Memperjelas alur fun football rutin, roster, kuota, pembayaran dan koreksi hasil; menghilangkan tiebreak ad hoc; membedakan baseline 10–12 minggu dan target percepatan; memajukan pengujian ke setiap tahap; memperjelas persetujuan dan cakupan visual mobile | Permintaan pengguna untuk review dan optimasi |
| 1.2 | Gunakan logo pengguna dan palet hitam–emas–perak pada seluruh mockup; 12 gambar direvisi, versi lama disimpan sebagai arsip | Instruksi pengguna “gunakan icon tersebut dan sesuaikan warnanya dengan icon tersebut”; otorisasi revisi visual, tanpa gate baru |
| 1.3 | Tegaskan konsep Kick & Kumpul sebagai website komunitas fun football: bermain bersama, pertemanan dan kebersamaan; liga tetap fasilitas kegiatan | Instruksi pengguna “makesure Konsep adalah \"Kick & Kumpul\", website komunitas fun football”; otorisasi klarifikasi konsep tanpa perubahan fitur/stack |

Hasil review: stack inti dan seluruh fitur prioritas pengguna tetap tercakup. Temuan utama v1.0 adalah beberapa aturan operasional belum dibatasi, tabel jadwal tidak mendukung target delapan minggu tanpa percepatan, dan persetujuan berisiko dianggap menutup pilihan yang belum tersedia. Revisi ini memperbaiki ketiganya. Belum ada validasi estimasi dengan tim pelaksana, uji aplikasi, atau riset kompetitor; rencana tidak mengklaim hal tersebut selesai. Referensi hukum/teknologi awal tidak diperbarui melalui review editorial ini dan tetap perlu verifikasi sesuai tahap riset.

Tidak ada dokumen paket atau gambar yang dibuat sebelum persetujuan.

### Log pelaksanaan setelah persetujuan — 8 November 2026

1. Instruksi “proses implementation plan tersebut” diterima sebagai persetujuan v1.1; progres disetujui → dikerjakan → selesai untuk paket dokumentasi.
2. Rekomendasi eksplisit dijadikan baseline: usia 18+, liga satu putaran, dua mode kegiatan terpisah, aturan poin/peringkat, pembagian berdasarkan jumlah, pembayaran manual dan 12 gambar. Provider hosting/email/berkas, UI library serta anggaran tetap terbuka.
3. Product Planning memuat tiga pembanding terverifikasi dari sumber resmi: Tournify, Challonge dan TeamSnap ONE. Harga, market fit dan observasi proses manual tidak diklaim sudah terbukti.
4. Perincian dalam scope: superadmin membuat/menyalin draft dan menugaskan admin; default operasional walkover, withdrawal, kebijakan biaya dan zona waktu ditandai usulan. Default itu perlu disahkan sebelum kegiatan nyata, tanpa menghambat kelengkapan dokumen.
5. Gambar menggunakan built-in imagegen, disimpan ke direktori proyek, diperiksa langsung dan dikoreksi pada layar masuk, jadwal, dashboard serta konfigurasi. Format PNG asli dipertahankan; dimensi aktual dan catatan visual minor tersedia pada manifest Mockup SRS.
6. Pemeriksaan ID, resolusi tautan lokal, karakter encoding, cakupan MVP/lanjutan dan batas konten PRD dijalankan. Kebutuhan nonfungsional dan skenario uji produk dipetakan pada traceability; belum merupakan hasil uji aplikasi.
7. Tidak ada deliverable paket yang tertunda. Validasi lapangan, keputusan operasional produksi, pemilihan provider dan pembangunan aplikasi tetap pekerjaan berikutnya sesuai batas scope. Tidak ada pengiriman pesan ke anggota, deployment atau implementasi aplikasi dalam pekerjaan ini.
8. Revisi v1.2: instruksi pengguna beserta logo mengotorisasi penggantian identitas visual. Logo asli disimpan tanpa modifikasi, semua 12 mockup diperbarui dengan built-in image editing; gambar lama dipertahankan pada mockups/archive/v1.0. Planning/PRD/Details/Mockup/Traceability menjadi v1.1 dengan sumber versi yang sinkron. Tidak ada perubahan stack, scope fitur, ID, KPI atau aturan bisnis.

### Hasil pelaksanaan revisi — 9 November 2026

1. Instruksi pengguna memproses rencana tertunda mengotorisasi v1.8; rencana menjadi v1.9 dan paket turunan v2.0. Scope tetap dokumentasi, tanpa pembangunan/deployment/pengiriman pesan.
2. Planning/PRD/Details/Mockup/Traceability diselaraskan substantif: 28 fitur/story, 90 AC, 21 flow, 36 layar (satu toko kondisional). ID lama dipertahankan, pengingat dan skill masuk MVP; default yang belum disahkan berlabel usulan.
3. Riset tiga pembanding resmi diperiksa ulang; kajian WhatsApp memakai dokumentasi resmi provider. Tidak mengklaim provider/biaya dipilih atau riset anggota selesai.
4. Dua belas PNG hijau–putih–kuning dibuat dengan built-in imagegen, disalin lokal dan dilihat langsung: delapan mobile (dua gelap), empat desktop. Dimensi berbeda dari target viewport dicatat. Baseline hitam–emas diarsipkan v1.1, arsip v1.0 tetap ada.
5. Audit tidak menemukan definisi ID ganda/tidak terdefinisi, tautan lokal rusak, tabel tidak konsisten atau karakter pengganti UTF-8. Semua 90 AC ada pada traceability; 84 AC MVP terhubung ke flow/layar. Manifest dimensi dan batas isi PRD diperiksa. Ini audit dokumen, bukan hasil uji aplikasi.
6. Estimasi usaha indikatif dihitung setelah perincian dengan asumsi eksplisit; tim perlu memvalidasi sebelum mengunci kalender rilis.
7. **Sisa tertunda konten:** foto aksi/profil asli dan izin tayang, nomor admin/media sosial resmi dan logo sponsor. Berikutnya komunitas menyediakan aset/data; petugas meninjau izin/audiens lalu mengganti state belum tersedia. Provider/akun WhatsApp, hosting dan pengesahan aturan produksi mengikuti daftar keputusan pada Traceability; spesifikasi sudah tersedia tetapi belum siap dipakai di produksi.

Status akhir: **dokumen dan 12 gambar referensi selesai; paket tetap sebagian-tertunda pada konten autentik yang memerlukan masukan komunitas**. Tidak ada gate persetujuan ulang atas perubahan yang sudah diperintahkan.
## 12. Revisi visual terotorisasi — 10 November 2026

Instruksi pengguna melanjutkan SRS dengan skill srs-documentation dan hitam–gold mengotorisasi revisi paket yang ada, tanpa gate baru. Rencana v2.0 dan dokumen v2.1 mempertahankan ID dan scope: 28 fitur/story, 90 AC, 21 flow, 36 layar. AC-71 dan FLOW-20 diselaraskan untuk default tema gelap; AC-73 diselaraskan ke palet baru. Dua belas referensi v3 menjadi acuan aktif: delapan mobile dan empat desktop. Berkas v2 serta arsip v1 dipertahankan sebagai riwayat.

Inspeksi visual, dimensi PNG, kontras token solid, tautan lokal dan encoding dicatat pada [Mockup SRS](./04-mockup-srs.md) dan [Traceability](./05-traceability.md). Foto asli/izin, kontak, sponsor, provider dan pengesahan aturan produksi masih tertunda. Tidak ada implementasi aplikasi, deployment atau pengiriman pesan.

## 13. Implementation plan penilaian pemain berbasis posisi — 11 November 2026

### 13.1. Dasar, otorisasi dan batas tahap

**Tahap saat ini: pelaksanaan rencana v2.7 yang disetujui.** Instruksi pengguna 11 November 2026 “proses implementation plan tersebut” diberikan setelah review v2.7. Ini menerima scope dan rekomendasi eksplisit untuk baseline dokumentasi. Persetujuan pemrosesan tidak mengarang jawaban atas nominal penalti yang saat itu ambigu. Klarifikasi pengguna 12 November 2026 kini menetapkan nominal pada 13.6/13.12; konten asli dan keputusan produksi tetap terpisah. Riwayat sebelumnya meminta rencana disusun dahulu; gate tersebut sudah terpenuhi. Tidak menjalankan implementasi aplikasi.

Bagian 13 menjadi acuan aktif untuk perubahan penilaian pemain. Bagian 1–12 merekam baseline/riwayat sebelumnya; aturan statistik, fase atau persetujuan historis yang bertentangan tidak mengungguli keputusan pengguna terbaru di bagian ini. Review historis saja tidak mengotorisasi pemrosesan, tetapi instruksi terbaru mengotorisasi dokumen turunan dan visual dalam scope; implementasi aplikasi tetap di luar tugas. Status/progres terbaru ada pada 13.11. Stack inti React, TypeScript, ASP.NET Core Web API, PostgreSQL dan Docker tetap berlaku. Model ini menggunakan perhitungan berbasis aturan tanpa kebutuhan AI atau layanan statistik tambahan.

| Sumber | Kebutuhan atau keputusan |
|---|---|
| Instruksi pengguna 11 November 2026 | Satu event berisi 4 tim, masing-masing 3 pertandingan, total 6 pertandingan |
| Instruksi pengguna | Statistik individu pertandingan hanya Gol dan Assist untuk seluruh tim; kebobolan berasal dari skor tim |
| Instruksi pengguna | Gol/Assist menjadi dasar utama Striker dan Midfielder; kebobolan tim menjadi dasar utama Defender dan Goalkeeper |
| Instruksi pengguna | Achievement memberikan poin peningkat statistik; instruksi penalti terbaru menggantikan potongan semua kategori dengan potongan kategori terkait |
| Keputusan pengguna mengenai cakupan | Sembilan kategori tetap skala 0–100; statistik event menempatkan pemain dengan poin tertinggi pada setiap kategori di 100, sedangkan statistik profil adalah akumulasi seluruh permainan, bukan rerata event |
| Klarifikasi pengguna mengenai baseline | Setiap pemain memiliki poin mentah awal 50 pada kesembilan kategori setelah tim dibentuk/diacak dalam setiap event; nilai mentah ini berbeda dari normalisasi event dan profil |
| Masukan achievement terbaru pengguna | Pertimbangkan 12 achievement individu dan 9 achievement/penyesuaian tim dengan nominal dan kategori tujuan yang dirinci pada 13.5; Brace/Hat-Trick tidak ditumpuk, penyesuaian klasemen sekali setelah event selesai, hanya pemain yang benar-benar berpartisipasi |
| Instruksi penalti terbaru | Kartu dicatat per pertandingan; penalti keikutsertaan meliputi outlist setelah pengacakan, terlambat, absen dan tidak mengikuti laga. Status/alasan harus diverifikasi admin; kejadian yang sama tidak dihitung dua kali dan pemain berhak mengajukan koreksi |
| Keputusan kategori yang sudah diklarifikasi | Gunakan sembilan kategori: Finishing, Playmaking, Defending, Teamwork, Discipline, Commitment, Consistency, Impact dan Achievement. Penyebutan awal “lima” tidak lagi menjadi pertanyaan terbuka setelah klarifikasi pengguna |
| Usulan agen untuk review | Nilai awal 50 per kategori/event sudah ditetapkan pengguna. Bobot posisi, potongan kebobolan, metode rasio normalisasi, kelayakan perbandingan event dan formula akumulasi profil (baseline 50 sekali, cap setelah setiap event) masih usulan. Nominal achievement/penalti mengikuti masukan pengguna; definisi gol penentu, metadata hasil, aturan seri, streak dan pengecualian tumpukan selain Brace/Hat-Trick adalah usulan perincian, bukan aturan klub yang sudah diuji |

### 13.2. Format event dan input sederhana

Gunakan satu putaran dengan pasangan **A–B, A–C, A–D, B–C, B–D, C–D**. Pemeriksaan: 4 × 3 ÷ 2 = 6 pertandingan; setiap pasangan bertemu tepat sekali, tanpa tim melawan diri sendiri. Urutan pelaksanaan mengikuti slot, lapangan dan waktu istirahat, sehingga daftar pasangan bukan janji jadwal serentak. Format ini menjadi preset event yang diminta, bukan penghapusan konfigurasi kegiatan lain.

| Kelompok data | Input atau sumber | Batas kesederhanaan |
|---|---|---|
| Konfigurasi event | Empat tim, jadwal enam laga, roster dan posisi event | Posisi disalin dari profil untuk dikonfirmasi sebelum main; tidak otomatis mengikuti perubahan profil berikutnya |
| Hasil tiap laga | Skor dua tim | Kebobolan tim otomatis = skor lawan; tidak ada kolom input kebobolan pemain |
| Statistik individu | Gol dan Assist tiap pemain yang tampil | Dua kolom bilangan bulat nonnegatif; tidak meminta tackle, save, shot, passing, menit bermain atau rating manual |
| Metadata partisipasi | Konfirmasi pemain tampil/tidak tampil per laga | Daftar tampil dapat diprapenuhi dari roster hadir tetapi harus dikonfirmasi petugas; hadir event tidak otomatis berarti tampil tiga laga |
| Insiden pengecualian | Kartu, outlist, terlambat, absen dan keputusan alasan mendesak | Dicatat hanya bila ada kejadian; ini catatan disiplin/keikutsertaan, terpisah dari dua statistik performa |
| Achievement | Dihitung dari Gol/Assist, skor, partisipasi, posisi event, presensi dan klasemen sah | Bonus hanya pada kategori tujuan; tidak ada pemberian poin bebas |
| Metadata hasil khusus | Pencetak gol pertama, pencetak gol penentu dan konfirmasi comeback bila diketahui | Pilihan pemain/konfirmasi kejadian, bukan statistik individu baru; rincian dan status belum diketahui pada 13.5.4 |

Petugas mengonfirmasi kelengkapan Gol dan Assist per tim/laga. Kolom kosong berarti belum dicatat, sedangkan angka 0 berarti sudah dikonfirmasi nol. Gol bunuh diri dicatat sebagai penyesuaian skor tim beserta alasan, bukan statistik individu ketiga dan bukan Gol/Assist pemain lawan. Usulan satu gol hanya dapat memiliki maksimal satu assist; pencetak gol dan pemberi assist harus berbeda, satu tim dan tercatat tampil. Karena input berupa total, validasi minimal adalah jumlah Gol + gol bunuh diri lawan = skor tim, jumlah Assist ≤ Gol pemain tim, serta Assist seorang pemain ≤ Gol pemain tim selain dirinya. Hubungan assist dengan gol tertentu tidak diklaim tersedia dari input agregat.

Skor tetap dapat final saat rincian belum lengkap sebagaimana baseline. Namun penilaian komparatif baru menunggu kelengkapan seluruh enam laga; data parsial ditampilkan sebagai sementara, tidak dianggap nol. Walkover, laga batal dan skor administratif tidak menghasilkan gol, clean sheet atau bonus performa bermain. Event yang tidak menyelesaikan enam laga tidak diberi label penilaian event lengkap.

### 13.3. Model saldo statistik: mulai 50 setiap event

**Keputusan pengguna terbaru:** setelah pembentukan/pengacakan tim disimpan, setiap pemain dalam roster event memperoleh **50 pada kesembilan kategori**. Nilai 50 adalah titik awal netral event, bukan hasil Gol, Assist atau penilaian kemampuan. Model ini menggantikan seluruh rumus nilai dasar lama: 1 Gol = 50, 1 Assist = 50, Defending/Discipline/Commitment mulai 100, Achievement mulai 0, serta Consistency/Impact yang sebelumnya dihitung sebagai skor turunan.

| Kategori | Nilai awal setiap event |
|---|---|
| Finishing | 50 |
| Playmaking | 50 |
| Defending | 50 |
| Teamwork | 50 |
| Discipline | 50 |
| Commitment | 50 |
| Consistency | 50 |
| Impact | 50 |
| Achievement | 50 |

Untuk pemain p dan kategori C pada event e, simpan **`delta_p,e,C = total_tambahan_p,e,C − total_pengurangan_p,e,C`**, kemudian **`poin_mentah_p,e,C = max(0, 50 + delta_p,e,C)`**. Delta bertanda tetap disimpan utuh walaupun poin mentah mencapai batas 0. Poin mentah bukan nilai tampilan event; tidak dibatasi 100 sebelum normalisasi agar nilai 110 dan 120 tetap dapat dibedakan. Semua sumber dihitung ulang dari data sah, bukan ditambah lagi saat halaman dibuka atau disimpan ulang. Metode normalisasi pada 13.5.1 mengubah poin mentah menjadi skala 0–100 event.

Tiga besaran harus diberi nama/label berbeda: **Poin mentah event**, **Nilai event (relatif)** dan **Statistik profil (akumulasi)**. Contoh 1 Gol memberi Goal Scorer +3: delta Finishing +3 dan poin mentah 53; nilai event bergantung poin tertinggi peserta, sedangkan profil hanya menerima perubahan +3 sekali.

Siklus yang direncanakan:

1. Sebelum tim dibentuk, belum ada saldo penilaian event untuk pemain tersebut. Pratinjau pengacakan belum membuat saldo.
2. Pembagian tim disimpan: buat satu saldo awal 50 per pemain/kategori/event dan snapshot posisi penilaian. Menyimpan ulang atau mengacak ulang tim pada event yang sama tidak memberi 50 lagi dan tidak menghapus perubahan yang sudah sah.
3. Selama event, hasil laga dan kejadian yang disahkan menambah/mengurangi kategori terkait. Semua perubahan menampilkan sumber, angka, waktu dan status; sumber belum diverifikasi ditandai tertunda.
4. Event selesai: hitung bonus/potongan tingkat tim sekali, verifikasi kelengkapan, normalisasikan setiap kategori terhadap maksimum peserta event dan finalkan snapshot nilai event. Posting delta sumber yang sah ke akumulasi profil sekali; jangan mem-posting nilai normalisasi atau baseline event. Kehadiran event tidak otomatis berarti tampil tiga laga.
5. Event berikutnya mempunyai saldo baru 50 untuk semua kategori, bukan membawa saldo akhir atau nilai profil event sebelumnya. Histori event terdahulu tidak direset.

Posisi penilaian ditetapkan sebelum main dan tidak diganti untuk mengejar bonus sesudah hasil diketahui. Statistik individu tetap hanya Gol dan Assist per pertandingan; status tampil, kartu, kehadiran dan metadata hasil khusus merupakan catatan pendukung. Gol/Assist memicu bonus pada katalog 13.5; **tidak ada tambahan 50 per Gol/Assist atau poin dasar lain yang dihitung lagi di luar katalog**. Contoh 1 gol menghasilkan Goal Scorer Finishing +3 sehingga 50 menjadi 53, sebelum bonus atau potongan lain.

**Rancangan pengurangan defensif yang masih usulan agen:** Defender/Goalkeeper yang terkonfirmasi tampil mendapat Defending −2 per gol kebobolan tim pada laga tersebut. Tidak perlu input kebobolan individu; sumbernya skor lawan. Clean Sheet +5 atau Defensive Resilience +2 tetap mengikuti katalog, sehingga laga kebobolan 1 memberi bersih +2 −2 = 0, laga clean sheet +5, dan laga kebobolan 2 memberi −4 sebelum bonus event. Angka −2 belum ditetapkan pengguna; perlu kalibrasi agar kebobolan menjadi sumber penurunan yang jelas dalam model saldo. Striker/Midfielder tidak otomatis dipotong Defending dari aturan khusus ini.

Kebobolan tetap proksi pertahanan tim; dua Defender yang tampil pada laga sama menerima perubahan defensif yang sama. Tidak ada klaim mengukur tackle/save atau menit bermain. Pemain tidak tampil tidak mendapat bonus laga atau potongan kebobolan laga itu. Poin mentah dapat tetap 50 tanpa kontribusi tercatat; ini **baseline netral**, bukan bukti kemampuan. Setelah normalisasi, kategori dengan semua peserta berpoin sama akan menampilkan nilai sama 100 bila maksimum lebih dari 0; ini menunjukkan kesetaraan relatif, bukan performa sempurna. Jumlah penampilan wajib ditampilkan untuk konteks.

### 13.4. Ringkasan perubahan sembilan kategori dan overall

| Kategori | Poin mentah awal event | Sumber tambahan | Sumber pengurangan |
|---|---|---|---|
| Finishing | 50 | First Blood +2; Goal Scorer +3; Brace +3 tambahan atau Hat-Trick +5 tambahan | Belum ada potongan langsung Finishing dalam katalog pengguna; tidak mencetak Gol bukan penalti otomatis |
| Playmaking | 50 | Assist Maker +3; Playmaker +4 tambahan | Belum ada potongan langsung Playmaking; Assist nol bukan penalti otomatis |
| Defending | 50 | Clean Sheet +5 atau Defensive Resilience +2; Best Defense +3, khusus Defender/Goalkeeper | Usulan −2 per kebobolan tim pada laga tampil untuk Defender/Goalkeeper |
| Teamwork | 50 | Unbeaten +4 atau Perfect Victory +6; Comeback +3 per laga sah | Merah −3; Last Place −6; Winless −3; tidak hadir tanpa alasan mendesak −5 |
| Discipline | 50 | Belum ada bonus Discipline pada katalog pengguna | Kuning −3; merah −10; outlist −12; terlambat −2/−4/−6; absen −15 sesuai 13.6 |
| Commitment | 50 | Perfect Attendance +4 | Outlist −8; terlambat 11–20 menit −2, >20 menit −3; absen tanpa alasan mendesak −12; tidak mengikuti laga −4 |
| Consistency | 50 | Consistent Performer +4 pada event penutup streak tiga event | Belum ada potongan langsung; tidak lagi memakai simpangan performa otomatis |
| Impact | 50 | First Blood +1; Goal Contribution +3; Match Winner +4; Best Attack +2 | Belum ada potongan langsung; tidak lagi dihitung ulang dari F/P/D |
| Achievement | 50 | Champion +6; Runner-Up +3 | Belum ada potongan langsung; tidak lagi mulai 0 atau mengonversi poin ×10 |

Contoh poin mentah tanpa perubahan lain: Perfect Attendance menghasilkan Commitment **54**, Champion menghasilkan Achievement **56**, Runner-Up **53**. Nilai tampilan event masih dinormalisasi terhadap maksimum kategori; angka tersebut bukan nilai profil. Tidak memperoleh achievement berarti kategori terkait tetap 50, bukan 0. Pada katalog sekarang Discipline hanya dapat tetap/menurun, sementara beberapa kategori hanya tetap/naik; kebutuhan penambahan sumber poin lain merupakan review lanjutan, bukan aturan yang diasumsikan ada.

**Usulan bobot overall baru per posisi:** karena Impact sekarang saldo mandiri, bobot lama 70% Impact tidak lagi menjadikan Gol/Assist atau kebobolan dasar utama. Ganti dengan rerata tertimbang langsung atas sembilan saldo akhir berikut. Angka bobot adalah usulan agen untuk diuji, bukan keputusan pengguna.

| Posisi event | Finishing | Playmaking | Defending | Teamwork | Discipline | Commitment | Consistency | Impact | Achievement |
|---|---|---|---|---|---|---|---|---|---|
| Striker | 45% | 20% | 5% | 5% | 5% | 5% | 5% | 5% | 5% |
| Midfielder | 20% | 45% | 5% | 5% | 5% | 5% | 5% | 5% | 5% |
| Defender | 5% | 5% | 60% | 5% | 5% | 5% | 5% | 5% | 5% |
| Goalkeeper | 0% | 0% | 70% | 5% | 5% | 5% | 5% | 5% | 5% |

Setiap baris berjumlah 100%. Tampilkan overall dengan konteks yang jelas: `overall_event_relatif = sum(bobot_posisi_event,C × nilai_normalisasi_event_C)`; nilai ini bergantung peserta event dan tidak diposting ke profil. Seluruh poin mentah 50 memberikan overall mentah 50; setelah semua kategori dinormalisasi dengan maksimum 50, overall relatif seluruh pemain menjadi 100. Jangan menjanjikan bahwa baseline tampilan event selalu 50.

Gol/Assist menjadi komponen utama Striker/Midfielder melalui Finishing/Playmaking; hasil pertahanan menjadi utama Defender/Goalkeeper. Tidak ada potongan overall tambahan di luar kategori. Peringkat seri tetap bersama, bukan dipecahkan memakai Gol sebagai aturan universal. Top skor dan voting MVP tetap mekanisme terpisah. Bobot dan skala perubahan per posisi perlu diuji; baseline setara dan normalisasi maksimum belum membuktikan keadilan distribusi peran. Formula overall profil terpisah pada 13.5.1.

### 13.5. Achievement dan pembatasan pertumbuhan

Rancangan terbaru menggunakan **bonus langsung pada kategori yang dituju**. Katalog ini menggantikan enam achievement usulan sebelumnya, bonus universal maksimal +5 dan konversi semua poin ×10. MVP laga dan Pemain Terbaik Bulan Ini tetap tersedia sebagai hasil voting/badge, tetapi tidak menambah nilai dalam katalog ini agar tidak ada bonus lama yang terselip. Tidak ada bonus achievement ke Discipline pada daftar terbaru; kategori tersebut tetap dipengaruhi penalti Discipline.

Nominal pada tabel mengikuti masukan pengguna. Detail operasional di bawah merupakan usulan untuk review. Achievement laga dievaluasi pada hasil final dengan data lengkap; achievement event dievaluasi setelah seluruh enam laga bermain selesai dan sumbernya disahkan. Laga administratif tidak menghasilkan bonus bermain.

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

Last Place dan Winless Team adalah penyesuaian negatif hasil tim, ditampilkan terpisah dari badge prestasi positif dan dari pelanggaran individu. Potongan keduanya **hanya Teamwork**, bukan seluruh kategori. Anggota yang resmi dibatalkan sebelum pembagian tim tidak menerima bonus/potongan tim. Aturan penerima selengkapnya ada pada 13.5.2.

**Keputusan terbaru pengguna membedakan event dan profil:** pemilik poin tertinggi setiap kategori pada event ditampilkan sebagai 100; profil mengakumulasi seluruh permainan. Nilai event bukan angka yang dirata-ratakan atau dijumlahkan ke profil. Rekap Gol/Assist tetap fakta pertandingan tersendiri.

#### 13.5.1. Poin mentah, normalisasi event dan akumulasi profil

Untuk pemain p, event e dan kategori C, gunakan sumber unik:

- `B_p,e,C`: jumlah bonus kategori dari achievement laga/event/streak yang sah setelah aturan tumpukan.
- `T_p,e,C`: potongan hasil tim, hanya Teamwork, bernilai 0/3/6/9.
- `P_p,e,C`: potongan pelanggaran kategori dari insiden sah pada 13.6.
- `K_p,e,C`: usulan potongan kebobolan, hanya Defending pada Defender/Goalkeeper, 2 × kebobolan pada laga tampil.
- `delta_p,e,C = B_p,e,C − T_p,e,C − P_p,e,C − K_p,e,C`.

**A. Poin mentah event:** `mentah_p,e,C = max(0, 50 + delta_p,e,C)`. Baseline 50 dibuat sekali per event, bukan sekali per laga atau pengacakan. Delta asli tetap tersimpan; nilai mentah tidak dicap 100 karena masih menjadi bahan perbandingan event. Bonus pertandingan tidak dibagi jumlah laga, bonus event tidak dikalikan tiga, dan satu sumber tidak masuk dua komponen potongan.

**B. Nilai event relatif (usulan rumus rasio):** tentukan kelompok pembanding Q_e berupa pemain snapshot event yang terkonfirmasi tampil minimal satu laga sah. Perbandingan mencakup empat tim dan semua posisi, bukan hanya satu tim/posisi. Peserta batal sebelum pembagian tim atau snapshot dibatalkan secara sah dikecualikan. Outlist/absen tanpa penampilan tetap memiliki poin mentah dan penalti profil, tetapi **tidak masuk maksimum, nilai relatif atau peringkat event**, sehingga baseline pemain yang tidak bermain tidak menjadi acuan performa. Pemain tampil sebagian tetap memiliki nilai relatif dengan jumlah laga terlihat; usulan peringkat overall event mensyaratkan tampil ketiga laga.

Hitung `maks_e,C = max(mentah_p,e,C untuk p dalam Q_e)`. Untuk maksimum >0, `nilai_event_p,e,C = 100 × mentah_p,e,C / maks_e,C` hanya bagi anggota Q_e. Tampilkan dua desimal setelah perhitungan penuh; penyimpanan, bobot overall dan penentuan seri memakai nilai penuh, sehingga peringkat tidak bergantung pembulatan UI. Tertinggi tiap kategori mendapat 100 dan seri tertinggi mendapat 100 bersama. Satu pemain tidak harus menjadi pemilik 100 pada semua kategori. Kelayakan Q_e adalah usulan optimasi, bukan penghapusan histori pelanggaran pemain yang tidak tampil.

Contoh Finishing A/B/C memiliki poin mentah 60/55/50: nilai event masing-masing **100/91,67/83,33**. Achievement mentah Champion 56, Runner-Up 53 dan peserta lain 50 menjadi **100/94,64/89,29**. Nilai 100 menandakan posisi tertinggi pada kategori/event itu, bukan 100 poin tambahan atau performa absolut sempurna. Bila semua peserta masih 50 dan maksimum 50, semuanya 100 secara relatif; sebelum pertandingan tampilkan poin awal 50 dengan label “Nilai relatif belum final”, tanpa menerbitkan peringkat prestasi dari event yang belum dimainkan.

Jika Q_e kosong, tampilkan “Belum ada pemain tampil terkonfirmasi” dan jangan menerbitkan normalisasi/peringkat. Jika maksimum seluruh anggota Q_e pada suatu kategori 0, pembagian tidak dapat dilakukan. Usulan state khusus: tampilkan poin mentah 0 dan nilai relatif “Tidak dapat dinormalisasi — seluruh poin 0”, bukan mengarang nilai sempurna atau membagi nol. Ini pengecualian degenerat yang perlu dicatat pada AC. Perbandingan yang belum lengkap diberi label sementara; bonus/koreksi satu pemain dapat mengubah maksimum sehingga nilai relatif semua peserta pada kategori terkait harus dihitung ulang. Kejadian ini tidak mengubah delta atau profil peserta lain.

**Batas interpretasi penalti relatif:** bila semua anggota Q_e mendapat potongan Discipline yang sama sehingga seluruh poin menjadi 47, normalisasi tetap menampilkan semuanya 100. Potongan tetap tercatat pada delta dan profil; jangan menyimpulkan bahwa nilai relatif 100 berarti bebas kartu. Jika pemimpin kategori terkena potongan tetapi masih tertinggi, ia juga tetap 100. Selalu sediakan poin mentah dan riwayat perubahan di samping angka relatif. Tidak ada normalisasi tambahan overall yang menjanjikan pemenang overall pasti 100; aturan maksimum 100 berlaku per kategori.

**C. Akumulasi profil (usulan optimasi batas 0–100):** baseline profil `profil_0,C = 50` hanya sekali. Urutkan event yang sumbernya final menurut waktu mulai event lalu ID event yang stabil; untuk setiap event e, hitung **`profil_e,C = clamp(profil_sebelum_e,C + delta_p,e,C, 0, 100)`**. Akumulasi mengikuti urutan event permainan, bukan urutan admin menginput atau memperbaiki data. Semua perubahan dalam event digabung menjadi delta bersih sebelum cap, sehingga urutan pencatatan kejadian dalam event yang sama tidak memberi hasil berbeda.

Metode cap per event ini menggantikan usulan cap tampilan setelah menjumlah delta sepanjang karier: bonus melampaui 100 atau potongan melampaui 0 tidak menjadi cadangan tersembunyi yang melindungi saldo pada event berikutnya. Ledger tetap menyimpan delta nominal dan perubahan efektif yang benar-benar diterapkan. Baseline profil 50 dan pilihan cap per event masih **usulan untuk review**, bukan keputusan pengguna yang sudah disahkan. Keputusan pengguna yang tetap adalah akumulasi seluruh permainan; profil tidak dirata-ratakan, tidak direset, tidak ditambah baseline 50 per event dan tidak memakai nilai relatif event.

Contoh perubahan Finishing tiga event +10, +3 dan 0 memberikan profil **63**, bukan rerata 54,33 atau jumlah poin mentah 163. Contoh Teamwork +6, −9 dan −3 memberi profil **44**. Baseline event berikutnya tetap 50 meskipun profil Finishing sudah 63. Karena katalog Finishing belum mempunyai potongan langsung, nilai itu tidak turun hanya karena event baru tanpa Gol; ini berbeda dari model rerata yang sudah dibatalkan.

Akumulasi mencakup sumber seluruh pertandingan/event yang sah, termasuk penalti outlist/absen terverifikasi walaupun pemain tidak tampil. Gol/Assist dan bonus laga memerlukan partisipasi sebenarnya; baseline netral tidak menghasilkan penampilan atau badge palsu. Pembatalan sebelum pembagian tim tidak menghasilkan snapshot atau delta. Event dibatalkan tidak mem-posting perubahan; hasil sah pada event yang belum selesai ditampilkan sebagai delta sementara hingga finalisasi/salvage sumber disahkan pengelola. Achievement tim tetap menunggu klasemen final, streak hanya diposting pada event penutupnya sekali.

Contoh batas: profil awal 50, event pertama delta +70 menghasilkan 100 dengan kenaikan efektif +50; event kedua delta −3 menghasilkan **97**, bukan tetap 100. Sebaliknya delta −70 menghasilkan 0, lalu event berikutnya +3 menghasilkan 3. Selisih nominal dan efektif tetap bisa diaudit, tetapi tidak disimpan sebagai saldo cadangan. Akumulasi jumlah delta mentah boleh menjadi data audit terpisah, bukan nilai kategori profil yang ditampilkan.

**Usulan overall profil:** hitung `delta_overall_e = sum_C(bobot_posisi_p,e,C × perubahan_efektif_profil_e,C)` menggunakan posisi/versi aturan event sumber. Akumulasikan `overall_e = clamp(overall_sebelum_e + delta_overall_e, 0, 100)` dengan baseline overall 50 satu kali. Menggunakan perubahan efektif kategori memastikan bonus yang seluruhnya tertahan batas kategori tidak menaikkan overall. Overall profil bukan normalisasi dan bukan rerata overall event. Untuk posisi/bobot tetap dan tanpa cap overall, hasilnya setara bobot atas kategori profil; pergantian posisi dapat berbeda sehingga rincian pengaruh per event perlu ditampilkan. Bobot posisi tetap usulan untuk kalibrasi.

Koreksi sumber mengganti delta aktif lama dengan versi baru, kemudian menghitung ulang poin mentah event dan maksimum/normalisasi peserta terkait. Untuk profil, replay dari event paling awal yang berubah sampai event terbaru dengan urutan permainan tetap, termasuk streak dan overall; tidak cukup menambah selisih koreksi pada saldo terkini karena cap historis dapat mengubah perubahan efektif sesudahnya. Mengoreksi poin pemain A yang mengubah maksimum event dapat mengubah nilai relatif pemain B, tetapi **tidak** mengubah profil B karena delta B tetap. Sumber unik, versi aturan dan finalisasi idempoten mencegah poin ganda. Filter event menampilkan nilai relatif dan poin mentah; profil menampilkan akumulasi berikut jumlah event/penampilan, riwayat perubahan dan sumbernya. Tidak ada data berarti belum ada riwayat; baseline profil usulan 50 diberi label awal, bukan bukti penampilan.

**Versi aturan dan finalisasi yang perlu dirinci saat implementasi:** snapshot baseline, posisi, bobot, katalog, aturan tumpukan dan himpunan sumber dikaitkan dengan versi aturan event saat pembagian tim disimpan. Perubahan usulan setelahnya berlaku untuk event baru; perhitungan ulang histori memakai versi event sumber. Migrasi histori harus eksplisit, beralasan, berjejak dan mereplay profil, bukan otomatis menerapkan konfigurasi terbaru.

Pisahkan status skor laga, kelengkapan statistik dan pengesahan poin. Untuk kejadian khusus, gunakan Terverifikasi ada / Terverifikasi tidak ada / Tidak berlaku / Belum diketahui. “Belum diketahui” menahan badge terkait, bukan skor pertandingan. Jika kejadian tidak mungkin diverifikasi, admin dapat menutupnya dengan status “Tidak dapat diverifikasi” dan alasan: tidak memberi bonus, tidak mengklaim kejadian tidak terjadi, dan tetap memberi label kekurangan bukti. Koreksi kemudian dapat membuka revisi poin. Data wajib Gol/Assist, partisipasi, kartu atau alasan kehadiran yang belum diperiksa tidak boleh ditutup dengan mekanisme metadata opsional ini.

Finalisasi poin adalah satu versi snapshot konsisten untuk poin mentah, Q_e/maksimum, nilai relatif dan delta posting profil. Retry terhadap versi yang sama tidak mem-posting ulang. Dua admin yang menyimpan koreksi bersamaan harus membandingkan versi dan meninjau perubahan terbaru; jangan mencampur skor versi lama dengan badge/penalti versi baru. Rincian teknis transaksi diuji pada implementasi; rencana ini tidak mengklaim aplikasi sudah dibangun.

#### 13.5.2. Partisipasi, tumpukan dan klasemen

- Achievement individu laga mensyaratkan pemain terkonfirmasi tampil; Goal Scorer/Assist Maker berlaku bagi semua posisi. Defender/Goalkeeper mengikuti posisi penilaian event yang ditetapkan sebelum bermain; perubahan posisi sesudah hasil diketahui tidak digunakan untuk berburu badge. Rotasi posisi dalam laga tetap keterbatasan model sederhana.
- Penerima bonus/potongan tim tingkat event adalah anggota roster tim tersebut yang tercatat tampil minimal satu laga sah dalam event. Satu penampilan cukup untuk Champion/Last Place, tetapi tidak cukup untuk Perfect Attendance. Pemain tidak tampil, peserta batal resmi sebelum pembagian tim, dan pengganti yang belum bermain tidak menerima penyesuaian tim. Pemain yang sudah tampil lalu mengundurkan diri tetap menerima hasil tim terkait penampilannya; penalti outlist jika berlaku dihitung terpisah.
- **Aturan pengguna:** Goal Scorer +3 tetap diberikan pada ≥1 gol; pilih tepat satu bonus tambahan Brace +3 untuk 2 gol atau Hat-Trick +5 untuk ≥3 gol. Total Finishing: 1 gol +3, 2 gol +6, ≥3 gol +8, sebelum First Blood +2 jika sah. Tidak ada perkalian badge dasar dengan jumlah gol.
- Assist Maker +3 dan Playmaker +4 dapat ditumpuk: ≥2 assist menghasilkan Playmaking +7, bukan +3 untuk setiap assist. Goal Contribution, First Blood dan Match Winner dapat ditumpuk bila seluruh syarat masing-masing terbukti, tanpa cap 100 pada poin mentah; skala 0–100 diterapkan pada normalisasi event dan saldo profil.
- **Usulan pengecualian tumpukan tambahan:** Clean Sheet +5 menggantikan Defensive Resilience +2 untuk laga tanpa kebobolan; kebobolan tepat 1 mendapat +2. Perfect Victory +6 menggantikan Unbeaten Team +4. Badge tingkat lebih rendah boleh ditunjukkan sebagai tercakup, tetapi tidak mendapat poin kedua.
- **Usulan penyesuaian independen:** Last Place −6 dan Winless Team −3 dapat bersamaan, menghasilkan Teamwork −9. Tim dengan tiga seri memenuhi Unbeaten Team +4 dan Winless Team −3; keduanya tetap tercatat, bersih +1 sebelum cap dan penyesuaian lainnya. Best Defense/Best Attack dapat berdampingan dengan achievement lain. Kombinasi ini perlu masuk simulasi agar tidak dianggap kesalahan penghitungan.
- Champion/Runner-Up/Last Place mengikuti peringkat resmi event, bukan peringkat performa individu. Gunakan aturan klasemen yang diumumkan sebelum main (baseline usulan poin, selisih gol, gol memasukkan). Jangan menciptakan tiebreak setelah hasil diketahui. Jika seri masih menyisakan peringkat bersama dan penerima posisi 1/2/4 ambigu, tahan tiga penyesuaian peringkat tersebut sebagai “Menunggu penetapan peringkat”; Best Defense/Best Attack yang memiliki sumber lengkap boleh final bersama. Jangan memberikan Champion sekaligus Runner-Up pada tim yang sama.
- Unbeaten/Perfect Victory/Winless mensyaratkan tepat tiga laga bermain sah per tim, Best Defense/Best Attack mensyaratkan keenam laga lengkap, dan semua penyesuaian event menunggu event selesai. Event terpotong, hasil administratif atau walkover menahan achievement tim event; tidak diperlakukan sebagai clean sheet atau kemenangan bermain. Comeback Victory tetap per laga, bukan peringkat, sehingga dapat terjadi pada lebih dari satu laga event.

#### 13.5.3. Consistent Performer dan koreksi sumber

Usulan definisi “berkontribusi”: pada event lengkap pemain tampil minimal satu laga dan mencatat ≥1 Gol atau ≥1 Assist; alternatif setara kesempatan untuk Defender/Goalkeeper adalah tampil tiga laga dengan rerata kebobolan tim maksimal 1 per laga. Semua posisi dapat memenuhi jalur Gol/Assist. Ini proksi terukur, bukan klaim tingkat kesulitannya sudah setara.

Urutan streak memakai waktu mulai event lalu ID event sebagai pemecah waktu sama, bukan waktu data dimasukkan. Tiga event komitmen pemain yang memenuhi kontribusi secara berurutan menghasilkan Consistency +4 pada event ketiga. Usulan blok tidak tumpang tindih: streak enam event menghasilkan dua penghargaan pada event ketiga dan keenam; event keempat tidak otomatis memberi +4 lagi. Event tidak berkontribusi atau absen/outlist tanpa alasan mendesak memutus streak. Event yang tidak diikuti tanpa kewajiban roster, batal atau ketidakhadiran mendesak yang disetujui dilewati; event dengan sumber belum lengkap menahan evaluasi lanjutan, tidak dilewati untuk menciptakan streak palsu.

Simpan identitas pemain, kode achievement, kategori, sumber laga/event, nilai perubahan, versi aturan, status dan waktu evaluasi. Kunci sumber unik mencegah pemberian ulang saat retry: achievement laga per pemain/kode/laga; achievement event per pemain/kode/event; streak per pemain/kode/event penutup beserta tiga event sumber. Finalisasi event menghitung sumber yang belum diproses, bukan mengulang bonus semua laga. Koreksi Gol/Assist, skor, metadata hasil, partisipasi, posisi atau klasemen menarik/mengganti award terkait dan menghitung ulang kategori. Koreksi event lama menghitung ulang streak sesudahnya, termasuk pemindahan event penerima bonus; jejak versi lama disimpan tetapi tidak masuk agregat aktif dua kali.

#### 13.5.4. Data minimum untuk kejadian berurutan

Total Gol/Assist dan skor akhir tidak membuktikan gol pertama, gol penentu atau pernah tertinggal. **Usulan input tambahan minimal:** tiga metadata terstruktur dalam panel “Kejadian khusus” pada form hasil; statistik individu tetap dua kolom Gol dan Assist. Tidak meminta seluruh menit atau urutan gol. Petugas mencatat fakta yang disaksikan dan pengelola mengesahkan; validasi sistem hanya dapat memeriksa kecocokan dengan total dan hasil, bukan membuktikan urutan tanpa log kronologis.

| Metadata | Definisi dan validasi | Jika tidak diketahui |
|---|---|---|
| Gol pertama | Pilih pemain pencetak gol pertama pertandingan; harus tampil dan memiliki ≥1 Gol. Opsi khusus gol bunuh diri tidak memberikan First Blood kepada pemain lawan. Hasil 0–0 otomatis tidak memiliki gol pertama | Status belum terverifikasi; jangan memilih pencetak gol dari urutan roster atau total tertinggi |
| Gol penentu kemenangan | Usulan definisi: gol tim pemenang ke-(jumlah gol akhir lawan + 1). Contoh hasil 3–1: gol kedua pemenang menjadi penentu, tidak selalu gol terakhir. Pilih pencetaknya yang tampil dan memiliki ≥1 Gol; petugas memastikan urutannya. Jika gol tersebut bunuh diri, tidak ada penerima Match Winner | Status belum terverifikasi; skor akhir tetap dapat final, bonus ditahan. Hasil seri otomatis tidak memiliki penerima |
| Comeback | Konfirmasi apakah tim pemenang pernah tertinggal pada laga: Ya/Tidak/Belum diketahui. Ya hanya sah bila tim menang dan lawan mencetak ≥1 gol; semua pemain tim pemenang yang tampil mendapat bonus | “Belum diketahui” tidak disamakan dengan Tidak; tidak menghasilkan bonus sampai dikonfirmasi |

Tidak ada kewajiban mengarang metadata agar skor dapat difinalkan. Profil menandai kelengkapan bonus tertunda dan diperbarui sesudah konfirmasi. Catatan koreksi alasan diwajibkan saat mengubah metadata yang sudah disahkan. Kombinasi yang pasti bertentangan ditolak, misalnya comeback pada kemenangan 1–0, penerima Match Winner dari tim kalah, atau First Blood pada 0–0. Perluasan form kecil ini merupakan usulan untuk mendukung achievement yang diminta; bila input benar-benar dibatasi tanpa metadata tambahan, tiga achievement tersebut harus berstatus tidak dapat diverifikasi dan tidak diberikan otomatis.

### 13.6. Penalti per kategori, verifikasi dan hak koreksi

Masukan terbaru pengguna menggantikan potongan universal: setiap pelanggaran hanya memotong kategori yang disebutkan, lalu dapat menurunkan overall melalui bobot kategori pada 13.4. Finishing, Playmaking, Defending, Consistency, Impact dan Achievement tidak dipotong langsung oleh katalog pelanggaran ini; usulan potongan kebobolan Defending pada 13.3 terpisah dari pelanggaran. Potongan Last Place/Winless Team tetap khusus Teamwork dan dihitung terpisah. Nominal berikut mengikuti masukan pengguna dan klarifikasi 12 November 2026; dua ambiguitas sebelumnya telah ditutup.

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

Ketiga baris terlambat/absen yang sebelumnya ditahan kini mempunyai nominal final dari pengguna. Insiden tetap memerlukan verifikasi admin dan aturan satu kejadian tidak boleh dihitung dua kali. Sumber terlambat adalah batas hadir yang diumumkan sebelum event dan waktu hadir aktual. Usulan perincian durasi: 0 atau lebih awal = tepat waktu; lebih dari 0 sampai 10 menit = tingkat pertama, lebih dari 10 sampai 20 = kedua, lebih dari 20 = ketiga. Dengan demikian 10 menit 1 detik masuk tingkat kedua, tanpa celah pembulatan. Usulan toleransi bebas penalti 10 menit sebelumnya dihapus. Satu kejadian hanya mendapat tingkat tertinggi yang sesuai, bukan jumlah semua tingkat. Alasan mendesak yang disetujui tetap menjadi pengecualian kehadiran sesuai prinsip pengguna sebelumnya.

#### Perhitungan kategori dan overall

`P_p,e,C` adalah jumlah potongan kategori dari insiden sah unik. Masukkan potongan itu ke delta pada 13.5.1, lalu turunkan poin mentah event dari 50. Normalisasi menggunakan maksimum event setelah potongan. Pada profil, potongan yang sama masuk akumulasi delta sekali; **tidak** dirata-ratakan per event dan **tidak** dipotong lagi setelah nilai normalisasi. Last Place/Winless hanya T_p,e,Teamwork; usulan kebobolan hanya K_p,e,Defending. Nominal Commitment/Teamwork pada tabel telah ditetapkan pengguna 12 November 2026; perubahan berlaku melalui versi aturan yang tercatat, bukan mengubah histori final diam-diam.

Contoh baseline 50 tanpa perubahan lain: satu kuning menghasilkan Discipline mentah 47; jika maksimum Discipline event 50, nilai relatifnya **94**, sedangkan profil awal 50 menjadi **47**. Satu merah menghasilkan Discipline mentah 40 dan Teamwork mentah 47; jika maksimum kedua kategori 50, nilai event **80 dan 94**. Profil pertama dengan sumber yang sama menjadi Discipline 40 dan Teamwork 47. Nilai event akan berbeda bila maksimum peserta berbeda; potongan profil tetap −10/−3 sesuai sumber.

Overall profil dengan bobot usulan 5% untuk Discipline/Teamwork/Commitment: satu kuning memberi delta −0,15; satu merah −0,65; outlist −1,00, sebelum cap dan perubahan lain. Satu merah sepanjang lima event netral tetap mengurangi akumulasi profil Discipline 10 dan Teamwork 3, dengan overall awal 50 menjadi **49,35**, bukan 49,87. Overall event relatif dihitung terpisah dari kategori yang telah dinormalisasi; jangan memakai contoh angka overall profil sebagai nilai event.

Menambah insiden pada kumpulan sumber yang sama tidak menaikkan poin mentah atau profil kategori terkait. Namun nilai relatif pemain tetap dapat naik/turun ketika maksimum peserta lain berubah, sehingga UI harus selalu membedakan peringkat relatif dan akumulasi personal. Event berikutnya tanpa bonus/potongan **tidak memulihkan** penalti profil; delta 0 mempertahankan nilai karier. Pemulihan memerlukan tambahan poin kategori terkait atau koreksi insiden. Katalog saat ini belum menyediakan bonus Discipline, sehingga kategori itu tidak pulih hanya dengan hadir bersih; catat kebutuhan aturan pemulihan untuk review tanpa mengarang nominal.

#### Aturan kartu dan pencegahan hitung ganda

1. Kartu dicatat dengan pemain, pertandingan, jenis, kejadian dan pengesahan. Kartu merah **tidak otomatis menghapus kartu kuning sebelumnya**. Contoh satu kuning lalu merah langsung: Discipline −13, Teamwork −3. Dua kuning lalu merah menghasilkan Discipline −16, Teamwork −3 bila ketiga kartu tercatat sah. Ini beberapa kartu dalam satu rangkaian, bukan duplikasi entri kartu yang sama. Pengecualian hanya jika peraturan komunitas secara eksplisit menetapkan penggantian, disimpan sebagai versi sebelum berlaku; tidak diterapkan surut diam-diam.
2. Outlist sebelum pembagian tim bebas penalti. Pengacakan ulang/admin menghapus roster untuk koreksi, atau event batal, tidak otomatis menjadi pelanggaran. Outlist setelah pengacakan tetap memerlukan pemeriksaan alasan; persetujuan penghapusan dari roster bukan otomatis persetujuan alasan mendesak.
3. Outlist yang sudah disetujui keluar dari roster tidak ditambah penalti tidak hadir atau tidak mengikuti laga untuk kejadian yang sama. Tidak hadir seluruh event tidak ditambah tiga penalti tidak mengikuti pertandingan. Pemain yang tidak pernah tiba berstatus tidak hadir, bukan sekaligus terlambat.
4. Tidak mengikuti laga karena keterlambatan yang sama dicatat sebagai dampak insiden terlambat, tanpa potongan Commitment −4 tambahan untuk kejadian tersebut. Jika pemain kemudian menolak laga lain tanpa alasan yang disetujui, kejadian berbeda dapat dikenai −4 per laga yang terverifikasi. Ketidakhadiran tidak boleh disimpulkan hanya dari Gol/Assist nol atau kolom partisipasi kosong.
5. Usulan batas tanggung jawab: pemain yang dicadangkan oleh pengelola, cedera dengan alasan disetujui, tidak boleh bermain karena kartu merah, atau laganya batal tidak otomatis terkena penalti tidak mengikuti laga. Admin harus memastikan pemain memang dijadwalkan/diwajibkan tampil, tidak tampil atas keputusannya dan tanpa alasan yang disetujui. Tanpa bukti tanggung jawab, status menunggu verifikasi.
6. Satu identitas insiden beserta pemain dan sumber event/laga hanya memiliki satu keputusan aktif. Retry atau penyimpanan ulang tidak menambah potongan. Perubahan status terlambat/absen/outlist mengganti sumber terkait, bukan menambahkan hukuman untuk fakta yang sama.

#### Verifikasi admin dan pengajuan koreksi

- Insiden kehadiran memuat status presensi/partisipasi, batas hadir dan waktu aktual bila relevan, snapshot pembagian tim, laga terdampak, alasan pemain, catatan/bukti yang relevan, pencatat dan admin pemeriksa. Status: Dicatat → Menunggu verifikasi → Disahkan atau Ditolak. Hanya insiden disahkan masuk nilai final; alasan yang belum diputuskan memberi label sementara.
- Alasan mendesak/disetujui menghasilkan pembebasan sesuai jenis pelanggaran. Penolakan alasan memuat penjelasan admin. Data pribadi/alasan sensitif hanya dapat dibaca pemain terkait dan petugas berwenang; tidak diumumkan di peringkat.
- Pemain dapat melihat rincian penalti miliknya dan mengajukan koreksi dengan alasan/bukti. Usulan status koreksi: Diajukan → Ditinjau → Diterima atau Ditolak. Pengajuan sendiri tidak otomatis menghapus penalti yang sudah disahkan; tampilkan “Dalam peninjauan”. Admin memutuskan dengan alasan dan jejak audit; bila admin adalah pemain terkait, pemeriksaan dialihkan ke admin lain/superadmin.
- Koreksi diterima memperbarui/membatalkan insiden, lalu menghitung ulang kategori, overall, peringkat dan achievement/streak yang terpengaruh tanpa menghapus histori. Koreksi ditolak mempertahankan nilai dengan alasan keputusan yang dapat dibaca pemohon. Simpan aktor, waktu, versi dan nilai sebelum/sesudah; UI mempertahankan permintaan saat gagal simpan dan retry tidak menggandakan permohonan.
- Koreksi hasil, metadata kejadian, partisipasi, posisi, kartu atau klasemen mengikuti mekanisme perhitungan ulang yang sama. Koreksi voting hanya memperbarui badge voting karena katalog ini tidak memberi poin statistik untuk voting.

### 13.7. Dampak SRS dan urutan pekerjaan setelah review

| Urutan | Deliverable | Perubahan yang direncanakan | Status saat ini |
|---|---|---|---|
| 1 | Rencana ini | Kebutuhan, formula, pembatasan, simulasi dan scope sinkronisasi | v2.7 disetujui, pelaksanaan v2.8 dan klarifikasi v2.9 |
| 2 | [Product Planning](./01-product-planning.md) | Perluas FEAT-07/09/10/16 dan promosikan bagian FEAT-28 yang diminta ke MVP; metrik berbasis data sederhana, manfaat untuk semua posisi dan risiko proksi tim | Diselaraskan v2.5; penilaian pada 13.11, penalti pada 13.12, visualisasi pada 13.13 dan review pada 13.14 |
| 3 | [PRD](./02-prd.md) | Revisi kebutuhan hasil/statistik/skill; tambahkan story dan AC untuk Assist, sembilan kategori, 21 achievement/penyesuaian, partisipasi penerima, tumpukan, metadata hasil, streak dan koreksi. Pertahankan ID lama; AC baru setelah AC-90 | Diselaraskan v2.5; penilaian pada 13.11, penalti pada 13.12, visualisasi pada 13.13 dan review pada 13.14 |
| 4 | [Product Details](./03-product-details.md) | Perluas FLOW-03/05/06/07/08/09/10/14/16; rincikan bonus per kategori, penyesuaian klasemen sekali, kelengkapan metadata, baseline 50 per event, sumber tambahan/potongan, pemisahan tim/disiplin/kebobolan, poin mentah, maksimum/normalisasi event, akumulasi delta profil, bobot posisi, streak dan rekalkulasi | Diselaraskan v2.5; penilaian pada 13.11, penalti pada 13.12, visualisasi pada 13.13 dan review pada 13.14 |
| 5 | [Mockup SRS](./04-mockup-srs.md) | Perluas SCR-10/11/12/14/16/18/19/22/24/32/34; dua kolom Gol/Assist, panel Kejadian khusus, penyesuaian tim, sembilan poin mentah awal 50, nilai event relatif dengan maksimum 100, akumulasi profil, rincian delta, kelengkapan dan peringkat posisi | Diselaraskan v2.5; penilaian pada 13.11, penalti pada 13.12, visualisasi pada 13.13 dan review pada 13.14 |
| 6 | [Traceability](./05-traceability.md) | Pemetaan kebutuhan → story/AC → flow → layar; perbarui fase dan audit konsistensi, rumus, versi, ID dan tautan | Diselaraskan v2.5; penilaian pada 13.11, penalti pada 13.12, visualisasi pada 13.13 dan review pada 13.14 |

FEAT-28 tidak tetap diberi label seluruhnya fase lanjutan apabila penilaian ini masuk MVP; hanya analitik di luar input sederhana yang tetap lanjutan. Nilai performa otomatis dan skill manual 1–5 harus dibedakan. Usulan tahap awal: pertahankan skill manual untuk pembagian tim, tampilkan performa sebagai konteks; jangan otomatis memetakan nilai 0–100 menjadi skill 1–5 atau mengacak ulang roster terkunci. Pemakaian performa otomatis untuk pembagian tim memerlukan aturan terpisah agar achievement/penalti tidak dianggap murni kemampuan teknis.

Visual tetap hitam–gold, mobile-first dan mengikuti cakupan tayang profil/nilai privat baseline. Penambahan kategori tidak otomatis mengumumkan nilai ke publik; rincian alasan sensitif hanya bagi pemilik dan petugas berwenang. Gunakan spesifikasi tertulis untuk state baru. Anggaran tetap 12 gambar referensi; tahap revisi dapat mengganti subset Tim & Pemain dalam slot yang sama, mengarsipkan versi sebelumnya dan menginspeksi penggantinya. Input skor/insiden dan rincian profil mendapat spesifikasi tertulis; tidak menjanjikan gambar tambahan di luar anggaran.

### 13.8. Simulasi dan kriteria penerimaan rancangan

Contoh sintetis berikut menguji tiga besaran berbeda, bukan hasil uji aplikasi atau bukti keadilan posisi. Rumus normalisasi rasio dan baseline profil satu kali 50 masih usulan untuk review.

| Skenario | Poin mentah/perubahan | Nilai event relatif | Profil akumulasi usulan |
|---|---|---|---|
| Pembagian tim baru, belum ada pertandingan | Semua kategori mentah 50 | Belum final; jika rasio dihitung saat semua sama, semuanya 100 | Pembentukan roster tidak memberi delta profil |
| Finishing A/B/C: 60/55/50 | Delta +10/+5/0 | 100 / 91,67 / 83,33 | Profil awal masing-masing menjadi 60/55/50 setelah sumber final diposting |
| Hat-Trick dan First Blood | Delta Finishing +10; mentah 60 | 100 jika maksimum Finishing event 60 | Finishing awal 50 menjadi 60; bukan otomatis 100 |
| Perfect Attendance | Delta Commitment +4; mentah 54 | 100 jika maksimum Commitment 54 | Commitment awal 50 menjadi 54 |
| Champion dibanding Runner-Up | Achievement mentah 56 dan 53 | 100 dan 94,64 jika maksimum 56 | Achievement awal 50 menjadi 56 atau 53 |
| Kartu kuning, maksimum Discipline 50 | Delta −3; mentah 47 | Discipline 94 | Discipline awal 50 menjadi 47 |
| Kartu merah, maksimum Discipline dan Teamwork 50 | Delta Discipline −10, Teamwork −3; mentah 40/47 | Discipline 80 dan Teamwork 94 | Discipline 40, Teamwork 47; overall awal 50 menjadi 49,35 sesuai usulan bobot |
| Finishing tiga event memberi delta +10, +3, 0 | Mentah per event 60, 53, 50 | Masing-masing bergantung maksimum event | Finishing 63, bukan rerata 54,33 |
| Koreksi A mengubah maksimum Finishing 60 menjadi 66, B tetap mentah 55 | Delta B tetap +5 | Nilai B berubah 91,67 menjadi 83,33 | Profil B tidak berubah |
| Semua peserta pada satu kategori mentah 50 | Tidak ada delta | Semua 100 relatif, bukan performa sempurna | Tidak menambah profil |
| Semua pemain pembanding pada kategori mentah 0 | Delta masing-masing ≤−50 | State khusus pembagi nol, tidak menghasilkan angka palsu | Saldo mengikuti cap per event, minimal 0 |

Kriteria penerimaan yang perlu diturunkan ke AC saat pemrosesan SRS:

- Empat tim menghasilkan enam pasangan unik dan tiga laga per tim. Pembagian tim disimpan membentuk satu baseline mentah 50 per kategori/pemain/event; pratinjau, retry dan pengacakan ulang tidak membuat baseline tambahan atau menghapus delta.
- Poin mentah menggunakan baseline 50 dan penjumlahan perubahan sah. Nilai mentah 110 dan 120 tetap berbeda sebelum normalisasi; nilai relatif tertingginya 100. Jangan mencap poin mentah di 100 lebih dulu atau mencampurnya dengan kategori profil.
- Normalisasi independen per kategori atas Q_e (pemain terkonfirmasi tampil); pemain tidak tampil tetap memiliki delta penalti profil tetapi nilai relatif ditampilkan —. Seri maksimum mendapat 100 bersama; Q_e kosong atau maksimum 0 memakai state khusus. Nilai sama positif memberi semua pemain pembanding 100; potongan identik juga bisa tetap 100. Tampilkan poin mentah, sumber dan label relatif agar 100 tidak disalahartikan sebagai bebas penalti.
- Penilaian belum final saat data pertandingan/bonus/koreksi belum lengkap. Finalisasi menghitung bonus klasemen sekali, menormalisasi semua peserta dan mem-posting delta unik ke profil; normalisasi bukan sumber poin.
- Semua 12 achievement individu dan 9 achievement/penyesuaian tim memiliki nominal/kategori benar, dengan aturan tumpukan dan penerima partisipasi pada 13.5.2. Laga administratif, pemain batal sebelum pembagian dan pemain tidak tampil tidak mendapat badge bermain palsu.
- Uji sumber Gol/Assist nol vs kosong, gol bunuh diri, pemain tidak tampil, metadata First Blood/Match Winner/Comeback, seri klasemen dan streak tiga/enam event. Koreksi sumber lama menghitung ulang streak sesudahnya tanpa bonus ganda.
- Penalti sesuai kategori, kartu kuning/merah kumulatif dan tanpa duplikasi insiden; verifikasi admin dan hak koreksi berlaku. Uji nominal final pengguna: terlambat 11–20 Discipline −4/Commitment −2; >20 Discipline −6/Commitment −3; absen Discipline −15/Commitment −12/Teamwork −5.
- Akumulasi profil memakai baseline usulan 50 satu kali dan seluruh delta final, bukan rerata, jumlah poin mentah event atau jumlah nilai normalisasi. Event tanpa perubahan tidak memberi +50 atau memulihkan penalti. Tidak ada pembatasan lima event terakhir.
- Potongan kebobolan usulan hanya Defender/Goalkeeper pada laga tampil; jangan dihitung ulang sebagai pelanggaran. Potongan tim/individu/streak diposting tepat sekali. Kategori profil tetap dibatasi 0–100, akumulasi mentah bertanda tetap tersedia untuk audit.
- Uji cap profil per event: baseline 50, delta +70 menghasilkan 100, lalu event berikutnya −3 menghasilkan 97; baseline 50, delta −70 menghasilkan 0, lalu +3 menjadi 3. Ledger menyimpan nominal dan efektif tanpa cadangan. Mengubah urutan input tidak mengubah hasil bila urutan permainan sama; koreksi event lama wajib replay seluruh event sesudahnya.
- Koreksi pemain A dapat mengubah nilai event pemain B lewat maksimum, tetapi tidak mengubah delta/profil B. Perhitungan ulang profil hanya akibat perubahan sumber milik pemain atau sumber tim/streak yang memang berlaku baginya.
- Bobot posisi berjumlah 100%; overall relatif event memakai nilai relatif, overall profil mengakumulasi perubahan efektif kategori berbobot posisi/versi event. Tidak memakai posisi terbaru untuk mengubah histori. Uji pergantian posisi antar-event, seri dan cap yang membuat overall tidak selalu sama dengan bobot atas kategori profil yang sudah dicap.
- Simulasi satu event lengkap dan pilot membandingkan peluang kenaikan semua posisi serta dampak baseline netral. Normalisasi maksimum tidak dianggap bukti keadilan individual; kebobolan tetap proksi tim tanpa data save/tackle/menit.

### 13.9. Keputusan review, risiko dan progres

| Topik | Dasar terbaru | Status/tindak lanjut |
|---|---|---|
| Baseline event | Keputusan pengguna: sembilan poin mentah awal 50 setelah pembagian tim | Ditetapkan; berbeda dari normalisasi event dan profil |
| Nilai event | Keputusan pengguna: tertinggi tiap kategori = 100 | Usulan rasio maksimum atas pemain tampil lintas empat tim; tangani seri, semua sama, Q_e kosong dan pembagi nol |
| Profil | Keputusan pengguna: akumulasi seluruh permainan, bukan rerata | Usulan baseline profil 50 satu kali + delta setiap event dengan cap 0–100 setelah event; baseline dan cap belum disahkan eksplisit |
| Batas akumulasi | Usulan cap profil setelah delta bersih setiap event, dengan ledger nominal/efektif | Menghindari cadangan poin di atas 100/bawah 0; koreksi harus replay urutan event. Baseline profil dan mekanisme ini belum disahkan |
| Keterkaitan pemain | Normalisasi bergantung pemain lain, profil akumulasi personal tidak | Koreksi maksimum memperbarui nilai event semua peserta tanpa mengubah profil pemain yang deltanya tetap |
| Tambahan/pengurangan | Katalog kategori terarah tetap berlaku | Tidak menambah poin Gol/Assist kedua atau meneruskan nilai relatif ke profil |
| Overall per posisi | Bobot usulan pada 13.4 | Pisahkan overall relatif event dan akumulasi delta overall profil; perlu simulasi |
| Kebobolan | Usulan Defending −2 per gol kebobolan pada laga tampil Defender/Goalkeeper | Angka belum disahkan; diuji bersama peluang kenaikan posisi lain |
| Sumber yang belum tersedia | Discipline belum mempunyai bonus, beberapa kategori belum punya potongan | Event bersih tidak memulihkan profil dengan sendirinya; mekanisme pemulihan belum ditetapkan |
| Metadata hasil | First Blood/Match Winner/Comeback memerlukan konfirmasi | Data belum diketahui tidak diberi bonus otomatis |
| Klarifikasi penalti | Keputusan pengguna 12 November 2026: terlambat Commitment −2/−3 dan absen Discipline −15/Commitment −12/Teamwork −5 | Selesai; tabel, AC dan UI diselaraskan pada v2.9/v2.3, tanpa potongan Commitment ganda |
| Skill pembagian tim | Skill manual 1–5 tetap terpisah | Profil atau nilai relatif tidak otomatis dikonversi menjadi skill atau mengubah roster |
| Paket turunan | Perubahan saat ini hanya implementation plan | Planning/PRD/Details/Mockup/Traceability, gambar dan aplikasi belum diproses |

### 13.10. Hasil review dan simulasi sintetis v2.7

Review permintaan pengguna berfokus pada keputusan/scope sekitar bagian 13.1–13.2 beserta dampak rumus di seluruh bagian 13. Ini review rencana, bukan persetujuan seluruh parameter, pemrosesan SRS atau hasil pengujian aplikasi.

| Temuan review | Dampak | Optimasi atau status |
|---|---|---|
| Kategori sembilan masih ditulis sebagai interpretasi awal, padahal sudah diklarifikasi | Dapat memicu pertanyaan yang sama atau salah jumlah | Dicatat sebagai keputusan terbaru; ruang lingkup keikutsertaan juga mencakup tidak mengikuti laga |
| Beberapa kalimat tumpukan masih menyebut cap kategori 100 tanpa membedakan besaran | Poin mentah di atas 100 bisa salah dipotong sebelum normalisasi | Diperjelas: mentah minimal 0 tanpa batas atas, relatif 0–100, profil memakai cap tersendiri |
| Pemain tidak tampil ikut maksimum dari baseline 50 | Pemain absen dapat menjadi pembanding/pemilik nilai kategori tertinggi tanpa bermain | Usulan Q_e hanya pemain terkonfirmasi tampil; histori penalti profil tetap berlaku |
| Penalti sama untuk semua atau pemimpin tetap tertinggi tidak mengubah angka relatif 100 | Nilai relatif dapat disalahartikan sebagai bebas pelanggaran | Contoh dan aturan interpretasi ditambahkan; poin mentah/delta wajib bisa dilihat |
| Cap tampilan setelah akumulasi sepanjang karier menyimpan kelebihan poin | Penalti event berikutnya bisa tidak menurunkan profil yang tampak 100 | Usulan diganti cap setelah setiap event dengan nominal/efektif terpisah dan replay koreksi kronologis |
| Overall dapat meningkat meskipun bonus kategori tertahan cap | Overall dan kategori menjadi sulit dijelaskan | Usulan overall profil memakai perubahan efektif kategori, bukan delta nominal yang tidak diterapkan |
| Ketidakjelasan metadata opsional berpotensi menahan seluruh event | Skor sah dan penilaian lain menunggu kejadian yang tidak dapat dibuktikan | Disposisi metadata disahkan admin, dengan label bukti kurang; data wajib tetap wajib |
| Versi aturan/finalisasi belum cukup eksplisit | Perubahan serentak, retry dan koreksi bisa mencampur snapshot atau menggandakan profil | Rekomendasi snapshot versi konsisten, sumber unik dan replay histori ditambahkan |

#### Skenario enam pertandingan

Data ini sengaja sintetis. Setiap tim A/B/C/D memiliki empat pemain contoh: Striker, Midfielder, Defender dan Goalkeeper. Semua tampil tiga laga, hadir tepat waktu, tidak ada kartu, outlist atau streak sebelumnya; bukan keputusan jumlah pemain nyata tiap tim. Semua gol tim dicetak Striker dan semua assist diberikan Midfielder tim itu, sehingga Gol/Assist tidak dibuat oleh pemain yang sama. Tidak ada gol bunuh diri. Urutan gol pada laga seri dan kejadian khusus ditetapkan pada tabel agar tidak disimpulkan dari skor akhir saja. Tidak ada comeback pada keenam laga.

| Laga | Skor | Pencetak gol pertama | Pencetak gol penentu |
|---|---|---|---|
| A–B | 2–0 | A-Striker | A-Striker (gol pertama A) |
| A–C | 1–1 | C-Striker | Tidak berlaku |
| A–D | 1–0 | A-Striker | A-Striker |
| B–C | 0–1 | C-Striker | C-Striker |
| B–D | 1–1 | B-Striker | Tidak berlaku |
| C–D | 0–0 | Tidak berlaku | Tidak berlaku |

Klasemen memakai usulan 3/1/0: A peringkat 1, 7 poin, GF 4/GA 1; C peringkat 2, 5 poin, GF 2/GA 1; D peringkat 3, 2 poin, GF 1/GA 2; B peringkat 4, 1 poin, GF 1/GA 4. Semua tim bermain tiga laga dan jumlah pertandingan enam. Best Defense bersama A/C; Best Attack A; Unbeaten A/C; tidak ada Perfect Victory; Winless B/D; Last Place B. Hasil ini tidak memakai tiebreak ambigu.

Poin mentah sembilan kategori untuk empat posisi contoh tim A, setelah achievement, dengan usulan kebobolan −2 dan aturan tumpukan yang ada:

| Pemain sintetis | F | P | D | TW | Disc | Com | Cons | I | Ach | Overall event relatif |
|---|---|---|---|---|---|---|---|---|---|---|
| A-Striker | 66 | 50 | 50 | 54 | 50 | 54 | 50 | 62 | 56 | 94,84 |
| A-Midfielder | 50 | 63 | 50 | 54 | 50 | 54 | 50 | 52 | 56 | 93,31 |
| A-Defender | 50 | 50 | 63 | 54 | 50 | 54 | 50 | 52 | 56 | 96,95 |
| A-Goalkeeper | 50 | 50 | 63 | 54 | 50 | 54 | 50 | 52 | 56 | 99,19 |

Singkatan kolom mengikuti sembilan kategori pada 13.4. Maksimum seluruh 16 pemain sintetis: Finishing 66, Playmaking 63, Defending 63, Teamwork 54, Discipline 50, Commitment 54, Consistency 50, Impact 62 dan Achievement 56. Normalisasi menghitung tiap kategori terhadap maksimumnya; hasil yang disimpan belum dibulatkan. Contoh A-Striker memperoleh Finishing 100 tetapi Playmaking 79,37 dan Defending 79,37. A-Goalkeeper memperoleh Defending 100 tanpa Gol/Assist. Pada event pertama dengan profil usulan mulai 50 dan belum menyentuh cap, sembilan nilai profil sama dengan poin mentah tabel, **bukan** sembilan nilai relatif event.

**Hasil perhitungan:** contoh ini memperlihatkan posisi bertahan dapat bersaing, tetapi overall Goalkeeper 99,19 dibanding Striker 94,84 juga menegaskan perlunya kalibrasi bobot dan peluang bonus dengan lebih banyak skenario. Angka ini tidak membuktikan model sudah adil atau bahwa Goalkeeper harus selalu unggul. Discipline/Consistency yang seluruhnya 50 dinormalisasi menjadi 100 untuk semua pemain; bukan bukti semua kategori sudah memberikan pembeda. Bobot, kebobolan −2, baseline profil dan cap per event masih berstatus usulan.

**Keputusan yang masih diperlukan sebelum aturan produksi difinalkan:** tanda Commitment pada keterlambatan dan pengulangan Commitment pada absen; baseline profil/cap per event; bobot posisi/kebobolan; serta aturan tumpukan dan metadata tambahan yang masih usulan. Rencana review dapat selesai tanpa mengarang keputusan tersebut. Pengguna telah mengotorisasi review/optimasi rencana, bukan pembuatan dokumen turunannya dalam instruksi ini.

**Progres revisi 11 November 2026:** rencana v2.7 selesai direview dan dioptimalkan pada keputusan, pemisahan tiga besaran, batas poin, partisipasi, interpretasi normalisasi, akumulasi profil, pengaruh overall, versi/finalisasi dan koreksi. Perhitungan sintetis enam laga/16 pemain serta kasus cap/normalisasi menjadi bukti aritmetis review; bukan UAT atau implementasi aplikasi. Dokumen turunan dan gambar belum diselaraskan. Keputusan pengguna tetap dipisahkan dari parameter rekomendasi; dua rincian penalti belum dijawab.

### 13.11. Pelaksanaan rencana v2.7 — 11 November 2026

**Bukti persetujuan terbaru:** pengguna meminta “proses implementation plan tersebut” dengan rujukan berkas ini setelah review v2.7. Status disetujui → dikerjakan. Rencana menjadi v2.8 untuk mencatat pelaksanaan; lima dokumen turunan menjadi v2.2. Tidak ada gate per dokumen, implementasi aplikasi, deployment atau pengiriman pesan dalam pekerjaan ini.

Ruang lingkup diterima meliputi parameter rekomendasi eksplisit sebagai baseline dokumentasi yang dapat diuji: poin mentah event mulai 50; Q_e pemain tampil; rasio maksimum relatif; profil baseline 50 satu kali dan cap setelah delta event; overall memakai perubahan efektif kategori/posisi historis; kebobolan −2 bagi Defender/Goalkeeper; katalog/tumpukan/streak/metadata dan privasi sebagaimana rencana. Label “usulan/belum disahkan” pada riwayat review 13.9–13.10 tidak berarti persetujuan pemrosesan masih menunggu: parameter tersebut sudah dipakai dalam SRS, tetapi kalibrasi lapangan/kebijakan produksi klub tetap diperlukan.

Persetujuan ini **tidak** memilih tanda Commitment 2/3 pada dua tingkat terlambat atau menentukan maksud Commitment −12/−5 pada absen. Tiga baris tersebut tetap ditandai menunggu klarifikasi pada PRD/Details/UI; pengesahan insiden terkait tidak boleh menebak potongan. Parameter yang tidak mempunyai pilihan konkret, aset asli dan provider juga tetap terbuka. Kelengkapan dokumen dipisahkan dari kesiapan produksi.

| Deliverable | Pelaksanaan |
|---|---|
| Product Planning v2.2 | FEAT-07/09/10 diperluas, FEAT-28 dipromosikan ke MVP penilaian; analitik berinput tambahan tetap roadmap |
| PRD v2.2 | 28 fitur, 33 story, 130 AC; US-29–US-33 dan AC-91–AC-130 baru, seluruh ID lama dipertahankan |
| Product Details v2.2 | Rumus/catalog 21 achievement/penyesuaian, delapan jenis pelanggaran, lima flow baru (26 total), lima state baru (22 unik), validasi/notifikasi/koreksi dirinci |
| Mockup SRS v2.2 | Inventaris tetap 36 layar (35 MVP + toko kondisional); 11 layar/panel lama diperluas, tanpa halaman baru |
| Visual aktif | Tetap 12 referensi: 11 v3 dan pengganti SCR-24 v4 berlabel Statistik Saya/Privat/Data contoh; versi lama tidak dihapus |
| Traceability v2.2 | Seluruh AC baru dipetakan ke flow/layar; audit aktual dan keputusan tertunda dicatat pada bagian terbaru |

Contoh sintetis dan hitungan baseline berada di 13.10 serta Details. Data/visual pada [examples/event-01/](./examples/event-01/) adalah pekerjaan contoh terpisah yang sudah ada dan tidak ditimpa atau diklaim sebagai hasil audit v2.2; perubahan ini memakai simulasi yang tercantum eksplisit di rencana/Details. Riset kompetitor/provider lama tidak diberi tanggal verifikasi baru karena revisi ini berfokus pada domain penilaian.

Audit akhir lulus: 28 fitur, 33 story, 130 AC, 26 flow, 22 state unik, 36 layar; seluruh rujukan ID, 130 AC pada matriks utama dan 40 AC baru pada Details/Mockup; versi upstream, tabel, UTF-8, tautan lokal/anchor; PRD tanpa detail stack/endpoint/skema; 12 PNG cocok dimensi manifest. Simulasi enam laga/16 pemain, maksimum sembilan kategori, overall empat posisi, normalisasi serta cap/replay cocok secara aritmetis. SCR-24 v4 diperiksa visual, 11 v3 dipakai ulang. Hasil aktual pada Traceability bagian 7. Konten autentik, dua rincian penalti, provider/aturan operasional dan validasi lapangan tetap terbuka. Status akhir sebagian-tertunda hanya pada hal-hal tersebut; bukan pekerjaan dokumen independen yang belum dijalankan.

### 13.12. Klarifikasi penalti — 12 November 2026

Instruksi pengguna menetapkan: terlambat 11–20 menit **Commitment −2**; terlambat >20 menit **Commitment −3**; tidak hadir tanpa alasan mendesak **Discipline −15, Commitment −12, Teamwork −5**. Potongan Discipline terlambat yang sudah ditetapkan tetap −4/−6. Tidak ada penambahan Commitment positif, penggabungan menjadi −17 atau potongan absen tambahan pada kategori lain. Tiga baris aktif 13.6 kini lengkap; catatan “belum diklarifikasi” pada riwayat review/pelaksanaan 11 November hanya menggambarkan status saat itu dan telah ditutup oleh keputusan ini.

Paket turunan v2.3 memperbarui Planning, PRD (AC-117/AC-118), Details (katalog, verifikasi, contoh), Mockup (panel SCR-19), dan Traceability. ID, jumlah kebutuhan, rumus baseline/normalisasi/profil, gambar aktif serta achievement tidak berubah. Penalti terkonfirmasi dapat disahkan admin ketika status/alasan/bukti memenuhi ketentuan; tidak lagi ditahan karena tanda/kategori nominal belum diketahui. Hak koreksi dan anti duplikasi tetap berlaku.

Versi aturan baru memuat klarifikasi pengguna. Event draft/insiden yang selama ini ditahan dapat ditinjau dengan versi lengkap sebelum disahkan. Riwayat yang sudah final tidak diubah diam-diam: jika perlu koreksi, admin mencatat alasan, meninjau dampak, mengesahkan versi baru dan mereplay profil sesuai aturan historis yang telah dipilih. Klarifikasi ini bukan alasan memotong absensi lagi atas outlist yang sudah diproses.

Contoh baseline 50 tanpa bonus/potongan lain: terlambat 11–20 → Discipline 46, Commitment 48; terlambat >20 → Discipline 44, Commitment 47; absen → Discipline 35, Commitment 38, Teamwork 45. Overall profil/event mentah pertama berkurang 0,30 / 0,45 / 1,60 menurut bobot 5% kategori terkait, menjadi 49,70 / 49,55 / 48,40. Nilai relatif event dihitung terpisah; pemain absen tanpa penampilan tetap bukan anggota Q_e.

Status keputusan penalti: **selesai**. Audit revisi lulus: nilai tabel Plan/Details konsisten, contoh potongan/overall sesuai, ID/cakupan tetap 28 fitur/33 story/130 AC/26 flow/36 layar, versi/tautan/anchor/tabel/encoding dan 12 dimensi gambar cocok; hasil pada Traceability bagian 8. Konten autentik, provider/aturan operasional lain dan validasi produksi tetap terbuka; tidak menjalankan aplikasi atau deployment.

### 13.13. Visualisasi statistik — keputusan berlaku

Statistik Event dan Profil memakai **spider chart (radar) sebagai satu-satunya jenis grafik**. Pemilih jenis grafik dan preferensi mode dihapus sesuai instruksi terbaru pengguna. Tab Event/Profil dan pemilih event tetap tersedia karena menentukan konteks data. Tabel/daftar angka sembilan kategori mendampingi grafik untuk aksesibilitas dan ketelitian.

Urutan sumbu tetap searah jarum jam dari Finishing di atas: Finishing, Playmaking, Defending, Teamwork, Discipline, Commitment, Consistency, Impact, Achievement. Pusat 0 dan cincin 25/50/75/100, tanpa penyesuaian skala otomatis. Event memakai nilai relatif, Profil memakai akumulasi personal; profil tidak dinormalisasi ulang. Overall terpisah dan poin mentah >100 hanya di rincian/ledger. Data hilang memakai — dan penjelasan; jangan membuat titik nol atau poligon penuh dari data tidak lengkap.

AC-131–AC-133 tetap memakai ID semula dengan perilaku yang direvisi; FLOW-22/FLOW-23 dan SCR-12/SCR-24 diselaraskan. Satu slot gambar diganti SCR-24 v6; total tetap 12 referensi. Referensi v4 berada di archive/v2.2, v5 dipindahkan ke archive/v2.4, dan manifest historis mempertahankan asal edit.

### 13.14. Review hasil pemrosesan dan optimasi — 12 November 2026

Otorisasi: pengguna meminta review/optimasi hasil implementation plan dan penghapusan opsi bar chart. Temuan: opsi lama masih ada pada AC, alur, spesifikasi layar dan gambar; aturan penyimpanan pilihan grafik ikut menjadi tidak relevan. Tindakan: sederhanakan menjadi satu grafik, pertahankan konteks Event/Profil dan tabel aksesibel, revisi AC-131/AC-132 tanpa mengganti ID atau menambah scope. Tidak ada perubahan rumus, poin, penalti, achievement atau izin baca.

Review juga membedakan hasil dokumentasi dari aplikasi: finalisasi/replay, anti duplikasi, privasi, status data hilang dan normalisasi tercakup dalam spesifikasi; penerapannya memerlukan pengujian aplikasi tersendiri. Proksi kebobolan tim dan bobot posisi tetap baseline yang memerlukan kalibrasi lapangan. Konten autentik, layanan/kebijakan dan validasi produksi belum ditutup oleh revisi ini.

Audit revisi lulus: 28 fitur, 33 story, 133 AC, 26 flow, 22 state dan 36 layar unik; seluruh ID/AC terpetakan, versi/tautan/anchor/tabel/UTF-8 valid, dan dimensi 12 PNG aktif sesuai manifest v6. Grafik pengganti diperiksa visual: sembilan sumbu dan legenda benar, pemilih jenis grafik tidak ada. Pemeriksaan ini atas dokumentasi dan referensi visual, bukan pengujian aplikasi.

## 14. Loyalitas dan klaim reward — 14 November 2026

**Otorisasi:** pengguna meminta menggunakan srs-documentation untuk menambahkan fitur klaim reward/poin member yang sering main, dengan poin loyalitas terpisah dari statistik pemain. Ini revisi paket yang sudah ada, sehingga instruksi tersebut mengotorisasi penyelarasan dokumen tanpa gate baru. FEAT-29 masuk MVP diperluas.

**Keputusan pengguna:** perolehan poin loyalitas, saldo/riwayat pribadi, katalog hadiah, klaim dan status pemenuhan. Tidak ada konversi dengan sembilan kategori statistik, overall, skill, voting atau klasemen. Perfect Attendance/Consistent Performer tetap achievement statistik.

**Usulan baseline operasional:** saldo loyalitas mulai 0, tanpa cap 100. Anggota komunitas aktif saat event memperoleh nominal tetap sekali per event selesai dengan kehadiran dan minimal satu laga bermain terverifikasi, termasuk sesi rutin. Tidak mensyaratkan gol, assist, kemenangan, tiga laga atau streak. RSVP, pembayaran dan roster saja tidak memberi poin; tamu tidak otomatis ikut program. Nominal dan versi program ditetapkan sebelum event mulai; event sebelum aktivasi tidak dikreditkan otomatis. Tidak ada pembelian, transfer, pencairan, kedaluwarsa poin atau penalti perilaku langsung pada baseline MVP. Koreksi kehadiran dapat membalik kredit tidak sah.

Klaim mereservasi saldo dan stok bersama; penolakan/pembatalan sebelum pemenuhan melepaskan keduanya. Pemenuhan mengonsumsi poin sekali. Koreksi yang membuat saldo tidak cukup menahan klaim belum dipenuhi; saldo negatif setelah reward diterima menjadi selisih poin, bukan utang uang. Rincian pada [Details bagian 14](./03-product-details.md#14-loyalitas-dan-klaim-reward).

| Dampak | Scope revisi |
|---|---|
| Planning | FEAT-29, PAIN-06/GOAL-06/KPI-06; risiko biaya dan dependensi presensi |
| PRD | US-34–US-36 dan AC-134–AC-153; total 29 fitur, 36 story dan 153 AC |
| Details | FLOW-27–FLOW-29 dan STATE-23–STATE-25; total 29 flow dan 25 state |
| Mockup | SCR-37 Reward Saya dan SCR-38 Kelola Reward; total 38 layar dengan spesifikasi teks responsif |
| Visual | 14 referensi aktif: 12 sebelumnya dan dua gambar loyalitas SCR-37/SCR-38, sesuai instruksi kelanjutan sampai mockup SRS |
| Pembangunan | Identitas/izin dan presensi → program/riwayat → katalog/klaim → pemenuhan/koreksi → pilot. Estimasi lama perlu reestimasi |

**Sebelum aktivasi:** sahkan baseline operasional, nominal poin per event, hadiah nyata/harga poin, stok/anggaran, ketentuan manfaat, tanggal berlaku/zona dan petugas reward. Program tetap Belum aktif sampai konfigurasi lengkap. Contoh "10 kali main" bukan ambang yang telah dipilih pengguna. Gratis main/merchandise dapat menjadi hadiah dengan pemenuhan manual tercatat; klaim tidak otomatis mengubah tagihan/kas atau mengaktifkan toko.

Progres: enam dokumen diselaraskan; pemeriksaan revisi dicatat pada [Traceability bagian 10](./05-traceability.md#10-revisi-loyalitas--14-november-2026). Scope revisi adalah dokumentasi, bukan pembangunan aplikasi. Keputusan program/produksi tetap terbuka.

### 14.1. Kelanjutan sampai mockup SRS

Instruksi pengguna melanjutkan hingga mockup SRS mengotorisasi dua gambar tambahan: SCR-37 Reward Saya (390x844) dan SCR-38 Kelola Reward (1440x900). Total referensi aktif menjadi 14 setelah produksi dan inspeksi. Ketentuan sebelumnya yang membatasi revisi loyalitas pada teks digantikan oleh scope ini. Angka gambar adalah data sintetis, bukan penetapan nominal program. Produksi menggunakan built-in imagegen; status produksi dan inspeksi selesai. Rencana v2.13 menyelaraskan paket v2.7 dengan manifest-v7.

Hasil kelanjutan: dua PNG asli tersimpan lokal, diperiksa visual, prompt aktual dan dimensi tercatat pada [manifest v7](./mockups/manifest-v7.json). Kedua layar memakai wordmark referensi; implementasi tetap memakai logo asli. Angka sintetis tidak menetapkan nominal klub. Seluruh kebutuhan sampai Mockup SRS tersedia; aktivasi program tetap memerlukan parameter yang tercatat.
