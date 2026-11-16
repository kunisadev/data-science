# Cheat Sheet: Anatomi Tanda & Logika Sintaks Pandas

Panduan ringkas memahami tanda baca, urutan *method chaining*, dan fungsi agregasi pada Python.

```python
df.groupby('petugas')['omzet_harian'].agg(q1=lambda x: np.percentile(x, 25))
```

---

## 1. Fungsi Tanda-Tanda pada Sintaks

| Tanda | Nama | Fungsi dalam Kode |
| :--- | :--- | :--- |
| `.` | *Dot Operator* | Menyambungkan objek dengan perintah/fungsi miliknya (misal: `df` memanggil fungsi `.groupby()`). |
| `(...)` | *Parentheses* | Mengurung argumen atau parameter input yang dimasukkan ke dalam sebuah fungsi/perintah (kata kerja). Hanya digunakan untuk menjalankan fungsi atau perintah seperti `print()`, `.groupby()`, dll. |
| `[...]` | *Square Brackets* | Memilih, mengiris, atau mengakses nama kolom tertentu dari dalam sebuah DataFrame (`df['nama_kolom']`). Untuk mengambil isi dari objek penyimpanan data (baik itu list, dictionary, maupun DataFrame) wajib menggunakan tanda ini. Untuk ambil 2 kolom: `df[['petugas', 'omzet']]`. |
| `'...'` | *Single Quotes* | Menandai bahwa teks di dalamnya adalah **String** (nama kolom atau nama fungsi bawaan Pandas), bukan variabel. |
| `=` | *Assignment / Named Agg* | Di dalam `.agg()` memberi nama kolom output (`nama_baru = 'fungsi'`). Di luar fungsi menyimpan hasil ke variabel. |
| `:` | *Colon (Lambda)* | Pemisah antara parameter input (`x`) dan rumus operasi (`np.percentile(...)`) pada fungsi anonim `lambda`. |

---

## 2. Mengapa `q1` dan `q3` Memakai `lambda x:`?

* **Fungsi Bawaan Sederhana:** Perintah seperti `'count'`, `'mean'`, `'median'`, dan `'std'` sudah dikenali langsung oleh Pandas cukup dengan menulis namanya di dalam tanda kutip.
* **Fungsi Eksternal dengan Parameter:** Fungsi kuartil dari NumPy (`np.percentile`) membutuhkan **dua input** (sekumpulan data dan angka persentil `25` atau `75`), sehingga tidak bisa ditulis hanya dengan teks kutip.
* **Peran `lambda x:`:** Bertindak sebagai fungsi sementara tanpa nama (*anonymous function*) yang menangkap deretan angka omzet milik tiap petugas sebagai `x`, lalu meneruskannya ke rumus `np.percentile(x, 25)`.

---

## 3. Grammar & Urutan `df.groupby('petugas')['omzet_harian'].agg(...)`

1. **Kenapa `'petugas'` pakai `(...)` bukan `[...]`?**  
   `.groupby()` adalah **kata kerja** (perintah), sehingga inputnya wajib masuk ke kurung fungsi `(...)`. Sebaliknya, `['omzet_harian']` hanya **mengambil bagian** (*selecting*), sehingga memakai kurung siku `[...]`.
2. **Kenapa `'omzet_harian'` ditaruh di belakang `.groupby()`?**  
   Data dikelompokkan dulu seluruhnya berdasarkan petugas, baru kemudian disaring hanya kolom targetnya saja agar kolom lain (seperti `id_usaha`) tidak ikut dihitung.
3. **Kenapa `.agg()` ditaruh setelah `.groupby()`?**  
   `.agg()` adalah perintah lanjutan yang membutuhkan kelompok terlebih dahulu. Output dari langkah sebelah kiri langsung menjadi input bagi langkah sebelah kanannya (*Method Chaining*).

**Alur Eksekusi (*Method Chaining*):**  
`[Tabel Utuh]` $\rightarrow$ `.groupby('petugas')` $\rightarrow$ `['omzet_harian']` $\rightarrow$ `.agg(...)` $\rightarrow$ `[Tabel Ringkasan]`

---

## 4. Cara Baca Sintaks dalam Bahasa Manusia

> **`df.groupby('petugas')`**  
> "Dari tabel **df**, kelompokkan baris-baris datanya berdasarkan nilai yang sama di kolom **'petugas'**."