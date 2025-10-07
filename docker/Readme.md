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
	output
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


### 🐳 Contoh Docker Registry
- [Docker Hub](https://hub.docker.com/)  
- [DigitalOcean Container Registry (DOCR)](https://www.digitalocean.com/products/container-registry/)  
- [Google Cloud Container Registry (GCR)](https://cloud.google.com/container-registry)  
- [Amazon Elastic Container Registry (ECR)](https://aws.amazon.com/ecr/)  
- [Azure Container Registry (ACR)](https://azure.microsoft.com/en-us/products/container-registry/)

## Melihat Docker Image
- Docker Image mirip seperti installer aplikasi , dimana di dalamnya terdapat aplikasi dan dependency.
- Perintah untuk melihat Docker Image pada docker daemon, bisa menggunakan perintah
	```sh
	docker images
	```
	output 
	```zsh   
	REPOSITORY   TAG       IMAGE ID       CREATED
	```
## Download Docker Image
- jika awalan akan terlihat kosong, untuk mengisinya kita perlu menggunakan perintah:
	```sh
	docker pull <nameimage:tag>
	```
	contoh
	```sh
	docker pull redis:latest
	```
	output
	```zsh
	Using default tag: latest
	latest: Pulling from library/redis
	f4e51325a7cb: Pull complete 
	14bc907446c4: Pull complete 
	49bedfd7c51d: Pull complete 
	4fd85a8f632f: Pull complete 
	b2a4cbf7c41a: Pull complete 
	4f4fb700ef54: Pull complete 
	1284bf9d175c: Pull complete 
	Digest: sha256:f0957bcaa75fd58a9a1847c1f07caf370579196259d69ac07f2e27b5b389b021
	Status: Downloaded newer image for redis:latest
	docker.io/library/redis:latest
	```
	kemudian kita perintah:
	```sh
	docker images
	```
	untuk melihat kembali image yang telah di download.
	```zsh   
	REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
	redis        latest    5b7e962f53cb   4 days ago   159MB	
	```
## Hapus Docker Image
- Cara menghapus docker image yang sudah dibuat ataupun di download, bisa menggunakan perintah:
	```sh
	docker image rm <namaimage:tag>
	```
	contoh 
	```sh
	docker image rm redis:latest
	```
	output
	```zsh
	Untagged: redis:latest
	Untagged: redis@sha256:f0957bcaa75fd58a9a1847c1f07caf370579196259d69ac07f2e27b5b389b021
	Deleted: sha256:5b7e962f53cb8271dcda7f2946c9406d60b9a465235faa39c31a2d01844e2af2
	Deleted: sha256:d68aed1832d50472d5e097778653179aba2ebc4cb9c62c4b466990ce7e6b9a5c
	Deleted: sha256:724b1c62d61d6d62166762042e3189ffeb0b4ded218c3c1e4fd58a6d4481b590
	Deleted: sha256:c1a824f6329765dc4bdf249357531555320bb80223327c7d00ad0f3954699c78
	Deleted: sha256:130b0cbbf526ea7b5d271aa1ee6a8539725694af47f6dce2d3763eae5c6a05c9
	Deleted: sha256:fd1cd23e196471b95a2dc026c8c7c68ae0e522e45a95e3a798bcb9cd637e8126
	Deleted: sha256:a78fe7aa66b30a6dc14afd4a85c0dcd4f501f8a13d5037833005dee89a879841
	Deleted: sha256:3e5d01a55aea2c9ea64c3cfef2bb870df4b01c7e206103a970d4c3dc6caed96b
	```
	kemudian lihat docker images list:
	```sh
	docker images
	```
	output 
	```zsh   
	REPOSITORY   TAG       IMAGE ID       CREATED
	```
## Docker Container
- Jika docker image seperti installer aplikasi, maka docker container seperti aplikasi hasil installer.
- Satu Docker Image bisa digunakan untuk membuat docker container, asalkan nama docker container berbeda-beda.
- Jika kita sudah membuat docker container, maka docker image yang sedang digunakan tidak bisa dihapus, hal ini karena docker container tidak melakukan copy isi docker image ke dalam docker container, tetapi hanya menggunakan isi dari docker image tersebut.
## Status Docker Container
- Saat membuat container secara default tidak akan berjalan otomatis.
- Maka setelah membuat container, kita perlu menjalankan container tersebut ketika diperlukan.
## Melihat Docker Container
- untuk melihat semua container , baik yang sedang berjalan maupun tidak di dalam docker daemon, perintahnya:
	```sh
	docker container ls -a
	```
	output
	```zsh
	CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
	```
	jika ingin melihat hanya container yang berjalan, maka perintahnya
	```sh
	docker container ls
	```
	atau
	```sh
	docker ps
	```
## Membuat Docker Container
- Perintah untuk membuat container:
	```sh
	docker container create --name <namacontainer> <namaimage:tag>
	```
	contoh:
	```sh
	docker create --name contohredis redis:latest
	```
	output
	```zsh
	docker create --name contohredis redis:latest
	85ec49c236972e55bbb3380d9a1683ad19c4285247524af7b2e98784cd4c4156
	```
	maka kita bisa melihat lagi list container
	
	```sh
	docker container ls -a
	```
	output

	```zsh
	CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS    PORTS     NAMES
	85ec49c23697   redis:latest   "docker-entrypoint.s…"   About a minute ago   Created             contohredis					
	```
## Run Docker Container
- untuk menjalankan docker container menggunakan perintah:
	```sh
	docker container start <containerID/namacontainer>
	```
	atau
	```sh
	docker run <containerID/namacontainer>
	```
	contoh
	```sh
	docker container start contohredis
	```
	kemudian kita lihat lagi list container yang berjalan
	```sh
	docker container ls 
	```
	output
	```zsh              
	CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS          PORTS      NAMES
	85ec49c23697   redis:latest   "docker-entrypoint.s…"   5 minutes ago   Up 18 seconds   6379/tcp   contohredis
	```
	terlihat bahwa container sedang berjalan dengan status dan port sudah terisi.

## Cara Menghentikan Docker Container
- perintah untuk menghentikan docker container :
```sh
docker container stop contohredis
```
atau 
```sh
docker stop contohredis
```
kemudian kita bisa melihat kembali list container
```sh
docker container ls
```
output

	```zsh
	CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES 
	```










<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM1NTgwMzE2NiwxOTMwMTU4MDYzLDE3Nj
MxMTA4NiwtNjI4NTY4Njk3LDUxNzA0MjQ5NywzNjY2MjYzMjZd
fQ==
-->