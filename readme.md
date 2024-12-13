# BatikLens

This is the public repository for the **BatikLens** mobile application, developed by team **C242-PS249** as part of the Bangkit program.  
Several directories in the repository contain a **delete_this.txt** file. These files are included to serve as placeholders for the repository's folder structure template, as Git does not allow the storage of empty directories.


## Application Description
**BatikLens** is an Android-based application that leverages machine learning technology to accurately identify and classify batik patterns. The app enables users to recognize various types of batik, such as Batik Cendrawasih, Batik Dayak, Batik Geblek Renteng, Batik Megamendung, Batik Parang, Batik Pring Sedapur, Batik Tambal, and Batik Truntum, simply by taking a photo with their smartphone camera. As an interactive educational tool, BatikLens provides detailed information about the history and meaning of each pattern while fostering appreciation for Indonesia's cultural heritage.


## Application Architecture  
The BatikLens application, as depicted in the image above, utilizes the ResNet50 model developed using the Python programming language. The model is then converted into TensorFlow Lite (TFLite) format for compatibility with mobile platforms. After conversion, the TFLite model is integrated into the existing Android project, enabling the application to accurately classify various types of batik.


## Dataset  
### BatikLens Dataset Description  
The BatikLens dataset comprises 8 types of traditional Indonesian batik used to train the classification model. Each type of batik contains an equal number of data samples, with 101 images per category, resulting in a total of 808 images in the dataset. Below is the detailed data distribution for each type of batik:  

<table>
  <thead>
    <tr>
      <th>Types of Batik</th>
      <th>Data Count</th>
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
      <td>Batik Pring Sedapur</td>
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

#### Example of Batik Images  
![foto_batik](![batik_foto](https://github.com/user-attachments/assets/b04aa0f9-b267-4ae0-ac54-4a880568dcb5)


## Model 
### CNN from Scratch
![cnn](https://github.com/user-attachments/assets/7c9f9805-bde8-41ec-a425-949a3c3738b9)


Source: [CNN architecture](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.upgrad.com%2Fblog%2Fbasic-cnn-architecture%2F&psig=AOvVaw0Ne8d0ge4sh8XZUWx6rhif&ust=1734098780282000&source=images&cd=vfe&opi=89978449&ved=0CBQQjRxqFwoTCIjd_7-zoooDFQAAAAAdAAAAABAE)

For the initial Deep Learning model, we built a Deep Learning model from scratch utilizing a Convolutional Neural Network (CNN). The CNN model we developed consists of Convolutional layers, Pooling layers, several hidden layers, and finally, an output layer. We started with a simple CNN model using filters of size (3, 3) and then progressed to a more complex CNN model with filters of size (7, 7).

![model_summary_simple_cnn](https://github.com/user-attachments/assets/beaf357c-e1e0-4973-81e2-fb66e2809e70)


##### CNN Model Summary  
After experimenting with several CNN models, ranging from simple to deeper architectures, our model experienced overfitting (without data augmentation) and underfitting when data augmentation was applied to the image dataset. This may be due to the relatively small size of the batik dataset used for training. Considering these challenges, we decided to implement a transfer learning model to build the batik classification system.  




### ResNet50 Model  
After attempting to build a model from scratch and achieving low accuracy, we opted to use a transfer learning approach as an alternative. The selected transfer learning model was ResNet50. The process began with importing the necessary libraries and loading the batik dataset from Google Drive, followed by splitting the dataset into training and validation sets. ResNet50, initialized with pre-trained weights from ImageNet, was used as the base model without its top layers to allow customization of the output layer to match the number of batik classes. Several custom layers, such as GlobalAveragePooling2D, Dense, and Dropout, were added on top of the base model. The model was compiled using the Adam optimizer and the categorical crossentropy loss function. During training, the model was trained for 10 epochs and validated using the testing dataset. After the training process, the model was used to predict new images, and its performance was evaluated by generating a confusion matrix.

![model resnet](https://github.com/user-attachments/assets/58e4859f-e52f-4551-a7ab-0e76f28addbb)



Source:  [model resnet50 model architecture](https://commons.wikimedia.org/wiki/File:ResNet50.png)




![resnet_summary](https://github.com/user-attachments/assets/6189c213-7101-4c14-bbbd-9555f24cab1b)


##### Model resnet50 summary

### Model Result and Evaluation
From the modeling process, a comparison was obtained between the CNN model built from scratch, both with and without data augmentation, and the model using transfer learning with ResNet50, as follows:  
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


We also utilized a confusion matrix to analyze the errors and performance of the model. Below are the confusion matrix results for the models we used:  

### Confusion matrix CNN 
![confusion matrix_cnn](https://github.com/user-attachments/assets/8aa4816f-c06a-41e1-9ed7-1f94e8cb54eb)



### Confusion matrix ResNet50

![confusion matrix untuk resnet](https://github.com/user-attachments/assets/f0ae1d9f-ac33-4b0f-b50e-83a017b57205)


Based on the comparison of the model results, with significantly different levels of accuracy and model quality, we decided to use the ResNet50 transfer learning model.  


### Link Project Google Colab

[Batik Classification](https://colab.research.google.com/drive/1kKXlR55oiDNd74e430Bpk3KRBSVOXA2J?usp=sharing)

### Reference
[1] A. Alya and T. Wibowo, “Classification of Batik Motif Using Transfer Learning on Convolutional Neural Networks,” Semantic Scholar, [Online]. Available: https://www.semanticscholar.org/paper/CLASSIFICATION-OF-BATIK-MOTIF-USING-TRANSFER-ON-Alya-Wibowo/3a7227bcb9dd85f00e660690534a06a9a62a1c32  

[2] R. Minarno and S. Sumadi, “Classification of Batik Patterns Using K-Nearest Neighbors,” Semantic Scholar, [Online]. Available: https://www.semanticscholar.org/paper/Classification-of-batik-patterns-using-K-Nearest-Minarno-Sumadi/b0d71fc8b69e0c725c68a86176c76ed03bfe26dd  

[3] H. Ramadani, “Image Classification on Garutan Batik Using Convolutional Neural Network with Data Augmentation,” ResearchGate, 2023, [Online]. Available: https://www.researchgate.net/publication/370645801_Image_Classification_On_Garutan_Batik_Using_Convolutional_Neural_Network_with_Data_Augmentation  

[4] R. Alyasari, A. Arin, and H. Sumadi, “Classification of Batik Patterns Using Machine Learning,” Telkomnika, vol. 17, no. 3, pp. 1212–1220, 2019, [Online]. Available: https://telkomnika.uad.ac.id/index.php/TELKOMNIKA/article/view/12701/7036  

[5] A. Zainuddin and M. F. Ali, “Batik Motif Detection Using Transfer Learning,” JTSI, vol. 12, no. 4, pp. 451–460, 2021, [Online]. Available: https://openjournal.unpam.ac.id/index.php/JTSI/article/download/32142/pdf  

[6] I. Suwandi, “Automated Classification of Malaysian Batik Patterns Using AI,” AMCI, vol. 14, no. 2, pp. 78–88, 2021, [Online]. Available: https://ejournal.unimap.edu.my/index.php/amci/article/view/690/788  

