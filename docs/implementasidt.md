##Implementasi
###Package Requirement
Penelitian ini menggunakan Scikit-learn untuk mengolah dan menganalisis data. Didalam scikit juga telah terdapat berbagai algoritma analisis data yang dapat digunakan tanpa harus menulis source code dari awal. Untuk menggunakan Scikit-learn kita harus menginstall software berikut.

<li>Python (>= 3.5)</li>
<li>NumPy (>= 1.11.0)</li>
<li>SciPy (>= 0.17.0)</li>
<li>joblib (>= 0.11)</li>
<li>Matplotlib (>= 1.5.1)</li>
<li>Pandas (>= 0.18.0)</li>

###Package Installation
Untuk menginstall package tersebut kita bisa menggunakan perintah pip. Sebagai catatan, instalasi package tersebut membutuhkan koneksi internet

``` sh
pip install numpy,scipy,joblib,matplotlib,pandas
```
###Coding
 Setelah proses instalasi selesai kita bisa mengimport library-library tersebut dari python kita.

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn import preprocessing
from sklearn.tree import DecisionTreeClassifier # Import Decision Tree Classifier
from sklearn import metrics #Import scikit-learn metrics module for accuracy calculation
from sklearn.model_selection import train_test_split
import pandas as pd

```
Selanjutnya kita dapat memasukkan data set berupa file csv

```python
lokasi_file=input("Masukkan lokasi file data training : ")
data_train= pd.read_csv(lokasi_file)
```
Data dalam dataset terbagi atas atribut fitur dan atribut target. Dalam dataset NSL-KDD atribut target adalah atribut'status' sedangkan fitur adalah atribut selain 'status'. Nantinya atribut fitur akan digunakan untuk memprediksi nilai dari atribut target. Untuk itu dalam source code kita harus disertakan kode untuk membagi data dalam dataset kedalam atribut fitur dan atribut target, serta kode untuk mengambil nama kolom atribut. 

```python
#Mengambil dataset dari atribut selain status sebagai data atribut fitur
#dan dari data status sebagai atribut target
x,y = data_train.loc[:,data_train.columns != 'status'], data_train.loc[:,'status']
#Mengambil data kolom atiribut fitur
col=data_train.columns.tolist()
col.remove('status')

```
Selanjutnya dilakukan pre-processing data berupa encoding dengan metode one hot encoding seperti dijelaskan pada bagian metode penelitian. Variable x akan berisi data dari atribut fitur sedangkan y akan berisi data dari atribut target.
```python
#Melakukan enccoding pada atribut yang berisi string.
x=pd.get_dummies(x, columns=col)
y=pd.get_dummies(y, columns=['status'])
```
Setelah pre-processing, tahap berikutnya adalah pengujian. Pada tahap ini 80% dataset akan digunakan sebagai data training dan 20% sebagai data uji. 

```python

# membangun traning set, dimana 80% utk training dan 20% utk testing
x_train,x_test,y_train,y_test = train_test_split(x,y,test_size = 0.2)

```
###Pembangunan Model, Pengujian dan Visualiasi Hasil 
Dan pada akhirnya hasil pengujian tersebut ditampilkan dalam bentuk grafik agar  sebuah model Decision Tree akan dibangun di traning dan diuji dengan data yang telah disiapkan. Hasil akurasi akan ditampilkan untuk mengetahui efektifitas dari model. Tampak bahwa model ini mampu mendeteksi 94% serangan dengan tepat

```python

# Membangun desision tree classifier
clf = DecisionTreeClassifier()

# Melakukan traning
clf = clf.fit(x_train,y_train)

#Melakukan pengujian dan menampilkan akurasi
y_pred = clf.predict(x_test)
print("Accuracy:",metrics.accuracy_score(y_test, y_pred))

```

<center>[![Material for MkDocs](assets/images/hasildt.png)](assets/images/hasildt.png)</center>


