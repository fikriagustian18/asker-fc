# Traceability dan Pemeriksaan — Asker FC

- Versi: 2.9 · Tanggal: 14 November 2026
- Status: revisi akses profil, penghapusan peran Kapten dan penghargaan tahunan selesai sampai spesifikasi Mockup SRS v2.8; referensi gambar statis menyertai revisi. Parameter tahunan belum disahkan; bukan implementasi aplikasi.
- Upstream: [rencana v2.16](./00-implementation-plan.md), [Planning v2.8](./01-product-planning.md), [PRD v2.8](./02-prd.md), [Details v2.8](./03-product-details.md), [Mockup SRS v2.8](./04-mockup-srs.md). Perluasan loyalitas diotorisasi pengguna 14 November 2026.
- Dasar otorisasi: instruksi pengguna 11 November 2026 “proses implementation plan tersebut” setelah review v2.7. Tidak menjalankan implementasi aplikasi, pengiriman pesan atau deployment.


> **Keputusan berlaku v2.9 — pembagian tim:** Anggota mendaftar kegiatan secara individu; sistem mengacak peserta dengan mempertimbangkan skill agar tim seimbang. Admin mengatur jumlah/kapasitas tim, memeriksa kelengkapan data, memicu pembagian, meninjau hasil dan mengunci susunan; bukan memilih pemain ke tim secara manual. UI Roster Admin, undangan/pengajuan/penerimaan roster antartim dan mode acak murni tidak digunakan pada alur ini. Ketentuan antartim/tagihan tim pada bagian lama menjadi catatan scope sebelumnya, bukan alur aktif; bila dibutuhkan sebagai format terpisah harus ditentukan kembali. Daftar tim hasil sistem tetap dapat dibaca. Rating tetap dihitung sistem; sumber/skala skill dan toleransi keseimbangan harus ditetapkan sebelum implementasi, tanpa otomatis menyamakan skill dengan rating.

## 1. Matriks kebutuhan

ID lama dipertahankan. FEAT-15/FEAT-16 dipromosikan ke MVP; FEAT-28 kini MVP penilaian sederhana, sedangkan analitik yang memerlukan data tambahan tetap roadmap. US-29–US-33 dan AC-91–AC-130 memperluas fitur lama tanpa memakai ulang ID. Iuran bulanan manual FEAT-23 dipisah dari penerbitan tagihan otomatis FEAT-14. Rentang AC pada tabel mencakup semua nomor di antaranya. Semua definisi AC berada di [PRD](./02-prd.md), alur di [Details](./03-product-details.md), layar/gambar di [Mockup SRS](./04-mockup-srs.md).

