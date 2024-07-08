## Dito Imanuel Arminto
## 20231074
## Pengolahan Citra Digital - A
---
# LAPRAK 3 DAN 4 PENGOLAHAN CITRA DIGITAL
Berikut berisi penjelasan laporan praktikum saya mengenai Deteksi Garis Tepi Pada Citra dan Konversi Ekstraksi dan Visualisasi Properti GLCM dari Kanal V Gambar HSV. Program akan dibagi menjadi 2 yaitu Laporan Praktikum 3 dan Laporan Praktikum 4.
## Laporan Praktikum 3 - Tepi Garis
***Import Library***<br>
```
import cv2 
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import skimage
```
- import cv2: bagian untuk mengimport modul OpenCV yang menyediakan fungsi untuk memanipulasi gambar dan video.
- import numpy as np: Ini mengimport modul NumPy dengan alias np. NumPy digunakan dalam perhitungan numerik dan pemrosesan citra dalam bentuk array.
- import matplotlib.pyplot as plt: Bagian ini mengimport modul pyplot dari Matplotlib dengan alias plt. Matplotlib biasanya digunakan untuk memvisualisasikan data. pyplot untuk menampilkan data secara interaktif atau non-interaktif dalam bentuk plot.
- %matplotlib inline: Perintah khusus dalam jupyter, fungsinya memastikan bahwa plot yang dihasilkan oleh Matplotlib akan ditampilkan di dalam notebook.
- import skimage: Ini mengimport modul skimage. skimage (scikit-image) adalah kumpulan algoritma untuk pengolahan citra yang dibangun di atas NumPy, SciPy, dan Matplotlib.
  
***Membaca Citra***<br>
```
image = cv2.imread("2.jpg")
```
- Perintah diatas berfungsi untuk membaca citra dengan nama '2.jpg' yang selanjutnya akan disimpan dalam sebuah variable image

***Citra ke Grayscale, dan Deteksi Tepi dengan Canny***<br>
```
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
edges = cv2.Canny(image, 100, 150)
```
- gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY): Mengonversi citra berwarna yang disimpan dalam variabel image menjadi citra skala abu-abu.
- edges = cv2.Canny(image, 100, 150): Mendeteksi tepi pada citra dengan menggunakan algoritma deteksi tepi Canny, dengan ambang batas minimum 100 dan ambang batas maksimum 150.

***Visualisasi Citra Asli dan Hasil Deteksi Tepi dalam Plot***
```
fig,axs = plt.subplots(1,2, figsize = (18,10)) 
ax = axs.ravel()

ax[0].imshow(gray,cmap = "gray")
ax[0].set_title("Gambar Asli Grayscale")

ax[1].imshow(edges, cmap="gray")
ax[1].set_title("Gambar Hasil Deteksi Garis")

plt.show()
```
- fig, axs = plt.subplots(1, 2, figsize=(18, 10)): Membuat sebuah figure dengan dua subplot (1 baris, 2 kolom) dan menentukan ukuran figure menjadi 18x10 inci.
- ax = axs.ravel(): Mengubah array dua dimensi dari objek subplot menjadi array satu dimensi.
- ax[0].imshow(gray, cmap="gray"): Menampilkan citra grayscale (gray) pada subplot pertama (kiri) dengan colormap grayscale.
- ax[0].set_title("Gambar Asli Grayscale"): Memberi judul "Gambar Asli Grayscale" pada subplot pertama.
- ax[1].imshow(edges, cmap="gray"): Menampilkan citra hasil deteksi tepi (edges) pada subplot kedua (kanan) dengan colormap grayscale.
- ax[1].set_title("Gambar Hasil Deteksi Garis"): Memberi judul "Gambar Hasil Deteksi Garis" pada subplot kedua.
- plt.show(): Menampilkan figure dengan kedua subplot yang berisi citra.

***Deteksi Garis dengan Hough Transform***<br>
```
lines = cv2.HoughLinesP(edges, 1, np.pi/180, threshold=10, maxLineGap=-15)
image_line = image.copy()
```
- lines = cv2.HoughLinesP(edges, 1, np.pi/180, threshold=10, maxLineGap=-15): Mendeteksi garis dalam citra hasil deteksi tepi (edges) menggunakan algoritma HoughLinesP dengan parameter:

  - edges: Citra biner hasil deteksi tepi.
  - 1: Resolusi jarak (rho) dalam piksel.
