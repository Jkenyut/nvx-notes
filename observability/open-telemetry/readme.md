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
![Permasalahan Open Telemetry](https://raw.githubusercontent.com/Jkenyut/nvx-notes/master/observability/open-telemetry/permasalahan.png)

# Intrumentation
-   Pada permasalahan sebelumnya, bagaimana kita mencari tahu masalah di aplikasi microservice secara tepat?, tentunya perlu adanya pengamatan (observasi)
-   Agar Suatu sistem dapat melakukan observasi, perlu adanya intrumentasi terhadap sistem tersebut.
-   Intrumentasi merupakan suatu hal yang dapat “memancarkan” (merujuk pada traces, metrics, dan logs).

## Signal
-   Telemetry merupakan data yang dipancarkan oleh suatu sistem, berasal dari traces, metrics, dan logs.
-   Pada Open Telemetry ada istilah bernama Signal. Signal adalah kategori dari telemetry, seperti :
-   Traces.
-   Metrics.
-   Logs.
-   Baggage.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMjYzNzQyMCwxNTU3Mjg2NTUyLDI5OD
M2MzU4OCwtMTE4NzI3OTcxMCwtNjk0MjEzNTMzXX0=
-->