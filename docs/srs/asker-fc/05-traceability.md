# Traceability dan Pemeriksaan — Asker FC

- Versi: 2.1 · Tanggal: 10 November 2026
- Status: matriks kebutuhan revisi tersusun; hasil pemeriksaan aktual dicatat pada bagian 4, terpisah dari uji aplikasi dan kesiapan konten produksi.
- Upstream: [rencana v2.0](./00-implementation-plan.md), [Planning v2.1](./01-product-planning.md), [PRD v2.1](./02-prd.md), [Details v2.1](./03-product-details.md), [Mockup SRS v2.1](./04-mockup-srs.md).
- Dasar otorisasi: instruksi pengguna untuk memproses implementation plan yang masih tertunda setelah revisi v1.8. Tidak menjalankan implementasi aplikasi, pengiriman pesan atau deployment.

## 1. Matriks kebutuhan

ID lama dipertahankan. FEAT-15/FEAT-16 dipromosikan ke MVP; statistik lanjutan yang semula tergabung dipisah menjadi FEAT-28. Iuran bulanan manual FEAT-23 dipisah dari penerbitan tagihan otomatis FEAT-14. Rentang AC pada tabel mencakup semua nomor di antaranya. Semua definisi AC berada di [PRD](./02-prd.md), alur di [Details](./03-product-details.md), layar/gambar di [Mockup SRS](./04-mockup-srs.md).

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
| GOAL-01 / PAIN-01 | FEAT-25 / MVP | US-25 | AC-71–AC-73 | FLOW-20 / preferensi UI | Semua; khusus SCR-12, SCR-33 | 12 referensi v3 gelap hitam–gold; tema terang dispesifikasikan | Tema tidak mengubah status bisnis |
| GOAL-01 / PAIN-01 | FEAT-26 / MVP | US-26 | AC-74–AC-76 | FLOW-21,FLOW-04 / STATE-05,STATE-06 | SCR-05,SCR-06,SCR-07,SCR-15,SCR-21 | SCR-05 mobile;roster/tamu teks | Akun tamu terbatas satu kegiatan |
| GOAL-04 / PAIN-04 | FEAT-27 / Opsional nonaktif | US-27 | AC-77 | Belum diaktifkan | SCR-36 kondisional | Tidak dijadwalkan | Katalog/pesan usulan; tanpa stok/checkout |
| GOAL-04 / PAIN-04 | FEAT-28 / Lanjutan | US-28 | AC-78 | Belum dijadwalkan | — | Tidak dijadwalkan | Statistik di luar kebutuhan dasar |

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