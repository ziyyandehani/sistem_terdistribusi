# MQTT Project
## Cara Menjalankan

1. Jalankan service MQTT:
   
   ```bash
    docker compose -f compose/mqtt.yml up -d

![Screenshot](/MQTT/images/docker_up-d_start.png)

Perintah ini digunakan untuk membuat dan menyalakan container MQTT (publisher dan subscriber) secara background.
Untuk mengecek apakah container sudah berjalan. saya jalankan:
    
    ```bash
    docker ps

![Screenshot](/MQTT/images/docker_ps.png)

2. Mengecek network interface Docker
Setelah itu saya jalankan:
    
    ```bash
     ip a

![Screenshot](/MQTT/images/ip-a.png)


Dari sini terlihat bahwa Docker membuat sebuah jembatan jaringan (bridge) baru. Alamat bridge inilah yang nantinya dipakai untuk melakukan capture trafik dengan tcpdump.

3. Menangkap trafik MQTT
Dengan mengetahui interface bridge, saya kemudian menjalankan:
    
    ```bash
    sudo tcpdump -nnvi <nama_bridge> -w mqtt.pcap

![Screenshot](/MQTT/images/menangkap-trafik.png)

Perintah ini akan menyimpan semua paket yang lewat di interface tersebut ke dalam file mqtt.pcap agar bisa dianalisis lebih lanjut.

4. Menjalankan subscriber dan publisher
Supaya ada trafik MQTT yang lewat, saya jalankan subscriber dan publisher dari dalam container:
    
    ```bash
    docker compose -f compose/mqtt.yml exec mqtt-sub python sub.py
    docker compose -f compose/mqtt.yml exec mqtt-pub python pub.py

![Screenshot](/MQTT/images/menangkap-trafik.png)

Subscriber mendengarkan topik tertentu, sedangkan publisher mengirimkan pesan ke topik itu.

5. Menghentikan subscriber dan publisher
Setelah selesai, saya menekan Ctrl + C pada keduanya dan mqtt.pcap untuk menghentikan proses.

![Screenshot](/MQTT/images/menutup-tangkapan.png)

6. Menghentikan layanan Docker Compose
Layanan kemudian dimatikan dengan:
    
    ```bash
     docker compose -f compose/mqtt.yml down

![Screenshot](/MQTT/images/docker-otw-down.png)
![Screenshot](/MQTT/images/docker-removed.png)

Ketika ini dilakukan, bridge network yang sebelumnya muncul juga ikut hilang, sehingga jika hendak menangkap trafik disana, pesannya seperti ini:

![Screenshot](/MQTT/images/bridge-notfound.png)

---------------------------------------------------------------------------------

## Modifikasi Kode Program

Memodifikasi nama topic dan pesan yang dikirim

1. Kode Publisher 
Sebelum dimodifikasi 

    ```python
     topic = "sister/temp"
     suhu = 28  # Contoh nilai suhu

     message = f"Suhu {suhu} °C"
    ```
    #Dimodifikasi menjadi
    ```python
     topic = "sister/dht22_data"
     hum = 60  # Contoh nilai kelembaban
     suhu = 25  # Contoh nilai suhu

     message = f"Suhu {suhu} °C | Kelembaban: {hum} %"
    ```
2. Kode Subcriber 
Sebelum dimodifikasi

    ```python
    topic = "sister/temp"
    ```

Dimodifikasi menjadi

    ```python
    topic = "sister/dht22_data"

3. Hasil 

![Screenshot](/MQTT/images/hasil-modif.png)





