# Pengantar
-   Saat ini banyak aplikasi yang mengadopsi arsitektur microservice dengan alasan tertentu. Biasanya tidak lepas dengan teknologi contrainer atau container ochestration (e.g kubernetes).
-   jumlah microservices bisa banyak sekali.(e.g netflix)
-   Setiap microservice biasanya memanggil microservice lainnya, contohnya layanan A memanggil layanan B, dan B memanggil layanan C (service dependency).


## Netflix Architecture
![enter image description here](https://d3an9kf42ylj3p.cloudfront.net/uploads/2023/07/netflix_microservices.png)

# Permasalahan
-   Sebuah aplikasi tidak selalu berjalan normal terus-menerus, terkadang ada suatu kondisi yang menyebabkan suatu error.
-   Susahnya mencari dimana letak error tersebut.
-   Proses dalam mencari root error dan memperbaiki error bisa memakan waktu lama.
![enter image description here](https://github.com/Jkenyut/nvx-notes/blob/master/observability/open-telemetry/permasalahan.png)

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzMTk2NDQ5MSwxNTU3Mjg2NTUyLDI5OD
M2MzU4OCwtMTE4NzI3OTcxMCwtNjk0MjEzNTMzXX0=
-->