# Implementation Plan Proyek — Asker Football Club

- Versi: 1.9
- Tanggal: 9 November 2026
- Status workflow: **sebagian-tertunda** — rencana v1.8 diproses atas instruksi pengguna; dokumen revisi v2.0 serta 12 mockup pengganti selesai diselaraskan dan diperiksa. Konten foto asli, kontak dan sponsor belum tersedia; keputusan operasional produksi tetap terbuka. Implementasi aplikasi belum dijalankan.
- Bahasa: Indonesia; user story mengikuti format Inggris pada referensi skill.
- Tujuan file: rencana penyusunan paket SRS dan gambaran pembangunan produk; bukan otorisasi implementasi aplikasi.
- Upstream: jawaban pengguna butir 1–8 pada percakapan ini (tanpa nomor versi), ide awal, lampiran Untitled-1.txt, dan skill srs-documentation beserta referensi intake-and-plan.
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
| Mode gelap dan mobile-first | Pilihan tema tersimpan; rekomendasi awal mengikuti preferensi perangkat. Tetapkan token hijau lapangan, putih dan aksen oranye atau kuning untuk tema terang; adaptasikan ke permukaan gelap dengan identitas hijau dan aksen tetap terjaga, kontras teks/kontrol, fokus dan target sentuh; status tidak hanya dibedakan lewat warna | RSVP, pembayaran, voting dan pembagian tim dapat dipakai pada HP; pemeriksaan pada lebar 360/390/430 px dan desktop tanpa luapan halaman; tema tetap setelah muat ulang dan komponen/state utama terbaca dalam mode gelap |

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
- Identitas visual mengikuti instruksi terbaru v1.7: nuansa santai dan energik dengan hijau lapangan, putih, serta aksen oranye atau kuning. Arahan ini menggantikan palet hitam–emas–perak sebagai tema antarmuka utama pada baseline. Gunakan [logo Asker FC yang dikirim pengguna](./assets/logo-asker-fc.png) dalam warna dan proporsi aslinya; warna logo tidak mengharuskan seluruh antarmuka mengikuti paletnya. Mockup SRS v1.3 dan gambar lama masih menjadi baseline historis yang perlu diselaraskan.
- Pelaksanaan v1.9: 12 PNG pengganti dibuat dengan built-in imagegen dan diinspeksi; SCR-28 dikoreksi terarah. Foto aksi/profil, kontak resmi dan logo sponsor asli belum tersedia, sehingga konten terkait memakai state menunggu unggahan yang berlabel. Gambar referensi UI selesai, konten autentik tetap tertunda. Prompt, dimensi aktual dan batas raster tercatat di [Mockup SRS v2.0](./04-mockup-srs.md) dan [manifest gambar](./mockups/manifest-v2.json).
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

- Gaya santai dan energik tercermin pada hijau lapangan/putih dengan aksen oranye atau kuning, judul tebal, foto aksi asli anggota dan copy akrab/humoris. Token dua tema, contoh copy dan aturan foto konsisten pada spesifikasi serta subset gambar; palet UI baseline lama tidak lagi menjadi acuan aktif.

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