# Laporan Analisis Malware Cryptojacking Varian XMRig Linux
**Identitas Berkas:** `b20f39fc00d242e706b6c30367ad811c676e0575050a4ec2f30104b696944b49.elf`

---

## BAB I: PENDAHULUAN

### 1.1 Latar Belakang
Seiring dengan pesatnya perkembangan infrastruktur teknologi informasi, ancaman keamanan siber juga mengalami evolusi yang signifikan. Saat ini, peretas tidak hanya berfokus pada pencurian data, tetapi juga merancang kode berbahaya yang mampu mengeksploitasi sumber daya perangkat keras korban secara diam-diam, seperti pada kasus cryptojacking. Ancaman ini umumnya didistribusikan dan dieksekusi di dalam sistem operasi target melalui file executable (file biner yang dapat dijalankan). Oleh karena itu, kemampuan untuk melakukan analisis mendalam terhadap file executable menjadi keterampilan yang sangat esensial dalam bidang keamanan siber.

Namun, file executable yang ditemukan di lapangan umumnya telah dikompilasi ke dalam bahasa mesin, sehingga kode sumber aslinya (source code) tidak lagi tersedia. Dalam kondisi inilah teknik Reverse Engineering (rekayasa balik) menjadi sangat penting. Reverse engineering adalah proses dekonstruksi sistem atau perangkat lunak untuk mengekstraksi arsitektur, desain, dan alur kerjanya. Dengan melakukan rekayasa balik menggunakan alat dekompilator (seperti Ghidra), analis keamanan dapat membedah instruksi biner, memahami logika program, dan mengekstraksi Indikator Kompromi (IoC) dari sebuah file executable yang dicurigai sebagai malware. Analisis ini sangat krusial untuk mengidentifikasi ancaman, memetakan potensi kerusakan yang dapat ditimbulkan, serta merumuskan strategi mitigasi dan pertahanan sistem yang tepat tanpa harus mengambil risiko mengeksekusi file tersebut secara sembarangan.

### 1.2 Rumusan Masalah
Berdasarkan latar belakang yang telah diuraikan, maka rumusan masalah dalam praktikum dan laporan analisis ini adalah sebagai berikut:
1. Bagaimana karakteristik dan struktur internal dari file executable (.ELF) yang dianalisis?
2. Apa fungsi utama, mekanisme eksekusi, serta tujuan (niat) dari program tersebut saat berada di dalam sistem?

### 1.3 Tujuan
Adapun tujuan yang ingin dicapai dari pelaksanaan praktikum dan penulisan laporan ini adalah:
1. **Mengidentifikasi struktur file:** Memetakan format, arsitektur, dan metadata penyusun file executable target.
2. **Melakukan analisis statis:** Membedah payload biner menggunakan metode rekayasa balik (reverse engineering) untuk mengekstraksi teks tersembunyi (strings), fungsi sistem yang dipanggil, serta identitas program tanpa harus mengeksekusinya.
3. **Menentukan perilaku program:** Mengobservasi dampak, konsumsi sumber daya, dan aktivitas jaringan yang dihasilkan oleh program saat dijalankan dalam lingkungan sistem operasi yang terisolasi.



## BAB II: DASAR TEORI

### 2.1 Reverse Engineering (Rekayasa Balik)
Merupakan metode analitis untuk membedah dan mendekonstruksi sebuah perangkat lunak atau file executable (biner) yang telah dikompilasi. Tujuannya adalah untuk memahami arsitektur internal, algoritma, serta alur instruksi pemrograman tanpa membutuhkan ketersediaan kode sumber (source code) aslinya. Pada ranah keamanan siber, pendekatan ini amat krusial untuk membongkar anatomi ancaman malware, menemukan Indikator Kompromi (IoC) seperti alamat server Command and Control (C2) atau strings tersembunyi, serta menganalisis taktik penyerangan.

### 2.2 Struktur Executable and Linkable Format (ELF)
Adalah standar format file biner utama yang digunakan oleh sistem operasi berbasis Unix dan Linux. Struktur ELF berfungsi sebagai cetak biru yang memandu kernel sistem operasi dalam memuat program ke dalam memori komputer. Secara arsitektural, file ELF tersusun atas beberapa komponen kunci:
- **ELF Header:** Terletak di posisi paling awal (offset 0). Selalu diawali dengan Magic Number `0x7F 'E' 'L' 'F'`. Memuat informasi arsitektur target, endianness, dan menentukan Entry Point.
- **Program Header Table (Segments):** Mendeskripsikan segmen-segmen memori untuk memetakan file biner ke dalam memori virtual saat proses eksekusi (runtime).
- **Section Header Table & Sections:** - `.text`: Area yang menampung instruksi kode program utama (executable code).
  - `.data`: Menyimpan variabel global/statis yang telah diinisialisasi.
  - `.rodata`: Berisi data konstan (Read-Only) seperti teks karakter (strings) statis.
  - `.bss`: Area khusus untuk variabel global/statis yang belum diinisialisasi.

### 2.3 Tools yang Digunakan
1. **Ghidra:** Aplikasi reverse engineering buatan NSA untuk membedah file ELF dan melakukan dekompilasi statis ke bahasa C.
2. **Oracle VM VirtualBox:** Hypervisor untuk membangun Virtual Machine (VM) sebagai ruang isolasi (sandbox).
3. **htop / top:** Utilitas terminal Linux untuk memantau konsumsi sumber daya CPU secara real-time.
4. **MalwareBazaar:** Portal repositori threat intelligence untuk mengunduh sampel malware.



## BAB III: METODOLOGI

### 3.1 Objek Analisis
Objek utama analisis ini adalah file executable biner Linux yang diperoleh secara legal dari repositori MalwareBazaar (abuse.ch).
- **Nama File Asli:** y11
- **Nama File Biner:** `b20f39fc00d242e706b6c30367ad811c676e0575050a4ec2f30104b696944b49.elf`
- **Ukuran File:** 8.350.992 bytes (Sekitar 8.35 MB)
- **Format Eksekusi:** Executable and Linkable Format (ELF) 64-bit LSB
- **Nilai Hash SHA-256:** `b20f39fc00d242e706b6c30367ad811c676e0575050a4ec2f30104b696944b49`
- **Signature:** Terdeteksi terafiliasi dengan varian Mirai / XMRig.

### 3.2 Metode Analisis
Metode yang digunakan adalah pendekatan hibrida (Analisis Statis dan Dinamis):
1. **Identifikasi File & Lingkungan Isolasi:** Analisis dilakukan di VM Ubuntu dengan adaptor jaringan dinonaktifkan secara total (disconnected).
2. **Analisis Header:** Memuat file ke platform Ghidra dan melakukan validasi Import Results Summary. Biner diketahui berstatus *stripped*.
3. **Ekstraksi String:** Pencarian hardcoded strings untuk menemukan Indikator Kompromi (IoC).
4. **Analisis Import & Fungsi:** Menggunakan decompiler untuk mengkonversi instruksi Assembly menjadi pseudokode bahasa C.
5. **Analisis Dinamis:** Memberikan izin eksekusi (`chmod +x`), menjalankan biner, dan memantau dampaknya pada utilisasi hardware sistem.
