# machine-learning-railvision
Tolong deskripsiin pendek

**Dataset used**: https://www.kaggle.com/datasets/tthien/shanghaitech

## Models
We trained 3 previously researched models based on the benchmark [here](https://paperswithcode.com/sota/crowd-counting-on-shanghaitech-a), which are [CSRNet](link referensi), [Fusion-Count](), and [YOLO]()
all our models are trained using TensorFlow architecture optimized for large scale production.

1. **CSRNet(Congested Scene Recognition Network)**

CSRNet adalah model berbasis Convolutional Neural Network (CNN) yang dirancang khusus untuk crowd counting. Arsitektur CSRNet terdiri dari dua bagian utama: frontend dan backend. Frontend dibangun berdasarkan VGG-16 (tanpa fully connected layer), yang bertugas mengekstraksi fitur spasial dari citra input. Backend menggunakan dilated convolutions untuk memperluas receptive field tanpa kehilangan resolusi spasial. CSRNet sangat unggul dalam mendeteksi kerumunan dengan kepadatan tinggi dan tidak teratur, menjadikannya cocok untuk lingkungan seperti stasiun kereta. Model ini dilatih menggunakan ground truth berupa peta kepadatan (density map), bukan bounding box, sehingga sangat efisien dalam menghitung jumlah objek kecil yang saling berdekatan.


2. **FusionCount:** Fusion count is a model that works by...

3. **YOLOv8n (You Only Look Once versi 8 - Nano)**

YOLOv8n adalah versi ringan dari keluarga YOLOv8, dirancang untuk deteksi objek real-time dengan efisiensi tinggi. Berbeda dengan CSRNet yang fokus pada estimasi jumlah dalam bentuk density map, YOLOv8n mendeteksi setiap objek (misalnya, kepala manusia) dalam citra dan memberikan output berupa bounding box. Model ini sangat cepat dan cocok untuk aplikasi real-time, tetapi dapat mengalami kesulitan dalam mendeteksi objek yang sangat padat atau saling tumpang tindih seperti dalam kasus kerumunan ekstrem di stasiun. Oleh karena itu, meskipun akurasinya baik pada kerumunan longgar, performanya kurang optimal untuk skenario kereta.

## Evaluation
We find that CSRNet is a better choice for implementation for reasons such as efficent storage use, and model compatibility overall, with a score of...
