# Batik Lens

## Proses Training dan Testing
### Proses Training
1. Data Training dan Data Testing 

    ```
    def train_val_datasets():
    training_dataset = tf.keras.utils.image_dataset_from_directory(
        base_dir,
        subset='training',
        validation_split=0.2,
        seed=42,
        batch_size=32,
        image_size=(256, 256),
        label_mode='categorical',
        color_mode="rgb",
        shuffle=True,
    )

    testing_dataset = tf.keras.utils.image_dataset_from_directory(
        base_dir,
        subset='validation',
        validation_split=0.2,
        seed=42,
        batch_size=32,
        image_size=(256, 256),
        label_mode='categorical',
        color_mode="rgb",
    )

    return training_dataset, testing_dataset
    ```
    Pembagian dataset menjadi data training dan data testing merupakan langkah awal sebelum membuat model yang bertujuan untuk menguji performa model. Sebanyak 20% data digunakan untuk validasi. Label dikodekan sebagai kategori dan semua gambar ukurannya diubah menjadi 256x256 piksel. Adapun jumlah data training adalah sebanyak 647 gambar dan data testing sebanyak 161 gambar. 
2. Augmentasi Data

    Augmentasi adalah teknik yang digunakan untuk memodifikasi gambar asli dengan mengubah bentuknya, sehingga menghasilkan gambar baru. Tujuan utama dari augmentasi data ini adalah memperbanyak jumlah data dalam dataset.
3. Implementasi Model

    Pengimplementasian algoritma ResNet50 diawali dengan pembuatan base model yang selanjutnya digabungkan dengan beberapa layer seperti pada kode di bawah ini.
    ```
    base_model = ResNet50(weights='imagenet', include_top=False, input_shape=(256, 256, 3))
    model_transfer = tf.keras.Sequential([
        base_model,
        tf.keras.layers.GlobalAveragePooling2D(),
        tf.keras.layers.Dense(64, activation='relu'),
        tf.keras.layers.Dropout(0.2),
        tf.keras.layers.Dense(8, activation='softmax')
    ])
    ```
    Penjelasan:
    - ResNet50(weights='imagenet', include_top=False): artinya menggunakan model ResNet50 dengan bobot yang telah dilatih pada dataset ImageNet, tetapi tanpa lapisan keluaran.
    - GlobalAveragePooling2D(): untuk mengubah output dari lapisan konvolusi menjadi satu nilai rata-rata untuk setiap fitur.
    - Dense(64, activation='relu'): untuk menambah lapisan dengan 64 neuron dan fungsi aktivasi ReLU.
    - Dropout(0.2): untuk mencegah overfitting dengan mengabaikan 20% neuron secara acak selama pelatihan.
    - Dense(8, activation='softmax'): artinya menggunakan dense layer dengan fungsi aktivasi softmax untuk prediksi akhir sebanyak 8 kelas.

4. Evaluasi Model

    Setelah model dibuat, langkah selanjutnya adalah melakukan evaluasi pada kinerja model. Kode yang digunakan adalah sebagai berikut.
    ```
    model_transfer.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=1e-4),
                loss='categorical_crossentropy',
                metrics=['accuracy'])
    model_transfer.summary()
    histoy_transfer = model_transfer.fit(
        training_dataset,
        validation_data=testing_dataset,
        epochs=10
    )
    ```
    Penjelasan:
    - model_transfer.compile:
        - optimizer=tf.keras.optimizers.Adam(learning_rate=1e-4): artinya menggunakan optimizer adam dengan learning rate 0.0001.
        - loss='categorical_crossentropy': untuk mengukur seberapa jauh prediksi model dari label sebenarnya. Categorical crossentropy loss function karena memiliki unit lebih dari 2 sehingga cocok untuk masalah klasifikasi dengan banyak kelas.
        - metrics=['accuracy']: untuk mengevaluasi performa model selama pelatihan, menggunakan akurasi sebagai metrik Utama.
    - model_transfer.summary(): untuk menampilkan struktur detail model, termasuk jumlah layer, jenis layer, dan jumlah parameter yang dapat dilatih.
    - model_transfer.fit: untuk melatih model menggunakan data pelatihan dan mengevaluasinya menggunakan data validasi.

### Proses Testing
Berikut adalah hasil yang diperoleh dari proses training model sebelumnya.
- accuracy:
- loss:
- val_accuracy:
- val_loss:

## Link Project
## Daftar Pustaka