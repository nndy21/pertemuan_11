# pertemuan_11

# Sistem Pendaftaran dan Pelayanan Pasien
**Universitas Dinamika — Latihan 7 (Array 1D & 2D)**

---

## Deskripsi Algoritma

Program ini dibuat berdasarkan algoritma deskriptif berikut:

> **Judul: Proses Pendaftaran dan Pelayanan Pasien**
>
> 1. Pasien datang ke loket pendaftaran.
> 2. Petugas meminta data diri pasien (nama, alamat, keluhan).
> 3. Jika pasien baru, petugas membuatkan kartu berobat dan rekam medis.
> 4. Jika pasien lama, petugas mencari data rekam medis yang sudah ada.
> 5. Petugas mencetak nomor antrean dan memberikan ke pasien.
> 6. Pasien menunggu panggilan untuk diperiksa oleh dokter.
> 7. Dokter memeriksa pasien dan memberikan diagnosa serta resep atau rujukan lab.
> 8. Jika perlu cek lab, pasien pergi ke bagian laboratorium untuk pengambilan sampel.
> 9. Pasien menunggu hasil lab, kemudian mengambil hasilnya.
> 10. Pasien membawa hasil dan resep ke apotek untuk menebus obat.
> 11. Apoteker menyiapkan obat dan menjelaskan cara pakai.
> 12. Pasien menerima obat dan proses selesai.

---

## Soal 1 — Variabel Diubah ke Array 1D dan 2D

Semua variabel dari algoritma UTS diubah ke bentuk array:

| Variabel Biasa (sebelum) | Array (sesudah) | Jenis |
|---|---|---|
| `nama_pasien` | `data_pasien[][0]` | Array 2D |
| `alamat_pasien` | `data_pasien[][1]` | Array 2D |
| `keluhan` | `data_pasien[][2]` | Array 2D |
| `status_pasien` | `data_pasien[][3]` | Array 2D |
| `diagnosa` | `data_pasien[][4]` | Array 2D |
| `resep_obat` | `data_pasien[][5]` | Array 2D |
| `no_antrean` | `antrean[]` | Array 1D |
| `hasil_laboratorium` | `hasil_lab[]` | Array 1D |

**Struktur Array 2D:**
```
data_pasien[0] = ['Budi', 'Jl. Merdeka', 'Demam', 'baru', 'Influenza', 'Paracetamol']
data_pasien[1] = ['Siti', 'Jl. Mawar',   'Batuk', 'lama', 'ISPA',      'Ambroxol'   ]
                   [0]      [1]            [2]      [3]     [4]           [5]
                  NAMA    ALAMAT        KELUHAN  STATUS  DIAGNOSA       OBAT
```

---

## Soal 2 — Deklarasi dan Inisialisasi (di Simbol Proses)

Setiap variabel ditandai komentar `# DEKLARASI` dan `# INISIALISASI` langsung pada kode:

```python
# DEKLARASI Array 1D
antrean   = []   # DEKLARASI: nomor antrean tiap pasien
hasil_lab = []   # DEKLARASI: hasil laboratorium tiap pasien

# DEKLARASI Array 2D
data_pasien = []  # DEKLARASI: semua data lengkap pasien

# DEKLARASI indeks kolom Array 2D
NAMA     = 0   # DEKLARASI kolom nama
ALAMAT   = 1   # DEKLARASI kolom alamat
KELUHAN  = 2   # DEKLARASI kolom keluhan
STATUS   = 3   # DEKLARASI kolom status (baru/lama)
DIAGNOSA = 4   # DEKLARASI kolom diagnosa
OBAT     = 5   # DEKLARASI kolom resep obat
```

Contoh inisialisasi di dalam fungsi proses:
```python
nama     = ""   # INISIALISASI
alamat   = ""   # INISIALISASI
diagnosa = "-"  # INISIALISASI (belum diisi dokter)
nomor_urut = len(antrean) + 1   # INISIALISASI
jumlah     = 0                  # INISIALISASI counter
```

---

## Soal 3a — Input

### 3a-1: Input dari User (Keyboard)

