# Mockup SRS — Asker FC

- Versi: 2.0 · Tanggal: 9 November 2026
- Status: spesifikasi revisi diselaraskan; manifest gambar dan hasil inspeksi terdapat pada bagian 5. Foto aksi asli, kontak dan sponsor tetap menunggu aset komunitas.
- Upstream: [Product Details v2.0](./03-product-details.md), [PRD v2.0](./02-prd.md), [rencana v1.9](./00-implementation-plan.md). Instruksi pengguna “proses implementation plan yang sebagin masih tertunda” mengotorisasi pengerjaan revisi scope v1.8.
- Seluruh angka/nama pada contoh sintetis. Gambar referensi bukan prototipe atau bukti uji aksesibilitas; teks spesifikasi mengatur perilaku. Default domain usulan mengikuti Details, belum aturan klub yang disahkan.

## 1. Sistem visual dan layout bersama

**Kick & Kumpul** tampil santai dan energik. Arah hijau lapangan/putih dengan aksen kuning berikut adalah perincian rekomendasi dalam alternatif yang diizinkan pengguna. Palet UI hitam–emas–perak baseline diganti; [logo asli pengguna](./assets/logo-asker-fc.png) tetap tanpa perubahan warna/desain/rasio 1:1. Implementasi harus memakai berkas asli, bukan menyalin logo hasil raster generatif.

| Token | Tema terang | Tema gelap | Pemakaian |
|---|---|---|---|
| Background | #F0F7F1 | #101C16 | Latar halaman |
| Surface | #FFFFFF | #182A20 | Kartu/dialog/input |
| Text | #14281D | #F4FAF5 | Teks utama |
| Secondary | #3E5848 | #B9CBBB | Helper/status sekunder |
| Brand/link | #166534 | #86EFAC | Identitas/tautan |
| Header/sidebar | #166534 / teks #FFFFFF | #101C16 / teks #F4FAF5 | Identitas dan navigasi |
| CTA | #FACC15 / teks #14281D | #FACC15 / teks #14281D | Aksi primer |
| Input border | #62796A | #809A87 | Batas kontrol |
| Focus | #166534 | #FACC15 | Outline dengan offset terhadap komponen |
| Error | #B91C1C | #FCA5A5 | Ikon + teks, bukan warna saja |

Kontras token dihitung pada manifest. Border dekoratif boleh lebih halus tetapi field yang perlu dikenali menggunakan token input border. Status selalu memakai label/ikon. Tombol nonaktif menyertakan alasan; tidak memakai kuning aktif tanpa aksi yang dapat dijalankan. Hover/selected harus mempertahankan kontras; pengujian aktual tetap pekerjaan implementasi.

Rekomendasi tipografi: **Poppins Bold 700** untuk judul (mobile 24–32 px, desktop 28–40 px), Poppins 400/500 untuk isi 16 px dengan tinggi baris 1,5; fallback system-ui/sans-serif. Bebas Neue tetap alternatif contoh pengguna, tidak dipakai bersamaan tanpa alasan. Teks kecil minimum usulan 14 px untuk helper, bukan informasi kritis. Label tindakan tidak memakai huruf dekoratif atau seluruh kapital panjang.

Foto aksi asli anggota pada hero/album, dengan izin tayang dan crop responsif yang mempertahankan subjek; overlay teks harus tetap terbaca. Belum ada foto asli di aset proyek: area terkait memakai placeholder berlabel “Foto aksi anggota • menunggu unggahan”. Ini sengaja menandai konten belum tersedia, bukan klaim bahwa foto asli telah diproduksi. Profil kosong memakai inisial. Sponsor tanpa aset tidak diberi logo fiktif. Logo asli 640×640 ditempatkan secara contain di header/brand sidebar, usulan 48–64 CSS px mobile dan 72–88 px desktop, alt “Logo Asker FC”; nama di sebelah tidak perlu dibaca ganda.

Tone: akrab dan humoris pada ajakan/empty state (“Sepatu siap, tinggal kamu!”, “Belum ada laga. Saatnya ajak teman main!”). Sukses RSVP: “Kamu sudah terdaftar. Sampai ketemu di lapangan!” Error: “Kuota baru saja penuh. Pendaftaranmu belum terkonfirmasi.” Pembayaran: “Bukti tersimpan, menunggu verifikasi petugas.” Jangan bercanda tentang tunggakan, kemampuan atau kegagalan pemain. Kata-kata nominal, waktu, status dan langkah pemulihan harus jelas.

Alur utama: **buka link → lihat jadwal → RSVP → terima pengingat → main → lihat skor dan foto → bayar iuran**. Navigasi anggota mobile: Beranda (SCR-04), Jadwal (SCR-23), Tim (SCR-24), Lainnya (panel Hasil SCR-11, Galeri SCR-26, Kas SCR-28, Kontak SCR-29; Toko SCR-36 hanya bila aktif). Profil SCR-12 dan preferensi SCR-33 melalui header/profil. Desktop memakai tautan area utama dan sidebar pengelolaan. Layar masuk/form tugas dapat hanya memiliki Kembali tanpa nav bawah.

Viewport referensi 390×844 mobile dan 1440×900 desktop; raster asli generator disimpan tanpa resize. Semua layar berfungsi pada 360/390/430 px dan desktop. Breakpoint usulan 768 px: sidebar menjadi drawer, form satu kolom, tabel menjadi kartu atau area scroll berlabel. Tidak ada fungsi pengelola yang hilang di HP. Target sentuh usulan ≥44×44 px; ruang bawah konten mencegah nav/CTA sticky menutup isi. Konten panjang membungkus; modal hampir penuh di HP, Escape/Batal menutup dan mengembalikan fokus ke pemicu. Form kotor meminta konfirmasi buang perubahan; memilih batal pada dialog mempertahankan input.

Aksesibilitas: skip link, heading berurutan, label permanen, nama ikon, fokus terlihat, urutan keyboard sesuai hierarki dan pesan diumumkan tanpa warna saja. Fokus ke judul setelah navigasi dan ke error pertama saat validasi; dialog menahan fokus selama terbuka. Semua pemindahan roster tersedia lewat pemilih/tombol tanpa drag wajib. Status bisnis terpisah dari loading/error UI. Cakupan publikasi diterapkan juga pada media/tautan, bukan hanya menyembunyikan menu.
## 2. State dan interaksi bersama

| State | Masuk | Respons dan keluar |
|---|---|---|
| Default/active/selected | Data tersedia/filter dipilih | Tampilkan status dan filter aktif dengan teks; tidak menyamakan selected dengan izin |
| Loading | Membuka/memuat ulang | Skeleton sesuai struktur, label Memuat; cegah aksi yang membutuhkan data sampai versi diketahui |
| Saving/disabled | Submit valid dimulai | CTA Sedang menyimpan dan cegah klik ganda; disabled memiliki alasan bila prasyarat belum lengkap |
| Empty | Tidak ada objek atau filter nol hasil | Jelaskan mana penyebabnya, tawarkan Ubah filter/CTA sesuai izin; bukan menampilkan data contoh sebagai data nyata |
| Validation/error | Input salah atau operasi gagal pasti | Field error + ringkasan, pertahankan input nonrahasia, Coba lagi bila aman |
| Unknown save | Koneksi putus setelah submit | Status penyimpanan belum diketahui + Cek status; jangan mengasumsikan gagal dan langsung membuat ulang |
| Unauthorized/session expired | Izin dicabut/sesi habis | Sembunyikan data terbatas, masuk ulang atau kembali SCR-04; cek ulang versi sebelum retry |
| Success | Hasil simpan terkonfirmasi | Pesan ringkas + status baru + tujuan navigasi; tidak menjanjikan kanal eksternal terkirim |
| Focus/hover | Keyboard/pointer | Outline fokus jelas, label dan konten tidak berubah hanya karena hover |

Pesan validasi dan batas input mengikuti bagian 5 Product Details. Kembali/Batal pada form kotor meminta konfirmasi Buang perubahan; membatalkan dialog mempertahankan form. Field rahasia tidak dipulihkan. Perilaku simpan tidak pasti, konflik versi dan presensi parsial mengikuti bagian 6 Product Details.

## 3. Inventaris final layar

Inventaris final revisi: 35 layar MVP dan satu layar toko kondisional, total 36. Semua mempunyai spesifikasi teks; hanya 12 gambar terpilih (8 mobile, 4 desktop). ID lama tetap, fungsi baru memakai ID tambahan.

