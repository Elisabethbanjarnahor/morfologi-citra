# morfologi-citra

## medis

Library `skimage.measure` digunakan untuk mengekstraksi properti spasial dan menganalisis objek pada citra yang telah disegmentasi menjadi biner (hitam-putih). Dalam proyek deteksi sel ini, ada dua fungsi utama yang digunakan:

### 1.`measure.label()` (Connected-Component Labeling)
Fungsi ini bertugas memindai citra biner dan memberikan label (nomor urut unik) pada setiap kumpulan piksel putih yang saling terhubung. Proses ini mengubah sekumpulan piksel putih biasa menjadi entitas atau "objek sel" yang terpisah dan dapat dikenali oleh sistem secara individual.

### 2. `measure.regionprops()` (Ekstraksi Properti Objek)
Setelah setiap sel memiliki label, fungsi ini mengekstrak informasi dan karakteristik fisik dari setiap objek tersebut. Pada kode ini, properti yang digunakan meliputi:
  * `area`: Mengukur luas objek berdasarkan jumlah piksel. Properti ini digunakan sebagai thresholding ukuran (seperti kondisi `if r.area > 500`). Objek dengan luas di bawah 500 piksel akan diabaikan karena dianggap sebagai noise atau kotoran, bukan sel darah.
  * `bbox` (Bounding Box): Mengambil titik koordinat ekstrem (batas atas, bawah, kiri, dan kanan) dari setiap sel. Koordinat ini digunakan oleh OpenCV untuk menggambar kotak penanda pembatas di sekeliling sel pada citra hasil akhir.
