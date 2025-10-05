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

# Masalah Intrumentasi Cross - Platform
-   Sekarang ada banyak SDK (software development kit) atau library dari layanan gratis ataupun berbayar yang dapat membuat intrumentasi pada sistem kita.
-   Namun setiap SDK mempunyai standar mereka sendiri, yang berarti jika menggunakan layanan dari A, maka harus menggunakan SDK A, Begitupun sebaliknya jika meggunakan layanan B. masalah ini bisa terjadi jika suatu saat ada keperluan untuk pindah dari suatu layanan maka perlu adanya perubahan yang sangat banyak.
![Permasalahan Open Telemetry](https://raw.githubusercontent.com/Jkenyut/nvx-notes/master/observability/open-telemetry/problem-cross-paltform.png)

# Open Telementry
-   Open telemetry merupakan open source project untuk men-standarisasikan data intrumentasi ( traces, metrics, logs)
-   Tujuan utama agar data hasil intrumentasi dapat di distribusikan ke berbagai platform yang berbeda.
![Permasalahan Open Telemetry](https://raw.githubusercontent.com/Jkenyut/nvx-notes/master/observability/open-telemetry/resolve-cross-paltform.png)

## Dev & Ops di Open Telemetry
-   Developer : Fokus pada bagaimana sistem dapat membuat dan memancarkan intrumentasi

-   Operation : Fokus dalam menangkap intrumentasi yang ada serta dapat melakukan visualisasi

## Automatic & Manual Intrumentation
-   Open telemetry mendukung intrumentasi otomatis dan manual
-   Intrumentasi otomatis artinya SDK / Library sudah mendukung hal-hal dalam memancarkan signal tanpa perlu di lakukan secara manual, sehingga menghemat waktu
-   Tetapi dalam praktek realnya perlu adanya intrumentasi manual untuk hal-hal yang tidak di cover oleh intrumentasi otomatis

# Open Telemetry : Collector

   Open Telemetry Collector merupakan jembatan penghubung antara data intrumentasi yang akan dikirimkan ke berbagai platform.
-   Aplikasi akan mengirimkan data intrumentasi ke Collector melalui OLTP.
-   OLTP ( Open Telemetry Protocol) merupakan standar protokol untuk ekosistem open telemetry, biasanya berjalan pada HTTP & GRCP.
![enter image description here](https://cdn.sanity.io/images/z7wg6mcy/production-2025/892268a99c29c144322b68f400faad286ae53f5c-1710x498.png?q=100&fit=max&auto=format)

-   Terdapat 4 komponen utama dalam Collector:
    -   Receivers : konfigurasi untuk bagaimana data SDK di aplikasi masuk ke Collector.
    -   Exporter: konfigurasi untuk bagaimana collector akan mengirimkan data ke platform.
    -   Processor: konfigurasi untuk bagaimana data diproses antara receivers & exporter
    -   Connector:  bagaimana cara untuk menghubungkan antara 3 bagian utama tersebut.
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

# Open Telemetry : Traces
-   Trace merupakan gambaran ketika aplikasi sedang di input / request sampi proses tersebut selesai.
-   didalam traces terdapat lebih dari 1 span.
-   span merupakan representasi sebuah unit work di aplikasi atau function aplikasi

![enter image description here](https://www.opensourcerers.org/wp-content/uploads/2022/04/OpenTelemetryJaeger.png)



 

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDI0OTA5MTc5LC0xMDM1MDk5MTM0LDEzNz
E3NzA4NzgsLTYzMTI3MTM0NCwtMTgwMDA5NTMwOCwxNDU4NzQx
ODkwLC0xODQwODM2NDEsMTM1MTg3MzI3N119
-->