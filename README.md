# morfologi-citra

## medis

Library `skimage.measure` digunakan untuk mengekstraksi properti spasial dan menganalisis objek pada citra yang telah disegmentasi menjadi biner (hitam-putih). Dalam proyek deteksi sel ini, ada dua fungsi utama yang digunakan:

### 1.`measure.label()` (Connected-Component Labeling)
Fungsi ini bertugas memindai citra biner dan memberikan label (nomor urut unik) pada setiap kumpulan piksel putih yang saling terhubung. Proses ini mengubah sekumpulan piksel putih biasa menjadi entitas atau "objek sel" yang terpisah dan dapat dikenali oleh sistem secara individual.

### 2. `measure.regionprops()` (Ekstraksi Properti Objek)
Setelah setiap sel memiliki label, fungsi ini mengekstrak informasi dan karakteristik fisik dari setiap objek tersebut. Pada kode ini, properti yang digunakan meliputi:
  * `area`: Mengukur luas objek berdasarkan jumlah piksel. Properti ini digunakan sebagai thresholding ukuran (seperti kondisi `if r.area > 500`). Objek dengan luas di bawah 500 piksel akan diabaikan karena dianggap sebagai noise atau kotoran, bukan sel darah.
  * `bbox` (Bounding Box): Mengambil titik koordinat ekstrem (batas atas, bawah, kiri, dan kanan) dari setiap sel. Koordinat ini digunakan oleh OpenCV untuk menggambar kotak penanda pembatas di sekeliling sel pada citra hasil akhir.

### Top Hat & Black Hat

### Top-Hat Transform (Sering disebut White Top-Hat):

  * Fungsi Utama: Mendeteksi atau menonjolkan objek/cacat berukuran kecil yang lebih terang daripada area sekitarnya.

  * Cara Kerja Matematis: Citra asli dikurangi dengan citra hasil operasi Opening.

  * Kapan Dipakainya?: Dipakai kalau lu mau mendeteksi goresan yang memantulkan cahaya (putih/terang) di atas permukaan plat besi yang warnanya gelap, atau mencari debu putih di atas kain berwarna gelap.

### Black-Hat Transform (Sering disebut Bottom-Hat):

  * Fungsi Utama: Mendeteksi atau menonjolkan objek/cacat berukuran kecil yang lebih gelap daripada area sekitarnya.

  * Cara Kerja Matematis: Citra hasil operasi Closing dikurangi dengan citra asli.

  * Kapan Dipakainya?: Dipakai kalau lu mau mendeteksi noda hitam, lubang, atau titik gosong di atas permukaan produk yang warnanya putih atau terang (kayak keramik atau kertas).

### 3. Algoritma zhang-suen
  * Cara Kerja: Bekerja secara paralel dengan membagi satu kali iterasi menjadi dua sub-tahap (mengecek piksel tetangga di arah genap dan ganjil secara bergantian).
  * Kelebihan: Kecepatan komputasinya sangat tinggi dan efisien, sehingga sangat cocok untuk pemrosesan citra real-time atau data teks yang masif.
  * Kekurangan: Kadang tidak menghasilkan kerangka yang benar-benar sempurna setebal 1 piksel. Pada beberapa pola diagonal atau sudut siku-siku, hasilnya bisa menyisakan garis dengan ketebalan 2 piksel atau efek tangga (staircase effect).

### 3.1 Algoritma Hilditch
  * Cara Kerja: Mengevaluasi piksel berdasarkan sekumpulan aturan ketat (biasanya ada 6 kondisi) dengan melihat jendela 3x3 piksel sekitarnya untuk memastikan penghapusan piksel tidak merusak konektivitas atau menghilangkan titik ujung (endpoint).
  * Kelebihan: Kualitas skeleton yang dihasilkan lebih bersih, mulus, dan terjamin konsisten menjadi garis tunggal setebal persis 1 piksel. Sangat ideal untuk akurasi tinggi pada Optical Character Recognition (OCR).
  * Kekurangan: Proses komputasinya lebih berat dan lambat dibandingkan Zhang-Suen karena pengecekan kondisionalnya yang lebih kompleks pada setiap piksel.

