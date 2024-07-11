Penjelasan:
import cv2 : Mengimpor library OpenCV untuk pengolahan gambar
import numpy as np :Mengimpor library NumPy untuk operasi numerik.
import matplotlib.pyplot as plt :Mengimpor modul pyplot dari library Matplotlib untuk menampilkan gambar.

penjelasan def detect_leaves(image_path):fungsi Python yang dirancang untuk mendeteksi daun dalam sebuah gambar.
    # Load image
    original_image = cv2.imread(image_path): untuk memuat gambar dari jalur yang diberikan (image_path). Hasilnya adalah gambar yang disimpan dalam bentuk array NumPy.
    -penjelasan codingan dibawah ini yaitu memeriksa apakah gambar berhasil dimuat. Jika cv2.imread gagal memuat gambar (misalnya, jika jalur gambar salah atau file tidak ada),maka original_image akan menjadi None. Dalam kasus ini, program mencetak pesan kesalahan dan keluar dari fungsi dengan 
return.
    if original_image is None:
        print(f"Cannot read image at {image_path}")
        return

 # Convert image to HSV color space
    hsv_image = cv2.cvtColor(original_image, cv2.COLOR_BGR2HSV) melakukan konversi gambar dari ruang warna BGR (Blue-Green-Red) ke ruang warna HSV (Hue-Saturation-Value). 

 # Define range of green color in HSV
    lower_green = np.array([25, 40, 50])
    upper_green = np.array([95, 255, 255])
    penejelasan:
    Rentang Warna Hijau dalam HSV:
    Hue (H): Nilai hue menentukan jenis warna. Dalam konteks hijau, hue berkisar antara 25 hingga 95 derajat pada skala OpenCV (0-179).
    Saturation (S): Nilai saturasi menentukan intensitas atau kekayaan warna. Rentang 40 hingga 255 mencakup warna hijau mulai dari agak pudar hingga sangat kaya.
    Value (V): Nilai value menentukan kecerahan warna. Rentang 50 hingga 255 mencakup warna hijau mulai dari agak gelap hingga sangat terang.

    # Threshold the HSV image to get only green colors
    mask = cv2.inRange(hsv_image, lower_green, upper_green)
    penjelasan:berfungsi untuk menerapkan threshold pada gambar dalam ruang warna HSV untuk mendapatkan hanya warna hijau.

    # Apply morphology operations to remove small noise
    kernel = np.ones((5, 5), np.uint8)
    mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)
    mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)
    penjelasan: melakukan operasi morfologi untuk menghilangkan noise kecil dari masker biner yang telah dibuat

    # Find contours of objects in the mask
    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    penjelasan:digunakan untuk menemukan kontur objek dalam gambar biner (masker). 

    # Create a blank mask and draw contours on it
    leaf_mask = np.zeros_like(original_image)
    cv2.drawContours(leaf_mask, contours, -1, (255, 255, 255), thickness=cv2.FILLED)
    penjelasan:membuat sebuah masker kosong dan menggambar kontur pada masker tersebut.

     # Bitwise AND operation between original image and leaf mask
    segmented_leaves = cv2.bitwise_and(original_image, leaf_mask)
    penjelasan:melakukan operasi AND bitwise antara gambar asli (original_image) dan masker daun (leaf_mask)

    # Invert the mask to get fruit mask
    fruit_mask = cv2.bitwise_not(leaf_mask)
    penjelasan:melakukan invert (pembalikan) dari masker yang telah diberikan (leaf_mask) menggunakan operasi bitwise NOT dari OpenCV

     # Segment fruit from original image
    segmented_fruit = cv2.bitwise_and(original_image, fruit_mask)
    penjelasan:melakukan segmentasi buah dari gambar asli menggunakan masker (fruit_mask)

    # Plotting using matplotlib
    fig, axs = plt.subplots(2, 2, figsize=(10, 10))
    penjelasan:menggunakan pustaka Matplotlib untuk membuat sebuah figure (gambar) yang berisi beberapa subplots (plot kecil).

     axs[0, 0].imshow(cv2.cvtColor(original_image, cv2.COLOR_BGR2RGB))
     axs[0, 0].set_title('Original Image')
     penjelasan: digunakan untuk menampilkan gambar asli dalam sebuah subplot menggunakan Matplotlib dan OpenCV.

     axs[0, 1].imshow(cv2.cvtColor(leaf_mask, cv2.COLOR_BGR2RGB))
     axs[0, 1].set_title('Leaf Mask')
     penjelasan:digunakan untuk menampilkan gambar leaf_mask pada subplot tertentu dalam grid matplotlib dengan judul 'Leaf Mask'

     axs[1, 1].imshow(cv2.cvtColor(segmented_leaves, cv2.COLOR_BGR2RGB))
     axs[1, 1].set_title('Segmentasi daun')
     penjelasan:digunakan untuk menampilkan gambar hasil segmentasi daun dengan menggunakan Matplotlib

     axs[1, 0].imshow(cv2.cvtColor(segmented_fruit, cv2.COLOR_BGR2RGB))
     axs[1, 0].set_title('Segmentasi buah')
     penjelasan:Baris-baris kode berikut adalah bagian dari skrip Python yang menggunakan Matplotlib untuk menampilkan gambar yang telah disegmentasi.

     plt.tight_layout()
     plt.show()
     penjelasan:- plt.tight_layout() kita memastikan bahwa tata letak plot terlihat rapi dan profesional.
                - plt.show() menampilkan semua plot yang telah dibuat. Ketika Anda memanggil plt.show(), semua figure yang telah didefinisikan akan 
                              dirender dan ditampilkan pada layar.          
     # Example usage:
    image_path = 'buahapel.png'  # Replace with your image path
    detect_leaves(image_path)
    penjelasan:-image_path = 'buahapel.png' menetapkan variabel image_path dengan jalur ke file gambar, dalam hal ini 'buahapel.png'. Anda dapat                                                  mengganti 'buahapel.png' dengan jalur gambar yang ingin Anda proses.
               -detect_leaves(image_path)   memanggil fungsi detect_leaves dengan argumen image_path, yang berarti fungsi tersebut akan memproses                                                 gambar yang ditentukan oleh image_path.
  
    
