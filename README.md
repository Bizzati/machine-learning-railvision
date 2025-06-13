# machine-learning-railvision

RailVision adalah aplikasi AI berbasis web yang dirancang untuk mendeteksi dan mengklasifikasikan tingkat kepadatan keramaian di stasiun kereta. Aplikasi ini menggunakan teknologi vision-based crowd counting dan klasifikasi berbasis deep learning. Output utama berupa prediksi jumlah orang dan kategori kepadatan: Sangat Sepi, Sepi, Sedang, Padat, Sangat Padat.

## Dataset
Link: https://www.kaggle.com/datasets/tthien/shanghaitech

Deskripsi:
Proyek ini menggunakan ShanghaiTech Crowd Counting Dataset yang merupakan salah satu dataset benchmark paling populer dalam tugas crowd counting. Dataset ini terdiri dari dua subset utama:
- Part A: Berisi gambar kerumunan dengan tingkat kepadatan tinggi, diambil dari internet (misalnya konser, demonstrasi).
- Part B: Berisi gambar kerumunan dengan kepadatan rendah hingga sedang, dikumpulkan dari area publik seperti jalanan kota di Shanghai.

Setiap gambar dalam dataset ini dilengkapi dengan file ground truth berformat `.mat` yang berisi koordinat titik kepala dari setiap individu di gambar.

Untuk meningkatkan variasi dan generalisasi model, dalam proyek ini kami menggabungkan Part A dan Part B menjadi satu dataset besar sebelum dilakukan proses pelatihan dan validasi. Setelah penggabungan, data kemudian dipecah kembali menjadi set training, validation, dan testing menggunakan rasio standar:
- 72% training
- 18% validation
- 10% testing

## Models
Kami melatih 3 model yang telah diteliti sebelumnya berdasarkan benchmark [disini](https://paperswithcode.com/sota/crowd-counting-on-shanghaitech-a), yaitu CSRNet, Fusion-Count, dan YOLO. Semua model kami dilatih menggunakan arsitektur TensorFlow yang dioptimalkan untuk produksi skala besar.

1. **CSRNet(Congested Scene Recognition Network)**

CSRNet adalah model berbasis Convolutional Neural Network (CNN) yang dirancang khusus untuk crowd counting. Arsitektur CSRNet terdiri dari dua bagian utama: frontend dan backend. Frontend dibangun berdasarkan VGG-16 (tanpa fully connected layer), yang bertugas mengekstraksi fitur spasial dari citra input. Backend menggunakan dilated convolutions untuk memperluas receptive field tanpa kehilangan resolusi spasial. CSRNet sangat unggul dalam mendeteksi kerumunan dengan kepadatan tinggi dan tidak teratur, menjadikannya cocok untuk lingkungan seperti stasiun kereta. Model ini dilatih menggunakan ground truth berupa peta kepadatan (density map), bukan bounding box, sehingga sangat efisien dalam menghitung jumlah objek kecil yang saling berdekatan.

- Link model: https://drive.google.com/drive/folders/1UJZJ-5ZZpQdrDwprQeJsqDXKjFKHL0Ls?usp=sharing

2. **YOLOv8n (You Only Look Once versi 8 - Nano)**

YOLOv8n adalah versi ringan dari keluarga YOLOv8, dirancang untuk deteksi objek real-time dengan efisiensi tinggi. Berbeda dengan CSRNet yang fokus pada estimasi jumlah dalam bentuk density map, YOLOv8n mendeteksi setiap objek (misalnya, kepala manusia) dalam citra dan memberikan output berupa bounding box. Model ini sangat cepat dan cocok untuk aplikasi real-time, tetapi dapat mengalami kesulitan dalam mendeteksi objek yang sangat padat atau saling tumpang tindih seperti dalam kasus kerumunan ekstrem di stasiun. Oleh karena itu, meskipun akurasinya baik pada kerumunan longgar, performanya kurang optimal untuk skenario kereta.

- Link model: 

3. **FusionCount**

Fusion Count adalah pendekatan eksperimental yang menggabungkan hasil prediksi dari dua atau lebih model, menggunakan rata-rata atau bobot tertentu untuk memperkirakan jumlah akhir. Tujuannya adalah memanfaatkan kekuatan masing-masing model: akurasi CSRNet dalam crowd padat dan deteksi objek YOLOv8n dalam kerumunan jarang. Namun, hasil akhir menunjukkan bahwa metode fusion ini tidak secara konsisten mengungguli CSRNet murni, terutama dalam skenario kepadatan tinggi seperti stasiun kereta saat jam sibuk.

- Link model: https://drive.google.com/drive/folders/1XiQFsC3ut978uqOt_82b3u2gza9PD4by?usp=drive_link

## Evaluation
Kami menemukan bahwa CSRNet merupakan pilihan terbaik untuk diimplementasikan karena efisiensi penggunaan penyimpanan dan kompatibilitas model secara keseluruhan, dengan skor evaluasi sebagai berikut:

- CSRNet: Model ini memberikan hasil estimasi jumlah kerumunan yang paling stabil dan akurat pada dataset validasi. Meskipun bukan model yang paling kompleks, CSRNet memberikan keseimbangan terbaik antara performance dan resource usage, dengan nilai MAE (Mean Absolute Error) yang relatif rendah dan inference time yang cukup cepat.

- YOLOv8n (object detection-based): Meskipun mampu memberikan bounding box pada tiap individu, pendekatan ini kurang cocok untuk situasi kerumunan padat di mana individu saling menutupi satu sama lain (occlusion). Akurasi pendeteksian cenderung menurun secara signifikan pada gambar dengan density tinggi, sehingga estimasi jumlahnya tidak stabil.

- Fusion Count: Pendekatan ini menggabungkan berbagai feature extraction namun memerlukan waktu pelatihan lebih lama dan kompleksitas tambahan dalam deployment. Hasilnya tidak menunjukkan peningkatan signifikan dibanding CSRNet, sehingga tidak dipilih untuk implementasi akhir.

Secara keseluruhan, CSRNet unggul karena:
- Akurasi cukup tinggi dan stabil di berbagai skenario kepadatan.
- Lebih ringan dan sederhana untuk di-deploy.
- Tidak bergantung pada bounding box sehingga lebih cocok untuk deteksi kerumunan padat.