| ID | Tujuan/aktor | Fitur / flow / AC | Masuk → keluar | Viewport dan state representatif | Gambar |
|---|---|---|---|---|---|
| SCR-01 Masuk | Calon anggota/anggota | FEAT-01 / FLOW-01 / AC-01–04 | Tautan internal atau SCR-04 saat belum masuk → SCR-02, SCR-03, SCR-04 atau tujuan internal semula | Responsif; 390×844; form kosong | Sesuai manifest v2.0 |
| SCR-02 Daftar dan lengkapi profil | Calon anggota | FEAT-01 / FLOW-01 / AC-01–03 | SCR-01 atau penyelesaian Google → SCR-03/SCR-04 atau kembali SCR-01 | Responsif; 390×844; default terisi | Tidak; spesifikasi teks |
| SCR-03 Verifikasi email dan pemulihan sandi | Pemilik akun | FEAT-01 / FLOW-01 / AC-01,04 | SCR-01/02 atau tautan email → SCR-01 atau tujuan masuk semula | Responsif; 390×844; default terisi | Tidak; spesifikasi teks |
| SCR-04 Beranda komunitas | Publik/anggota sesuai cakupan konten | FEAT-19, FLOW-13, AC-45–AC-47 | Tautan komunitas/nav Beranda → SCR-05/SCR-23/SCR-24 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-05 Detail kegiatan dan RSVP | Publik pembaca sesi terbit; anggota peserta; pengelola scoped | FEAT-03, FEAT-04, FEAT-12, FEAT-20, FLOW-03, FLOW-04, FLOW-12, FLOW-13, AC-08–AC-15, AC-37–AC-38, AC-48–AC-50 | Tautan langsung/SCR-04/SCR-23 → SCR-06/SCR-07/SCR-09/SCR-27/SCR-28/SCR-33 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-06 Status pendaftaran dan undangan | Anggota/kapten untuk diri/tim | FEAT-04, FEAT-11, FLOW-04, FLOW-11, AC-12–AC-15, AC-33, AC-36 | SCR-05/undangan → SCR-05/SCR-07/SCR-20/SCR-33 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-07 Tim dan roster | Anggota pembaca, kapten tim, pengelola | FEAT-04, FEAT-05 / FLOW-04, FLOW-05 / AC-13–18 | SCR-05/06/11 → SCR-06/05/20; pengelola SCR-15/16 | Responsif; 390×844; contoh terisi | Sesuai manifest v2.0 |
| SCR-08 Jadwal pertandingan | Anggota, pencatat, pengelola | FEAT-06, FEAT-12 / FLOW-06, FLOW-12 / AC-19–21,37–38 | SCR-04/05/17 → SCR-09/10 sesuai peran; kembali SCR-05 | Responsif; 390×844; contoh terisi | Sesuai manifest v2.0 |
| SCR-09 Detail hasil pertandingan | Pembaca sesuai izin hasil; petugas laga/pengelola | FEAT-07, FEAT-09, FEAT-21, FEAT-22, FEAT-23, FLOW-07, FLOW-09, FLOW-16, FLOW-17, FLOW-18, AC-22–AC-25, AC-29–AC-30, AC-51–AC-56, AC-59, AC-67 | SCR-08/SCR-11/SCR-12 → SCR-10/SCR-18/SCR-25/SCR-27/SCR-28 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-10 Pencatatan skor dan partisipasi | Pencatat laga/pengelola | FEAT-07, FEAT-09 / FLOW-07, FLOW-09 / AC-22,25,29 | SCR-08/09 → SCR-09; pengelola ke SCR-19 bila presensi kurang | Responsif; 390×844; contoh terisi | Sesuai manifest v2.0 |
| SCR-11 Hasil & Klasemen | Pembaca sesuai izin publikasi | FEAT-07, FEAT-08, FEAT-22, FEAT-23, FLOW-07, FLOW-08, FLOW-17, FLOW-18, AC-22–AC-27, AC-59, AC-67 | Lainnya/SCR-05 → SCR-09/SCR-07/SCR-25/SCR-27/SCR-28 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-12 Profil dan statistik anggota | Pemilik profil; pembaca sesuai cakupan foto/roster | FEAT-01, FEAT-09, FEAT-10, FEAT-25, FLOW-01, FLOW-09, FLOW-10, FLOW-20, AC-01–AC-04, AC-28, AC-31–AC-32, AC-71–AC-73, AC-87–AC-90 | Header/SCR-24/SCR-07 → SCR-05/SCR-09/SCR-25/SCR-33 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-13 Dashboard pengelola dan tugas | Pengelola scoped; pencatat hanya tugas laga; superadmin sesuai delegasi | FEAT-02, FEAT-03, FEAT-07, FEAT-15, FEAT-16, FEAT-21, FEAT-22, FEAT-23, FEAT-24, FLOW-02, FLOW-03, FLOW-07, FLOW-14–FLOW-19, AC-05–AC-11, AC-23, AC-79–AC-86 | SCR-04 Kelola → layar pengelolaan sesuai kewenangan | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-14 Form konfigurasi kegiatan | Superadmin membuat; pengelola mengubah kegiatan tugasnya | FEAT-03, FEAT-15, FEAT-20, FEAT-26, FLOW-03, FLOW-13, FLOW-15, FLOW-21, AC-08–AC-11, AC-48–AC-49, AC-82–AC-83, AC-74 | SCR-13 → SCR-13/SCR-05 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-15 Peserta dan penerimaan tim | Pengelola | FEAT-04 / FLOW-04 / AC-12–15 | SCR-13/05 Kelola peserta → SCR-07/16/13 | Responsif; 390×844; default terisi | Tidak; spesifikasi teks |
| SCR-16 Pembagian tim acak/skill dan penguncian roster | Pengelola kegiatan | FEAT-05, FEAT-16, FLOW-05, FLOW-14, AC-16–AC-18, AC-42, AC-79–AC-81 | SCR-13/SCR-15 → SCR-32/SCR-17/SCR-22 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-17 Penyusunan jadwal | Pengelola | FEAT-06 / FLOW-06 / AC-19–21 | SCR-13/16 → SCR-08/13 | Responsif; 390×844; default terisi | Tidak; spesifikasi teks |
| SCR-18 Validasi dan koreksi hasil | Pengelola | FEAT-07 / FLOW-07 / AC-23–25 | SCR-13/09 → SCR-09/11/22 | Responsif; 390×844; default terisi | Tidak; spesifikasi teks |
| SCR-19 Presensi kegiatan | Pengelola | FEAT-09 / FLOW-09 / AC-28–30 | SCR-13 atau konflik partisipasi SCR-10 → SCR-13/10/12 | Responsif; 390×844; default terisi | Tidak; spesifikasi teks |
| SCR-20 Tagihan dan verifikasi pembayaran | Pemilik tagihan; verifikator sesuai biaya kegiatan atau delegasi kas | FEAT-11, FEAT-23, FLOW-11, FLOW-18, AC-33–AC-36, AC-62–AC-67 | SCR-28/SCR-05/SCR-06/SCR-07 atau petugas SCR-13/SCR-31 → asal/SCR-22 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-21 Administrasi anggota, tamu dan akses | Superadmin | FEAT-02, FEAT-26, FLOW-02, FLOW-21, AC-05–AC-07, AC-74–AC-76 | SCR-13 → SCR-13/SCR-22 | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-22 Riwayat perubahan operasional | Petugas sesuai cakupan objek/delegasi | FEAT-02, FEAT-05, FEAT-06, FEAT-07, FEAT-09, FEAT-11, FEAT-15, FEAT-16, FEAT-21–FEAT-24, FLOW-02, FLOW-05–FLOW-07, FLOW-09, FLOW-11, FLOW-14–FLOW-19, AC-05, AC-18, AC-21, AC-24, AC-30, AC-35, AC-56, AC-61, AC-66 | Layar petugas → objek asal yang masih berhak | Responsif; default/empty/error/success | Lihat manifest; lainnya teks |
| SCR-23 Jadwal & Daftar | Publik/anggota | FEAT-20, FLOW-13, AC-48–AC-50 | SCR-04/nav Jadwal → SCR-05 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-24 Tim & Pemain | Anggota; publik hanya profil terbit | FEAT-01, FEAT-10, FEAT-21, FLOW-01, FLOW-10, FLOW-16, AC-87–AC-90, AC-55 | Nav Tim/SCR-04 → SCR-07/SCR-12/SCR-25 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-25 Voting dan hasil apresiasi | Pemilih berhak/pembaca hasil berizin | FEAT-21, FLOW-16, AC-51–AC-56 | SCR-09/SCR-24 → SCR-09/SCR-24 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-26 Galeri pertandingan | Pembaca sesuai cakupan | FEAT-22, FLOW-17, AC-58–AC-60 | Lainnya/SCR-04 → SCR-27 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-27 Album dan detail media | Pembaca sesuai cakupan | FEAT-22, FLOW-17, AC-58–AC-61 | SCR-26/SCR-09/SCR-05 → SCR-09/SCR-28 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-28 Kas & Iuran anggota | Anggota; tamu hanya tagihan kegiatan sendiri | FEAT-23, FLOW-18, AC-62–AC-67 | Lainnya/SCR-27/SCR-05 → SCR-20 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-29 Kontak & Sponsor | Publik | FEAT-24, FLOW-19, AC-68–AC-70 | Lainnya/SCR-04 → WhatsApp/tujuan luar | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-30 Kelola galeri | Pengelola sesi/petugas media berhak | FEAT-22, FLOW-17, AC-57–AC-61 | SCR-13 → SCR-26/SCR-27/SCR-22 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-31 Kelola kas, tagihan dan verifikasi | Petugas kas komunitas; biaya kegiatan sesuai penugasan | FEAT-23, FEAT-11, FLOW-18, FLOW-11, AC-62–AC-67, AC-33–AC-36 | SCR-13 → SCR-20/SCR-22/SCR-28 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-32 Penilaian skill privat | Penilai ditugaskan; pemain baca nilai diri | FEAT-16, FLOW-14, AC-42, AC-79–AC-81 | SCR-16/SCR-12 → SCR-16/SCR-22 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-33 Preferensi dan pemantauan pengingat | Anggota untuk diri; pengelola untuk sesi tugasnya | FEAT-15, FLOW-15, AC-41, AC-82–AC-86 | SCR-05/SCR-12/SCR-13 → asal/SCR-22 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-34 Kelola voting | Petugas voting laga atau delegasi bulanan | FEAT-21, FLOW-16, AC-51–AC-56 | SCR-13/SCR-25 → SCR-25/SCR-22 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-35 Kelola kontak dan sponsor | Delegasi publikasi komunitas | FEAT-24, FLOW-19, AC-68–AC-70 | SCR-13 → SCR-29/SCR-22 | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |
| SCR-36 Toko/Merchandise kondisional | Anggota/publik bila modul disahkan aktif | FEAT-27, AC-77 | Lainnya hanya jika aktif → WhatsApp/kembali | Responsif 390×844/1440×900; default/empty/error | Sesuai manifest; lainnya teks |

