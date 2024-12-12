# BatikLens

Ini adalah repository publik aplikasi mobile BatikLens yang di kembangkan oleh tim **C242-PS249** program bangkit<br>
Beberapa direktori di repository yang berisi file **delete_this.txt** adalah direktori kosong, file tersebut di sematkan untuk memuat template struktur folder repository. Karena git tidak dapat menyimpan folder kosong. 


## Deskripsi Aplikasi
**BatikLens** adalah aplikasi berbasis Android yang memanfaatkan teknologi machine learning untuk mengenali dan mengklasifikasikan motif batik secara akurat. Aplikasi ini memungkinkan pengguna untuk mengetahui jenis batik seperti Batik Cendrawasih, Batik Dayak, Batik Geblek Renteng, Batik Megamendung, Batik Parang, Batik Pring Sedapur, Batik Tambal, dan Batik Truntum hanya dengan mengambil foto menggunakan kamera ponsel. Sebagai alat edukasi interaktif, BatikLens memberikan informasi mendetail tentang sejarah dan makna setiap motif, sekaligus mendorong apresiasi terhadap warisan budaya Indonesia.


## Arsitektur Aplikasi
Aplikasi BatikLens yang ditampilkan dalam gambar di atas memanfaatkan model ResNet50 yang dikembangkan menggunakan bahasa pemrograman Python. Model ini kemudian dikonversi ke dalam format TensorFlow Lite (TFLite) agar dapat digunakan pada platform mobile. Setelah konversi, model TFLite diintegrasikan ke dalam proyek Android yang sudah ada, sehingga aplikasi ini dapat secara akurat mengklasifikasikan berbagai jenis batik.


## Dataset
### Deskripsi Dataset BatikLens
Dataset BatikLens terdiri dari 8 jenis batik tradisional Indonesia yang digunakan untuk melatih model klasifikasi. Setiap jenis batik memiliki jumlah data yang sama, yaitu 101 gambar, dengan total keseluruhan dataset sebanyak 808 gambar. Berikut adalah rincian jumlah data untuk setiap jenis batik:

<table>
  <thead>
    <tr>
      <th>Jenis Batik</th>
      <th>Jumlah Data</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Batik Cendrawasih</td>
      <td>101</td>
    </tr>
    <tr>
      <td>Batik Dayak</td>
      <td>101</td>
    </tr>
    <tr>
      <td>Batik Geblek Renteng</td>
      <td>101</td>
    </tr>
    <tr>
      <td>Batik Megamendung</td>
      <td>101</td>
    </tr>
    <tr>
      <td>Batik Parang</td>
      <td>101</td>
    </tr>
    <tr>
      <td>Batik Pring Sedawung</td>
      <td>101</td>
    </tr>
    <tr>
      <td>Batik Tambal</td>
      <td>101</td>
    </tr>
    <tr>
      <td>Batik Truntum</td>
      <td>101</td>
    </tr>
  </tbody>
</table>

