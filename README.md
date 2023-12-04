## Jarkom Modul 4 F08

    Nama : Keyisa Raihan Illah Setiadi
    NRP : 5025211002

    Nama : Syukra Wahyu Ramadhan
    NRP : 5025211037

    Kelas : JARKOM F

### VLSM
- Soal Topologi

    <img width="543" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/682a6227-a29a-45e9-bd2a-795af60aad05">

- Rute
  
  Kita Akan menghitung kebutuhan IP untuk semua node, setelah dihitung dibutuhkan 4255 Ip, sehingga netmask maksimal /19, dan terdapat 21 Subnet
  
    <img width="383" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/fc4b40f2-5a50-41d7-91de-b47ff747469d">

- Pembuatan Tree

  Kita perlu membuat Tree untuk membagi IP kepada setial subnet dari netmask tertinggi /19 terus menurun terbagi menjadi 2, dengan mengreservasi IP setiap kali menemukan subnet dengan netmask yang sesuai

    ![WhatsApp Image 2023-12-02 at 3 36 08 PM](https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/77d41d2d-4bf8-4386-8e31-fd1edceb1315)

- Pembagian IP

  Dengan berdasar tree yang telah dibuat, dibuatlah tabel pembagian IP untuk setiap subnet. setiap subnet didefinisikan Network ID atau titik awal dari IP Subnet, Broadcast yaitu IP akhir dari IP subnet yang tersedia, dan juga Netmask sesuai netmask setiap subnet dicocokkan dengan tabe netmask pada modul

    <img width="255" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/355f993b-c7ba-48f5-a857-95ebfd4d4323">

- GNS 3 (Topologi)

  Berdasar soal, kita buat topologi pada GNS 3
  
  <img width="576" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/3a6c8d48-2af1-4cf1-b695-a259a4a6f049">

- Subnetting

    Sesuai dengan pembagian IP yang ada pada tabel, ktia setting untuk setiap node sesuai dengan pembagian tersebut sehingga tidak terjadi tabrakan, setiap node perlu diberikan addres sesuai dengan etthernet dan pembagian ip, lalu diberikan netmask sesuai subnet netmasknya pada tabel, dan juga diberikan gateway bila ethernet tersebut merupakan pintu masuk dari router lainya *(Karena setting nya cukup banyak untuk menyambungkan ethernet, maka dicontohkan dengan Aura hingga subnet bagian atas kanan denken, royalcapital, willeregion)*
    - Aura
      ```
  
        # DHCP config for eth0
        auto eth0
        iface eth0 inet dhcp
        #	hostname ubuntu-1-1
        
        # Static config for eth1
        auto eth1
        iface eth1 inet static
        	address 192.225.24.133
        	netmask 255.255.255.252
        #	gateway 192.168.1.1
        #	up echo nameserver 192.168.1.1 > /etc/resolv.conf
        
        # Static config for eth2
        auto eth2
        iface eth2 inet static
        	address 192.225.24.129
        	netmask 255.255.255.252
        #	gateway 192.168.2.1
        #	up echo nameserver 192.168.2.1 > /etc/resolv.conf
        
        # Static config for eth3
        auto eth3
        iface eth3 inet static
        	address 192.225.24.137
        	netmask 255.255.255.252
        #	gateway 192.168.3.1
        #	up echo nameserver 192.168.3.1 > /etc/resolv.conf
      ```
    - Denken
      ```    
        # Static config for eth0
        auto eth0
        iface eth0 inet static
        	address 192.225.24.134
        	netmask 255.255.255.252
        	gateway 192.225.24.133
        #	up echo nameserver 192.168.0.1 > /etc/resolv.conf
        
        # Static config for eth1
        auto eth1
        iface eth1 inet static
        	address 192.225.23.1
        	netmask 255.255.255.0
        #	gateway 192.168.1.1
        #	up echo nameserver 192.168.1.1 > /etc/resolv.conf
      ```
    - RoyalCapital
      ```
        # Static config for eth0
        auto eth0
        iface eth0 inet static
        	address 192.225.23.2
        	netmask 255.255.255.0
        	gateway 192.225.23.1
        #	up echo nameserver 192.168.0.1 > /etc/resolv.conf
      ```
    - WilleRegion
      ```    
        # Static config for eth0
        auto eth0
        iface eth0 inet static
        	address 192.225.23.3
        	netmask 255.255.255.0
        	gateway 192.225.23.1
        #	up echo nameserver 192.168.0.1 > /etc/resolv.conf
      ```