## 4. Spesifikasi screen-by-screen

### SCR-01 — Masuk

**Konteks:** Calon anggota/anggota; izin mengikuti matriks Details. FEAT-01, FLOW-01, AC-01–04. Masuk: Tautan internal atau SCR-04 saat belum masuk. Keluar: SCR-02, SCR-03, SCR-04 atau tujuan internal semula.

**Hierarki layout:** Logo Asker FC dan nama klub → judul Masuk → email/sandi → tombol Masuk → Lanjutkan dengan Google → Daftar akun/Lupa sandi → batas usia 18+ dan kebijakan.

**Konten/komponen:** Label Email, Sandi; CTA Masuk. Tidak menampilkan sandi dalam ringkasan. Tampilkan pesan netral untuk kredensial tidak valid.

**State khusus:** Masuk nonaktif selama permintaan; Google dibatalkan kembali ke form. Error kredensial tidak menyebut apakah email terdaftar. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Masuk | Form valid | Sibuk → SCR-04/tujuan internal jika berhasil; error mempertahankan email |
| Lanjutkan dengan Google | Penyedia tersedia | Kembali ke SCR-02 bila profil belum lengkap; penautan akun perlu bukti akun lama |
| Daftar / Lupa sandi | Tidak sedang menyimpan | SCR-02 / SCR-03 |

**Constraints dan aksesibilitas:** Form/kartu bertumpuk di HP; desktop memakai lebar konten terkontrol tanpa menyembunyikan field. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.




### SCR-02 — Daftar dan lengkapi profil

**Konteks:** Calon anggota; izin mengikuti matriks Details. FEAT-01, FLOW-01, AC-01–03. Masuk: SCR-01 atau penyelesaian Google. Keluar: SCR-03/SCR-04 atau kembali SCR-01.

**Hierarki layout:** Kembali → Daftar akun/Lengkapi profil → nama, email, sandi/konfirmasi pada mode email → deklarasi 18+ → persetujuan kebijakan terpisah → Buat akun/Simpan profil.

**Konten/komponen:** Helper usia: Hanya untuk peserta 18 tahun ke atas. Kebijakan tersedia sebagai tautan konten, bukan checkbox terpilih awal. Field sandi tidak ada pada penyelesaian profil Google.

**State khusus:** Usia tidak memenuhi syarat menolak aktivasi, bukan menyembunyikan pesan. Sukses email menuju verifikasi; Google lengkap menuju kegiatan. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Buat akun | Semua wajib valid, 18+, persetujuan | SCR-03 menunggu email |
| Simpan profil | Identitas Google valid | SCR-04/tujuan internal |
| Kembali | Ada perubahan | Dialog buang perubahan; batal menutup dialog dan mengembalikan fokus |

**Constraints dan aksesibilitas:** Form/kartu bertumpuk di HP; desktop memakai lebar konten terkontrol tanpa menyembunyikan field. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.


### SCR-03 — Verifikasi email dan pemulihan sandi

**Konteks:** Pemilik akun; izin mengikuti matriks Details. FEAT-01, FLOW-01, AC-01,04. Masuk: SCR-01/02 atau tautan email. Keluar: SCR-01 atau tujuan masuk semula.

**Hierarki layout:** Judul sesuai mode → petunjuk/verifikasi link → kirim ulang atau email pemulihan → sandi baru/konfirmasi jika tautan reset valid → CTA.

**Konten/komponen:** Pesan permintaan reset: Jika alamat memenuhi syarat, instruksi akan dikirim. Verifikasi kedaluwarsa menawarkan kirim ulang; reset tidak menampilkan sandi sebelumnya.

**State khusus:** Mode menunggu email, tautan valid, kedaluwarsa, telah dipakai dan sukses dibedakan. Jeda kirim ulang memiliki label waktu, bukan hanya tombol abu-abu. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Kirim ulang | Jeda selesai | Pesan netral; tetap SCR-03 |
| Simpan sandi baru | Tautan sah sekali pakai | SCR-01 dengan pesan sukses |
| Kembali masuk | Semua mode | SCR-01, tanpa otomatis menganggap akun aktif |

**Constraints dan aksesibilitas:** Form/kartu bertumpuk di HP; desktop memakai lebar konten terkontrol tanpa menyembunyikan field. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.


### SCR-04 — Beranda komunitas

**Konteks/izin:** Publik/anggota sesuai cakupan konten. FEAT-19, FLOW-13, AC-45–AC-47. Masuk/keluar: Tautan komunitas/nav Beranda → SCR-05/SCR-23/SCR-24. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Logo/tema, hero foto aksi/placeholder, pesan Kick & Kumpul, sesi terdekat dan Gabung Main, ringkasan statistik sepanjang waktu, akses hasil/foto/iuran.

**State khusus:** Tidak ada sesi memberi ajakan dan Lihat jadwal; konten draft tidak tampil dan angka belum tersedia tidak dibuat nol; tanpa foto memakai placeholder berlabel. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Gabung Main ke SCR-05, Jadwal ke SCR-23, Tim ke SCR-24; Kelola ke SCR-13 hanya jika berhak. Contoh 128 anggota/24 laga/86 gol bukan data riil. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-05 — Detail kegiatan dan RSVP

**Konteks/izin:** Publik pembaca sesi terbit; anggota peserta; pengelola scoped. FEAT-03, FEAT-04, FEAT-12, FEAT-20, FLOW-03, FLOW-04, FLOW-12, FLOW-13, AC-08–AC-15, AC-37–AC-38, AC-48–AC-50. Masuk/keluar: Tautan langsung/SCR-04/SCR-23 → SCR-06/SCR-07/SCR-09/SCR-27/SCR-28/SCR-33. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Kembali, nama/status sesi, tanggal/zona, lokasi/alamat/peta dan Petunjuk arah, kuota/biaya/kebijakan, tim/jadwal/hasil/foto/kas, Bagikan, Gabung Main/status RSVP.

**State khusus:** Penuh/tertutup/batal/terdaftar menyesuaikan label dan CTA; gagal peta tetap alamat; skor/foto kosong tidak mengunci kas; gratis sesi bukan gratis iuran. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Gabung Main satu aksi bagi anggota aktif, tanpa formulir konfirmasi kedua; publik ke SCR-01 lalu kembali sebelum RSVP sadar. Opsi pengingat awal tidak dicentang dan membuka SCR-33 untuk nomor/izin. Tim ke SCR-07, jadwal laga ke SCR-08, Hasil ke SCR-09/SCR-11, Foto ke SCR-27, Kas ke SCR-28. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-06 — Status pendaftaran dan undangan

