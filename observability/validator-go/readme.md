# Validator Golang

## Table of Contents

1.Apa Itu Validasi
2.Kapan Dilakukan  Validasi
3.Mengapa  Validasi  Diperlukan
4.Perbedaan Manual dan Library Validasi
5.Validation Package
6.Backed-In Validation Package
7.Validasi Variable
8.Validasi Struct
9.Kustom Validasi
10.Errors Validasi

## Apa Itu Validasi
Validasi  merupakan salah satu  hal yang akan  selalu  dilakukan pada saat  membuat  suatu  aplikasi, untuk  memastikan  bahwa data yang akan  diolah  sudah  benar dan kesalahan data dapat  ditemukan  langsung  sebelum data tersebut  diolah.

## Kapan Dilakukan Validasi
Input Request User
Business Logic (Role Not Authorized)
Database (Constraint :Unique, Not Null)
## Mengapa  Validasi  Diperlukan
•Memastikan data yang dikirim oleh pengguna  sesuai  dengan  sistem.
•Keamanan  bahwa data yang dikirim  pengguna  bukan  inputan  perintah  berbahaya.
•Membantu  menjaga  konsitensi  integritas data sesuai format sistem.
•Tidak ada yang dapat  memastikan  inputan  pengguna  benar.

## Perbedaan Manual dan Library Validasi
Manual : 
•Rata-rata menggunakan if statement
•Semakin  banyak  validasi  maka  semakin  banyak if statement dibuat
Pembuatan  validasi manual memakan  waktu lama.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3Njg5MTIzMiwtMjA4ODc0NjYxMl19
-->