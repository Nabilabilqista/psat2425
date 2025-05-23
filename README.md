# Penilaian Sumatif Akhir Tahun
## Mapil DevOps XI TJKT 1 - Penilaian Praktek
### SMKN 1 Banyumas - TA. 2024 2025


#
# Cara mendeploy Aplikasi

## 1. Membuat EC2 Instance

1. Masuk ke **AWS Console**.
2. Buka **EC2**.
3. Pilih **Instances**.
4. Klik **Launch Instance**.
5. Konfigurasi instance:
   - **Name**: `nabila1` ~> namanya bebas
   - **AMI**: `Ubuntu`
   - **Instance type**: `t2.micro` (Free tier eligible)
   - **Key pair**: `vockey`
   - **Firewall (Security Group)**: `Create security group`
       Ceklis:
     - Allow SSH ~> port 22
     - Allow HTTPS ~> port 443
     - Allow HTTP ~> port 80
     - Pilih Annyware 0.0.0.0/0
6. Klik **Launch Instance**.
5. Klik **Connect**.

## 2. Membuka Terminal Instance

1. Klik **Intance ID** sesuai instance yang sudah dibuat
2. Klik **Connect**
3. Scroll ke bawah
4. Klik **Connect**

## 3. Masuk di EC2 Instance Connect

1. Masukkan perintah:
    $ nano otomatis.sh #!/bin/bash
    echo '#!/bin/bash
    sudo apt update -y
    sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
    sudo rm -rf /var/www/html/{,.}
    sudo git clone https://github.com/Nabilabilqista/psat2425 /var/www/html
    sudo chmod -R 777 /var/www/html
    echo DB_USER=admin > /var/www/html/.env
    echo DB_PASS=P4ssw0rd  >> /var/www/html/.env
    echo DB_NAME=nabila1  >> /var/www/html/.env
    echo DB_HOST=nabila.ctqieeo2mefg.us-east-1.rds.amazonaws.com >> /var/www/html/.env
    sudo apt install -y openssl
    sudo a2enmod ssl
    sudo a2ensite default-ssl.conf
    sudo systemctl reload apache2' > /home/ubuntu/otomatis.sh

    $ chmod +x otomatis.sh
    $ ./otomatis.sh

  Penjelasan:
    sudo git clone [nama repositories di github] /var/www/html
    DB_USER=....  ~> isi dengan username RDS (di bagian master username)
    DB_PASS=....  ~> isi dengan password RDS
    DB_NAME=....  ~> isi dengan nama database RDS (bebas bole)
    DB_HOST=....  ~> isi dengan Endpoint di RDS

## Uji Coba
1. Salin IP Publicnya
2. Tempel ke browser

## Selesai

## INFO TAMBAHAN:
## Troubleshooting AWS
Terkadang, saat menggunakan layanan AWS, kita bisa mengalami beberapa kendala teknis. Berikut ini adalah beberapa masalah umum beserta cara mengatasinya.

1. `Masalah:` ~> **Security group** (di bagian **EC2**)
Biasanya jika sudah punya security group, kita bisa hanya dengan:
- Pilih **Select existing security group**
- Pilih security group yang ingin di pakai, contoh: `SG-ServerDB`
Namun, jika terjadi error. Bisa menggunakan cara berikut. 
   **Solusi:**
- Masuk ke **AWS Console**
- Buka **EC2**
- Cari bagian **Security Groups**
- Pilih **Create security group**
    Ceklis:
   - Allow SSH ~> port 22
   - Allow HTTPS ~> port 443
   - Allow HTTP ~> port 80
   - Pilih Annyware 0.0.0.0/0

2. `Masalah` ~> **User data** (**EC2** bagian **Advanced details**)
Seharusnya, untuk bagian **User data** ini, diletakkan prompt berikut.
- Salin prompt di bawah ini
  #!/bin/bash
  echo '#!/bin/bash
  sudo apt update -y
  sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
  sudo rm -rf /var/www/html/{,.}
  sudo git clone [repository githubmu] /var/www/html
  sudo chmod -R 777 /var/www/html
  echo DB_USER=[username rds(master username)] > /var/www/html/.env
  echo DB_PASS=[password rds]  >> /var/www/html/.env
  echo DB_NAME=[nama database/bebas]  >> /var/www/html/.env
  echo DB_HOST=[endpoint rds] >> /var/www/html/.env
  sudo apt install -y openssl
  sudo a2enmod ssl
  sudo a2ensite default-ssl.conf
  sudo systemctl reload apache2' > /home/ubuntu/otomatis.sh

  chmod +x /home/ubuntu/otomatis.sh 
- Klik **Launch Instance**
- Klik **Intance ID** sesuai instance yang sudah dibuat
- Klik **Connect**
- Scroll ke bawah
- Klik **Connect**
- Masuk di **EC2 Instance Connect**
- Ketik perintah
  $ ./otomatis.sh

Namun, jika terjadi error, maka bisa menjalankan cara di bawah ini.
  **Solusi:**
- Masuk ke **EC2 Instance Connect**
- Masukkan perintah:
  $ nano otomatis.sh #!/bin/bash
  echo '#!/bin/bash
  sudo apt update -y
  sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
  sudo rm -rf /var/www/html/{,.}
  sudo git clone [repository githubmu] /var/www/html
  sudo chmod -R 777 /var/www/html
  echo DB_USER=[username rds(master username)] > /var/www/html/.env
  echo DB_PASS=[password rds]  >> /var/www/html/.env
  echo DB_NAME=[nama database/bebas]  >> /var/www/html/.env
  echo DB_HOST=[endpoint rds] >> /var/www/html/.env
  sudo apt install -y openssl
  sudo a2enmod ssl
  sudo a2ensite default-ssl.conf
  sudo systemctl reload apache2' > /home/ubuntu/otomatis.sh

  $ chmod +x otomatis.sh
  $ ./otomatis.sh

Itulah beberapa masalah yang saya temukan beserta solusinya.
Apabila ada kesalahan pengetikan, saya mohon maaf.