**Konteks/izin:** Anggota/kapten untuk diri/tim. FEAT-04, FEAT-11, FLOW-04, FLOW-11, AC-12–AC-15, AC-33, AC-36. Masuk/keluar: SCR-05/undangan → SCR-05/SCR-07/SCR-20/SCR-33. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Kembali, status diri, biaya/aturan, undangan beserta kapten/tim, Terima/Tolak undangan, batal sebelum kunci.

**State khusus:** Bukan konfirmasi wajib kedua setelah RSVP individu; tim diajukan belum berarti diterima. Roster terkunci mengarahkan pengunduran ke pengelola. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Terima/Tolak memperbarui undangan diri; batal memakai dialog akibat kuota/biaya dan tidak refund otomatis; lihat tagihan ke SCR-20 sesuai izin; kembali sesi SCR-05. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-07 — Tim dan roster

**Konteks:** Anggota pembaca, kapten tim, pengelola; izin mengikuti matriks Details. FEAT-04, FEAT-05, FLOW-04, FLOW-05, AC-13–18. Masuk: SCR-05/06/11. Keluar: SCR-06/05/20; pengelola SCR-15/16.

**Hierarki layout:** Nama tim dan status → jumlah roster/batas → daftar pemain dengan konfirmasi dan kapten → undang anggota bagi kapten → Ajukan roster → riwayat/status terkunci.

**Konten/komponen:** Contoh Tim Biru, 8 pemain, Kapten Dimas; data sintetis. Roster pembaca hanya nama dan peran; email/telepon tidak ditampilkan. Pemain belum menyetujui berlabel Menunggu.

**State khusus:** Draft, diajukan, diterima, perlu perbaikan dan terkunci ditunjukkan. Tombol anggota biasa tidak sama dengan kapten. Revisi tim diterima tidak mengganti versi resmi sebelum disahkan. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Undang anggota | Kapten, roster belum terkunci | Pemilih anggota aktif; simpan undangan tanpa kanal otomatis |
| Ajukan roster | Seluruh kandidat menyetujui dan jumlah valid | Status diajukan; pengelola meninjau SCR-15 |
| Tarik tim / lihat tagihan | Kapten sebelum kunci / tim berbayar | Dialog konfirmasi / SCR-20 sesuai izin |

**Constraints dan aksesibilitas:** Form/kartu bertumpuk di HP; desktop memakai lebar konten terkontrol tanpa menyembunyikan field. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.




### SCR-08 — Jadwal pertandingan

**Konteks:** Anggota, pencatat, pengelola; izin mengikuti matriks Details. FEAT-06, FEAT-12, FLOW-06, FLOW-12, AC-19–21,37–38. Masuk: SCR-04/05/17. Keluar: SCR-09/10 sesuai peran; kembali SCR-05.

**Hierarki layout:** Header kegiatan → zona/waktu pembaruan → filter hari/tim/lapangan → kartu laga dikelompokkan waktu → status terjadwal/ditunda/final → Bagikan.

**Konten/komponen:** Contoh 07.00 WIB, Tim Biru vs Tim Putih, Lapangan A. Setiap kartu menyebut status. Hanya tugas pencatat menampilkan Catat skor.

**State khusus:** Belum terbit, kosong karena filter, ditunda, versi terbaru dan error muat dibedakan. Draft hanya dapat dilihat pengelola pada SCR-17. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Buka laga | Berhak membaca | SCR-09 |
| Catat skor | Petugas laga/pengelola | SCR-10 |
| Ubah filter / Bagikan | Jadwal terbit | Filter mempertahankan kegiatan; berbagi FLOW-12 tanpa status terkirim |

**Constraints dan aksesibilitas:** Form/kartu bertumpuk di HP; desktop memakai lebar konten terkontrol tanpa menyembunyikan field. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.




### SCR-09 — Detail hasil pertandingan

**Konteks/izin:** Pembaca sesuai izin hasil; petugas laga/pengelola. FEAT-07, FEAT-09, FEAT-21, FEAT-22, FEAT-23, FLOW-07, FLOW-09, FLOW-16, FLOW-17, FLOW-18, AC-22–AC-25, AC-29–AC-30, AC-51–AC-56, AC-59, AC-67. Masuk/keluar: SCR-08/SCR-11/SCR-12 → SCR-10/SCR-18/SCR-25/SCR-27/SCR-28. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Tim/skor, status/versi, waktu/lokasi, rincian gol/partisipasi/kelengkapan, voting MVP, Foto, Kas & Iuran, aksi petugas.

**State khusus:** Belum ada skor memakai — bukan 0–0; diajukan belum resmi; administratif dilabel, foto belum terbit tidak mengunci iuran. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Pencatat ditugaskan ke SCR-10; pengelola ke SCR-18; Voting ke SCR-25 sesuai eligibility; Foto ke SCR-27; Kas ke SCR-28 tanpa menunggu hasil/foto. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-10 — Pencatatan skor dan partisipasi

**Konteks:** Pencatat laga/pengelola; izin mengikuti matriks Details. FEAT-07, FEAT-09, FLOW-07, FLOW-09, AC-22,25,29. Masuk: SCR-08/09. Keluar: SCR-09; pengelola ke SCR-19 bila presensi kurang.

**Hierarki layout:** Header laga → status/versi → input skor dua tim → waktu selesai aktual → pemain tampil → rincian gol dan gol bunuh diri → label kelengkapan → ringkasan → Kirim hasil.

**Konten/komponen:** Contoh skor 2–1; tombol tambah pencetak gol dari roster tampil; helper Rincian gol boleh dilengkapi kemudian. CTA Kirim hasil, bukan Finalkan bagi pencatat.

**State khusus:** Saving menahan submit; konflik meminta muat versi terbaru; timeout tampil status belum diketahui dengan Cek status. Pemain belum hadir tidak bisa dipilih tampil tanpa koreksi presensi. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Tambah gol/pemain | Pemain roster hadir | Ubah form, validasi jumlah gol ≤ skor |
| Kirim hasil | Skor/waktu valid, versi terkini | Konfirmasi → Diajukan → SCR-09 |
| Kembali / Cek status | Perubahan belum kirim / timeout | Dialog buang perubahan / muat hasil operasi sebelum kirim ulang |

**Constraints dan aksesibilitas:** Form/kartu bertumpuk di HP; desktop memakai lebar konten terkontrol tanpa menyembunyikan field. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.




### SCR-11 — Hasil & Klasemen

**Konteks/izin:** Pembaca sesuai izin publikasi. FEAT-07, FEAT-08, FEAT-22, FEAT-23, FLOW-07, FLOW-08, FLOW-17, FLOW-18, AC-22–AC-27, AC-59, AC-67. Masuk/keluar: Lainnya/SCR-05 → SCR-09/SCR-07/SCR-25/SCR-27/SCR-28. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Header/filter sesi, tab Hasil/Klasemen, kartu status hasil dan detail, tabel peringkat jika liga, aturan seri, Lihat foto/Voting MVP/Kas & Iuran.

**State khusus:** Rutin hanya Hasil; belum final tidak masuk klasemen; seri peringkat bersama, rincian gol kurang diberi label. Gambar contoh dua tim dari liga dua tim; skenario Biru/Putih pada layar lain terpisah. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Hasil ke SCR-09, tim ke SCR-07, voting ke SCR-25, foto ke SCR-27, iuran ke SCR-28. Tabel berheader M/Mn/S/K/GM/GK/SG/P, ringkasan boleh hanya M/SG/P dengan akses detail; aturan ke panel read-only. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-12 — Profil dan statistik anggota

**Konteks/izin:** Pemilik profil; pembaca sesuai cakupan foto/roster. FEAT-01, FEAT-09, FEAT-10, FEAT-25, FLOW-01, FLOW-09, FLOW-10, FLOW-20, AC-01–AC-04, AC-28, AC-31–AC-32, AC-71–AC-73, AC-87–AC-90. Masuk/keluar: Header/SCR-24/SCR-07 → SCR-05/SCR-09/SCR-25/SCR-33. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Foto/inisial, nama/posisi/nomor roster terkait, edit diri, filter periode, kehadiran/penampilan/gol/kelengkapan, top skor dan apresiasi terbit, preferensi.

