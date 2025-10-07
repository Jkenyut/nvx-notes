# Docker

## Pengenalan Virtual Machine
- VM umumnya berjalan diatas hypervisor
- Saat membuat VM perlu meng - install os (linux, windows, dll)
- VM tergolong lambat ketika dihidupkan karena adanya proses booting OS terlebih dahulu.
![enter image description here](https://media.geeksforgeeks.org/wp-content/uploads/20250823130235313168/virtual_machines.webp) 

 # Container
 - berbeda dengan VM, container hanya fokus pada aplikasi.
 - Container dapa berjalan diatas aplikasi container manager yang berjalan di OS.
 - Container bisa melakukan - package aplikasi dan dependency  tanpa harus menggabungkan OS.
 - container menggunakan sistem operasi host dimana container manager-nya berjalan, sehingga dapat berjalan cepat karena tidak perlu melakukan proses booting dan ukuran dari container lebih ringan dibandingkan VM, karena tidak membutuhkan sistem operasi sendiri.
 - ukuran container biasanya hitungan MB, berbeda dengan VM yang mencapai ratusan MB sampai GB karena ada OS di dalamnya
 - pada umumnya contrainer berjalan menggunakan OS linux:
	 - Container berjalan selain linux, biasanya akan membuat semacam lightweight VM.
	 - Di Windows ada WSL dan Mac akan otomatis menggunakan QEMU.

![enter image description here](https://s7280.pcdn.co/wp-content/uploads/2018/07/containers-vs-virtual-machines.jpg)

## Pengenalan Docker
- Docker adalah salah satu implementasi pada container manager yang saat ini populer.
- Docker merupakan teknologi yang di perkenalkan pada tahun 2013.
- Docker bersifat Open-Source, sehingga dapat digunakan secara bebas.

## Arsitektur Docker
- Docker menggunakan arsitektur Client & Server.
- Docker berkomunikasi dengan Docker Daemon (Server).
- Saat install docker, biasanya sudah terdapat docker client dan server.
- Docker Client dan Docker Daemon bisa  berjalan di satu sistem yang sama dan berkomunikasi menggunakan Rest API.

![enter image description here](https://miro.medium.com/v2/resize:fit:2000/0*vTzuVPKGzW3EwcVB)

## Install Docker
- Docker bisa install di semua OS.
- Untuk install di Windows dan Mac perlu menggunakan Docker Desktop.
- Untuk Linux, bisa menggunakan distro linux masing-masing.
- [Install Docker](https://docs.docker.com/get-docker/)
 
## Docker Version
- melakukan pengecekan docker daemon sudah berjalan bisa menggunakan perintah
	```sh
	docker version
	```
	
	```zsh
	Client:
	 Version:           28.0.4
	 API version:       1.48
	 Go version:        go1.23.7
	 Git commit:        b8034c0
	 Built:             Tue Mar 25 15:06:09 2025
	 OS/Arch:           darwin/arm64
	 Context:           desktop-linux

	Server: Docker Desktop 4.40.0 (187762)
	 Engine:
	  Version:          28.0.4
	  API version:      1.48 (minimum version 1.24)
	  Go version:       go1.23.7
	  Git commit:       6430e49
	  Built:            Tue Mar 25 15:07:18 2025
	  OS/Arch:          linux/arm64
	  Experimental:     false
	 containerd:
	  Version:          1.7.26
	  GitCommit:        753481ec61c7c8955a23d6ff7bc8e4daed455734
	 runc:
	  Version:          1.2.5
	  GitCommit:        v1.2.5-0-g59923ef
	 docker-init:
	  Version:          0.19.0
	  GitCommit:        de40ad0	
  ```

##  Docker Registry
- Docker Registry adalah tempat kita menyimpan docker image.
- Dengan adanya docker registry kita dapat menyimpan image yang akan kita buat ataupun image orang lain, sehingga bisa digunakan di docker daemon dimanapun selama bisa terkoneksi ke docker registry.

### Contoh Docker Registry
- Docker Hub
- Digital Ocean Container Registry
- Google Cloud Container Registry
- Amazon Elastic Container Registry
- Azure Container Registry









<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMDMxNTAzOSwtNjI4NTY4Njk3LDUxNz
A0MjQ5NywzNjY2MjYzMjZdfQ==
-->