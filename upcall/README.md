# upcall Project
## Cara Menjalankan

1. Jalankan service upcall:
   
   ```bash
    docker compose -f compose/upcall.yml up -d

![Screenshot](/upcall/images/image1.png)

Perintah ini digunakan untuk membuat dan menyalakan container upcall (client and server) secara background.
Untuk mengecek apakah container sudah berjalan. saya jalankan:
    
    ```bash
    docker ps

![Screenshot](/upcall/images/image2.png)

2. Mengecek network interface Docker
Setelah itu saya jalankan:
    
    ```bash
     ip a

![Screenshot](/upcall/images/image3.png)


Dari sini terlihat bahwa Docker membuat sebuah jembatan jaringan (bridge) baru. Alamat bridge inilah yang nantinya dipakai untuk melakukan capture trafik dengan tcpdump.

3. Menangkap trafik upcall
Dengan mengetahui interface bridge, saya kemudian menjalankan:
    
    ```bash
    sudo tcpdump -nnvi <nama_bridge> -w upcall.pcap

Perintah ini akan menyimpan semua paket yang lewat di interface tersebut ke dalam file upcall.pcap agar bisa dianalisis lebih lanjut.

4. Menjalankan client dan server
Supaya ada trafik upcall yang lewat, saya jalankan subscriber dan publisher dari dalam container:
    
    ```bash
    docker compose -f compose/upcall.yml exec upcall-server python servercall.py
    docker compose -f compose/upcall.yml exec upcall-client python clientcall.py

![Screenshot](/upcall/images/image4.png)
![Screenshot](/upcall/images/image6.png)

Server mendengarkan mendapatkan pesan dari client.

5. Menghentikan tcpdump dan client
Setelah selesai, saya menekan Ctrl + C pada keduanya dan upcall.pcap untuk menghentikan proses.

![Screenshot](/upcall/images/image7.png)

6. Menghentikan layanan Docker Compose
Layanan kemudian dimatikan dengan:
    
    ```bash
     docker compose -f compose/upcall.yml down

![Screenshot](/upcall/images/image8.png)
