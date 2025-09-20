# UDP Project
## Cara Menjalankan

1. Jalankan service UDP:
   
   ```bash
    docker compose -f compose/udp.yml up -d

![Screenshot](/oneway/images/image3.png)

Perintah ini digunakan untuk membuat dan menyalakan container UDP (client dan server) secara background.
Untuk mengecek apakah container sudah berjalan. saya jalankan:
    
    ```bash
     docker ps

![Screenshot](/oneway/images/image6.png)

2. Mengecek network interface Docker
Setelah itu saya jalankan:
    
    ```bash
     ip a

![Screenshot](/oneway/images/image4.png)


Dari sini terlihat bahwa Docker membuat sebuah jembatan jaringan (bridge) baru. Alamat bridge inilah yang nantinya dipakai untuk melakukan capture trafik dengan tcpdump.

3. Menangkap trafik UDP
Dengan mengetahui interface bridge, saya kemudian menjalankan:
    
    ```bash
    sudo tcpdump -nnvi <nama_bridge> -w udp.pcap

Perintah ini akan menyimpan semua paket yang lewat di interface tersebut ke dalam file udp.pcap agar bisa dianalisis lebih lanjut.

4. Menjalankan client dan server
Supaya ada trafik udp yang lewat, saya jalankan client dan server dari dalam container:
    
    ```bash
    docker compose -f compose/udp.yml exec udp-server python serverUDP.py
    docker compose -f compose/udp.yml exec udp-client python clientUDP.py

![Screenshot](/oneway/images/image1.png)
![Screenshot](/oneway/images/image2.png)

Server mendapatkan pesan dari client, namun client akan langsung berhenti ketika tidak ada respon, dan untuk protokol UDP memang connectionless, server tidak memberikan acknowledgment kepada sisi client sehingga tidak ada pemberitahuan pesan yang dikirim sudah diterima atau tidak. 

5. Menghentikan client dan server
Setelah selesai, saya menekan Ctrl + C pada keduanya dan udp.pcap untuk menghentikan proses.

![Screenshot](/oneway/images/image4.png)

6. Menghentikan layanan Docker Compose
Layanan kemudian dimatikan dengan:
    
    ```bash
    docker compose -f compose/udp.yml down

![Screenshot](/oneway/images/image7.png)

-----------------------------------------------------------------------------------------------
## Wireshark
Paket yang sudah disimpan dalam file udp.pcap tadi kemudian dapat dibuka dan dibaca pesan di dalamnya menggunakan Wireshark

![Screenshot](/oneway/images/udp-wireshark.png)

