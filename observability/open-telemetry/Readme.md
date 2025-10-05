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
-   Agar Suatu sistem dapat melakukan observasi, perlu adanya instrumentasi terhadap sistem tersebut.
-   Instrumentasi merupakan suatu hal yang dapat “memancarkan” (merujuk pada traces, metrics, dan logs).

## Signal
-   Telemetry merupakan data yang dipancarkan oleh suatu sistem, berasal dari traces, metrics, dan logs.
-   Pada Open Telemetry ada istilah bernama Signal. Signal adalah kategori dari telemetry, seperti :
-   Traces.
-   Metrics.
-   Logs.
-   Baggage.

# Masalah Intrumentasi Cross - Platform
-   Sekarang ada banyak SDK (software development kit) atau library dari layanan gratis ataupun berbayar yang dapat membuat intrumentasi pada sistem kita.
-   Namun setiap SDK mempunyai standar mereka sendiri, yang berarti jika menggunakan layanan dari A, maka harus menggunakan SDK A, Begitupun sebaliknya jika meggunakan layanan B. masalah ini bisa terjadi jika suatu saat ada keperluan untuk pindah dari suatu layanan maka perlu adanya perubahan yang sangat banyak.
![Permasalahan Open Telemetry](https://raw.githubusercontent.com/Jkenyut/nvx-notes/master/observability/open-telemetry/problem-cross-paltform.png)

# Open Telementry
-   Open telemetry merupakan open source project untuk men-standarisasikan data intrumentasi ( traces, metrics, logs)
-   Tujuan utama agar data hasil intrumentasi dapat di distribusikan ke berbagai platform yang berbeda.
![Permasalahan Open Telemetry](https://raw.githubusercontent.com/Jkenyut/nvx-notes/master/observability/open-telemetry/resolve-cross-paltform.png)

## Dev & Ops di Open Telemetry
-   Developer : Fokus pada bagaimana sistem dapat membuat dan memancarkan instrumentasi

-   Operation : Fokus dalam menangkap instrumentasi yang ada serta dapat melakukan visualisasi

## Automatic & Manual Intrumentation
-   Open telemetry mendukung intrumentasi otomatis dan manual
-   Intrumentasi otomatis artinya SDK / Library sudah mendukung hal-hal dalam memancarkan signal tanpa perlu di lakukan secara manual, sehingga menghemat waktu
-   Tetapi dalam praktek real-nya perlu adanya instrumentasi manual untuk hal-hal yang tidak di cover oleh instrumentasi otomatis

# Open Telemetry : Collector

   Open Telemetry Collector merupakan jembatan penghubung antara data intrumentasi yang akan dikirimkan ke berbagai platform.
-   Aplikasi akan mengirimkan data intrumentasi ke Collector melalui OLTP.
-   OLTP ( Open Telemetry Protocol) merupakan standar protokol untuk ekosistem open telemetry, biasanya berjalan pada HTTP & GRCP.
![enter image description here](https://cdn.sanity.io/images/z7wg6mcy/production-2025/892268a99c29c144322b68f400faad286ae53f5c-1710x498.png?q=100&fit=max&auto=format)

-   Terdapat 4 komponen utama dalam Collector:
    -   Receivers : konfigurasi untuk bagaimana data SDK di aplikasi masuk ke Collector.
    -   Exporter: konfigurasi untuk bagaimana collector akan mengirimkan data ke platform.
    -   Processor: konfigurasi untuk bagaimana data diproses antara receivers & exporter
    -   Connector:  bagaimana cara untuk menghubungkan antara bagian-bagian komponen.
![enter image description here](https://i.ytimg.com/vi/7T2SdvYW-eI/maxresdefault.jpg)

 

## Konfigurasi Collector
- buat file config-otel-collector.yaml
- didalamnya ada beberapa root element konfigurasi:
1.	 Receivers
2.	Proccesors
3.	Exporters
- Ketiga konfigurasi tersebut dipanggil dalam root element bernama services
 
```yaml
# ========================
# OpenTelemetry Collector Configuration
# ========================

receivers:
  otlp:
    protocols:
      grpc:
      http:

processors:
  memory_limiter:
    check_interval: 1s
    limit_percentage: 50
    spike_limit_percentage: 30

  batch:
    send_batch_size: 512
    timeout: 5s

  tail_sampling:
    decision_wait: 10s
    num_traces: 1000
    expected_new_traces_per_sec: 100
    policies:
      - name: error-only
        type: status_code
        status_code:
          status_codes: [ERROR]

exporters:
  jaeger:
    endpoint: ${JAEGER_ENDPOINT:-jaeger-aio:14250}
    tls:
      insecure: ${JAEGER_INSECURE:-true}

  prometheus:
    endpoint: 0.0.0.0:8889

  logging:
    verbosity: detailed

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [memory_limiter, batch, tail_sampling]
      exporters: [jaeger, logging]

    metrics:
      receivers: [otlp]
      processors: [memory_limiter, batch]
      exporters: [prometheus]

  telemetry:
    logs:
      level: info

```

## Open Telemetry : Traces

   Trace merupakan gambaran ketika aplikasi sedang di input / request sampi proses tersebut selesai.
-   didalam traces terdapat lebih dari 1 span.
-   span merupakan representasi sebuah unit work di aplikasi atau function aplikasi

![enter image description here](https://www.opensourcerers.org/wp-content/uploads/2022/04/OpenTelemetryJaeger.png)


## Open Telemetry : Metrics

 -   Trace merupakan gambaran ketika aplikasi sedang di input / request sampi proses tersebut selesai.
-   didalam traces terdapat lebih dari 1 span.
-   span merupakan representasi sebuah unit work di aplikasi atau function aplikasi
![enter image description here](https://www.mytechramblings.com/img/otel-metrics-app-diagram.png)

## Open Telemetry : Logs
-   Logs merupakan salah satu metrics mudah dibuat, tujuan utama hanya menampilkan informasi “apa yang terjadi”
-   Setiap bahasa pemrograman dapat mebuat log secara langsung.
![enter image description here](https://openobserve.ai/assets/image8_98decc9ee1.png)


## Signal : Baggages (Distribusi Tracing)
-   Aplikasi microservices pastinya sangat banyak dan memerlukan komunikasi antar service.
-   Dalam setiap komunikasi service perlu dilakukan tracing, tracing juga bisa disebut distribusi tracing.
-   Agar aplikasi dapat mengirimkan informasi tersebut, dinamakan baggages.
-   baggages merupakan data dengan format key-value pairs.
![enter image description here](https://opentelemetry.io/docs/concepts/signals/otel-baggage.svg)

## Propagation
-   Propagation adalah bagaimana kita mengirimkan traceID (berserta parent spanID) ke aplikasi lain yang dipanggil di header datanya, tujuannya agar antar service memiliki distribusi tracing.
-   contohnya aplikasi A memanggil B dan B memanggil C, setiap pemanggilan service, data propagation akan dikirimkan juga.

![enter image description here](https://trstringer.com/images/otel-propagation1.png)

# Samples
-   Permasalah adalah apakah kita perlu semua hasil data trace tersebut?
-   Pada Open Telementry ada konsep bernama samples, dimana kita dapat mengambil informasi  atau sample dari suatu trace dengan jenis tertentu
-   tujuannya agar biaya storage / network murah, fokus pada traces tertentu dan menghilanglan noise ( trace yang tidak diperlukan)
-   ada dua tipe samples yaitu head sampling dan tail sampling.
![enter image description here](https://opentelemetry.io/docs/concepts/sampling/traces-venn-diagram.svg)

## Head Sampling
-   head sampling merupakan cara mengambil sample paling mudah, letak konfigurasi ada pada level SDK aplikasi.
-   head sampling menggunakan teknik probabilitas untuk menentukan trace akan diteruskan ke collector atau tidak,
-   cara kerja head sampling membuat keputusan sampling adalah aplikasi pertama kali yang membuat trace (parent)

## Tail Sampling
-   Tail sampling merupakan cara mengambil sample melalu policy atau aturan yang telah dibuat.
-   policy dapat diterapkan dari level dasar sampai advance (contohnya, hanya mengambil, latensi, sukses/error, atribute terterntu)
-   letak konfigurasi tail sampling berada pada processor collector pada open telementry.
![enter image description here](https://cdn-ak.f.st-hatena.com/images/fotolife/q/quoll00/20230318/20230318152653.png)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc1MDM1NjIzNiwxOTkzMjMxMDEyLC02NT
c2NjU5MDYsODA4NTc3MDczLDE4ODA1MDg1MTIsLTE1MDc2NTc5
OTYsNDI0OTA5MTc5LC0xMDM1MDk5MTM0LDEzNzE3NzA4NzgsLT
YzMTI3MTM0NCwtMTgwMDA5NTMwOCwxNDU4NzQxODkwLC0xODQw
ODM2NDEsMTM1MTg3MzI3N119
-->