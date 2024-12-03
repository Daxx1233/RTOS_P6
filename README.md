# Praktikum RTOS - SEMESTER 5 (3 Teknik Komputer A)
- Triyas Rahmadiyanti (3222600001)
- Duta Fithra Qolby (3222600003)
# Exercise 6 – Demonstrate a simple way to eliminate
resource contention - suspending the scheduler.

## Deskripsi Projek
Proyek ini adalah implementasi sistem mikrokontroler berbasis STM32 yang menggunakan CMSIS-RTOS (Real-Time Operating System) untuk mengelola beberapa tugas secara paralel. 

Sistem ini memiliki tiga tugas utama:
1. Default Task: Sebuah placeholder thread yang tidak memiliki fungsi spesifik saat ini.
2. Green LED Task: Menyalakan dan mematikan LED hijau dengan pola tertentu.
3. Red LED Task: Menyalakan dan mematikan LED merah dengan pola tertentu.
Proyek juga melibatkan penggunaan fitur seperti delay berbasis DWT (Data Watchpoint and Trace) dan mekanisme kritikal untuk mengatur akses eksklusif ke sumber daya bersama.

## Deskripsi Alur Kerja
1. Inisialisasi Sistem:
   - Mikrokontroler diinisialisasi menggunakan fungsi HAL_Init(), yang mereset perangkat keras dan menyiapkan sistem.
   - Clock sistem dikonfigurasi dengan SystemClock_Config().
2. Inisialisasi GPIO:
   - GPIO port diatur menggunakan fungsi MX_GPIO_Init() untuk mengontrol LED (kuning, merah, hijau).
   - LED awalnya dimatikan (GPIO_PIN_RESET).
3. Inisialisasi Scheduler:
   - RTOS diinisialisasi melalui osKernelInitialize().
   - Tugas-tugas (threads) dibuat menggunakan osThreadNew():
      - DefaultTask: Tidak aktif saat ini, hanya osDelay(1) dalam loop.
      - GreenLEDTask: Mengontrol LED hijau.
      - RedLEDTask: Mengontrol LED merah.
 4. Mekanisme Delay dan Akses Data:
    - Fungsi Delay_ms() menggunakan register DWT untuk mengimplementasikan delay presisi.
    - Fungsi accessData() mengatur akses kritis ke sumber daya bersama, yaitu sebuah variabel global flagg, yang diproteksi dengan taskENTER_CRITICAL() dan taskEXIT_CRITICAL().
 5. Eksekusi Tugas Paralel:
    - GreenLEDTask:
      - Menyalakan LED hijau.
      - Memanggil accessData() untuk interaksi data.
      - Mematikan LED hijau setelah selesai.
      - Delay selama 500 ms sebelum loop berikutnya.
    - RedLEDTask:
      - Menyalakan LED merah.
      - Memanggil accessData() untuk interaksi data.
      - Mematikan LED merah setelah selesai.
      - Delay selama 100 ms sebelum loop berikutnya.
  6. Eksekusi Kernel: Scheduler dimulai menggunakan osKernelStart(), mengelola semua thread secara paralel.
  7. Error Handling: Jika terjadi error dalam konfigurasi clock atau inisialisasi lainnya, sistem akan masuk loop infinite pada Error_Handler().

## Fungsi Utama
  1. Pengaturan LED: Masing-masing thread (task) mengontrol LED tertentu, menciptakan pola berkedip berbeda untuk setiap warna.
  2. RTOS: Digunakan untuk multitasking sehingga tugas-tugas dapat berjalan secara independen namun terkoordinasi.
  3. Akses Data Bersama: accessData() menghindari race condition menggunakan critical section, memastikan akses eksklusif terhadap flagg.
  4. Delay Presisi: DWT digunakan untuk menghasilkan delay dengan presisi tinggi, menggantikan delay berbasis timer konvensional.

# Gambar Hardware
![image](https://github.com/user-attachments/assets/a11a2382-90d8-4ee1-b372-d8c99cb257d1)

# Video Hardware

https://github.com/user-attachments/assets/3adc9215-1b22-41b9-83b7-a54033813d6a
