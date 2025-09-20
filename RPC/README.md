# RPC Project
## Cara Menjalankan

1. Jalankan service RPC:
   
   ```bash
    docker compose -f compose/rpc.yml up -d

![Screenshot](/RPC/images/image2.png)

Perintah ini digunakan untuk membuat dan menyalakan container RPC (client dan server) secara background.
Untuk mengecek apakah container sudah berjalan. saya jalankan:
    
    ```bash
    docker ps

![Screenshot](/RPC/images/image6.png)

2. Mengecek network interface Docker
Setelah itu saya jalankan:
    
    ```bash
     ip a

![Screenshot](/RPC/images/image3.png)


Dari sini terlihat bahwa Docker membuat sebuah jembatan jaringan (bridge) baru. Alamat bridge inilah yang nantinya dipakai untuk melakukan capture trafik dengan tcpdump.

3. Menangkap trafik UDP
Dengan mengetahui interface bridge, saya kemudian menjalankan:
    
    ```bash
    sudo tcpdump -nnvi <nama_bridge> -w rpc.pcap

Perintah ini akan menyimpan semua paket yang lewat di interface tersebut ke dalam file rpc.pcap agar bisa dianalisis lebih lanjut.

4. Menjalankan client dan server
Supaya ada trafik udp yang lewat, saya jalankan client dan server dari dalam container:
    
    ```bash
    docker compose -f compose/rpc.yml exec rpc-server python rpcserver.py
    docker compose -f compose/rpc.yml exec rpc-client python rpcclient.py

![Screenshot](/RPC/images/image4.png)

Server mendapatkan pesan dari client, namun client akan langsung berhenti setelah mendapatkan hasil perhitungan dari sisi server.

5. Menghentikan tcpdump
Setelah selesai, saya menekan Ctrl + C pada server dan tcpdump rpc.pcap untuk menghentikan proses.

![Screenshot](/RPC/images/image5.png)

6. Menghentikan layanan Docker Compose
Layanan kemudian dimatikan dengan:
    
    ```bash
    docker compose -f compose/rpc.yml down

![Screenshot](/RPC/images/image7.png)

------------------------------------------------------------------------------------------

## Modifikasi kode program

1. Memodifikasi kode program rpcserver.py
Sebelum modifikasi

    ```python

    @dispatcher.add_method
    def add(a, b):
        return a / b

    # Mendefinisikan metode perkalian
    @dispatcher.add_method
    def multiply(a, b):
        return a * b
    ```
    Dimodifikasi menjadi

    ```python

    @dispatcher.add_method
    def add(a, b):
        return a ** b

    # Mendefinisikan metode perkalian
    @dispatcher.add_method
    def multiply(a, b):
        return a * (a*b)
    ```

2. Hasil

![Screenshot](/RPC/images/image1.png)