np.pi/180: Resolusi sudut (theta) dalam radian.
  - threshold=10: Ambang batas minimum untuk mendeteksi sebuah garis.
  - maxLineGap=-15: Parameter yang menunjukkan jarak maksimum antar segmen garis yang dapat dihubungkan menjadi satu garis tunggal (nilai negatif mungkin tidak diinginkan, biasanya nilai positif digunakan).
  - image_line = image.copy(): Membuat salinan dari citra asli (image) untuk kemudian digunakan menggambar garis-garis yang terdeteksi tanpa mengubah citra asli.
- image_line = image.copy(): Membuat salinan dari citra asli (image) untuk kemudian digunakan menggambar garis-garis yang terdeteksi tanpa mengubah citra asli.

***Menggambar Garis pada Gambar dengan Loop***<br>
```
for line in lines:
    x1,y1, x2, y2 = line[0]
    cv2.line(image_line,(x1,y1), (x2,y2), (100,8,255),1)
```
- for line in lines:: Melakukan iterasi melalui setiap garis yang terdeteksi oleh cv2.HoughLinesP.
- x1, y1, x2, y2 = line[0]: Mengambil koordinat awal (x1, y1) dan akhir (x2, y2) dari setiap garis yang terdeteksi.
- cv2.line(image_line, (x1, y1), (x2, y2), (100, 8, 255), 1): Menggambar garis pada citra image_line dengan koordinat yang ditentukan, menggunakan warna (100, 8, 255) dalam format BGR, dan ketebalan garis 1 piksel.

***Menampilkan hasil olah Citra***<br>
```
fig,axs = plt.subplots(1,2, figsize = (10,10))
ax = axs.ravel()

ax[0].imshow(gray,cmap = "gray")
ax[0].set_title("Gambar Asli")

ax[1].imshow(image_line,cmap = "gray")
ax[1].set_title("Gambar hasil seleksi tepi")
```
- fig, axs = plt.subplots(1, 2, figsize=(10, 10)): Membuat sebuah figure dengan dua subplot (1 baris, 2 kolom) dan menentukan ukuran figure menjadi 10x10 inci.
- ax = axs.ravel(): Mengubah array dua dimensi dari objek subplot menjadi array satu dimensi untuk memudahkan iterasi dan akses.
- ax[0].imshow(gray, cmap="gray"): Menampilkan citra grayscale (gray) pada subplot pertama (kiri) dengan menggunakan colormap grayscale.
- ax[0].set_title("Gambar Asli"): Memberi judul "Gambar Asli" pada subplot pertama.
- ax[1].imshow(image_line, cmap="gray"): Menampilkan citra hasil deteksi garis (image_line) pada subplot kedua (kanan) dengan menggunakan colormap grayscale.
- ax[1].set_title("Gambar hasil seleksi tepi"): Memberi judul "Gambar hasil seleksi tepi" pada subplot kedua.
- plt.show(): Menampilkan figure dengan kedua subplot yang berisi citra.

## Laporan Praktikum 4 - Ekstraksi dengan Skimage, RGB to HSV
***Import Library***<br>
```
import cv2
import matplotlib.pyplot as plt
import skimage.io
import numpy as np
from skimage.feature import graycomatrix, graycoprops
```
- import cv2: Mengimpor OpenCV, sebuah pustaka yang banyak digunakan untuk berbagai tugas dalam pengolahan citra dan visi komputer, seperti membaca dan memanipulasi gambar dan video.

- import matplotlib.pyplot as plt: Mengimpor pyplot dari Matplotlib, sebuah pustaka yang digunakan untuk membuat visualisasi data, seperti plot dan grafik.

- import skimage.io: Mengimpor modul input/output dari skimage (scikit-image), yang menyediakan fungsi untuk membaca dan menulis gambar dalam berbagai format.

