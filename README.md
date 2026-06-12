# Surat Penagihan SRR — Generator

Aplikasi Streamlit untuk membuat surat penagihan KJPP Suwendho Rinaldy dan Rekan.

## Struktur File

```
├── app.py                   ← Aplikasi utama
├── requirements.txt         ← Dependensi Python
├── .streamlit/
│   └── config.toml          ← Konfigurasi tema & server
└── README.md
```

## Fitur

- Ambil data pemberi tugas dari Google Sheets secara otomatis
- Upload CSV / Excel sebagai alternatif sumber data
- Dropdown pemberi tugas (diurutkan A–Z) + pemilihan bertingkat per No. Proposal
- Upload template surat penagihan (`.docx`)
- Pengisian otomatis semua placeholder `{{...}}`
- Kalkulasi otomatis Fee, DPP, PPN, Jumlah
- Konversi angka ke terbilang Bahasa Indonesia
- Preview surat sebelum generate
- Simpan sementara beberapa dokumen sekaligus
- Download individual atau ZIP semua dokumen

---

## Deploy ke Streamlit Cloud

1. Push folder ini ke GitHub repository (semua file termasuk `.streamlit/config.toml`)
2. Buka https://streamlit.io/cloud → **New app**
3. Pilih repo → branch → set `app.py` sebagai entrypoint
4. Klik **Deploy**

> **Catatan:** Streamlit Cloud otomatis membaca `requirements.txt` untuk menginstal dependensi.

---

## Jalankan Lokal

```bash
pip install -r requirements.txt
streamlit run app.py
```

---

## Placeholder Template

| Placeholder | Sumber |
|---|---|
| `{{Nomor_Srt}}` | Auto: `YYMMDD.NNN` |
| `{{Kode_PT}}` | Input manual |
| `{{Tgl_Srt}}` | Input tanggal |
| `{{pemberi_tugas}}` | Google Sheets / file |
| `{{alamat_1}}`, `{{alamat_2}}` | Google Sheets / file |
| `{{kota}}`, `{{kode_pos}}` | Google Sheets / file |
| `{{up}}` | Google Sheets / file |
| `{{penugasan}}` / `{{Penugasan}}` / `{{PENUGASAN}}` | Google Sheets / file (3 varian casing) |
| `{{tagih_ke}}` / `{{Tagih_ke}}` / `{{TAGIH_KE}}` | Input manual (3 varian casing) |
| `{{no_proposal}}` | Google Sheets / file |
| `{{tanggal_proposal}}` | Google Sheets / file |
| `{{proposed_fee}}` | Google Sheets / file |
| `{{persentase}}` | Input manual |
| `{{Fee_Tagih}}` | Auto: `proposed_fee × persentase` |
| `{{DPP}}` | Auto: `Fee × 11/12` |
| `{{PPN}}` | Auto: `12% × DPP` |
| `{{Jumlah}}` | Auto: `Fee + PPN` |
| `{{Jumlah_Terbilang}}` | Auto: terbilang(Jumlah) |
| `{{Bank}}` | Dropdown |
| `{{Norek}}` | Dropdown |
| `{{title_Up}}` / `{{title_up}}` | Input manual |

---

## Kolom Google Sheets / File yang Dibutuhkan

| Kolom (nama di header, case-insensitive) | Keterangan |
|---|---|
| `pemberi_tugas` | **Wajib** — nama klien |
| `alamat_1`, `alamat_2` | Baris alamat |
| `kota`, `kode_pos` | Kota & kode pos |
| `up` | Nama u.p. |
| `penugasan` | Jenis penugasan |
| `no_proposal` | Nomor proposal |
| `tanggal_proposal` | Tanggal proposal |
| `proposed_fee` | Angka fee proposal (numerik) |
| `nama_file` | *(Opsional)* untuk pemilihan bertingkat |

---

## Rekening Bank

| Label | Bank | Nomor Rekening |
|---|---|---|
| Bank Mandiri | Bank Mandiri KCP JKT Kalibata Rawajati | 126-0005748719 |
| Bank JTrust | Bank JTrust Indonesia | 1001883933 |
| BRI | Bank Rakyat Indonesia Jkt Kalibata | 042601000618306 |
| BNI | BNI, Kantor Cabang Tebet | 0981981462 |

> Untuk mengubah rekening, edit konstanta `BANK_OPTIONS` di `app.py`.