| Goal/pain | Fitur/fase | Story | AC | Flow/status | Layar | Gambar target / alasan tidak digambar | Status |
|---|---|---|---|---|---|---|---|
| GOAL-01 / PAIN-01 | FEAT-01 / MVP | US-01 | AC-01–AC-04, AC-87–AC-88 | FLOW-01 / STATE-01 | SCR-01,SCR-02,SCR-03,SCR-12,SCR-24 | SCR-24 mobile | Teks tersusun; gambar direktori mewakili profil |
| GOAL-05 / PAIN-05 | FEAT-02 / MVP | US-02 | AC-05–AC-07 | FLOW-02 / STATE-01 | SCR-13,SCR-21,SCR-22 | SCR-13 desktop | Izin scoped dan delegasi eksplisit |
| GOAL-02 / PAIN-02 | FEAT-03 / MVP | US-03 | AC-08–AC-11 | FLOW-03 / STATE-02,STATE-03 | SCR-04,SCR-05,SCR-13,SCR-14 | SCR-04,SCR-05 mobile;SCR-13,SCR-14 desktop | Sesi rutin/liga dan publikasi |
| GOAL-01 / PAIN-01 | FEAT-04 / MVP | US-04 | AC-12–AC-15, AC-50 | FLOW-04,FLOW-13 / STATE-04,STATE-05,STATE-06 | SCR-05,SCR-06,SCR-07,SCR-15,SCR-23 | SCR-05,SCR-23 mobile | RSVP satu klik dan roster tetap beda mode |
| GOAL-02 / PAIN-02 | FEAT-05 / MVP | US-05 | AC-16–AC-18 | FLOW-05,FLOW-14 / STATE-07 | SCR-07,SCR-16,SCR-32 | SCR-13 desktop menautkan; pembagian teks | Mode acak/skill, penguncian |
| GOAL-02 / PAIN-02 | FEAT-06 / MVP | US-06 | AC-19–AC-21 | FLOW-06 / STATE-08,STATE-09 | SCR-08,SCR-17 | SCR-23 mobile kalender sesi; jadwal laga teks | Benturan dan hasil final terlindungi |
| GOAL-03 / PAIN-03 | FEAT-07 / MVP | US-07 | AC-22–AC-25 | FLOW-07 / STATE-09 | SCR-09,SCR-10,SCR-18,SCR-22 | SCR-11 mobile ringkasan hasil | Finalisasi dan koreksi |
| GOAL-03 / PAIN-03 | FEAT-08 / MVP | US-08 | AC-26–AC-27 | FLOW-08 / hasil final STATE-09 | SCR-11 | SCR-11 mobile | Klasemen liga; rutin hanya hasil |
| GOAL-04 / PAIN-04 | FEAT-09 / MVP | US-09 | AC-28–AC-30 | FLOW-09 / STATE-11 | SCR-10,SCR-12,SCR-19 | Teks; subset tidak memerlukan gambar tiap layar | Hadir berbeda dengan tampil |
| GOAL-04 / PAIN-04 | FEAT-10 / MVP | US-10 | AC-31–AC-32, AC-89–AC-90 | FLOW-10 / agregasi sah | SCR-12,SCR-24 | SCR-24 mobile | Top skor dan kelengkapan, apresiasi dari voting |
| GOAL-05 / PAIN-05 | FEAT-11 / MVP | US-11 | AC-33–AC-36 | FLOW-11 / STATE-10 | SCR-20,SCR-28,SCR-31 | SCR-28 mobile;SCR-31 desktop | Bukti privat dan verifikasi manual |
| GOAL-03 / PAIN-03 | FEAT-12 / MVP | US-12 | AC-37–AC-38 | FLOW-12 / tanpa status kirim manual | SCR-05,SCR-08 | SCR-05 mobile | Berbagi manual pelengkap pengingat otomatis |
| GOAL-05 / PAIN-05 | FEAT-13 / Lanjutan | US-13 | AC-39 | Belum dijadwalkan | — | Tidak dijadwalkan | US/AC konseptual; bukan tenant aktif |
| GOAL-05 / PAIN-05 | FEAT-14 / Lanjutan | US-14 | AC-40 | Belum dijadwalkan | — | Tidak dijadwalkan | Gateway/penerbitan otomatis bukan iuran manual |
| GOAL-03 / PAIN-03 | FEAT-15 / MVP | US-15 | AC-41, AC-82–AC-86 | FLOW-15 / STATE-12 | SCR-05,SCR-14,SCR-33 | SCR-05 mobile;SCR-13,SCR-14 desktop | Preferensi/status kirim; proses otomatis tidak memerlukan layar khusus lain |
| GOAL-02 / PAIN-02 | FEAT-16 / MVP | US-16 | AC-42, AC-79–AC-81 | FLOW-14,FLOW-05 / snapshot nilai STATE-07 | SCR-16,SCR-32 | SCR-13 desktop menautkan; penilaian teks | Nilai privat dan ukuran keseimbangan |
| GOAL-02 / PAIN-02 | FEAT-17 / Lanjutan | US-17 | AC-43 | Belum dijadwalkan | — | Tidak dijadwalkan | Format tambahan konseptual |
| GOAL-01 / PAIN-01 | FEAT-18 / Lanjutan bersyarat | US-18 | AC-44 | Belum dijadwalkan | — | Tidak dijadwalkan | Anak/wali bukan aktif |
| GOAL-03 / PAIN-03 | FEAT-19 / MVP | US-19 | AC-45–AC-47 | FLOW-13 / publikasi | SCR-04 | SCR-04 mobile | Hero, statistik dan CTA |
| GOAL-01 / PAIN-01 | FEAT-20 / MVP | US-20 | AC-48–AC-50 | FLOW-13,FLOW-04 / STATE-04 | SCR-23,SCR-05 | SCR-23,SCR-05 mobile | Kalender/peta/RSVP |
| GOAL-04 / PAIN-04 | FEAT-21 / MVP | US-21 | AC-51–AC-56 | FLOW-16 / STATE-13 | SCR-25,SCR-34,SCR-24,SCR-09 | SCR-24,SCR-11 mobile; layar voting teks | Dua pemilihan berbeda, seri/tanpa suara |
| GOAL-04 / PAIN-04 | FEAT-22 / MVP | US-22 | AC-57–AC-61 | FLOW-17 / STATE-14 | SCR-26,SCR-27,SCR-30 | SCR-26 mobile;SCR-30 desktop | Album dan publikasi, foto asli masih menunggu |
| GOAL-05 / PAIN-05 | FEAT-23 / MVP | US-23 | AC-62–AC-67 | FLOW-18,FLOW-11 / STATE-15,STATE-16,STATE-10 | SCR-28,SCR-20,SCR-31 | SCR-28 mobile;SCR-31 desktop | Tagihan periode, pemasukan sekali, pengeluaran |
| GOAL-03 / PAIN-03 | FEAT-24 / MVP | US-24 | AC-68–AC-70 | FLOW-19 / STATE-17 | SCR-29,SCR-35 | SCR-29 mobile | Kontak/sponsor, aset asli menunggu |
| GOAL-01 / PAIN-01 | FEAT-25 / MVP | US-25 | AC-71–AC-73 | FLOW-20 / preferensi UI | Semua; khusus SCR-12, SCR-33 | 12 referensi aktif hitam–gold (11 v3 + SCR-24 v6); tema terang dispesifikasikan | Tema tidak mengubah status bisnis |
| GOAL-01 / PAIN-01 | FEAT-26 / MVP | US-26 | AC-74–AC-76 | FLOW-21,FLOW-04 / STATE-05,STATE-06 | SCR-05,SCR-06,SCR-07,SCR-15,SCR-21 | SCR-05 mobile;roster/tamu teks | Akun tamu terbatas satu kegiatan |
| GOAL-04 / PAIN-04 | FEAT-27 / Opsional nonaktif | US-27 | AC-77 | Belum diaktifkan | SCR-36 kondisional | Tidak dijadwalkan | Katalog/pesan usulan; tanpa stok/checkout |
| GOAL-04 / PAIN-04 | FEAT-28 / MVP | US-28 | AC-78 | FLOW-24 / STATE-18 | SCR-12,SCR-24,SCR-18 | SCR-24 v6; lainnya teks | Sembilan kategori dengan sumber dan kelengkapan; analitik tambahan tetap roadmap |
| GOAL-04 / PAIN-04 | FEAT-10 / MVP | US-29 | AC-91–AC-98, AC-131, AC-133 | FLOW-22 / STATE-18,STATE-21 | SCR-10,SCR-12,SCR-14,SCR-16,SCR-24 | SCR-24 v6; input/config teks | Poin mentah 50, input Gol/Assist, Q/maksimum/relatif dan overall event |
| GOAL-04 / PAIN-04 | FEAT-10 / MVP | US-30 | AC-99–AC-105, AC-132 | FLOW-23 / STATE-19 | SCR-12,SCR-22,SCR-24 | SCR-24 v6; audit teks | Profil akumulasi, cap per event dan replay historis privat |
| GOAL-04 / PAIN-04 | FEAT-28 / MVP | US-31 | AC-106–AC-114 | FLOW-24 / STATE-18,STATE-21 | SCR-10,SCR-11,SCR-12,SCR-18,SCR-22,SCR-24,SCR-34 | SCR-24 v6; lainnya teks | 12 individu + 7 tim + 2 penyesuaian negatif; sumber unik/streak |
| GOAL-04,GOAL-05 / PAIN-04,PAIN-05 | FEAT-09 / MVP | US-32 | AC-115–AC-123 | FLOW-25 / STATE-20,STATE-22 | SCR-10,SCR-12,SCR-19,SCR-22 | Teks; panel terhubung dari Statistik Saya | Kartu/kehadiran/koreksi dan anti duplikasi; angka terlambat/absen ditetapkan 12 November |
| GOAL-03,GOAL-04 / PAIN-03,PAIN-04 | FEAT-07 / MVP | US-33 | AC-124–AC-130 | FLOW-26 / STATE-18,STATE-19,STATE-21,STATE-22 | SCR-12,SCR-14,SCR-16,SCR-18,SCR-22 | Teks; finalisasi dan audit tidak menambah jatah gambar | Versi aturan/sumber, metadata opsional, finalisasi/retry/koreksi konsisten |

