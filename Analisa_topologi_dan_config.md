# Analisa Topologi


1. Kampus CR (192.168.10.0/24):
   - Kampus CR memiliki jaringan lokal dengan alamat IP dalam subnet 192.168.10.0/24.
   - Terdapat tiga komputer (PC A1, PC A2, dan PC A3) yang terhubung ke Switch CR.
   - Switch CR terhubung ke Router CR melalui port ether 2 pada alamat 192.168.10.1 yang berfungsi sebagai gateway bagi jaringan lokal di Kampus CR.
   - Router CR juga memiliki koneksi WAN dengan alamat IP publik 203.0.113.1, yang digunakan untuk terhubung ke internet dan menghubungkan Kampus CR dengan Kampus KJ dan KHI melalui tunneling.

2. Kampus KJ (192.168.20.0/24):
   - Kampus KJ memiliki jaringan lokal dengan subnet 192.168.20.0/24.
   - Tiga komputer (PC B1, PC B2, dan PC B3) terhubung ke Switch KJ.
   - Router KJ berfungsi sebagai gateway untuk jaringan lokal Kampus KJ, dengan IP 192.168.20.1 pada port ether 2.
   - Router KJ memiliki koneksi internet dengan IP publik 203.0.113.2 dan terhubung ke Router CR dan Router KHI melalui tunnel.
  
3. Kampus KHI (192.168.30.0/24):
   - Kampus KHI memiliki jaringan lokal dengan subnet 192.168.30.0/24.
   - Tiga komputer (PC C1, PC C2, dan PC C3) terhubung ke Switch KHI.
   - Router KHI berfungsi sebagai gateway untuk jaringan lokal Kampus KHI dengan IP 192.168.30.1 pada port ether 2.
   - Router KHI terhubung ke internet dengan IP publik 203.0.113.3 dan menghubungkan Kampus KHI ke Kampus CR dan KJ melalui tunneling.

4. Koneksi Antar Kampus melalui Tunneling:
   - Router CR, KJ, dan KHI terhubung satu sama lain menggunakan tunnel point-to-point untuk membuat jaringan terisolasi antar kampus.
   - Tunneling digunakan untuk menghubungkan jaringan internal setiap kampus dengan alamat IP yang berbeda:
        - Router CR menggunakan subnet 10.0.1.0/30 untuk menghubungkan ke Router KJ dan 10.0.2.0/30 untuk menghubungkan ke Router KHI.
        - Router KJ menggunakan subnet 10.0.3.0/30 untuk menghubungkan ke Router KHI.
   - Alamat IP publik digunakan untuk mengidentifikasi router setiap kampus, yang mengarah ke internet dan untuk komunikasi antar-router


# Konfigurasi Router

1. Konfigurasi Router CR
enable
configure terminal

! Konfigurasi LAN ke Switch CR
interface Ethernet0/2
 ip address 192.168.10.1 255.255.255.0
 no shutdown

! Konfigurasi Tunnel ke Router KJ
interface Tunnel0
 ip address 10.0.1.1 255.255.255.252
 tunnel source Ethernet0/1
 tunnel destination 203.0.113.2

! Konfigurasi Tunnel ke Router KHI
interface Tunnel1
 ip address 10.0.2.1 255.255.255.252
 tunnel source Ethernet0/1
 tunnel destination 203.0.113.3

! Konfigurasi WAN ke Internet
interface Ethernet0/1
 ip address 203.0.113.1 255.255.255.0
 no shutdown

! Routing statik untuk jaringan lain
ip route 192.168.20.0 255.255.255.0 10.0.1.2
ip route 192.168.30.0 255.255.255.0 10.0.2.2

end
write memory



2. Konfigurasi Router KJ
enable
configure terminal

! Konfigurasi LAN ke Switch KJ
interface Ethernet0/2
 ip address 192.168.20.1 255.255.255.0
 no shutdown

! Konfigurasi Tunnel ke Router CR
interface Tunnel0
 ip address 10.0.1.2 255.255.255.252
 tunnel source Ethernet0/1
 tunnel destination 203.0.113.1

! Konfigurasi Tunnel ke Router KHI
interface Tunnel1
 ip address 10.0.3.1 255.255.255.252
 tunnel source Ethernet0/1
 tunnel destination 203.0.113.3

! Konfigurasi WAN ke Internet
interface Ethernet0/1
 ip address 203.0.113.2 255.255.255.0
 no shutdown

! Routing statik untuk jaringan lain
ip route 192.168.10.0 255.255.255.0 10.0.1.1
ip route 192.168.30.0 255.255.255.0 10.0.3.2

end
write memory



3. Konfigurasi Router KHI
enable
configure terminal

! Konfigurasi LAN ke Switch KHI
interface Ethernet0/2
 ip address 192.168.30.1 255.255.255.0
 no shutdown

! Konfigurasi Tunnel ke Router CR
interface Tunnel0
 ip address 10.0.2.2 255.255.255.252
 tunnel source Ethernet0/1
 tunnel destination 203.0.113.1

! Konfigurasi Tunnel ke Router KJ
interface Tunnel1
 ip address 10.0.3.2 255.255.255.252
 tunnel source Ethernet0/1
 tunnel destination 203.0.113.2

! Konfigurasi WAN ke Internet
interface Ethernet0/1
 ip address 203.0.113.3 255.255.255.0
 no shutdown

! Routing statik untuk jaringan lain
ip route 192.168.10.0 255.255.255.0 10.0.2.1
ip route 192.168.20.0 255.255.255.0 10.0.3.1

end
write memory



# Penjelasan Konfigurasi

- Interface Ethernet: Setiap router memiliki konfigurasi interface Ethernet yang terhubung ke jaringan lokal dengan IP address yang menjadi gateway untuk perangkat-perangkat di jaringan tersebut.
- Interface Tunnel: Konfigurasi tunneling antar-router menggunakan IP point-to-point untuk menghubungkan masing-masing kampus secara virtual. Tunnel ini juga memiliki konfigurasi sumber dan tujuan, yaitu IP publik router lain.
- Routing Static: Routing statik ditambahkan pada setiap router untuk mengarahkan lalu lintas ke subnet yang berada di kampus lain. Dengan cara ini, setiap router dapat mengarahkan paket data ke jaringan yang benar melalui interface tunneling yang telah disediakan.