```python
def input_dari_user():
    nama    = input("Nama pasien             : ")
    alamat  = input("Alamat pasien           : ")
    keluhan = input("Keluhan                 : ")
    status  = input("Pasien (baru/lama)      : ").strip().lower()

    if status == "baru":
        print("[PROSES] Membuat kartu berobat dan rekam medis baru...")
    else:
        print("[PROSES] Mencari rekam medis pasien lama...")

    nomor_urut = len(antrean) + 1
    no_pasien  = f"P{nomor_urut:03d}"
    antrean.append(no_pasien)
    data_pasien.append([nama, alamat, keluhan, status, "-", "-"])
    ...
```

### 3a-2: Input dari File

Format file `data_pasien.txt`:
```
# Format: nama|alamat|keluhan|status|diagnosa|obat|hasil_lab
Budi Santoso|Jl. Merdeka 10|Demam tinggi|baru|Influenza|Paracetamol 500mg|Tidak perlu cek lab
Siti Rahayu|Jl. Mawar 5|Batuk pilek|lama|ISPA ringan|Ambroxol + Vit C|Tidak perlu cek lab
Andi Wijaya|Jl. Kenanga 3|Nyeri perut|baru|Gastritis|Antasida|Leukosit 12.000
Dewi Lestari|Jl. Dahlia 7|Pusing berputar|lama|Vertigo|Betahistine|Tidak perlu cek lab
```

```python
def input_dari_file(nama_file="data_pasien.txt"):
    with open(nama_file, "r") as f:
        semua_baris = f.readlines()
    for baris in semua_baris:
        baris = baris.strip()
        if baris == "" or baris.startswith("#"):
            continue
        kolom = baris.split("|")
        antrean.append(f"P{len(antrean)+1:03d}")
        data_pasien.append([kolom[0], kolom[1], kolom[2], kolom[3], kolom[4], kolom[5]])
        hasil_lab.append(f"P{len(antrean):03d}: {kolom[6]}")
```

---

## Soal 3b — Proses Menggunakan Operator

| Operator | Jenis | Digunakan Untuk |
|---|---|---|
| `+` | Aritmatika | Hitung nomor urut pasien, counter jumlah |
| `==` | Relasi | Cek status baru/lama, cek perlu lab |
| `<` | Relasi | Validasi jumlah kolom saat baca file |
| `or` | Logika | Lewati baris kosong atau komentar di file |
| `not in` | Logika | Cek pasien yang tidak perlu cek lab |

Contoh penggunaan di kode:
```python
# Operator + (aritmatika)
nomor_urut = len(antrean) + 1
jumlah     = jumlah + 1

# Operator == (relasi)
if status == "baru":
    ...

# Operator < (relasi)
if len(kolom) < 7:
    continue

# Operator or (logika)
if baris == "" or baris.startswith("#"):
    continue

# Operator not in (logika)
if "Tidak" not in h:
    jml_lab = jml_lab + 1
```

---

## Soal 3c — Output

### 3c-1: Output ke Console

```python
def output_ke_console():
    # Array 1D
    for i in range(len(antrean)):
        print(f"  antrean[{i}] = {antrean[i]}")

    for i in range(len(hasil_lab)):
        print(f"  hasil_lab[{i}] = {hasil_lab[i]}")

    # Array 2D
    for i in range(len(data_pasien)):
        baris = data_pasien[i]
        print(f"  {antrean[i]:<6}{baris[NAMA]:<17}{baris[STATUS]:<7}"
              f"{baris[KELUHAN]:<18}{baris[DIAGNOSA]}")
```

Contoh tampilan output console:
```
[ARRAY 1D] Nomor Antrean Pasien:
  antrean[0] = P001
  antrean[1] = P002

[ARRAY 1D] Hasil Laboratorium:
  hasil_lab[0] = P001: Tidak perlu cek lab
  hasil_lab[1] = P002: Leukosit 12.000

[ARRAY 2D] Data Lengkap Pasien:
------------------------------------------------------------
  No    Nama             Status Keluhan           Diagnosa
------------------------------------------------------------
  P001  Budi Santoso     baru   Demam tinggi      Influenza
  P002  Andi Wijaya      baru   Nyeri perut       Gastritis
------------------------------------------------------------
  Total pasien terdaftar : 2
  Pasien baru            : 2
  Pasien lama            : 0
  Pasien perlu cek lab   : 1
```

### 3c-2: Output ke File

