# Docker

Table of content
- Pengenalan Virtual Machine
- Pengenalan Container
- Pengenalan Docker
- Menginstall Docker
- Docker Image
- Docker Registry
- Docker Contrainer
- Docker Network

## Pengenalan Virtual Machine
- VM umumnya berjalan diatas hypervisor
- Saat membuat VM perlu meng-install os (linux,windows,dll)
- VM tergolong lambat ketika dihidupkan karena adanya proses booting OS terlebih dahulu.
![enter image description here](https://media.geeksforgeeks.org/wp-content/uploads/20250823130235313168/virtual_machines.webp) 

 # container
 - berbeda dengan VM, container dapat melakukan penggabungan aplikasi dengan dependensi.
 - Container berjalan menggunakan OS pada host (biasanya OS linux).
 - ukuran dari container lebih ringan dibandingkan VM.
 - container dapat berjalan cepat karena tidak perlu melakukan proses booting.
 - pada umumnya contrainer berjalan menggunakan OS linux:
	 - Container berjalan selain linux biasanya akan membuat semacam lightweight VM.
	 - Di windows ada WSL dan Mac akan otomatis menggunakan Qemu.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNzQ5MDE2ODksMzY2NjI2MzI2XX0=
-->