**State khusus:** Kontak pribadi hanya diri/petugas yang perlu; skill diri privat terpisah, nilai anggota lain tidak tampil. Foto kosong berlabel; nol beda belum dicatat. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Edit profil memvalidasi foto/posisi/nomor 1–99 dalam tim terkait; apresiasi ke SCR-25; pengingat ke SCR-33; ubah tema tanpa kehilangan form. Nomor bukan unik seluruh klub. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-13 — Dashboard pengelola dan tugas

**Konteks/izin:** Pengelola scoped; pencatat hanya tugas laga; superadmin sesuai delegasi. FEAT-02, FEAT-03, FEAT-07, FEAT-15, FEAT-16, FEAT-21, FEAT-22, FEAT-23, FEAT-24, FLOW-02, FLOW-03, FLOW-07, FLOW-14–FLOW-19, AC-05–AC-11, AC-23, AC-79–AC-86. Masuk/keluar: SCR-04 Kelola → layar pengelolaan sesuai kewenangan. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Sidebar, filter kegiatan, ringkasan peserta/hasil/bukti/pengingat, tabel sesi, tugas, Pembagian/Voting/Galeri/Kas, Buat kegiatan.

**State khusus:** Dashboard tidak memberi izin baru; kas komunitas/publikasi/bulanan perlu delegasi. Pencatat tidak melihat bukti. Skenario gambar 32/32,2 hasil,3 bukti,1 pengingat perlu diperiksa adalah contoh terpisah. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Buat/Salin hanya superadmin ke SCR-14, admin membuka draft tugasnya; tugas ke SCR-15/SCR-16/SCR-17/SCR-18/SCR-19/SCR-20; akses baru ke SCR-30/SCR-31/SCR-32/SCR-33/SCR-34/SCR-35 bila berhak; riwayat SCR-22. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-14 — Form konfigurasi kegiatan

**Konteks/izin:** Superadmin membuat; pengelola mengubah kegiatan tugasnya. FEAT-03, FEAT-15, FEAT-20, FEAT-26, FLOW-03, FLOW-13, FLOW-15, FLOW-21, AC-08–AC-11, AC-48–AC-49, AC-82–AC-83, AC-74. Masuk/keluar: SCR-13 → SCR-13/SCR-05. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Informasi liga/rutin, mode, tanggal/zona, alamat/pin/lapangan/slot, kapasitas/roster, biaya/aturan, cakupan audiens, terima tamu bila antartim, waktu pengingat, Simpan draft/Publikasikan.

**State khusus:** Mode tidak diubah setelah peserta; kapasitas tidak di bawah terdaftar. Provider belum siap jelas belum aktif; draft boleh tersimpan, publikasi menolak field domain wajib kurang. Gratis hanya biaya sesi. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Simpan draft tetap SCR-14; Publikasikan dialog audiens/aturan ke SCR-05; perubahan waktu/lokasi memutakhirkan pekerjaan belum kirim. Batalkan alasan dan dampak; biaya di muka hanya bila diumumkan. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-15 — Peserta dan penerimaan tim

**Konteks:** Pengelola; izin mengikuti matriks Details. FEAT-04, FLOW-04, AC-12–15. Masuk: SCR-13/05 Kelola peserta. Keluar: SCR-07/16/13.

**Hierarki layout:** Header kuota → tab Individu/Tim sesuai mode → filter status → tabel peserta atau tim → detail persetujuan roster → Terima/Kembalikan → ekspresi sisa kuota.

**Konten/komponen:** Tampilkan identitas minimum, jumlah disetujui dan status bayar tanpa bukti gambar. Alasan pengembalian diketik, pemain belum setuju disorot dengan teks.

**State khusus:** Kuota penuh/konflik anggota saat menerima memberi penolakan spesifik. Tidak menerima tim parsial sebagai lengkap. Data diubah bersamaan memerlukan muat ulang. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Terima tim | Roster lengkap, unik dan kuota tersedia | Dialog → diterima, kuota diperbarui |
| Kembalikan | Tim diajukan, alasan valid | Draft dengan alasan yang dilihat kapten |
| Lanjut roster | Daftar ditutup dan siap | SCR-16 |

**Constraints dan aksesibilitas:** Tabel/list mempunyai header, filter berlabel dan alternatif kartu di HP; scroll horizontal dibatasi pada area tabel, bukan halaman. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.


### SCR-16 — Pembagian tim acak/skill dan penguncian roster

**Konteks/izin:** Pengelola kegiatan. FEAT-05, FEAT-16, FLOW-05, FLOW-14, AC-16–AC-18, AC-42, AC-79–AC-81. Masuk/keluar: SCR-13/SCR-15 → SCR-32/SCR-17/SCR-22. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Status peserta/tim, radio Acak/Seimbang berdasarkan skill, kelengkapan nilai, Susun/Acak ulang, kartu jumlah/rerata/selisih, Pindahkan, Simpan draft/Kunci.

**State khusus:** Pratinjau bukan versi tersimpan; nilai kurang menahan mode skill, pilih Lengkapi atau pindah Acak eksplisit. Antartim tidak diacak; roster terkunci hanya koreksi beralasan. Tidak menjanjikan optimum fairness. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Lengkapi nilai ke SCR-32; susun pratinjau lalu simpan; pindah via pemilih tanpa drag wajib memperbarui metrik; Kunci setelah draft tersimpan dan daftar ditutup → SCR-17; batal tidak menimpa. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-17 — Penyusunan jadwal

**Konteks:** Pengelola; izin mengikuti matriks Details. FEAT-06, FLOW-06, AC-19–21. Masuk: SCR-13/16. Keluar: SCR-08/13.

**Hierarki layout:** Konfigurasi slot/durasi/istirahat → daftar pasangan → pratinjau waktu/lapangan → panel konflik → edit manual → Terbitkan.

**Konten/komponen:** Tampilkan n(n−1)/2 laga liga dan jumlah terjadwal. Baris ditunda menyebut alasan. Jadwal rutin memiliki Tambah laga tanpa memaksakan round robin.

**State khusus:** Jadwal tidak layak memberi daftar belum terjadwal; Terbitkan ditahan. Versi terbit lama tetap dapat dibaca sampai revisi disimpan. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Susun jadwal | Roster terkunci dan slot valid | Draft lengkap atau panel kendala |
| Ubah waktu/lapangan | Laga belum final | Validasi benturan dan istirahat |
| Terbitkan | Pasangan lengkap, konflik nol | Dialog ringkasan → SCR-08 versi resmi |

**Constraints dan aksesibilitas:** Tabel/list mempunyai header, filter berlabel dan alternatif kartu di HP; scroll horizontal dibatasi pada area tabel, bukan halaman. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.


### SCR-18 — Validasi dan koreksi hasil

**Konteks:** Pengelola; izin mengikuti matriks Details. FEAT-07, FLOW-07, AC-23–25. Masuk: SCR-13/09. Keluar: SCR-09/11/22.

**Hierarki layout:** Antrean hasil diajukan → ringkasan laga → skor/waktu/partisipasi dan kelengkapan → riwayat versi → alasan → Kembalikan/Finalkan atau Koreksi final.

**Konten/komponen:** Konfirmasi menyebut efek klasemen/statistik. Koreksi menampilkan nilai sebelum/sesudah dan alasan 10–500 karakter. Diajukan tidak disebut final.

**State khusus:** Finalisasi busy; jika versi berubah hentikan simpan dan tawarkan muat terbaru. Hasil administratif memiliki tipe dan kebijakan yang dipakai. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Finalkan | Hasil diajukan valid | Dialog → hasil final, tautan SCR-11 |
| Kembalikan | Alasan valid | Perlu perbaikan, pencatat kembali SCR-10 |
| Koreksi | Hasil final, alasan dan versi terkini | Pratinjau dampak → final versi baru, SCR-22 |

**Constraints dan aksesibilitas:** Form/kartu bertumpuk di HP; desktop memakai lebar konten terkontrol tanpa menyembunyikan field. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.


### SCR-19 — Presensi kegiatan

**Konteks:** Pengelola; izin mengikuti matriks Details. FEAT-09, FLOW-09, AC-28–30. Masuk: SCR-13 atau konflik partisipasi SCR-10. Keluar: SCR-13/10/12.

**Hierarki layout:** Header kegiatan → filter tim/status → ringkasan hadir/tidak/belum → daftar peserta → pilihan eksplisit per baris → Simpan perubahan → penutupan presensi.

**Konten/komponen:** Tiga status: Belum dicatat, Hadir, Tidak hadir; tidak memilih Tidak hadir otomatis. Detail koreksi setelah selesai meminta alasan.