- Routing
      
  Kita perlu membuat routing dari subnet kecil kecil ke router terdekat yang tidak tersambung langsung, lalu akan ditambahkan ke router selanjutnya yang terdekat agar setiap node saling tersambung, lakukan terus hingga semua tersambung melewati aura
    - Flamme
      
      Kita Perlu menyambungkan flamme dengan subnet dari router fern diatasnya, dan juga himmel dibawahnya
      ```
      up route add -net 192.225.8.0 netmask 255.255.248.0 gw 192.225.24.150
      up route add -net 192.225.24.104 netmask 255.255.255.248 gw 192.225.24.146
      ```
      
    - Frieren

      Kita perlu menyambungkan frieren dengan subnet dari router fern, dan juga himmel, tapi dengan gateway IP Flamme, ditambah dengan subnet Flamme ke RohrRoad karena tidak terhubung langsung
      ```
      up route add -net 192.225.8.0 netmask 255.255.248.0 gw 192.225.24.142
      up route add -net 192.225.24.104 netmask 255.255.255.248 gw 192.225.24.142
      up route add -net 192.225.0.0 netmask 255.255.252.0 gw 192.225.24.142
      ```
    - Lawine

      Kita perlu menyambungan lawine dengan subnet pada router Heiter
      ```
      up route add -net 192.225.16.0 netmask 255.255.252.0 gw 192.225.24.3
      ```
    -  Linie
 
       Kita perlu menyambungan Linie dengan subnet pada router Heiter, tapi dengan IP gatewat Lawine, ditambah dengan subnet Lawine ke BredtRegion karena tidak terhubung langsung
       ```
       up route add -net 192.225.16.0 netmask 255.255.252.0 gw 192.225.24.114
       up route add -net 192.225.24.0 netmask 255.255.255.192 gw 192.225.24.114
       ```
    -  Eisen

       Kita perlu meynambukan Elsen dengan subnet pada router Heiter, Lawine tapi dengan gateway Line, ditambah dengan subnet Linie ke lawine dan Linie ke GranzChannel. Lalu ktia juga perlu menambahkan sambungan ke subnet Lugner ke turk region dan juga GrobeForest.
       ```
       up route add -net 192.225.16.0 netmask 255.255.252.0 gw 192.225.24.118
       up route add -net 192.225.24.0 netmask 255.255.255.192 gw 192.225.24.118
       up route add -net 192.225.24.112 netmask 255.255.255.252 gw 192.225.24.118
       up route add -net 192.225.20.0 netmask 255.255.254.0 gw 192.225.24.118
       up route add -net 192.225.22.0 netmask 255.255.255.0 gw 192.225.24.122
       up route add -net 192.225.4.0 netmask 255.255.252.0 gw 192.225.24.122
       up route add -net 192.225.4.0 netmask 255.255.252.0 gw 192.225.24.122
       ```
    -  Aura
 
       Kita Menyambungkan Aura dengan semua subnet dari kiri atas melalui gateway Frieren, tapi denga ntambahan subnet frieren ke LakeKorridor. Llau kita sambungkan subnet pada bagian bawah tapi dengan gateway Eisen tapi dengan tambahan Eisen ke ritcher dan recolter, dan juga Eisen ke Stark karena tidak tersambung langsung. Terakhir kita perlu menyambungkan Aura dengan subnet pada bagian atas kanan, yaitu dengan router Denken
       ```
       up route add -net 192.225.23.0 netmask 255.255.255.0 gw 192.225.24.134

        up route add -net 192.225.8.0 netmask 255.255.248.0 gw 192.225.24.138
        up route add -net 192.225.24.104 netmask 255.255.255.248 gw 192.225.24.138
        up route add -net 192.225.24.64 netmask 255.255.255.224 gw 192.225.24.138
        up route add -net 192.225.0.0 netmask 255.255.252.0 gw 192.225.24.138
        
        up route add -net 192.225.16.0 netmask 255.255.252.0 gw 192.225.24.130
        up route add -net 192.225.24.120 netmask 255.255.255.252 gw 192.225.24.130
        up route add -net 192.225.24.0 netmask 255.255.255.192 gw 192.225.24.130
        up route add -net 192.225.24.112 netmask 255.255.255.252 gw 192.225.24.130
        up route add -net 192.225.20.0 netmask 255.255.254.0 gw 192.225.24.130
        up route add -net 192.225.22.0 netmask 255.255.255.0 gw 192.225.24.130
        up route add -net 192.225.4.0 netmask 255.255.252.0 gw 192.225.24.130
        up route add -net 192.225.24.96 netmask 255.255.255.248 gw 192.225.24.130
        up route add -net 192.225.24.124 netmask 255.255.255.252 gw 192.225.24.130
       ```

- Testing
  
  Untuk testing kita akan mengetest ping antar node, akan ktia contohkan dengan beberapa node
  - LaubHills -> GranzChannel
    
    <img width="406" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/0b68bb31-aa58-4a88-ae51-04677b5a4614">

  - LaubHills -> RoyalCapital

    <img width="376" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/136c4bda-aad4-4dae-9747-8171e5c50229">

  - LaubHills -> RiegelCanyon

    <img width="380" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/f6f343ee-94a7-474b-8374-37ba12576ffd">

  - LaubHills -> TurkRegion

    <img width="381" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/d07ecd27-c255-4931-a611-aed444161acd">

  - LaubHills -> Stark

    <img width="379" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/d47f048b-9a8a-431a-98c2-0fcc3570713c">

  - LaubHills -> Ritcher
    
    <img width="385" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/22940dfe-4074-460a-8fc8-50642e45276d">

  - LaubHills -> SchwersMountain

    <img width="379" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/c92c008a-5819-4ddc-9ef8-5109b7aef258">
  - LaubHills -> LakeKorridor

    <img width="405" alt="image" src="https://github.com/MyNameIsSyukra/Jarkom-Modul-4-F08-2023/assets/85614845/489c8085-7183-40e3-b746-1b2d65d8a9ed">
      
### CIDR