### Pemetaan alur utama dan halaman

| Tahap/area | Fitur / AC | Flow | Layar | Pemeriksaan perilaku |
|---|---|---|---|---|
| Buka link / Beranda | FEAT-19, AC-45–AC-47 | FLOW-13 | SCR-04/SCR-05 | Tujuan sesi dipertahankan; konten draft tidak terungkap |
| Lihat jadwal / Jadwal & Daftar | FEAT-20, AC-48–AC-50 | FLOW-13 | SCR-23/SCR-05 | Peta gagal tetap ada alamat; tanggal/zona jelas |
| RSVP | FEAT-04, AC-12–AC-15, AC-50 | FLOW-04 | SCR-05/SCR-06 | Satu aksi, slot terakhir, kembali sesudah login |
| Terima pengingat | FEAT-15, AC-41, AC-82–AC-86 | FLOW-15 | SCR-33/SCR-14 | Opt-in terpisah; status/retry/jadwal revisi |
| Main / Tim & Pemain | FEAT-05, FEAT-16, FEAT-09, AC-16–AC-18, AC-28–AC-30, AC-79–AC-81 | FLOW-05/FLOW-14/FLOW-09 | SCR-24/SCR-07/SCR-16/SCR-19 | Tim unik, nilai sah, kehadiran bukan asumsi RSVP |
| Lihat skor / Hasil & Klasemen | FEAT-07, FEAT-08, AC-22–AC-27 | FLOW-07/FLOW-08 | SCR-09/SCR-11 | Hanya final jadi resmi; koreksi berjejak |
| Lihat foto / Galeri | FEAT-22, AC-57–AC-61 | FLOW-17 | SCR-26/SCR-27 | Foto belum ada tidak mengunci Kas |
| Bayar iuran / Kas & Iuran | FEAT-11, FEAT-23, AC-33–AC-36, AC-62–AC-67 | FLOW-11/FLOW-18 | SCR-28/SCR-20/SCR-31 | Bukti belum berarti lunas; pemasukan sekali |
| Kontak & Sponsor | FEAT-24, AC-68–AC-70 | FLOW-19 | SCR-29/SCR-35 | Hanya kontak/logo resmi yang diterbitkan |
| Toko/Merchandise | FEAT-27, AC-77 | Opsional | SCR-36 | Tidak aktif dan tidak menambah checkout |

## 2. Kebutuhan nonfungsional

| ID | Area dan dukungan | Rencana verifikasi produk |
|---|---|---|
| NFR-01 | Seluruh izin; FLOW-02, FLOW-15–FLOW-19, FLOW-21 | Uji lintas objek/delegasi/tamu, termasuk URL langsung dan berkas; pencabutan berlaku permintaan berikutnya |
| NFR-02 | Kontak, bukti, foto, nilai skill, suara individual dan nomor pengingat | Uji audiens publik/anggota/kegiatan, metadata foto, log dan viewer; tidak ada rahasia di audit umum |
| NFR-03 | Semua layar, 360/390/430 px dan desktop, kedua tema | Uji form/roster/tabel tanpa wajib drag atau luapan horizontal halaman |
| NFR-04 | Token/label/fokus/keyboard/modal | Perhitungan token pada Mockup; audit komponen aktual tetap diperlukan |
| NFR-05 | Kuota/hasil/suara/tagihan/pemasukan unik | Usulan 500 akun/50 bersamaan/15 menit, p95 baca 2 dtk/simpan 3 dtk; belum diuji, provider/unggah dikecualikan sesuai PRD |
| NFR-06 | Backup data dan media privat/publik | Usulan harian, RPO24 jam/RTO4 jam; latihan restore sebelum pilot produksi |
| NFR-07 | Audit peran, skill/roster, hasil, voting, pengingat, publikasi dan kas | Periksa pelaku/waktu/alasan/versi dan akses audit sesuai cakupan |
| NFR-08 | Simpan tidak diketahui dan status provider | Cek status sebelum retry, tanpa menganggap gagal pasti atau menjanjikan exactly-once lintas provider |
| NFR-09 | Zona sesi, periode voting/iuran dan jadwal pengingat | Uji perangkat zona lain dan batas bulan/tenggat; zona kegiatan tetap berlabel |
| NFR-10 | Pemilik retensi/dukungan/kebijakan konten | Keputusan sebelum produksi; bukan klaim kepatuhan yang telah diaudit |

## 3. Skenario pemeriksaan lintas fitur

Daftar berikut adalah rencana uji aplikasi yang diturunkan dari spesifikasi, **belum dieksekusi pada aplikasi**.

| Kasus | Hasil yang disyaratkan | Relasi |
|---|---|---|
| Buka link saat belum masuk | Sesi publik terbaca; login untuk RSVP kembali ke sesi yang sama | AC-45, AC-50; FLOW-13 |
| Dua peserta ambil slot terakhir | Satu terdaftar dan satu ditolak, tidak lebih kuota | AC-12, AC-14; FLOW-04 |
| Tim tanpa persetujuan / pemain ganda | Roster tidak diterima hingga lengkap dan unik | AC-13, AC-14; FLOW-04 |
| Pembagian 32 peserta ke 4 tim | Tiap peserta sekali, delapan per tim; Acak dan Skill tidak disamakan | AC-16–AC-18; FLOW-05 |
| Nilai kosong / berubah bersamaan | Skill menolak data kurang; pindah Acak eksplisit; pratinjau usang dihitung ulang | AC-79–AC-81; FLOW-14 |
| Lima tim liga | Sepuluh pasangan unik tanpa tim/laga fiktif; tidak ada benturan | AC-19–AC-21; FLOW-06 |
| Hasil diajukan / final dikoreksi | Hanya versi resmi masuk agregasi; koreksi tidak menambah laga ganda | AC-22–AC-27; FLOW-07/FLOW-08 |
| Hadir tidak sama tampil / gol parsial | Statistik memakai cakupan yang dicatat, label kelengkapan terlihat | AC-28–AC-32, AC-89; FLOW-09/FLOW-10 |
| Suara ganda / terlambat / diri sendiri | Maksimal satu suara aktif yang sah; ditolak menurut eligibility dan tenggat | AC-52, AC-53; FLOW-16 |
| Voting seri / nol / dua jenis | Pemenang bersama atau tanpa pemenang; bulanan terpisah dari laga/gol | AC-54, AC-55; FLOW-16 |
| Jadwal berubah / peserta mundur / opt-out | Pekerjaan usang dibatalkan dan versi belum terkirim diperbarui | AC-41, AC-83, AC-86; FLOW-15 |
| Timeout provider / restart | Rekonsiliasi sebelum retry, tidak mengulang yang mungkin telah berhasil | AC-84, AC-85; FLOW-15 |
| Foto/hasil belum ada / media ditarik | State jelas, akses Kas tetap tersedia; media ditarik tidak bisa diakses baru | AC-59, AC-61, AC-67; FLOW-17/FLOW-18 |
| Tagihan terbit ulang / verifikasi diulang | Satu tagihan aktif dan satu pemasukan neto terkait pembayaran | AC-62, AC-64; FLOW-18 |
| Bukti ditolak / koreksi kas | Alasan wajib, bukti pengganti berjejak, saldo terkoreksi tanpa hapus audit | AC-35, AC-66; FLOW-11/FLOW-18 |
| Tamu membuka kas/direktori internal | Ditolak tanpa data, roster sesi berhak tetap terbaca | AC-74–AC-76; FLOW-21 |
| Tema gelap/muat ulang/HP | Tema tersimpan, data form utuh, semua kontrol terbaca tanpa scroll halaman menyamping | AC-71–AC-73; FLOW-20 |

