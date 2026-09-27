# Kontrol-Simulator-Robot-Persegi-Panjang

Repository ini berisi program ROS 2 berbasis Python untuk mengontrol pergerakan robot *differential drive* (`robin`) di dalam simulasi Gazebo agar membentuk lintasan berbentuk persegi panjang secara presisi dengan ukuran **3000 mm × 1500 mm (3.0 m × 1.5 m)** menggunakan metode *open-loop* (berbasis durasi waktu).

## Fitur Utama
* **Tanpa Sensor Odometri:** Menggunakan pendekatan berbasis waktu (*time-based sequence*) murni sehingga terhindar dari kendala *blocking* atau kegagalan pembacaan topik `/odom` pada simulator.
* **Parameter Kecepatan Stabil:** Dikonfigurasi dengan kecepatan linear $0.3\text{ m/s}$ dan kecepatan sudut $0.4\text{ rad/s}$ untuk meminimalkan selip roda (*slippage*).
* **Urutan Langkah Terstruktur (*Step Sequence*):** Menggunakan tuple list berurutan untuk mengatur pola gerak maju (sisi panjang dan lebar) berselang-seling dengan putaran sudut $90^\circ$ secara otomatis.

---

## Parameter Robot & Lintasan
* **Target Ukuran:** 3000 mm (Panjang) $\times$ 1500 mm (Lebar)
* **Kecepatan Linear (`linear_speed`):** $0.3\text{ m/s}$
* **Kecepatan Angular (`angular_speed`):** $0.4\text{ rad/s}$
* **Durasi Sisi Panjang (`t_long`):** $10.0\text{ detik}$
* **Durasi Sisi Lebar (`t_short`):** $5.0\text{ detik}$
* **Durasi Belok 90° (`t_turn`):** $\approx 1.96\text{ detik}$

---

## Cara Menjalankan Program

1. Pastikan file kode node penjelajah sudah dimasukkan ke dalam package ROS 2 Anda (misalnya di `simple_mover/simple_mover/rectangle_mover_node.py`).
2. Masuk ke direktori *workspace* ROS 2 Anda, lalu lakukan *build* package:
   ```bash
   cd ~/ros2_ws
   colcon build --packages-select simple_mover
