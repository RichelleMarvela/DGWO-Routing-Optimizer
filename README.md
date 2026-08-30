# DGWO Routing Optimizer

Optimisasi rute kendaraan berkapasitas untuk distribusi bantuan saat bencana banjir.

Ringkasan singkat
-----------------
Proyek ini menyelesaikan Capacitated Vehicle Routing Problem with Time Windows (CVRPTW) pada konteks distribusi bantuan bencana banjir. Setiap lokasi diberi skor prioritas menggunakan sistem Fuzzy Mamdani (victims, damage, risk). Di atas instance benchmark CVRPLIB (A-n80-k10) ditambahkan overlay simulasi banjir (BNPB-calibrated): delay factor, probabilitas pemblokiran jalan dinamis, dan time windows yang disesuaikan berdasarkan prioritas.

Metode utama yang diusulkan adalah DGWO-F2OPT (Grey Wolf Optimizer + Floating 2-opt) dan dibandingkan dengan baseline Adaptive GA dan MPSO. Evaluasi menggunakan metrik multi-kriteria: total distance, time-window penalty, risk exposure, blocking penalty, dan feasibility rate.

Fitur utama
-----------
- Fuzzy Mamdani untuk penentuan priority score per node (victims, damage, risk).
- Simulasi overlay banjir terkalibrasi (BNPB) yang menghasilkan: damage level, road access, delay factor, dynamic block events.
- Implementasi pipeline end-to-end dalam Jupyter Notebook: parsing CVRPLIB, pembuatan scenario bencana, perhitungan distance matrix, dan eksperimen metaheuristik.
- Metode optimisasi: DGWO-F2OPT (proposed), Adaptive GA, MPSO.
- Reproducible: seed global diset (GLOBAL_SEED = 42).

Isi repositori penting
----------------------
- VRP_Final_Complete_Fixed.ipynb — Notebook utama: implementasi fuzzy system, flood overlay, perhitungan distance, eksperimen optimisasi, visualisasi.
- DGWO_Paper_JARIE_Indonesia.docx — Naskah paper (Bahasa Indonesia) yang menjelaskan metodologi dan hasil eksperimen.
- A-n80-k10.vrp — Instance CVRPLIB yang diparsing oleh notebook (pastikan file ini ada di folder kerja).
- fuzzy_mf_plot.png — Dihasilkan oleh notebook (visualisasi membership functions).

Quickstart — Menjalankan project
--------------------------------
Prasyarat:
- Python 3.8+ dan pip
- Disarankan: virtual environment (venv / conda)

Langkah singkat:
1. Clone repo:

   git clone https://github.com/RichelleMarvela/DGWO-Routing-Optimizer.git
   cd DGWO-Routing-Optimizer

2. (Opsional) Buat dan aktifkan virtualenv:

   python -m venv .venv
   source .venv/bin/activate   # macOS / Linux
   .\.venv\Scripts\activate  # Windows

3. Install dependency dasar (notebook akan meng-install paket jika diperlukan):

   pip install numpy pandas matplotlib jupyterlab

4. Pastikan file instance CVRPLIB tersedia: `A-n80-k10.vrp` dalam folder repo.

5. Buka notebook dan jalankan sel-selnya dari atas ke bawah:

   jupyter notebook VRP_Final_Complete_Fixed.ipynb

Catatan: notebook sudah mengatur GLOBAL_SEED = 42 untuk reproduksi hasil. Jangan ubah nilai ini jika ingin mendapatkan hasil yang sama.

Penjelasan singkat arsitektur notebook
-------------------------------------
- Cell 1: Instalasi & import library, pengaturan seed.
- Cell 2: Implementasi Fuzzy Mamdani (trimf/trapmf), FuzzyPrioritySystem class, dan plot membership functions.
- Cell 3: Data preparation — parser CVRPLIB (.vrp), generator overlay banjir (BNPB-calibrated), pembuatan time windows, delay factor, dynamic t_block, dan perhitungan distance matrix.
- Selanjutnya (di notebook): implementasi DGWO-F2OPT dan baseline (GA/MPSO), objective function multi-kriteria, eksperimen perbandingan, visualisasi hasil dan statistik deskriptif.

Hasil yang diharapkan
---------------------
Notebook akan menghasilkan:
- Visualisasi fungsi keanggotaan fuzzy (fuzzy_mf_plot.png).
- Tabel sample node dengan atribut: victims, damage_level, road_access, priority_score, earliest/latest time, dynamic_t_block, will_block.
- Eksperimen per-method: tabel & grafik per-metrik (distance, TW penalty, risk, feasibility).

Kontribusi dan kegunaan
-----------------------
- Mengintegrasikan domain knowledge (BNPB historical distribution) dan fuzzy logic ke dalam pipeline routing — membuat skenario lebih realistis untuk humanitarian logistics.
- Menyajikan metode hybrid DGWO-F2OPT sebagai alternatif metaheuristik untuk CVRPTW under disaster constraints.
- Menyediakan notebook reproducible yang bisa diperluas (contoh: integrasi network-based routing, robust/stochastic optimization, heterogeneous fleet).

Limitasi
--------
- Simulasi banjir adalah pendekatan stokastik berbasis parameter historis — perlu validasi lapangan.
- Perhitungan jarak menggunakan Euclidean pada koordinat titik (bukan network routing riil/OSM).
- Asumsi fleet identik dan kapasitas sederhana.

Ide pengembangan lanjutan
-------------------------
- Integrasi peta jalan nyata (OpenStreetMap) dan routing berbasis network.
- Robust/stochastic optimization: mengoptimalkan rute untuk banyak skenario bencana.
- Real-time update: adaptasi algoritma untuk menerima streaming data (pemblokiran jalan, kondisi traffic).

Referensi & file pendukung
-------------------------
- Notebook: VRP_Final_Complete_Fixed.ipynb — implementasi utama.
- Paper (Bahasa Indonesia): DGWO_Paper_JARIE_Indonesia.docx — deskripsi metodologi dan eksperimen.

Kontak
------
Untuk informasi lebih lanjut atau request fitur (README, slide presentasi, ringkasan bahasa Inggris), hubungi pemilik repo.

Lisensi
-------
Silakan tambahkan lisensi proyek sesuai kebutuhan (mis. MIT, CC-BY) — belum disertakan secara default.
