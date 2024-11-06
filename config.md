1. R1 CR (Router 1 - CR):
- Terhubung dengan sebuah laptop melalui antarmuka Ethernet (eth2) dengan alamat IP 192.168.11.1/24.
- Pada antarmuka eth3, terhubung ke jaringan 10.10.10.0/24 dengan alamat IP 10.10.10.1.
- Terhubung ke R3 KHI melalui antarmuka eth4 dengan jaringan 30.30.30.0/24 dan alamat IP 30.30.30.1.

2. R2 KJ (Router 2 - KJ):
- Terhubung dengan sebuah laptop melalui antarmuka Ethernet (eth2) dengan alamat IP 192.168.22.1/24.
- Pada antarmuka eth3, terhubung ke jaringan 20.20.20.0/24 dengan alamat IP 20.20.20.1.
- Terhubung ke R3 KHI melalui antarmuka eth4 dengan jaringan 30.30.30.0/24 dan alamat IP 30.30.30.2.

3. R3 KHI (Router 3 - KHI):
- Terhubung dengan sebuah laptop melalui antarmuka Ethernet (eth2) dengan alamat IP 192.168.33.1/24.
- Terhubung ke R1 CR melalui antarmuka eth4 pada jaringan 10.10.10.0/24 dengan alamat IP 10.10.10.2.
- Terhubung ke R2 KJ melalui antarmuka eth3 pada jaringan 20.20.20.0/24 dengan alamat IP 20.20.20.2.

4. Jaringan dan Alamat IP:
- 192.168.11.0/24: Digunakan oleh perangkat laptop yang terhubung ke R1.
- 192.168.22.0/24: Digunakan oleh perangkat laptop yang terhubung ke R2.
- 192.168.33.0/24: Digunakan oleh perangkat laptop yang terhubung ke R3.
- 10.10.10.0/24: Jaringan penghubung antara R1 dan R3.
- 20.20.20.0/24: Jaringan penghubung antara R2 dan R3.
- 30.30.30.0/24: Jaringan penghubung antara R1 dan R2.

5. Penggunaan Routing RIP:
RIP digunakan untuk pertukaran informasi rute antar router (R1, R2, dan R3) agar setiap router dapat mengetahui jalur ke jaringan lainnya.
Setiap router akan mengirimkan tabel rute secara berkala untuk memastikan setiap perangkat dalam jaringan dapat saling berkomunikasi.
