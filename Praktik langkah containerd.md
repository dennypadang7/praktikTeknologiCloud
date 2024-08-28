# Praktik Containerd di Windows menggunakan WSL2
Panduan ini menjelaskan langkah-langkah untuk menginstal dan mengkonfigurasi Containerd di Windows melalui WSL2, termasuk langkah-langkah untuk menarik dan menjalankan container menggunakan CLI ctr.

## Langkah 1: Setup WSL2 di Windows
 # Aktifkan WSL2
    Buka PowerShell sebagai Administrator dan jalankan perintah berikut:

     wsl --install
 
## Setelah proses instalasi selesai, restart komputer Anda jika diminta.

 # Instalasi Distribusi Linux (Ubuntu)
Buka Microsoft Store, cari 'Ubuntu', dan instal distribusi tersebut. Setelah instalasi, jalankan Ubuntu untuk menyelesaikan konfigurasi awal.
 # Set WSL2 sebagai Default
Buka kembali PowerShell dan jalankan perintah berikut untuk memastikan WSL2 digunakan sebagai versi default:

 wsl --set-default-version 2

 Anda bisa memverifikasi bahwa Ubuntu menggunakan WSL2 dengan menjalankan:

 wsl -l -v
 
## Langkah 2: Instalasi Docker Desktop dan Konfigurasi WSL2
 # Unduh dan Instal Docker Desktop
Kunjungi situs Docker Desktop dan unduh installer untuk Windows. Jalankan installer dan ikuti petunjuk instalasi.
 # Integrasi Docker dengan WSL2
Buka Docker Desktop, masuk ke Settings > General dan aktifkan opsi 'Use the WSL 2 based engine'. Di Settings > Resources > WSL Integration, pilih distribusi Ubuntu untuk diintegrasikan.
## Langkah 3: Instalasi Containerd di WSL2
 # Instal Containerd
Buka terminal Ubuntu di WSL2 dan jalankan perintah berikut untuk memperbarui paket dan menginstal Containerd:

 sudo apt-get update
 sudo apt-get install -y containerd

 # Konfigurasi Containerd
Buat file konfigurasi default untuk Containerd dengan menjalankan perintah:

sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml

 Anda bisa mengedit file konfigurasi `/etc/containerd/config.toml` jika diperlukan, tetapi pengaturan default sudah cukup untuk memulai.

# Mulai Containerd
Mulai layanan Containerd dengan menjalankan perintah berikut:

sudo systemctl start containerd

 Untuk memastikan bahwa Containerd berjalan dengan baik, gunakan perintah:

 sudo systemctl status containerd
 
## Langkah 4: Menjalankan Container Menggunakan Containerd
 # Tarik Image dari Docker Hub
Untuk menarik image `hello-world` dari Docker Hub, jalankan perintah:

sudo ctr image pull docker.io/library/hello-world:latest
 
# Jalankan Container
Untuk menjalankan container `hello-world`, gunakan perintah berikut:

sudo ctr run --rm -t docker.io/library/hello-world:latest hello-world

 Jika berhasil, Anda akan melihat output 'Hello from Docker!' di terminal.
Langkah 5: Mengelola Container dan Resources
# Lihat Daftar Container yang Berjalan
Untuk melihat daftar container yang sedang berjalan, jalankan perintah:

sudo ctr container list

 # Hentikan dan Hapus Container
Untuk menghentikan container, gunakan perintah:

sudo ctr tasks kill <container-id>

 Untuk menghapus container yang dihentikan, jalankan perintah:

 sudo ctr container delete <container-id>

 # Hapus Image yang Tidak Diperlukan
Untuk membersihkan image yang tidak diperlukan lagi, jalankan:

sudo ctr image remove docker.io/library/hello-world:latest
 