**State khusus:** Simpan sebagian gagal menyebut baris yang belum berubah. Pemain sudah tampil tidak dapat diubah tidak hadir tanpa koreksi partisipasi terkait. State bersama pada bagian 2 tetap berlaku, termasuk fokus, loading, error dan sukses.

| Aksi | Kondisi | Respons/tujuan dan hasil |
|---|---|---|
| Tandai status | Peserta terdaftar | Perubahan lokal per baris |
| Simpan | Pengelola, data terbaru | Jumlah berhasil/gagal; baris gagal tetap bisa diperiksa |
| Kembali ke laga | Dari SCR-10 | SCR-10 dengan data presensi terbaru, bukan otomatis memilih partisipasi |

**Constraints dan aksesibilitas:** Tabel/list mempunyai header, filter berlabel dan alternatif kartu di HP; scroll horizontal dibatasi pada area tabel, bukan halaman. Header dan CTA sticky hanya bila tidak menutup konten; bagian tengah scroll. Urutan keyboard mengikuti hierarki di atas; pesan error memindahkan fokus ke input terkait. Label status dapat dipahami tanpa warna. Modal/panel mengikuti aturan fokus bersama.


### SCR-20 — Tagihan dan verifikasi pembayaran

**Konteks/izin:** Pemilik tagihan; verifikator sesuai biaya kegiatan atau delegasi kas. FEAT-11, FEAT-23, FLOW-11, FLOW-18, AC-33–AC-36, AC-62–AC-67. Masuk/keluar: SCR-28/SCR-05/SCR-06/SCR-07 atau petugas SCR-13/SCR-31 → asal/SCR-22. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Jenis/periode/nominal/tenggat/instruksi, status, unggah/pratinjau bukti, riwayat; mode petugas antrean/detail/alasan/Terima/Tolak.

**State khusus:** Gratis biaya sesi tanpa unggah tagihan kegiatan; iuran tetap terpisah. Menunggu bukan lunas; Terverifikasi ditampilkan Lunas. Bukti ditolak bisa diganti, lunas tidak bisa ditimpa pengirim; bukti privat. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Unggah JPG/PNG/PDF ≤5 MB → Menunggu verifikasi. Petugas cek mutasi manual; tolak alasan wajib; terima menautkan pemasukan sekali. Koreksi/refund berjejak tanpa transaksi bank otomatis; Cek status jika timeout. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-21 — Administrasi anggota, tamu dan akses

**Konteks/izin:** Superadmin. FEAT-02, FEAT-26, FLOW-02, FLOW-21, AC-05–AC-07, AC-74–AC-76. Masuk/keluar: SCR-13 → SCR-13/SCR-22. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Cari/filter anggota/tamu, identitas minimum, status, peran dan cakupan sesi/tim/laga, delegasi kas/penilai/media/publikasi/voting, alasan.

**State khusus:** Superadmin terakhir terlindungi; akun tamu bukan anggota aktif otomatis. Field tidak lengkap menahan simpan, konflik meminta data terbaru; kontak hanya seperlunya. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Simpan peran/delegasi berlaku permintaan berikutnya; Nonaktifkan dialog dampak beralasan; Riwayat ke SCR-22. Tidak membuat identitas orang lain tanpa persetujuan. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-22 — Riwayat perubahan operasional

**Konteks/izin:** Petugas sesuai cakupan objek/delegasi. FEAT-02, FEAT-05, FEAT-06, FEAT-07, FEAT-09, FEAT-11, FEAT-15, FEAT-16, FEAT-21–FEAT-24, FLOW-02, FLOW-05–FLOW-07, FLOW-09, FLOW-11, FLOW-14–FLOW-19, AC-05, AC-18, AC-21, AC-24, AC-30, AC-35, AC-56, AC-61, AC-66. Masuk/keluar: Layar petugas → objek asal yang masih berhak. Hak terbaru diperiksa pada tiap aksi.

**Hierarki dan konten:** Cakupan, filter waktu/jenis/pelaku, daftar perubahan, versi sebelum/sesudah/alasan, detail read-only, tautan objek.

**State khusus:** Tanpa hak tidak membaca lintas objek. Sandi/token/isi bukti/nomor privat/pilihan suara tidak tampil di audit umum. Objek nonaktif tetap berjejak, kosong memberi pesan. Default/loading/selected/saving/empty/error/success/focus mengikuti bagian 2; status UI tidak mengubah status bisnis. Saat hasil simpan belum diketahui, Cek status mendahului retry.

**Interaksi:** Filter dalam cakupan; detail membuka panel tanpa edit/hapus; tutup kembali fokus; objek asal sesuai ID dan izin saat ini. Batal/Tutup tidak melakukan commit; form kotor meminta konfirmasi Buang perubahan, membatalkan dialog mempertahankan input dan mengembalikan fokus. Data tersimpan tetap saat kembali/masuk ulang; data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Constraints/responsive:** Referensi 390×844 atau 1440×900, uji 360/390/430 px dan desktop. Form bertumpuk, toolbar membungkus; tabel menjadi kartu atau scroll di area berlabel, bukan halaman. Konten panjang dapat dibaca; ruang bawah mencegah CTA/nav sticky menutupi isi. Sidebar admin menjadi drawer; dialog hampir penuh di HP.

**Aksesibilitas:** Label permanen, nama ikon, heading berurutan, urutan fokus sesuai hierarki, pesan tanpa warna saja dan target sentuh usulan ≥44 px. Fokus ke field salah pertama; dialog menahan fokus dan kembali ke pemicu saat tutup. Pemilih menggantikan drag wajib. Kedua tema mengikuti token bagian 1.

**Referensi gambar:** Lihat manifest bagian 5; layar lain tetap spesifikasi teks sesuai batas 12 gambar.

### SCR-23 — Jadwal & Daftar

**Konteks dan izin:** Publik/anggota. FEAT-20, FLOW-13, AC-48–AC-50. Masuk/keluar: SCR-04/nav Jadwal → SCR-05. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Judul, zona, filter tanggal, kalender desktop/agenda HP, kartu waktu/lokasi/kuota/biaya, Lihat sesi. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Kosong filter memberi Reset; gagal peta tidak memblokir agenda; draft tidak tampil. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Pilih hari memfilter; Lihat sesi membuka SCR-05 dengan konteks; reset mempertahankan akun/tema. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-24 — Tim & Pemain

**Konteks dan izin:** Anggota; publik hanya profil terbit. FEAT-01, FEAT-10, FEAT-21, FLOW-01, FLOW-10, FLOW-16, AC-87–AC-90, AC-55. Masuk/keluar: Nav Tim/SCR-04 → SCR-07/SCR-12/SCR-25. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Cari pemain, tab Tim/Pemain, kartu foto/inisial/posisi/nomor, top skor, apresiasi bulanan berperiode. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Tidak ada hasil pencarian beda dengan data privat; belum ada pemenang tidak memalsukan prestasi; nilai/kontak orang lain tersembunyi. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Pilih pemain ke SCR-12; tim ke SCR-07; voting/hasil apresiasi ke SCR-25, filter periode dipertahankan. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-25 — Voting dan hasil apresiasi

**Konteks dan izin:** Pemilih berhak/pembaca hasil berizin. FEAT-21, FLOW-16, AC-51–AC-56. Masuk/keluar: SCR-09/SCR-24 → SCR-09/SCR-24. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Jenis Laga/Bulanan, konteks/periode, aturan dan waktu, kandidat radio, pilihan tersimpan, Konfirmasi/Ganti suara; hasil setelah terbit. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Tidak berhak/tertutup menonaktifkan kirim dengan alasan; seri/tanpa suara eksplisit; timeout Cek status. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Pilih kandidat mengubah lokal; Konfirmasi dialog lalu satu suara; Batal dialog mempertahankan pilihan lokal; hasil ditutup menunggu publikasi. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-26 — Galeri pertandingan

**Konteks dan izin:** Pembaca sesuai cakupan. FEAT-22, FLOW-17, AC-58–AC-60. Masuk/keluar: Lainnya/SCR-04 → SCR-27. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Judul Momen di lapangan, filter sesi, grid album, tanggal dan cakupan, thumbnail/foto berlabel. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Belum ada album memberi pesan; foto belum tersedia berlabel; konten draft tidak muncul. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Pilih album ke SCR-27; ubah filter tidak mengubah data; kembali lewat Lainnya/SCR-04. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-27 — Album dan detail media

**Konteks dan izin:** Pembaca sesuai cakupan. FEAT-22, FLOW-17, AC-58–AC-61. Masuk/keluar: SCR-26/SCR-09/SCR-05 → SCR-09/SCR-28. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Kembali, nama sesi/laga, grid foto/caption, viewer, highlight/link sumber, Kas & Iuran. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Foto kosong/gagal dan video gagal punya pesan/fallback; konten ditarik tidak ditampilkan dari akses baru. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Buka foto ke viewer dengan Prev/Next/tutup keyboard; video ke sumber sah; Kas ke SCR-28 tanpa menunggu foto. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-28 — Kas & Iuran anggota