## 4. Pemeriksaan aktual dokumen

Audit kebutuhan di bawah dilakukan 9 November 2026 pada baseline v2.0; audit revisi visual 10 November 2026 dicatat terpisah di akhir dokumen. Pemeriksaan dokumen bukan pengujian runtime atau pengesahan aturan klub. Hasil audit baseline tidak otomatis diklaim berlaku untuk revisi ini.

| Pemeriksaan aktual | Hasil dan batas bukti |
|---|---|
| Persetujuan | Instruksi pengguna memproses rencana yang tertunda setelah v1.8 dicatat sebelum revisi paket |
| Definisi/rujukan ID | Pemeriksaan struktural: 28 fitur, 28 story, 90 AC, 21 flow dan 36 layar; tidak ada definisi ganda atau rujukan ID tidak terdefinisi |
| Cakupan | 22 fitur MVP memiliki story/AC dan mekanika/layar, 5 fitur lanjutan konseptual, 1 toko opsional; ID lama tidak digunakan ulang |
| Alur utama | Tujuh tahap dipetakan pada matriks; foto/hasil belum terbit tidak mengunci Kas & Iuran, bukti bukan otomatis lunas |
| Dokumen upstream | Planning/PRD/Details/Mockup/Traceability v2.1 merujuk rencana v2.0 dan versi turunan yang sama |
| Scope lama | Skill/pengingat dipromosikan, publikasi foto/iuran manual menjadi MVP, UI hitam–gold dominan sesuai instruksi 10 November 2026 menggantikan hijau–putih–kuning; dua tema tetap tersedia |
| KPI | KPI-01–KPI-04 dipertahankan; KPI-05 mencakup keputusan iuran bulanan tanpa mengubah rumus kelengkapan, dengan periode relevan dan target usulan 100%. Baseline seluruh KPI belum diukur |
| PRD | Pemeriksaan kosakata implementasi tidak menemukan detail stack/endpoint/skema dalam PRD; perilaku integrasi ditulis sebagai outcome |
| Gambar | 12 PNG pengganti disalin lokal dan dilihat langsung: 7 mobile 852×1846, 1 mobile 853×1844 setelah koreksi, 4 desktop 1487×1058; target390×844/1440×900 berbeda dari raster, khusus desktop bukan rasio identik |
| Inspeksi/koreksi | SCR-28 dikoreksi agar seluruh kartu gelap dan instruksi bayar tidak mengaku bukti sudah menunggu; hasil lain dan keterbatasan minor dicatat pada manifest |
| Prompt/aset | Prompt aktual dan koreksi tersimpan; logo asli dipertahankan, versi lama diarsipkan. Foto/kontak/sponsor belum tersedia dan tidak direkayasa |
| Kontras | Luminansi relatif dihitung dari 10 pasangan token dua tema, bukan audit raster/implementasi; nilai tersedia pada Mockup |
| Riset | Tiga sumber pembanding resmi diverifikasi ulang; kajian WhatsApp memakai dokumentasi resmi Twilio, bukan pengesahan provider atau asumsi biaya |
| Batas pengujian | Belum ada aplikasi, UAT, pengukuran beban, audit keamanan, pengiriman pesan nyata atau wawancara; daftar skenario adalah rencana uji |

Pemeriksaan tautan/encoding dan kelengkapan AC-ke-flow/layar dijalankan ulang setelah finalisasi: lulus; 84 AC MVP terpetakan ke alur/layar, 90 AC masuk traceability, 12 berkas cocok dengan dimensi manifest (7 mobile 852×1846, 1 mobile 853×1844, 4 desktop 1487×1058). Catatan konten asli dan keputusan produksi pada bagian 5 tetap terbuka; paket tidak dilabel siap produksi.

## 5. Keputusan produksi/konten yang masih terbuka

| Keputusan/aset | Dampak dan tindak lanjut | Pemilik usulan |
|---|---|---|
| Foto aksi/profil asli dan izin tayang | Ganti placeholder berlabel setelah aset sah tersedia; jangan menyebut foto generatif sebagai anggota nyata | Pemilik komunitas/petugas media |
| Kontak publik, media sosial, logo sponsor | Isi data resmi lalu tinjau publikasi; gambar menunjukkan state belum tersedia | Pemilik komunitas/petugas publikasi |
| Hosting/email/berkas/peta/video/UI library/anggaran | Pilih provider dan uji akses, backup, biaya serta dukungan sebelum implementasi terkait | Pemilik produk/tim pengembang |
| WhatsApp | Pilih provider/akun/sender, verifikasi biaya/ketentuan/template, persetujuan/opt-out dan status; gunakan penerima uji sebelum produksi | Pemilik komunitas/tim pengembang |
| Aturan skill/voting/pengingat | Sahkan atau revisi default yang dilabel usulan pada Details, termasuk eligibility, waktu dan seri | Pengelola/pemilik komunitas |
| Tarif/tenggat/anggota dikenai/saldo awal/delegasi kas | Isi keputusan nyata, hindari menganggap Rp50.000 contoh sebagai tarif klub | Petugas kas/pemilik komunitas |
| Usia/negara/privasi/retensi/izin tamu | Sahkan kebijakan dan penanggung jawab sebelum data produksi | Pemilik komunitas |
| Kapasitas/tanggal rilis | Reestimasi scope revisi dengan kapasitas tim; jadwal historis tidak menjanjikan rilis baru | Pemilik produk/tim pengembang |
| Validasi persona/proses/adopsi | Simulasi lalu pilot tujuh tahap dan evaluasi empat kegiatan; bukan hasil riset dokumen | Pengelola/perwakilan anggota |

