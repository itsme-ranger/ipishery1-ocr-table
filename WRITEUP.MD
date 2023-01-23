# No. 1: OCR Laporan
Skill Task - ML Engineer RnD. Ini adalah cara saya mengekstrak hasil laporan yang tertulis tangan menjadi terdigitalisasi.

## Metodologi saya
Berikut adalah ekspektasi arsitektur pipeline OCR ini dari saya:
![enter image description here](https://images2.imgbox.com/6f/73/yt1ye9UW_o.jpg)
Arsitektur tersebut sebagian diadaptasi dari PaddleOCR. Ada beberapa modifikasi dari arsitektur.
1. Image diproses dulu ke rectifier dan illuminator. Alasannya adalah setelah saya coba menggunakan beberapa sample data yang diberikan efishery langusng masuk ke inference PaddleOCR, tidak ada yang berhasil tabelnya terekstrak (bisa dilihat di folder 	. Hal ini sesuai ekspektasi awal karena dataset yang digunakan PP-structure v2 (PaddleOCR) adalah kondisi screenshot, bukan hasil foto. Hal ini menyebabkan saya berinisiatif untuk implementasi preprocess berupa rectifier+illuminator yang berdasarkan [paper ini](https://arxiv.org/pdf/2110.12942v2.pdf)
2.  OCR. OCR saya pecah jadi 2: OCR text (hasil cetak) dan OCR handwritten 

## Cara menjalankan
Melihat hasil dari preprocess tadi (rectification) masih mengecewakan (bisa dilihat di folder `illumination_rectified_results`), menyebabkan saya tidak dapat melanjutkan pengerjaan soal ini menjadi runnable executable. Tapi jika berhasil, bagaimana cara menjalankan akan kurang lebih seperti jawaban pada no. 3.

Outcome yang akan dimunculkan berupa: atas adalah image, bawahnya adalah generated tabel yang masing2 cell berisi entri dan ada warnanya, yang merepresentasikan keyakinan mesin inference terhadap output masing-masing cell. Jika warna latar suatu cell adalah merah, maka, cell tersebut memiliki keyakinan rendah menyebabkan user harus lebih aware terhadap value dari cell tersebut.

## Possibility next update
1. Melihat saya approachnya menyelesaikan sisi awal dulu (tahap sebelum masuk ocr), maka nextnya bisa jadi attack langsung ke OCRnya (handwritten), karena:
	1. satu cell biasanya berupa satu kata/angka. Sementara itu, model inference OCR raw biasanya mengoutputkan dalam 1 kata saja, sehingga secara tidak langsung challenge di table processing sebagian sudah tersolve di sini
2. Jika SOTA table recognition dari photo masih kurang baik, sepertinya attack berikutnya berupa enforce SOP foto ke user. Saya melihat banyak foto tabel yang miring dan/atau terpotong, dan sebagian part foto ada yang kena bayangan.
3. Jika masih kurang berhasil nomor 2, bisa menggunakan konvensional image processing: recognize line satu satu terus jika ada overlap dan lain-lain jadilah sebuah tabel. Atau, mungkin naive row and column matching algorithm bisa works
4. Untuk pengembangan model OCR handwritten: pemilihan model char recognition/classifiernya berupa model yang semacam few-shot, one-shot. atau bahkan zero-shot utk fine tuning kualitas, seperti penggunaan siamese network atau triplet loss. Alasannya, font style tangan tiap orang beda-beda tapi tiap orang bisa jadi cenderung sama setiap saat, dan ini langkah yang effortnya minimal.