- import numpy as np: Mengimpor NumPy, sebuah pustaka fundamental untuk komputasi ilmiah dengan Python yang mendukung array multidimensi dan berbagai fungsi matematika.

- from skimage.feature import graycomatrix, graycoprops: Mengimpor fungsi graycomatrix dan graycoprops dari modul fitur di skimage.

  - graycomatrix: Digunakan untuk menghitung matriks co-occurrence (co-occurrence matrix) dalam skala abu-abu (gray-level co-occurrence matrix, GLCM) yang mengukur seberapa sering pasangan piksel dengan nilai tertentu dan hubungan spasial tertentu terjadi dalam citra.
  - graycoprops: Digunakan untuk menghitung properti tekstur dari matriks co-occurrence yang telah dihitung oleh graycomatrix, seperti kontras, homogenitas, energi, dan korelasi.

***Membaca Citra***<br>
```
image = cv2.imread("2.jpg")
```
- Perintah diatas berfungsi untuk membaca citra dengan nama '2.jpg' yang selanjutnya akan disimpan dalam sebuah variable image.

***Konversi RGB ke HSV***<br>
```
img_hsv = cv2.cvtColor(img, cv2.COLOR_RGB2HSV)
```
- img_hsv = cv2.cvtColor(img, cv2.COLOR_RGB2HSV): Mengonversi gambar yang disimpan dalam variabel img dari ruang warna RGB ke ruang warna HSV menggunakan fungsi cv2.cvtColor dari OpenCV. Ruang warna HSV (Hue, Saturation, Value) memisahkan informasi warna (hue), kejenuhan (saturation), dan kecerahan (value/intensity).
  
***Ekstraksi kanal V dari gambar HSV***<br>
```
img_v = img_hsv[:, :, 2]
```
- img_v = img_hsv[:, :, 2]: Mengekstraksi kanal kecerahan (Value, V) dari citra HSV yang telah dikonversi. Kanal V adalah kanal ke-3 dalam ruang warna HSV yang menyimpan informasi kecerahan atau intensitas piksel.
  
***Gray level Co-occurrence Matrix (GLCM) pada kanal V***
```
glcm = graycomatrix(img_v, distances=[1], angles=[0], levels=256, symmetric=True, normed=True)
```
- glcm = graycomatrix(img_v, distances=[1], angles=[0], levels=256, symmetric=True, normed=True): Menghitung Gray-Level Co-occurrence Matrix (GLCM) dari kanal V citra.

***Menghitung Properti GLCM***<br>
```
contrast = graycoprops(glcm, 'contrast')[0, 0]
dissimilarity = graycoprops(glcm, 'dissimilarity')[0, 0]
homogeneity = graycoprops(glcm, 'homogeneity')[0, 0]
energy = graycoprops(glcm, 'energy')[0, 0]
correlation = graycoprops(glcm, 'correlation')[0, 0]
ASM = graycoprops(glcm, 'ASM')[0, 0]
```
- contrast = graycoprops(glcm, 'contrast')[0, 0]: Menghitung kontras (contrast) dari GLCM menggunakan fungsi graycoprops dari scikit-image. Kontras mengukur perbedaan intensitas antara pasangan piksel yang berdekatan.

- dissimilarity = graycoprops(glcm, 'dissimilarity')[0, 0]: Menghitung disimilaritas (dissimilarity) dari GLCM. Dissimilarity mengukur seberapa berbedanya intensitas antara pasangan piksel.

- homogeneity = graycoprops(glcm, 'homogeneity')[0, 0]: Menghitung homogenitas (homogeneity) dari GLCM. Homogenitas mengukur seberapa seragam intensitas antara pasangan piksel.

- energy = graycoprops(glcm, 'energy')[0, 0]: Menghitung energi (energy) dari GLCM. Energi merupakan akar kuadrat dari jumlah kuadrat elemen GLCM, dan mengukur homogenitas tekstur.

- correlation = graycoprops(glcm, 'correlation')[0, 0]: Menghitung korelasi (correlation) dari GLCM. Korelasi mengukur hubungan linier antara intensitas piksel dalam pasangan.

