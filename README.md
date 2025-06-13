# machine-learning-railvision

RailVision adalah aplikasi AI berbasis web yang dirancang untuk mendeteksi dan mengklasifikasikan tingkat kepadatan keramaian di stasiun kereta. Aplikasi ini menggunakan teknologi vision-based crowd counting dan klasifikasi berbasis deep learning. Output utama berupa prediksi jumlah orang dan kategori kepadatan: Sangat Sepi, Sepi, Sedang, Padat, Sangat Padat.

**Dataset used**: https://www.kaggle.com/datasets/tthien/shanghaitech

## Models
We trained 3 previously researched models based on the benchmark [here](https://paperswithcode.com/sota/crowd-counting-on-shanghaitech-a), which are [CSRNet](link referensi), [Fusion-Count](), and [YOLO]()
all our models are trained using TensorFlow architecture optimized for large scale production.

1. **CSRNet(Congested Scene Recognition Network)**

CSRNet adalah model berbasis Convolutional Neural Network (CNN) yang dirancang khusus untuk crowd counting. Arsitektur CSRNet terdiri dari dua bagian utama: frontend dan backend. Frontend dibangun berdasarkan VGG-16 (tanpa fully connected layer), yang bertugas mengekstraksi fitur spasial dari citra input. Backend menggunakan dilated convolutions untuk memperluas receptive field tanpa kehilangan resolusi spasial. CSRNet sangat unggul dalam mendeteksi kerumunan dengan kepadatan tinggi dan tidak teratur, menjadikannya cocok untuk lingkungan seperti stasiun kereta. Model ini dilatih menggunakan ground truth berupa peta kepadatan (density map), bukan bounding box, sehingga sangat efisien dalam menghitung jumlah objek kecil yang saling berdekatan.

Link model: https://drive.google.com/file/d/10tE-fVzzK8-qKsyUNSUhERHTgOURaudd/view?usp=sharing


2. **FusionCount:**

Fusion Count adalah pendekatan eksperimental yang menggabungkan hasil prediksi dari dua atau lebih model (dalam proyek ini: CSRNet + YOLOv8n), menggunakan rata-rata atau bobot tertentu untuk memperkirakan jumlah akhir. Tujuannya adalah memanfaatkan kekuatan masing-masing model: akurasi CSRNet dalam crowd padat dan deteksi objek YOLOv8n dalam kerumunan jarang. Namun, hasil akhir menunjukkan bahwa metode fusion ini tidak secara konsisten mengungguli CSRNet murni, terutama dalam skenario kepadatan tinggi seperti stasiun kereta saat jam sibuk.

Link model:

3. **YOLOv8n (You Only Look Once versi 8 - Nano)**

YOLOv8n adalah versi ringan dari keluarga YOLOv8, dirancang untuk deteksi objek real-time dengan efisiensi tinggi. Berbeda dengan CSRNet yang fokus pada estimasi jumlah dalam bentuk density map, YOLOv8n mendeteksi setiap objek (misalnya, kepala manusia) dalam citra dan memberikan output berupa bounding box. Model ini sangat cepat dan cocok untuk aplikasi real-time, tetapi dapat mengalami kesulitan dalam mendeteksi objek yang sangat padat atau saling tumpang tindih seperti dalam kasus kerumunan ekstrem di stasiun. Oleh karena itu, meskipun akurasinya baik pada kerumunan longgar, performanya kurang optimal untuk skenario kereta.

Link model:

## Evaluation
We find that CSRNet is a better choice for implementation for reasons such as efficent storage use, and model compatibility overall, with a score of...