Keputusan di atas bukan alasan menunda penulisan spesifikasi yang independen; bagian terkait tetap berlabel usulan/belum siap produksi. Paket berstatus sebagian-tertunda untuk konten asli yang belum tersedia meskipun dokumen dan gambar referensi dapat diselesaikan.
## 6. Audit revisi visual — 10 November 2026

Otorisasi: instruksi pengguna melanjutkan SRS dan memakai hitam–gold dominan sesuai logo. FEAT-25 / US-25 / AC-71–AC-73 / FLOW-20 dan SCR-01–SCR-36 mengikuti sistem visual [Mockup SRS v2.1](./04-mockup-srs.md). AC-71/FLOW-20 menyelaraskan default gelap dan pilihan Sesuai perangkat; AC-73 memperbarui palet. ID, aturan bisnis dan scope fitur tetap. Rencana v2.0 menjadi sumber terbaru. Audit 9 November di atas adalah riwayat baseline, bukan pengujian baru.

Dua belas PNG v3 diperiksa visual satu per satu: delapan mobile dan empat desktop, memakai layout v2 dengan palet baru, disimpan lokal. Prompt aktual, target, dimensi serta inspeksi tersedia pada [manifest v3](./mockups/manifest-v3.json). Kontras token dihitung ulang dan tautan/encoding diperiksa; tidak menjalankan aplikasi/UAT.

Batas: logo generatif hanya referensi; implementasi memakai logo asli. SCR-05 menyisakan ikon Tim hijau kecil; implementasi mengikuti token gold. SCR-29 memperlihatkan tombol kontak gold berlabel Belum tersedia; implementasi harus disabled abu-abu tanpa tautan sampai nomor resmi diisi. Badge Berjalan hijau SCR-13 dan pemasukan hijau/pengeluaran merah SCR-31 adalah status semantik berlabel. Raster tidak menjanjikan ukuran viewport pixel-perfect. Tema terang dispesifikasikan tetapi 12 gambar revisi semuanya tema gelap hitam–gold.

Status revisi warna: **selesai** dengan batas raster di atas. Paket tetap **sebagian-tertunda** untuk konten autentik dan keputusan produksi pada bagian 5.
## 7. Pelaksanaan dan audit penilaian — 11 November 2026

Scope diterima dari rencana v2.7, dicatat dalam rencana v2.8. Parameter eksplisitnya digunakan sebagai baseline dokumentasi, termasuk profil awal 50 sekali/cap per event, bobot posisi, Q_e pemain tampil, rasio maksimum, kebobolan −2, metadata dan aturan tumpukan/streak. Belum merupakan hasil kalibrasi klub. Dua rincian penalti yang belum jelas pada 11 November kini ditutup oleh keputusan pengguna 12 November 2026, dengan perubahan AC-117/AC-118 dan sumber tabel yang konsisten.

### 7.1. Skenario penerimaan tambahan

| Kasus | Hasil acuan | Relasi |
|---|---|---|
| Simpan pembagian berulang | Baseline mentah 50 satu kali, bukan +50 tiap simpan | AC-91–AC-92; FLOW-22; SCR-14/SCR-16 |
| Gol/Assist parsial dan metadata tidak diketahui | Skor dapat final, poin terkait sementara; jangan mengarang data | AC-93–AC-94, AC-125, AC-128; FLOW-22/FLOW-26; SCR-10/SCR-18 |
| Pemain tidak tampil / Q kosong / semua nol | Tidak menjadi pembanding; nilai — atau state khusus, bukan angka rekaan | AC-95–AC-97; FLOW-22; SCR-12 |
| Pemimpin kategori/seri dan penalti bersama | Maksimum 100; semua mentah sama dapat 100 relatif meskipun terkena potongan | AC-96, AC-98; FLOW-22; SCR-12/SCR-24 |
| Event vs profil | F mentah 60/55/50 → relatif 100/91,67/83,33; profil delta +10/+3/0 → 63 | AC-99–AC-100, AC-103; FLOW-23; SCR-12/SCR-22 |
| Cap dan koreksi lama | +70/−3 →100/97; koreksi +70 menjadi +40 mereplay →90/87, bukan 67 | AC-101–AC-102, AC-104; FLOW-23; SCR-22 |
| Gol/assist tier dan defensif | F bonus +3/+6/+8; P≥2 assist +7; D clean +5, satu kebobolan +2−2=0 | AC-106–AC-109; FLOW-24; SCR-10/SCR-12 |
| Kehadiran/streak | Tampil semua laga +4 Commitment; tiga/enam event menghasilkan satu/dua bonus Consistency | AC-110–AC-111; FLOW-24; SCR-12/SCR-19 |
| Hasil tim final / pemain batal | Champion/Runner-Up sekali; Last Place+Winless hanya TW−9 untuk pemain tampil | AC-112–AC-114; FLOW-24; SCR-11/SCR-18 |
| Kuning lalu merah / outlist / menolak laga | Potongan kategori sesuai sumber; kejadian sama tidak dihukum dua kali | AC-115–AC-120; FLOW-25; SCR-10/SCR-19 |
| Alasan dan nominal final / koreksi penalti | Nominal terlambat/absen eksplisit; verifikasi admin tetap wajib dan pemilik berhak koreksi | AC-118, AC-121–AC-123; FLOW-25; SCR-12/SCR-19 |
| Konflik/retry dan versi aturan | Snapshot poin konsisten, sumber unik; versi sama tidak mem-posting dua kali | AC-124–AC-129; FLOW-26; SCR-18/SCR-22 |
| Enam laga/16 pemain sintetis | A/C/D/B, maksimum 66/63/63/54/50/54/50/62/56 dan overall empat posisi cocok | AC-130; FLOW-26; Details 13.5 dan rencana 13.10 |

