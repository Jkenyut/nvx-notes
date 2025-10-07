# Docker

## Pengenalan Virtual Machine
- VM umumnya berjalan diatas hypervisor
- Saat membuat VM perlu meng - install os (linux, windows, dll)
- VM tergolong lambat ketika dihidupkan karena adanya proses booting OS terlebih dahulu.
![enter image description here](https://media.geeksforgeeks.org/wp-content/uploads/20250823130235313168/virtual_machines.webp) 

 # container
 - berbeda dengan VM, container hanya fokus pada aplikasi.
 - Container dapa berjalan diatas aplikasi container manager yang berjalan di OS.
 - Container bisa melakukan - package aplikasi dan dependency  tanpa harus menggabungkan OS.
 - container menggunakan sistem operasi host dimana container manager-nya berjalan, sehingga dapat berjalan cepat karena tidak perlu melakukan proses booting dan ukuran dari container lebih ringan dibandingkan VM, karena tidak membutuhkan sistem operasi sendiri.
 - ukuran container biasanya hitungan MB, berbeda dengan VM yang mencapai ratusan MB sampai GB karena ada OS di dalamnya
 - pada umumnya contrainer berjalan menggunakan OS linux:
	 - Container berjalan selain linux, biasanya akan membuat semacam lightweight VM.
	 - Di Windows ada WSL dan Mac akan otomatis menggunakan Qemu.

![enter image description here](https://s7280.pcdn.co/wp-content/uploads/2018/07/containers-vs-virtual-machines.jpg)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxODY3NTY1MSw1MTcwNDI0OTcsMzY2Nj
I2MzI2XX0=
-->