```python
def output_ke_file(nama_file="laporan_pasien.txt"):
    with open(nama_file, "w") as f:
        f.write("[ARRAY 1D] Nomor Antrean Pasien:\n")
        for i in range(len(antrean)):
            f.write(f"  antrean[{i}] = {antrean[i]}\n")
        f.write("\n[ARRAY 2D] Data Lengkap Pasien:\n")
        for i in range(len(data_pasien)):
            baris = data_pasien[i]
            f.write(f"  {antrean[i]:<6}{baris[NAMA]:<17}{baris[STATUS]:<7}"
                    f"{baris[KELUHAN]:<18}{baris[DIAGNOSA]}\n")
```

---

## Kode Program Lengkap

```python
# ============================================================
#  PROGRAM  : Sistem Pendaftaran dan Pelayanan Pasien
#  ALGORITMA: Berdasarkan UTS - Proses Pendaftaran Pasien
#  LATIHAN  : Soal 1, 2, 3a, 3b, 3c
# ============================================================

import os


# ==============================================================
# SOAL 1 - DEKLARASI VARIABEL MENGGUNAKAN ARRAY 1D DAN 2D
# ==============================================================

# ----- DEKLARASI Array 1D -----
antrean   = []   # DEKLARASI: nomor antrean tiap pasien
hasil_lab = []   # DEKLARASI: hasil laboratorium tiap pasien

# ----- DEKLARASI Array 2D -----
data_pasien = []  # DEKLARASI: semua data lengkap pasien

# ----- DEKLARASI indeks kolom Array 2D -----
NAMA     = 0   # DEKLARASI kolom nama
ALAMAT   = 1   # DEKLARASI kolom alamat
KELUHAN  = 2   # DEKLARASI kolom keluhan
STATUS   = 3   # DEKLARASI kolom status (baru/lama)
DIAGNOSA = 4   # DEKLARASI kolom diagnosa
OBAT     = 5   # DEKLARASI kolom resep obat


# ==============================================================
# SOAL 3A-1 : INPUT DARI USER
# ==============================================================

def input_dari_user():
    print("\n" + "="*55)
    print("   INPUT DATA PASIEN - Dari User (Keyboard)")
    print("="*55)

    # SOAL 2 - INISIALISASI variabel sebelum digunakan
    nama     = ""   # INISIALISASI
    alamat   = ""   # INISIALISASI
    keluhan  = ""   # INISIALISASI
    status   = ""   # INISIALISASI
    diagnosa = "-"  # INISIALISASI (belum diisi dokter)
    obat     = "-"  # INISIALISASI (belum diisi dokter)

    nama    = input("Nama pasien             : ")
    alamat  = input("Alamat pasien           : ")
    keluhan = input("Keluhan                 : ")
    status  = input("Pasien (baru/lama)      : ").strip().lower()

    # SOAL 3B - PROSES: operator == (relasi)
    if status == "baru":
        print("[PROSES] Membuat kartu berobat dan rekam medis baru...")
    else:
        print("[PROSES] Mencari rekam medis pasien lama...")

    # SOAL 2 - INISIALISASI | SOAL 3B - operator + (aritmatika)
    nomor_urut = len(antrean) + 1          # INISIALISASI
    no_pasien  = f"P{nomor_urut:03d}"      # INISIALISASI

    antrean.append(no_pasien)
    print(f"[PROSES] Nomor antrean dicetak: {no_pasien}")

    data_pasien.append([nama, alamat, keluhan, status, diagnosa, obat])

    print("\n--- Pemeriksaan Dokter ---")
    idx = len(data_pasien) - 1             # INISIALISASI indeks baris terakhir
    data_pasien[idx][DIAGNOSA] = input("Diagnosa dokter         : ")
    data_pasien[idx][OBAT]     = input("Resep obat              : ")

    # SOAL 3B - PROSES: operator == (relasi)
    cek_lab = input("Perlu cek lab? (ya/tidak): ").strip().lower()
    if cek_lab == "ya":
        hasil = ""   # INISIALISASI
        hasil = input("Hasil laboratorium      : ")
        hasil_lab.append(f"{no_pasien}: {hasil}")
    else:
        hasil_lab.append(f"{no_pasien}: Tidak perlu cek lab")

    print(f"\n[SELESAI] Data pasien '{nama}' berhasil disimpan.")
    return no_pasien


# ==============================================================
# SOAL 3A-2 : INPUT DARI FILE
# ==============================================================

def input_dari_file(nama_file="data_pasien.txt"):
    print(f"\n[INPUT FILE] Membaca dari: {nama_file}")

    if not os.path.exists(nama_file):
        print("[INFO] File tidak ditemukan. Membuat file contoh...")
        buat_file_contoh(nama_file)

    with open(nama_file, "r") as f:
        semua_baris = f.readlines()   # INISIALISASI: tampung isi file

    jumlah = 0   # INISIALISASI

    for baris in semua_baris:
        baris = baris.strip()

        # SOAL 3B - PROSES: operator or (logika)
        if baris == "" or baris.startswith("#"):
            continue

        kolom = baris.split("|")

        # SOAL 3B - PROSES: operator < (relasi)
        if len(kolom) < 7:
            print(f"  [SKIP] Format tidak lengkap: {baris}")
            continue

        # SOAL 3B - PROSES: operator + (aritmatika)
        nomor_urut = len(antrean) + 1     # INISIALISASI
        no_pasien  = f"P{nomor_urut:03d}" # INISIALISASI

        antrean.append(no_pasien)
        hasil_lab.append(f"{no_pasien}: {kolom[6].strip()}")

        data_pasien.append([
            kolom[0].strip(),   # -> data_pasien[][NAMA]
            kolom[1].strip(),   # -> data_pasien[][ALAMAT]
            kolom[2].strip(),   # -> data_pasien[][KELUHAN]
            kolom[3].strip(),   # -> data_pasien[][STATUS]
            kolom[4].strip(),   # -> data_pasien[][DIAGNOSA]
            kolom[5].strip(),   # -> data_pasien[][OBAT]
        ])

        jumlah = jumlah + 1   # SOAL 3B - operator +

    print(f"[SELESAI] {jumlah} data pasien berhasil dimuat dari file.")


def buat_file_contoh(nama_file):
    isi = (
        "# Format: nama|alamat|keluhan|status|diagnosa|obat|hasil_lab\n"
        "Budi Santoso|Jl. Merdeka 10|Demam tinggi|baru|Influenza|Paracetamol 500mg|Tidak perlu cek lab\n"
        "Siti Rahayu|Jl. Mawar 5|Batuk pilek|lama|ISPA ringan|Ambroxol + Vit C|Tidak perlu cek lab\n"
        "Andi Wijaya|Jl. Kenanga 3|Nyeri perut|baru|Gastritis|Antasida|Leukosit 12.000\n"
        "Dewi Lestari|Jl. Dahlia 7|Pusing berputar|lama|Vertigo|Betahistine|Tidak perlu cek lab\n"
    )
    with open(nama_file, "w") as f:
        f.write(isi)
    print(f"[INFO] File contoh '{nama_file}' berhasil dibuat.")


# ==============================================================
# SOAL 3C-1 : OUTPUT KE CONSOLE
# ==============================================================

def output_ke_console():
    print("\n" + "="*60)
    print("       OUTPUT DATA PASIEN - Ke Console")
    print("="*60)

    print("\n[ARRAY 1D] Nomor Antrean Pasien:")
    for i in range(len(antrean)):
        print(f"  antrean[{i}] = {antrean[i]}")

    print("\n[ARRAY 1D] Hasil Laboratorium:")
    for i in range(len(hasil_lab)):
        print(f"  hasil_lab[{i}] = {hasil_lab[i]}")

    print("\n[ARRAY 2D] Data Lengkap Pasien:")
    print("-"*60)
    print(f"  {'No':<6}{'Nama':<17}{'Status':<7}{'Keluhan':<18}{'Diagnosa'}")
    print("-"*60)
    for i in range(len(data_pasien)):
        baris = data_pasien[i]
        print(f"  {antrean[i]:<6}{baris[NAMA]:<17}{baris[STATUS]:<7}"
              f"{baris[KELUHAN]:<18}{baris[DIAGNOSA]}")
    print("-"*60)

    # SOAL 3B - operator + dan ==
    total    = len(antrean)
    jml_baru = 0   # INISIALISASI
    jml_lama = 0   # INISIALISASI
    jml_lab  = 0   # INISIALISASI

    for p in data_pasien:
        if p[STATUS] == "baru":
            jml_baru = jml_baru + 1
        else:
            jml_lama = jml_lama + 1

    for h in hasil_lab:
        if "Tidak" not in h:
            jml_lab = jml_lab + 1

    print(f"  Total pasien terdaftar : {total}")
    print(f"  Pasien baru            : {jml_baru}")
    print(f"  Pasien lama            : {jml_lama}")
    print(f"  Pasien perlu cek lab   : {jml_lab}")


# ==============================================================
# SOAL 3C-2 : OUTPUT KE FILE
# ==============================================================

def output_ke_file(nama_file="laporan_pasien.txt"):
    with open(nama_file, "w") as f:
        f.write("="*60 + "\n")
        f.write("       OUTPUT DATA PASIEN - Ke File\n")
        f.write("="*60 + "\n\n")

        f.write("[ARRAY 1D] Nomor Antrean Pasien:\n")
        for i in range(len(antrean)):
            f.write(f"  antrean[{i}] = {antrean[i]}\n")

        f.write("\n[ARRAY 1D] Hasil Laboratorium:\n")
        for i in range(len(hasil_lab)):
            f.write(f"  hasil_lab[{i}] = {hasil_lab[i]}\n")

        f.write("\n[ARRAY 2D] Data Lengkap Pasien:\n")
        f.write("-"*60 + "\n")
        f.write(f"  {'No':<6}{'Nama':<17}{'Status':<7}{'Keluhan':<18}{'Diagnosa'}\n")
        f.write("-"*60 + "\n")
        for i in range(len(data_pasien)):
            baris = data_pasien[i]
            f.write(f"  {antrean[i]:<6}{baris[NAMA]:<17}{baris[STATUS]:<7}"
                    f"{baris[KELUHAN]:<18}{baris[DIAGNOSA]}\n")
        f.write("-"*60 + "\n")

        total    = len(antrean)
        jml_baru = 0
        jml_lama = 0
        jml_lab  = 0
        for p in data_pasien:
            if p[STATUS] == "baru":
                jml_baru = jml_baru + 1
            else:
                jml_lama = jml_lama + 1
        for h in hasil_lab:
            if "Tidak" not in h:
                jml_lab = jml_lab + 1

        f.write(f"\n  Total pasien terdaftar : {total}\n")
        f.write(f"  Pasien baru            : {jml_baru}\n")
        f.write(f"  Pasien lama            : {jml_lama}\n")
        f.write(f"  Pasien perlu cek lab   : {jml_lab}\n")

    print(f"[SELESAI] Laporan berhasil disimpan ke '{nama_file}'")


# ==============================================================
# PROGRAM UTAMA
# ==============================================================

def main():
    print("\n" + "="*55)
    print("   SISTEM PENDAFTARAN DAN PELAYANAN PASIEN")
    print("   Universitas Dinamika")
    print("="*55)

    while True:
        print("\n--- MENU UTAMA ---")
        print("  1. Input dari user    (Soal 3a-1)")
        print("  2. Input dari file    (Soal 3a-2)")
        print("  3. Output ke console  (Soal 3c-1)")
        print("  4. Output ke file     (Soal 3c-2)")
        print("  5. Keluar")

        pilihan = input("\nPilihan (1-5): ").strip()

        if pilihan == "1":
            input_dari_user()
        elif pilihan == "2":
            nama_file = input("Nama file (Enter = data_pasien.txt): ").strip()
            if nama_file == "":
                nama_file = "data_pasien.txt"
            input_dari_file(nama_file)
        elif pilihan == "3":
            if len(antrean) == 0:
                print("[INFO] Belum ada data. Lakukan input terlebih dahulu.")
            else:
                output_ke_console()
        elif pilihan == "4":
            if len(antrean) == 0:
                print("[INFO] Belum ada data. Lakukan input terlebih dahulu.")
            else:
                nama_file = input("Nama file output (Enter = laporan_pasien.txt): ").strip()
                if nama_file == "":
                    nama_file = "laporan_pasien.txt"
                output_ke_file(nama_file)
        elif pilihan == "5":
            print("\nProgram selesai.")
            break
        else:
            print("[ERROR] Pilihan tidak valid.")


if __name__ == "__main__":
    main()
```

---

## Cara Menjalankan

```bash
python pendaftaran_pasien.py
```

Urutan penggunaan yang disarankan:
1. Pilih `2` — load data dari file contoh (dibuat otomatis)
2. Pilih `3` — lihat output di console
3. Pilih `4` — simpan laporan ke file
4. Pilih `1` — coba input manual dari keyboard
