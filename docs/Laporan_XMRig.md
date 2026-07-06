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