**Konteks dan izin:** Anggota; tamu hanya tagihan kegiatan sendiri. FEAT-23, FLOW-18, AC-62–AC-67. Masuk/keluar: Lainnya/SCR-27/SCR-05 → SCR-20. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Periode, saldo awal/pemasukan/pengeluaran/saldo, tagihan saya bulanan/kegiatan dipisah, Bayar iuran, riwayat diri. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Tidak ada tagihan bukan tagihan nol rekaan; menunggu/ditolak/lunas berlabel; tamu tidak membaca ringkasan kas klub. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Bayar tagihan aktif ke SCR-20; ganti periode mempertahankan konteks; bukan membuat tagihan baru setelah laga. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-29 — Kontak & Sponsor

**Konteks dan izin:** Publik. FEAT-24, FLOW-19, AC-68–AC-70. Masuk/keluar: Lainnya/SCR-04 → WhatsApp/tujuan luar. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Judul ajakan akrab, WhatsApp admin publik, media sosial, sponsor nama/logo/tautan opsional. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Kontak belum diisi tampil belum tersedia; sponsor kosong tidak diisi logo rekaan. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Klik kontak membuka kanal hanya setelah aksi; salin nomor publik sebagai fallback; sponsor ke tautan HTTPS berlabel. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-30 — Kelola galeri

**Konteks dan izin:** Pengelola sesi/petugas media berhak. FEAT-22, FLOW-17, AC-57–AC-61. Masuk/keluar: SCR-13 → SCR-26/SCR-27/SCR-22. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Pilih laga, album draft, unggah per item, caption/alt/izin, tautan highlight, audiens, pratinjau, Simpan/Terbitkan/Tarik. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Unggah parsial menjelaskan item berhasil/gagal; izin kurang menahan terbit; konten lama tetap sampai perubahan sah. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Simpan draft tidak publik; Terbitkan dialog audiens; Tarik alasan wajib; Batal mempertahankan versi tersimpan. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-31 — Kelola kas, tagihan dan verifikasi

**Konteks dan izin:** Petugas kas komunitas; biaya kegiatan sesuai penugasan. FEAT-23, FEAT-11, FLOW-18, FLOW-11, AC-62–AC-67, AC-33–AC-36. Masuk/keluar: SCR-13 → SCR-20/SCR-22/SCR-28. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Ringkasan periode, daftar transaksi, Terbitkan iuran, Catat pengeluaran, saldo awal, antrean bukti, detail/alasan, Terima/Tolak. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Duplikat terbit membuka tagihan lama; bukti bukan pembayaran otomatis; konflik muat versi; akses tanpa delegasi ditolak. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Pratinjau iuran lalu Terbitkan; transaksi draft lalu konfirmasi; keputusan bukti menambah pemasukan sekali; koreksi membuka ringkasan dampak beralasan. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-32 — Penilaian skill privat

**Konteks dan izin:** Penilai ditugaskan; pemain baca nilai diri. FEAT-16, FLOW-14, AC-42, AC-79–AC-81. Masuk/keluar: SCR-16/SCR-12 → SCR-16/SCR-22. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Daftar peserta tugasnya, nilai1–5, penilai/waktu, kelengkapan, alasan perubahan, Simpan. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Data kurang bukan nol; nilai berubah bersamaan meminta muat ulang; tanpa izin read/write ditolak. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Simpan memvalidasi skala dan cakupan; kembali ke pembagian meminta pratinjau ulang jika nilai berubah; batal edit tidak menimpa snapshot. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-33 — Preferensi dan pemantauan pengingat

**Konteks dan izin:** Anggota untuk diri; pengelola untuk sesi tugasnya. FEAT-15, FLOW-15, AC-41, AC-82–AC-86. Masuk/keluar: SCR-05/SCR-12/SCR-13 → asal/SCR-22. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Mode anggota: nomor privat/persetujuan/opt-out; mode petugas: sesi/waktu/status/referensi dimask, alasan gagal, Periksa status/Retry sah. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Provider belum aktif berbeda dengan terjadwal; gagal pasti beda timeout; status baca hanya bila provider membuktikan. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Simpan preferensi tidak mengubah RSVP; matikan membatalkan pekerjaan belum kirim; retry hanya gagal pasti beralasan; tidak ada kirim massal bebas. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-34 — Kelola voting

**Konteks dan izin:** Petugas voting laga atau delegasi bulanan. FEAT-21, FLOW-16, AC-51–AC-56. Masuk/keluar: SCR-13/SCR-25 → SCR-25/SCR-22. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Jenis/konteks/periode, kandidat/pemilih, zona/buka/tutup, aturan, pratinjau, Buka/Terbitkan/Batalkan. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Data partisipasi kurang menahan buka; snapshot aktif tidak diedit diam-diam; seri/tanpa suara memiliki hasil sah. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Buka dengan konfirmasi snapshot; Terbitkan hanya setelah tutup; koreksi/pembatalan alasan wajib dan riwayat versi. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-35 — Kelola kontak dan sponsor

**Konteks dan izin:** Delegasi publikasi komunitas. FEAT-24, FLOW-19, AC-68–AC-70. Masuk/keluar: SCR-13 → SCR-29/SCR-22. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Nomor admin publik, URL sosial, unggah logo/nama/alt/sponsor, urutan, aktif, pratinjau, Simpan/Terbitkan. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** URL/logo salah diberi pesan; tanpa aset jangan isi fiktif; pengelola sesi tanpa delegasi ditolak. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Simpan draft; Terbitkan panel pratinjau; Nonaktifkan menghilangkan dari publik dan menyimpan jejak. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

### SCR-36 — Toko/Merchandise kondisional

**Konteks dan izin:** Anggota/publik bila modul disahkan aktif. FEAT-27, AC-77. Masuk/keluar: Lainnya hanya jika aktif → WhatsApp/kembali. Periksa hak terbaru pada tiap aksi.

**Hierarki dan konten:** Batas usulan katalog jersey/atribut asli, deskripsi/harga yang diberikan, kontak pemesanan. Semua nilai/nama yang belum diberikan memakai label Data contoh; aturan domain mengikuti Details v2.0.

**State khusus:** Modul tidak aktif berarti menu/route tidak dipublikasikan; tanpa produk bukan toko siap transaksi. Default, loading, selected, saving, sukses, error, kosong, tanpa izin dan fokus mengikuti bagian 2. Tombol berulang dicegah selama simpan; status tidak diketahui memakai Cek status.

**Interaksi:** Pilih produk lalu pesan lewat WhatsApp hanya setelah scope disahkan; tidak menganggap stok, checkout atau pengiriman tersedia. Tombol Batal/Tutup menutup panel tanpa commit dan mengembalikan fokus; form kotor meminta konfirmasi buang perubahan. Data tersimpan tetap setelah kembali/masuk ulang, data lokal belum tersimpan tidak dijanjikan lintas perangkat.

**Layout/responsive:** Referensi mobile 390×844, desktop 1440×900; pemeriksaan 360/390/430 px. Form bertumpuk di HP, tabel menjadi kartu/scroll area berlabel, toolbar membungkus dan teks panjang tetap dapat dibaca. Header/nav/CTA sticky tidak menutup konten. Modal hampir penuh di HP dan drawer admin memiliki tutup.

**Aksesibilitas:** Heading dan tab berlabel, urutan fokus mengikuti hierarki, pesan tidak hanya warna, tombol target usulan ≥44 px, label permanen; setelah error fokus field pertama salah. Viewer/dialog memiliki fokus terkurung lalu kembali ke pemicu; aksi tersedia dengan keyboard tanpa drag wajib. Tema terang/gelap memakai token bagian 1.

**Referensi gambar:** Hanya SCR yang tercantum manifest mendapat gambar; lainnya spesifikasi teks sesuai anggaran.

## 5. Manifest gambar dan pemeriksaan

Dua belas gambar terpilih diproduksi dengan built-in imagegen dan dilihat langsung. Satu edit terarah dilakukan pada SCR-28 untuk kartu gelap, helper pembayaran dan penghapusan kartu foto yang tidak relevan. Format PNG dan dimensi asli dipertahankan. Nama file menyatakan **target viewport**, bukan dimensi raster aktual. Delapan mobile mendekati rasio target; empat desktop lebih tinggi daripada target 1440×900, sehingga bukan screenshot pixel-perfect viewport itu.

