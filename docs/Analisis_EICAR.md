# Analisis Statis File EICAR (Standard Antivirus Test)

### 1. Informasi Biner
- **Nama File:** `eicar.com`
- **Hash MD5:** `44d88612fea8a8f36de82e1278abb02f`
- **Hash SHA256:** `275a021bbfb6489e54d471899f7db9d1663fc695ec2fe2a2c4538aabf651fd0f`
- **Tipe File:** DOS Executable (.COM) / 16-bit

### 2. Analisis Strings (Floss/Strings)
Berbeda dengan malware asli yang disandikan (obfuscated), file ini menampilkan string yang sangat jelas dan eksplisit saat diekstrak:
`X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*`

### 3. Analisis Import Table
File biner ini tidak memiliki Import Table (IAT) atau pemanggilan API library Windows/Linux eksternal karena berjalan langsung sebagai instruksi DOS murni.

### 4. Kesimpulan Awal
Berdasarkan hasil pembedahan hash dan ekstraksi string, file ini **bukanlah malware destruktif**. Ini adalah file simulasi standar buatan *European Institute for Computer Antivirus Research* (EICAR) yang secara universal digunakan oleh analis keamanan dan perusahaan vendor untuk menguji apakah mesin deteksi antivirus berfungsi dengan baik tanpa risiko merusak sistem operasi.