### 7.2. Bukti audit aktual revisi

Audit akhir dijalankan 11 November 2026 setelah penulisan dan pembaruan tautan/versi; pemeriksaan struktural dan aritmetis berikut lulus pada paket v2.2. Riwayat audit v2.0/v2.1 pada bagian 4/6 bukan pengujian ulang aplikasi.

| Area | Artefak dan hasil yang dicatat |
|---|---|
| Otorisasi | Instruksi pengguna memproses rencana v2.7 tersedia; rencana v2.8 merekam disetujui/dikerjakan |
| Struktur kebutuhan | Lulus: 28 fitur, 33 story, 130 AC, 26 flow, 22 state unik, 36 layar; definisi unik dan rujukan ID terdefinisi |
| Cakupan penilaian | AC-91–AC-130 dipetakan eksplisit pada matriks; FEAT-28 dipromosikan ke MVP, tanpa perubahan ID lama |
| Rumus/domain | Simulasi enam laga/16 pemain dan contoh batas/replay diperiksa secara aritmetis; bukan UAT atau bukti keadilan statistik |
| Visual | SCR-24 v4 disalin lokal dan dilihat langsung; sembilan kategori, overall 94,84, privasi, contoh data, ledger dan navigasi sesuai. Sebelas v3 digunakan kembali, bukan diklaim dibuat ulang |
| Manifest | Lulus: manifest aktif v4 berisi 12 referensi dan setiap dimensi PNG cocok dengan header raster, termasuk SCR-24 v4 852×1846. Versi v3 tetap riwayat |
| Batas raster | Label 4 Assist tim bukan Assist personal A-Striker; teks spesifikasi menjelaskan nilainya. Logo generatif referensi, implementasi memakai aset asli. Bukan screenshot aplikasi atau audit aksesibilitas |
| PRD | Story/AC berupa outcome pengguna; pemeriksaan batas konten dan kosakata implementasi dilakukan sebelum penyerahan |
| Tautan/ID/encoding | Lulus: semua definisi/rujukan ID, 130 AC pada matriks utama, 40 AC baru pada Details/Mockup, versi upstream, tabel, tautan lokal beserta anchor dan UTF-8 |

### 7.3. Bagian yang tetap tertunda

| Topik | Status dan langkah berikutnya |
|---|---|
| Kalibrasi keadilan posisi | Bobot dan kebobolan −2 baseline dokumentasi; pilot lebih dari satu skenario diperlukan sebelum aturan klub dipakai nyata |
| Pemulihan Discipline/sumber potongan kategori lain | Katalog belum menyediakan; jangan menambahkan aturan yang tidak disetujui |
| Konten autentik/provider/aturan produksi | Tetap sesuai bagian 5, tidak ditutup oleh penulisan SRS atau gambar sintetis |

Paket revisi dapat selesai pada dokumentasi independen, tetapi status keseluruhan tetap sebagian-tertunda untuk konten autentik/keputusan layanan/kebijakan serta validasi lapangan; rincian penalti sudah ditetapkan. Belum ada aplikasi, uji runtime, pengiriman pesan, deployment atau data anggota nyata yang diproses.

Audit aritmetis acuan juga memeriksa sembilan maksimum dan overall empat posisi dari enam laga, akumulasi +10/+3/0 =63, cap +70/−3 =97, koreksi +40/−3 =87 serta kasus seluruh poin Discipline 47 tetap relatif 100. Seluruh hasil cocok. Ini verifikasi dokumen/simulasi, bukan tes aplikasi atau pengesahan parameter klub.

## 8. Penutupan klarifikasi penalti — 12 November 2026

Sumber pengguna: terlambat 11–20 Commitment −2; >20 Commitment −3; absen Discipline −15/Commitment −12/Teamwork −5. Dua keputusan yang sebelumnya tercantum pada bagian 7.3 **selesai**, bukan lagi pekerjaan tertunda. Rencana v2.9 dan kelima dokumen v2.3 diselaraskan; AC-117/AC-118 tetap ID yang sama dengan hasil eksplisit, tanpa perubahan jumlah requirement, flow, layar atau gambar.

| Keputusan | Pengujian acuan |
|---|---|
| Terlambat 11–20 menit | D−4/C−2; dari 50 menjadi 46/48, overall profil 49,70 tanpa perubahan lain |
| Terlambat >20 menit | D−6/C−3; dari 50 menjadi 44/47, overall profil 49,55 tanpa perubahan lain |
| Tidak hadir tanpa alasan mendesak | D−15/C−12/TW−5; dari 50 menjadi 35/38/45, overall profil 48,40; tidak ditambah tiga penalti melewatkan laga |

Perubahan numerik tidak menghapus kewajiban bukti/alasan, admin berhak, anti duplikasi atau hak koreksi. Pemain absen tetap dikecualikan dari pembanding event, tetapi potongan profil berlaku. Gambar aktif v4 tetap relevan dan tidak perlu digenerasi ulang; spesifikasi panel SCR-19 yang menetapkan nominal.

Audit revisi lulus 12 November 2026: tiga baris penalti sama pada Plan/Details; overall 49,70/49,55/48,40 sesuai potongan; struktur tetap 28 fitur/33 story/130 AC/26 flow/22 state/36 layar, cakupan AC/rujukan/versi sumber/tautan/anchor/tabel/UTF-8 dan dimensi 12 gambar aktif lulus. Ini bukan pengujian aplikasi atau perubahan aturan historis otomatis. Sisa tertunda hanya konten autentik, layanan/kebijakan operasional dan kalibrasi/validasi produksi.

## 9. Review dan visualisasi hanya Spider — 12 November 2026

Keputusan terbaru pengguna menghapus opsi bar chart. AC-131–AC-133 memperluas US-29/US-30 di FEAT-10/28, FLOW-22/FLOW-23 dan SCR-12/SCR-24 dengan ID tetap. Tidak ada story/flow/layar baru; total tetap 133 AC. Nilai, normalisasi, profil akumulasi, penalti dan privasi tidak berubah. Pemilih/preferensi jenis grafik dihapus dari persyaratan dan referensi aktif.

