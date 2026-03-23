# React
## Sejarah
1. Nama pertamanya FaxJS.
2. Dikembangkan oleh facebook tahun 2010 untuk menangani update halaman feed di facebook tanpa harus me-refresh.
3. Tahun 2011 di integrasikan bersama XHP, kemudian mengganti nama menjadi React JS.
4. Tahun 2013 , Facebook merilis ReactJS menjadi Open Source.
5. Framework populer untuk front-end GitHub star saat penulisan ini dibuat sekitar 244K.

## Component
1. merupakan kumpulan kode yang (bagian) bisa digunakan secara independen) mirip seperti function
2. Component sendiri bisa berisikan HTML,CSS, dan JavaScript.
3. karena seperti function maka component bisa digunakan oleh component lain (layaknya Document Object Model).

### struktur component
App
->> Header --> Menu
->> Content --> Content 1 , Content 2
->> Footer --> Copyright

## JSX (JavaScript Syntax Extension)
1. React menggunakan sintaks extension bernama JSX yang memberikan kemudahan untuk membuat component, guna menulis HTML, CSS di dalam javascript dalam satu file.

## Membuat Project (Vite)
1. membuat project menggunakan vite sebagai build tool
2. pastikan nodejs & npm sudah ter install
3. `npm create vite@latest`
4. lalu `$ npm create vite@latest my-vue-app -- --template nama framework`
5. maka contoh yang saat ini adalah (javascript) 
6. `$ npm create vite@latest my-vue-app -- --template react`
7. jika ingin menggunakan typescript `$ npm create vite@latest my-vue-app -- --template react-ts`
8. panduan penggunaan vite : https://vite.dev/guide/


contoh folder

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMyOTUyMDU3OSwtMjQwNTY2MjEyLC0xMT
cwNTQ2MjM1LC0xNDM2NTc3MDUxXX0=
-->