- ASM = graycoprops(glcm, 'ASM')[0, 0]: Menghitung angular second moment (ASM) dari GLCM. ASM juga dikenal sebagai kontras kedua, dan merupakan ukuran kedekatan distribusi keabuan dengan nol.

***Plot Gambar RGB dan HSV***<br>
```
fig, axs = plt.subplots(1, 3, figsize=(20, 10))

ax = axs.ravel()

ax[0].imshow(img)
ax[0].set_title("RGB")

ax[1].imshow(img_hsv)
ax[1].set_title("HSV")

ax[2].imshow(img_v, cmap='gray')
ax[2].set_title("Kanal V dari HSV")

plt.show()
```
- fig, axs = plt.subplots(1, 3, figsize=(20, 10)): Membuat sebuah figure dengan tiga subplot (1 baris, 3 kolom) dan menentukan ukuran figure menjadi 20x10 inci.

- ax = axs.ravel(): Mengubah array dua dimensi dari objek subplot menjadi array satu dimensi untuk memudahkan iterasi dan akses.

- ax[0].imshow(img): Menampilkan citra asli (img) pada subplot pertama (kiri) tanpa spesifikasi colormap, sehingga akan menggunakan colormap default untuk citra RGB.

- ax[0].set_title("RGB"): Memberi judul "RGB" pada subplot pertama untuk menunjukkan bahwa ini adalah citra dalam ruang warna RGB.

- ax[1].imshow(img_hsv): Menampilkan citra dalam format HSV (img_hsv) pada subplot kedua (tengah) tanpa spesifikasi colormap.

- ax[1].set_title("HSV"): Memberi judul "HSV" pada subplot kedua untuk menunjukkan bahwa ini adalah citra dalam ruang warna HSV.

- ax[2].imshow(img_v, cmap='gray'): Menampilkan kanal V dari citra HSV (img_v) pada subplot ketiga (kanan) dengan menggunakan colormap grayscale ('gray').

- ax[2].set_title("Kanal V dari HSV"): Memberi judul "Kanal V dari HSV" pada subplot ketiga untuk menunjukkan bahwa ini adalah kanal V dari citra dalam ruang warna HSV.

- plt.show(): Menampilkan figure dengan ketiga subplot yang berisi citra-citra dalam representasi yang berbeda.
***Mencetak Properti GLCM***<br>
```
print(f"Contrast: {contrast}")
print(f"Dissimilarity: {dissimilarity}")
print(f"Homogeneity: {homogeneity}")
print(f"Energy: {energy}")
print(f"Correlation: {correlation}")
print(f"ASM: {ASM}")
```
- fig, axs = plt.subplots(1, 3, figsize=(20, 10)): Membuat sebuah figure dengan tiga subplot (1 baris, 3 kolom) dan menentukan ukuran figure menjadi 20x10 inci.

- ax = axs.ravel(): Mengubah array dua dimensi dari objek subplot menjadi array satu dimensi untuk memudahkan iterasi dan akses.

- ax[0].imshow(img): Menampilkan citra asli (img) pada subplot pertama (kiri) tanpa spesifikasi colormap, sehingga akan menggunakan colormap default untuk citra RGB.

- ax[0].set_title("RGB"): Memberi judul "RGB" pada subplot pertama untuk menunjukkan bahwa ini adalah citra dalam ruang warna RGB.

- ax[1].imshow(img_hsv): Menampilkan citra dalam format HSV (img_hsv) pada subplot kedua (tengah) tanpa spesifikasi colormap.

- ax[1].set_title("HSV"): Memberi judul "HSV" pada subplot kedua untuk menunjukkan bahwa ini adalah citra dalam ruang warna HSV.

- ax[2].imshow(img_v, cmap='gray'): Menampilkan kanal V dari citra HSV (img_v) pada subplot ketiga (kanan) dengan menggunakan colormap grayscale ('gray').

- ax[2].set_title("Kanal V dari HSV"): Memberi judul "Kanal V dari HSV" pada subplot ketiga untuk menunjukkan bahwa ini adalah kanal V dari citra dalam ruang warna HSV.

- plt.show(): Menampilkan figure dengan ketiga subplot yang berisi citra-citra dalam representasi yang berbeda.