| Layar/tema | Target | Raster aktual | Gambar final | Hasil inspeksi dan batas |
|---|---|---|---|---|
| SCR-04 / terang | 390x844 | 852×1846 | [scr-04-390x844-v2.png](./mockups/scr-04-390x844-v2.png) | Diperiksa langsung: logo, hero berlabel menunggu foto, CTA Gabung Main, statistik contoh dan empat navigasi sesuai. Ukuran logo pada raster hanya ilustratif. |
| SCR-23 / terang | 390x844 | 852×1846 | [scr-23-390x844-v2.png](./mockups/scr-23-390x844-v2.png) | Diperiksa langsung: tanggal 14 November 2026, agenda dua sesi, kuota, gratis dan status belum buka terbaca; navigasi Jadwal benar. |
| SCR-05 / terang | 390x844 | 852×1846 | [scr-05-390x844-v2.png](./mockups/scr-05-390x844-v2.png) | Diperiksa langsung: waktu/zona, kuota, gratis biaya sesi, peta skematis, pengingat opt-in terpisah dan CTA RSVP tersedia. Pada implementasi helper login memakai 'Masuk untuk melanjutkan RSVP'; istilah 'sesi aktif' pada raster hanya ilustrasi. |
| SCR-24 / gelap | 390x844 | 852×1846 | [scr-24-390x844-v2.png](./mockups/scr-24-390x844-v2.png) | Diperiksa langsung: mode gelap, inisial/nomor/posisi, apresiasi Oktober, top skor dengan label belum lengkap, tombol voting dan nav Tim sesuai; tidak menampilkan nilai skill/kontak. |
| SCR-11 / terang | 390x844 | 852×1846 | [scr-11-390x844-v2.png](./mockups/scr-11-390x844-v2.png) | Diperiksa langsung: final 2–1, rincian gol 2 vs 1, poin3/0 dan SG+1/−1 konsisten; hasil/foto/voting/kas terhubung pada referensi. |
| SCR-26 / terang | 390x844 | 852×1846 | [scr-26-390x844-v2.png](./mockups/scr-26-390x844-v2.png) | Diperiksa langsung: dua album contoh dengan label foto asli menyusul, filter dan Lihat album; tidak ada foto anggota/sponsor rekaan. |
| SCR-28 / gelap | 390x844 | 853×1844 | [scr-28-390x844-v2.png](./mockups/scr-28-390x844-v2.png) | Diperiksa ulang setelah edit terarah: seluruh kartu gelap, saldo0+2.000.000−750.000=1.250.000, tagihan belum bayar dan instruksi unggah jelas. Kartu foto yang tidak relevan dihapus. |
| SCR-29 / terang | 390x844 | 852×1846 | [scr-29-390x844-v2.png](./mockups/scr-29-390x844-v2.png) | Diperiksa langsung: kontak belum tersedia, sosial menyusul, sponsor menunggu unggahan; tidak membuat nomor atau sponsor fiktif. |
| SCR-13 / terang | 1440x900 | 1487×1058 | [scr-13-1440x900-v2.png](./mockups/scr-13-1440x900-v2.png) | Diperiksa langsung: kartu32/32,2 hasil,3 bukti,1 pengingat perlu diperiksa; tautan skill/voting/media/kas. Tanggal sintetis dashboard merupakan skenario terpisah, bukan snapshot sama dengan mobile. Raster lebih tinggi dari rasio target. |
| SCR-14 / terang | 1440x900 | 1487×1058 | [scr-14-1440x900-v2.png](./mockups/scr-14-1440x900-v2.png) | Diperiksa langsung: draft Rutin/Individu32 peserta4 tim min/maks8; iuran terpisah; provider belum dikonfigurasi dan publikasi nonaktif disertai alasan. Raster lebih tinggi dari rasio target. |
| SCR-30 / terang | 1440x900 | 1487×1058 | [scr-30-1440x900-v2.png](./mockups/scr-30-1440x900-v2.png) | Diperiksa langsung: album Draft, unggah foto asli, alt/caption/tautan, izin belum dicentang dan Terbitkan nonaktif; tidak ada klaim foto sudah tersedia. Raster lebih tinggi dari rasio target. |
| SCR-31 / terang | 1440x900 | 1487×1058 | [scr-31-1440x900-v2.png](./mockups/scr-31-1440x900-v2.png) | Diperiksa langsung: saldo konsisten, pemasukan sekali, biaya lapangan, bukti dummy menunggu verifikasi dan alasan tolak; tidak memakai bukti atau rekening nyata. Raster lebih tinggi dari rasio target. |

Seluruh prompt aktual per aset (termasuk prompt koreksi), tool, target, dimensi dan catatan inspeksi tersimpan pada [manifest-v2.json](./mockups/manifest-v2.json). Gunakan prompt persis dari manifest untuk reproduksi intent, bukan janji hasil identik. Input gambar adalah [logo asli](./assets/logo-asker-fc.png) sebagai referensi identitas; koreksi SCR-28 memakai raster pertama sebagai target edit. Foto asli belum tersedia dan kontak/sponsor belum diisi; area berlabel pada gambar adalah state konten yang belum siap, bukan deliverable foto autentik.

Delapan mobile: Beranda, Jadwal & Daftar, detail sesi/RSVP, Tim & Pemain (gelap), Hasil & Klasemen, Galeri, Kas & Iuran (gelap), Kontak & Sponsor. Desktop: dashboard, konfigurasi, pengelolaan galeri dan kas/verifikasi. Gambar yang tidak dipilih tidak wajib dibuat ulang untuk semua 36 layar. Baseline hitam–emas di [arsip v1.1](./mockups/archive/v1.1/) dan versi awal di [arsip v1.0](./mockups/archive/v1.0/) dipertahankan sebagai riwayat, bukan aset terpilih saat ini.

Logo generatif dapat menyederhanakan ornamen/huruf kecil. Header/ukuran komponen pada gambar hanya referensi; implementasi memakai logo asli dan ukuran CSS pada spesifikasi. Tanggal/angka contoh antar gambar adalah skenario terpisah, bukan satu snapshot tersinkron. SCR-05 pada implementasi memakai helper “Masuk untuk melanjutkan RSVP” agar istilah sesi login tidak membingungkan. Keterbacaan raster bukan audit keyboard, fokus atau aksesibilitas komponen aktual.

### Pemeriksaan kontras token solid

Rasio dihitung menggunakan luminansi relatif sRGB, bukan sampling piksel raster. Target usulan teks normal ≥4,5:1 dan pembeda kontrol ≥3:1; UI aktual masih memerlukan pemeriksaan fokus/hover/disabled/foto/overlay.

| Pasangan | Rasio |
|---|---|
| Teks/surface terang (#14281D / #FFFFFF) | 15.54:1 |
| Sekunder/surface terang (#3E5848 / #FFFFFF) | 7.79:1 |
| Teks/surface gelap (#F4FAF5 / #182A20) | 14.26:1 |
| Sekunder/surface gelap (#B9CBBB / #182A20) | 8.86:1 |
| Teks/CTA kuning (#14281D / #FACC15) | 10.15:1 |
| Putih/header hijau (#FFFFFF / #166534) | 7.13:1 |
| Link/surface terang (#166534 / #FFFFFF) | 7.13:1 |
| Link/surface gelap (#86EFAC / #182A20) | 10.75:1 |
| Border/surface terang (#62796A / #FFFFFF) | 4.71:1 |
| Border/surface gelap (#809A87 / #182A20) | 4.95:1 |

## 6. Batas fase dan penggunaan

FEAT-13, FEAT-14, FEAT-17, FEAT-18 dan FEAT-28 hanya US/AC konseptual; belum dijadwalkan detail layar. FEAT-27/SCR-36 opsional dan belum diaktifkan. FEAT-15/FEAT-16 kini MVP dengan FLOW-14/FLOW-15 dan SCR-16/SCR-32/SCR-33. Gambar hanya subset representatif, bukan kewajiban satu gambar per layar. Implementasi aplikasi, deployment dan pengiriman pesan tidak termasuk tugas ini.

## 7. Riwayat dan sumber prompt

Baseline v1.3 memiliki 22 spesifikasi dan 12 gambar hitam–emas; revisi v2.0 mempertahankan ID lama, menambah 14 layar termasuk toko kondisional, mengganti palet UI, memperjelas foto asli dan menghubungkan tujuh tahap perjalanan. Prompt aktual per aset dan hasil inspeksi dicatat pada manifest pendamping; provider built-in imagegen, satu permintaan per aset. Referensi logo hanya untuk identitas, bukan contoh warna UI.