#### Contoh gambar batik
![foto_batik](https://github.com/user-attachments/assets/c5018323-86ec-4590-b048-cd36cd083f2d)

## Model 
### CNN from Scratch
![cnn](https://github.com/user-attachments/assets/7c9f9805-bde8-41ec-a425-949a3c3738b9)


Sumber: [CNN architecture](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.upgrad.com%2Fblog%2Fbasic-cnn-architecture%2F&psig=AOvVaw0Ne8d0ge4sh8XZUWx6rhif&ust=1734098780282000&source=images&cd=vfe&opi=89978449&ved=0CBQQjRxqFwoTCIjd_7-zoooDFQAAAAAdAAAAABAE)

Untuk model Deep Learning yang pertama, kami mencoba membangun model Deep Learning dari awal, kami memanfaatkan model CNN atau Convolutional Neural Network. Model CNN yang kami bangun terdiri dari Convolutional layer, Pooling Layer, beberapa hidden layers, dan yang terakhir output layer. Kami membuat model CNN dari yang simple dengan filter (3, 3) sampai dengan model cnn yang menggunakan filter (7, 7).


![model_summary_simple_cnn](https://github.com/user-attachments/assets/beaf357c-e1e0-4973-81e2-fb66e2809e70)


##### model CNN summary


Namun setelah mencoba beberapa model CNN, dari model yang simple sampai dengan model CNN yang lebih dalam, model kami mengalami overfitting (model overfitting tanpa augmentasi data) dan underfitting ketika kami menerapkan augmentasi pada data image. Mungkin karena data batik yang digunakan juga lumayan sedikit untuk di training sehingga berakibat demikian. Dengan pertimbangan ini kami memilih untuk menerapkan model transfer learning untuk membangun model klasifikasi batik.




### Model ResNet50
Setelah mencoba membangun model dari awal, dan hasil akurasi yang didapat sangat kecil, kami menggunakan model transfer learning sebagai alternatif. Model transfer learning yang dipilih adalah ResNet50, model ini dimulai dengan mengimpor pustaka yang diperlukan dan memuat dataset batik dari Google Drive, kemudian membagi dataset menjadi set pelatihan dan validasi. ResNet50, yang diambil dengan bobot yang telah dilatih sebelumnya dari ImageNet, digunakan sebagai model dasar tanpa lapisan atas untuk memungkinkan penyesuaian pada lapisan output sesuai dengan jumlah kelas batik yang ada. Setelah menambahkan beberapa lapisan kustom seperti GlobalAveragePooling2D, Dense, dan Dropout, model dikompilasi menggunakan optimizer Adam dan fungsi loss categorical crossentropy. Selama pelatihan, model ini dilatih selama 10 epoch dan divalidasi terhadap dataset pengujian. Setelah proses pelatihan, model dapat digunakan untuk memprediksi gambar baru, dan evaluasi kinerjanya dilakukan dengan membuat confusion matrix.

![model resnet](https://github.com/user-attachments/assets/58e4859f-e52f-4551-a7ab-0e76f28addbb)



sumber:[model resnet50 model architecture](https://commons.wikimedia.org/wiki/File:ResNet50.png)




![resnet_summary](https://github.com/user-attachments/assets/6189c213-7101-4c14-bbbd-9555f24cab1b)


##### model resnet50 summary

### Model Result and Evaluation
Dari proses modelling atau pembuatan model yang dilakukan, didapatkan hasil perbandingan model CNN dari scratch baik menggunakan data augmentasi ataupun tidak dan hasil model menggunakan transfer learning dengan ResNet50 sebagai berikut:
<table>
  <thead>
    <tr>
      <th>Metrics</th>
      <th>Model CNN From Scratch</th>
      <th>Model transfer learning (ResNet50)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Accuracy</td>
      <td>0.6156</td>
      <td>0.9998</td>
    </tr>
    <tr>
      <td>loss</td>
      <td>4.7679</td>
      <td>0.0119</td>
    </tr>
    <tr>
      <td>val accuracy</td>
      <td>0.4410</td>
      <td>0.9503</td>
    </tr>
    <tr>
      <td>val loss</td>
      <td>5.2817</td>
      <td>0.1336</td>
    </tr>
  </tbody>
</table>


kami juga menggunakan confusion matrix guna melihat kesalahan dan performa pada model, berikut hasil confusion matrix dari model yang kami gunakan:

### confusion matrix CNN 
![confusion matrix_cnn](https://github.com/user-attachments/assets/8aa4816f-c06a-41e1-9ed7-1f94e8cb54eb)



### confusion matrix ResNet50

![confusion matrix untuk resnet](https://github.com/user-attachments/assets/f0ae1d9f-ac33-4b0f-b50e-83a017b57205)


Dari perbandingan hasil model yang digunakan, dengan tingkat akurasi dan kualitas model yang sangat jauh berbeda, maka kami memilih untuk menggunakan model transfer learning Resnet50.


### Link Project Google Colab

[Batik Classification](https://colab.research.google.com/drive/1kKXlR55oiDNd74e430Bpk3KRBSVOXA2J?usp=sharing)

### Daftar Pustaka dan Referensi
Berikut beberapa paper yang kami jadikan sebagai referensi dalam membangun proyek klasifikasi batik ini. Kami telah meringkas semua isi paper dan menyertakan link url dari paper yang digunakan.

[sumber Referensi](https://docs.google.com/spreadsheets/d/1e-bderZlbDEQoaEPYR_jhdN8ZlTwu3w99RCkcTZ_5_Q/edit?usp=sharing)