| Skenario | Hasil yang disyaratkan | Relasi |
|---|---|---|
| Membuka statistik | Hanya Spider pada Event/Profil, tanpa pemilih jenis grafik, tabel angka tersedia | AC-131; FLOW-22/FLOW-23; SCR-12/SCR-24 |
| Beralih tab/event | Seri/label mengikuti konteks, event terpilih dipertahankan saat kembali dari Profil, filter tidak mengubah karier atau izin | AC-132; FLOW-23; SCR-12/SCR-24 |
| Sumbu/legenda/data kosong | Sembilan kategori berurutan dan skala 0–100, angka aksesibel, nol bukan data hilang, poin mentah >100 hanya pada rincian | AC-133; FLOW-22; SCR-12/SCR-24 |

Review menemukan ketidaksinkronan kebutuhan dan mockup yang masih menawarkan pemilih grafik, serta preferensi mode yang sudah tidak diperlukan. Paket v2.5 menyelesaikannya lewat simplifikasi kontrol, bukan perubahan perhitungan. Aturan finalisasi/replay, sumber unik, verifikasi penalti dan privasi sudah terhubung ke alur/state terkait; belum diuji pada aplikasi. Kalibrasi proksi kebobolan/bobot posisi, konten autentik dan validasi produksi tetap terbuka.

Audit revisi lulus: 28 fitur, 33 story, 133 AC, 26 flow, 22 state dan 36 layar unik; seluruh ID/AC terpetakan, versi/tautan/anchor/tabel/UTF-8 valid, dan dimensi 12 PNG aktif sesuai manifest v6. Grafik pengganti diperiksa visual: sembilan sumbu dan legenda benar, pemilih jenis grafik tidak ada. Pemeriksaan ini atas dokumentasi dan referensi visual, bukan pengujian aplikasi.

## 10. Revisi loyalitas — 14 November 2026

Otorisasi: pengguna meminta penambahan klaim reward/poin anggota yang sering main dengan loyalitas terpisah dari statistik, menggunakan srs-documentation. Rencana v2.12 dan lima dokumen v2.6 menyelaraskan fitur tanpa mengubah rumus atau katalog statistik. Matriks berikut melengkapi matriks utama; ID lama tetap.

| Goal/pain | Fitur/fase | Story | AC | Flow/status | Layar | Gambar/status |
|---|---|---|---|---|---|---|---|
| GOAL-06 / PAIN-06 | FEAT-29 / MVP diperluas | US-34 | AC-134–AC-139 | FLOW-27 / STATE-23/STATE-24 | SCR-19/SCR-37/SCR-38 | Spesifikasi teks; kredit sekali dan koreksi independen |
| GOAL-06 / PAIN-06 | FEAT-29 / MVP diperluas | US-35 | AC-140–AC-146 | FLOW-28 / STATE-25 | SCR-37 | Spesifikasi teks; saldo/stok direservasi bersama |
| GOAL-06 / PAIN-06 | FEAT-29 / MVP diperluas | US-36 | AC-147–AC-153 | FLOW-29 / STATE-23/STATE-25 | SCR-13/SCR-38/SCR-22 | Spesifikasi teks; delegasi, pemenuhan dan rekonsiliasi |

Tambahan NFR: privasi saldo/klaim mengikuti NFR-02, aksesibilitas/HP mengikuti NFR-03/04, audit mengikuti NFR-07 dan pemulihan simpan mengikuti NFR-08. Konsistensi reservasi/sumber perlu diuji bersama transaksi bersamaan; dokumen tidak mengklaim uji aplikasi telah berjalan.

| Skenario penerimaan | Hasil yang disyaratkan | AC |
|---|---|---|
| Saldo awal dan statistik | Loyalitas 0; sembilan kategori/baseline 50 dan overall tidak berubah | AC-134 |
| Main terverifikasi tanpa gol/assist; beberapa laga/event | Kredit tetap sekali/event, tidak berbasis performa | AC-135–AC-137 |
| RSVP saja, tamu, event batal/sebelum aktivasi, sumber parsial | Tidak ada kredit; sumber parsial menunggu | AC-136 |
| Koreksi presensi sebelum/sesudah pemenuhan | Klaim Ditahan atau selisih negatif tercatat; tanpa tagihan uang | AC-139, AC-150 |
| Dua klaim berebut poin/stok terakhir | Tidak ada reservasi sebagian/double spend | AC-141–AC-143 |
| Pembatalan versus pemenuhan bersamaan | Hanya satu transisi sah, reservasi dilepas/debit sekali | AC-144, AC-149, AC-151 |
| Harga berubah/program dijeda/akun nonaktif | Klaim baru mengikuti syarat terbaru, ketentuan klaim lama dipertahankan | AC-145–AC-148 |
| Pengelola event mencoba kelola reward tanpa delegasi | Ditolak meskipun berhak verifikasi presensi | AC-138, AC-147, AC-151 |
| Timeout lalu reload/retry | Pulihkan aksi/klaim semula tanpa penyerahan/debit kedua | AC-143, AC-149, AC-153 |
| Gratis main/merchandise | Manfaat tercatat; tidak otomatis mengubah tagihan/kas/toko atau slot RSVP | AC-152 |
| Manfaat dikembalikan sesudah Dipenuhi | Pengembalian poin sekali, stok hanya bila dapat dipakai lagi, histori tetap | AC-153 |

Keputusan belum ditutup: pengesahan baseline operasional, nominal kredit, hadiah nyata/harga/stok/anggaran, ketentuan manfaat, tanggal/zona dan petugas. Semua menahan aktivasi program, bukan penulisan bagian independen. Tidak ada ambang "10 kali main" yang dianggap keputusan pengguna. Dua layar baru hanya spesifikasi teks; 12 gambar aktif lama tidak diklaim menggambarkan reward. Paket tetap mempunyai pekerjaan produksi/konten terdahulu.

### Pemeriksaan aktual revisi

Hasil pemeriksaan struktural dicatat setelah verifikasi berkas; skenario di atas merupakan rencana uji produk, bukan hasil runtime.

Pemeriksaan berkas 14 November 2026: 29 ID fitur, 36 story, 153 definisi AC unik, 29 flow, 25 state dan 38 layar berurutan tanpa celah; 20 AC baru tercakup pada Details/Mockup/Traceability. Target tautan lokal dan anchor bagian baru valid, berkas UTF-8 terbaca. Lima contoh saldo/reservasi pada Details cocok secara aritmetis. Pemeriksaan Problems editor tidak menemukan error. Gambar lama tidak diubah atau diaudit ulang; tidak menjalankan aplikasi/UAT. Revisi dokumentasi selesai, parameter aktivasi dan pekerjaan produksi tetap terbuka.

