# CitraDigital (Kematangan Buah)
Alfia Rohmah Safara (23422039)

```python
import cv2 
import numpy as np 
from google.colab import files 
import io 
 
# Fungsi untuk mendeteksi kematangan tomat 
def deteksi_kematangan_tomat(image_path): 
    # Membaca gambar dari file 
    img = cv2.imread(image_path) 
     
    if img is None: 
        print(f"Error: Gambar '{image_path}' tidak ditemukan atau tidak dapat dibaca.") 
        return 
     
    # Mengubah gambar dari BGR ke HSV (Hue, Saturation, Value) 
    hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV) 
 
    # Menentukan rentang warna merah pada HSV 
    lower_red = np.array([0, 50, 50])   # Rentang bawah untuk warna merah 
    upper_red = np.array([10, 255, 255]) # Rentang atas untuk warna merah 
    mask1 = cv2.inRange(hsv_img, lower_red, upper_red) 
 
    lower_red2 = np.array([170, 50, 50])  # Rentang bawah untuk warna merah (warna merah lebih tua) 
    upper_red2 = np.array([180, 255, 255]) # Rentang atas untuk warna merah lebih tua 
    mask2 = cv2.inRange(hsv_img, lower_red2, upper_red2) 
 
    # Gabungkan dua masker (warna merah dari dua rentang) 
    mask = cv2.bitwise_or(mask1, mask2) 
 
    # Menghitung area yang terdeteksi sebagai merah 
    red_area = np.sum(mask > 0) 
 
    # Tentukan kematangan berdasarkan area warna merah 
    total_area = img.shape[0] * img.shape[1] 
    percentage_red = (red_area / total_area) * 100 
 
    # Menentukan kematangan berdasarkan persentase area merah 
    if percentage_red > 75: 
        result = "Tomat sangat matang" 
    elif percentage_red > 50: 
        result = "Tomat matang" 
    elif percentage_red > 25: 
        result = "Tomat hampir matang" 
    else: 
        result = "Tomat belum matang" 
 
    # Menampilkan hasil dan gambar yang diproses 
    print(f"Hasil: {result} ({percentage_red:.2f}% area merah)") 
 
    # Menampilkan gambar asli dan gambar hasil deteksi 
    from matplotlib import pyplot as plt 
    plt.figure(figsize=(10, 5)) 
 
    # Gambar Asli 
    plt.subplot(1, 2, 1) 
    plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB)) 
    plt.title("Gambar Asli") 
    plt.axis('off') 
 
    # Gambar Masker Merah 
    plt.subplot(1, 2, 2) 
    plt.imshow(mask, cmap='gray') 
    plt.title("Masker Merah") 
    plt.axis('off') 
 
    plt.show() 
 
# Fungsi untuk mengunggah file gambar 
def upload_file(): 
    uploaded = files.upload()  # Memulai upload file 
    for filename in uploaded.keys(): 
        print(f"File {filename} berhasil diunggah!") 
        # Memproses gambar yang diunggah 
        deteksi_kematangan_tomat(filename) 
 
# Panggil fungsi untuk mengunggah file gambar 
upload_file()


program ini untuk mendeteksi kematangan buah tomat berdasarkan warna, dan progam ini menggunakan bahasa pemrograman python