## 11. Penyelesaian sampai mockup SRS

Kelanjutan pengguna mengotorisasi dua gambar loyalitas, menggantikan batas teks saja pada revisi sebelumnya. Rencana v2.13 dan paket v2.7 mempertahankan 29 fitur, 36 story, 153 AC, 29 flow, 25 state dan 38 layar. Total referensi aktif kini 14 pada [manifest-v7](./mockups/manifest-v7.json).

| Kebutuhan | Gambar/perilaku diperiksa | Batas |
|---|---|---|
| US-34/US-35, AC-134/AC-138/AC-140/AC-141 | SCR-37: saldo/reservasi/tersedia, pemisahan statistik, harga/stok/periode, CTA cukup/kurang dan privasi | State alternatif dan modal mengikuti teks, nominal tetap sintetis |
| US-36, AC-149/AC-150/AC-152 | SCR-38: antrean tiga status, detail Menunggu, konfirmasi pemenuhan/catatan/waktu dan audit; batas tagihan/RSVP | Bukan hasil transaksi nyata; koreksi mengikuti teks |

Inspeksi visual dilakukan pada kedua keluaran; prompt aktual, dimensi PNG dan keterbatasan dicatat di Mockup bagian 11/manifest. Dua belas gambar lama dipertahankan tanpa klaim audit visual baru. Dokumen sampai mockup selesai; kebijakan/nominal program dan kesiapan produksi tetap terbuka.

Audit berkas kelanjutan: enam dokumen UTF-8 dan seluruh target tautan lokal valid; 14 PNG ada dan dimensi cocok manifest; 153 definisi AC tetap unik. Saldo contoh 50-20=30 dan 50-30=20 diperiksa; harga jersey 80 minus tersedia 30 = kekurangan 50. Tidak ada aplikasi yang diuji.

## 12. Revisi profil, peran dan penghargaan tahunan — v2.8

Otorisasi: pengguna menghapus peran Kapten dan meminta kelanjutan hingga Mockup SRS. Rencana v2.16 menyelaraskan lima dokumen v2.8 dan referensi gambar statis. Semua angka/penerima pada mockup sintetis; aturan tahunan belum aktif.

| Kebutuhan | Fitur / story / AC | Alur / state | Layar |
|---|---|---|---|
| Kustomisasi Member dan pemeliharaan klub/pemain | FEAT-01/02; US-37; AC-154/155/157 | FLOW-01/02/19 | SCR-12/21/35 |
| Tanpa Kapten, persetujuan roster pribadi | FEAT-02/04/05; US-37; AC-156, AC-13/18 | FLOW-02/04/05/21 | SCR-06/07/15/21 |
| Privasi tagihan tim | FEAT-11; US-37; AC-158 | FLOW-11 | SCR-20 |
| Poin tahunan terpisah dari saldo | FEAT-30; US-38; AC-159–162 | FLOW-30; STATE-26 | SCR-39/12/24/37 |
| Penghargaan dari hasil sistem dan koreksi | FEAT-30; US-39; AC-163–166 | FLOW-31; STATE-26 | SCR-40/39/22 |

| Skenario pemeriksaan rancangan | Hasil yang diwajibkan |
|---|---|
| Member mengedit orang lain/field resmi; Admin mengirim rating langsung | Ditolak tanpa perubahan parsial |
| Pemilik hak Kapten lama mengundang pemain | Ditolak; tidak otomatis menjadi Admin |
| Admin event mengubah profil klub/publikasi penghargaan tanpa delegasi | Ditolak |
| Pemilik tagihan berganti | Hak bukti historis ditinjau, tidak dibuka otomatis |
| Perolehan tahunan 100, klaim 30, reservasi 20 | Poin tahunan tetap 100; saldo total 70 dan tersedia 50 |
| Koreksi kredit 10 dari tahun lalu | Peringkat tahun sumber direvisi; bukan pengurangan perolehan tahun berjalan |
| Tahun baru, batas zona dan sumber belum final | Sumber masuk tahun sesuai zona program; hasil belum final berlabel |
| Seri, tidak ada peserta layak, finalisasi serentak | Aturan seri tersimpan; tanpa penerima; satu versi aktif |
| Koreksi mengubah penerima setelah terbit | Versi lama berlabel, revisi diaudit, badge aktif diganti saat publikasi ulang |

Inventaris terkini: 30 fitur, 39 story, 166 AC, 31 flow, 26 state dan 40 layar. Pemeriksaan dokumen/gambar tidak sama dengan pengujian aplikasi atau API. Basis poin, nama penghargaan, kelayakan, seri, zona, tanggal penutupan, visibilitas serta hadiah tambahan masih menahan aktivasi tahunan. Tidak ada hasil uji produksi yang diklaim.

### Pemeriksaan aktual v2.8 — 14 November 2026

- Semua target berkas relatif pada enam dokumen ditemukan; `git diff --check` dan diagnostik editor tidak menemukan error.
- AC-154–166 lengkap dan unik. PRD memuat 160 definisi AC berbentuk bullet dan enam definisi lama AC-39–44 di tabel, sehingga total 166; FLOW-30/31, STATE-26 dan SCR-39/40 tersedia beserta relasinya.
- Referensi HTML sebelumnya diganti atas instruksi pengguna dengan satu PNG empat panel; HTML dihapus. Pemeriksaan interaksi browser sebelumnya hanya berlaku pada artefak lama, bukan gambar pengganti.
- Inspeksi visual PNG: empat panel lengkap, foto menggunakan inisial, data resmi dilabeli Dikelola Admin, rating dihitung sistem, peringkat 2026 Sementara dan finalisasi Belum aktif. Tidak ada klaim uji responsif atau interaksi pada gambar statis.
- Pengujian backend, otorisasi, koreksi atomik dan konkurensi tetap pekerjaan implementasi.

### Penyelarasan pembagian tim v2.9

UI Roster Admin dihapus dari gambar aktif. Alur berlaku: RSVP individu → pengacakan otomatis berbasis skill → pratinjau keseimbangan → simpan/kunci → lihat susunan. Referensi antartim, undangan tim, acak murni dan penempatan manual dalam baseline lama tidak berlaku untuk alur ini. ID lama dipertahankan untuk ketertelusuran; bagian yang digantikan bukan persyaratan aktif.
