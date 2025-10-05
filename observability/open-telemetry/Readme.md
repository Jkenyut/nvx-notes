
# OpenTelemetry: Standar Observabilitas untuk Arsitektur Microservices

## Pengantar

Arsitektur microservices semakin populer karena fleksibilitas dan skalabilitas-nya, terutama dengan teknologi seperti container dan orchestrator seperti Kubernetes. Namun, kompleksitasnya menimbulkan tantangan, terutama dalam observabilitas sistem. OpenTelemetry hadir sebagai solusi open-source untuk menstandarisasi instrumentasi dan observabilitas sistem, memungkinkan data seperti traces, metrics, dan logs dikumpulkan serta didistribusikan ke berbagai platform dengan efisien, terlepas pada bahasa pemrograman, infrastruktur, dan environment yang digunakan.

### Karakteristik Microservices

-   **Jumlah Microservices**: Dalam sistem besar seperti Netflix, jumlah microservices bisa sangat banyak, masing-masing dengan dependensi kompleks.
-   **Service Dependency**: Microservices saling memanggil, misalnya Layanan A memanggil Layanan B, dan B memanggil Layanan C.

**Contoh Arsitektur Netflix**  
![Netflix Microservices](https://d3an9kf42ylj3p.cloudfront.net/uploads/2023/07/netflix_microservices.png)

## Permasalahan Observabilitas

-   **Error Sulit Dilacak**: Aplikasi microservices tidak selalu berjalan mulus. Menemukan root cause error sering kali sulit karena kompleksitas dependensi.
-   **Waktu Perbaikan Lama**: Proses debugging dan perbaikan memakan waktu karena kurangnya visibilitas ke dalam sistem.

**Ilustrasi Permasalahan**  
![Permasalahan Observabilitas](https://raw.githubusercontent.com/Jkenyut/nvx-notes/master/observability/open-telemetry/permasalahan.png)

## Sejarah Open Telemetry
- Merupakan project dari cloud native computing foundation (CNCF) yang dilakukan merger dari dua project yaitu Open Tracing dan Open Census yang memiliki permasalahan yang sama.
- masalah utamanya tidak adanya standar untuk melakukan instrumentasi kode untuk mengirim data telemetry ke observability.

## Instrumentasi

Untuk mengatasi masalah observabilitas, sistem perlu diinstrumentasi agar dapat menghasilkan data telemetry (traces, metrics, logs). Instrumentasi adalah proses "memancarkan" data yang memberikan wawasan tentang performa dan perilaku aplikasi. 

### Signal dalam OpenTelemetry

OpenTelemetry mendefinisikan beberapa kategori telemetry, yang disebut _Signal_:

1.  **Traces**: Melacak alur request dari awal hingga selesai.
2.  **Metrics**: Data kuantitatif seperti latensi atau penggunaan CPU.
3.  **Logs**: Catatan kejadian untuk debugging.
4.  **Baggage**: Data tambahan dalam format key-value untuk distributed tracing.

## Masalah Instrumentasi Cross-Platform

Banyak SDK atau library observabilitas memiliki standar sendiri, menyebabkan ketergantungan pada vendor tertentu. Jika beralih ke platform lain, perubahan kode besar-besaran diperlukan.

**Ilustrasi Masalah Cross-Platform**  
![Masalah Cross-Platform](https://raw.githubusercontent.com/Jkenyut/nvx-notes/master/observability/open-telemetry/problem-cross-paltform.png)

## OpenTelemetry: Solusi Standar

OpenTelemetry adalah proyek open-source yang menstandarisasi pengumpulan dan distribusi data telemetry (traces, metrics, logs) agar kompatibel dengan berbagai platform.

**Tujuan Utama**:

-   Menghilangkan ketergantungan pada vendor tertentu.
-   Memungkinkan distribusi data telemetry ke berbagai backend observabilitas.

**Ilustrasi Solusi Cross-Platform**  
![Solusi Cross-Platform](https://raw.githubusercontent.com/Jkenyut/nvx-notes/master/observability/open-telemetry/resolve-cross-paltform.png)

### Peran dalam OpenTelemetry

-   **Developer**: Fokus pada implementasi instrumentasi untuk menghasilkan data telemetry.
-   **Operations**: Mengelola, menangkap, dan memvisualisasikan data telemetry untuk analisis.

### Instrumentasi Otomatis dan Manual

-   **Otomatis**: SDK OpenTelemetry mendukung emisi signal tanpa konfigurasi manual, menghemat waktu.
-   **Manual**: Diperlukan untuk kasus spesifik yang tidak tercakup oleh instrumentasi otomatis.

## OpenTelemetry Collector

Collector adalah komponen kunci yang bertindak sebagai jembatan antara aplikasi dan platform observabilitas. Data telemetry dikirim melalui **OpenTelemetry Protocol (OTLP)**, yang berjalan di HTTP atau gRPC.

**Komponen Utama Collector**:

1.  **Receivers**: Mengatur cara data masuk dari aplikasi.
2.  **Processors**: Memproses data sebelum dikirim.
3.  **Exporters**: Mengirim data ke platform observabilitas.
4.  **Connectors**: Menghubungkan komponen internal Collector.

**Ilustrasi Arsitektur Collector**  
![OpenTelemetry Collector](https://cdn.sanity.io/images/z7wg6mcy/production-2025/892268a99c29c144322b68f400faad286ae53f5c-1710x498.png?q=100&fit=max&auto=format)

### Contoh Konfigurasi Collector

Berikut adalah contoh konfigurasi OpenTelemetry Collector dalam format YAML:

```yaml
# OpenTelemetry Collector Configuration
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

## Signal OpenTelemetry

### Traces

Traces melacak alur request dari input hingga selesai, terdiri dari beberapa **span**. Setiap span mewakili unit kerja dalam aplikasi.

**Ilustrasi Traces**  
![Traces](https://www.opensourcerers.org/wp-content/uploads/2022/04/OpenTelemetryJaeger.png)

### Metrics

Metrics memberikan data kuantitatif seperti latensi, penggunaan sumber daya, atau error rate.

**Ilustrasi Metrics**  
![Metrics](https://www.mytechramblings.com/img/otel-metrics-app-diagram.png)

### Logs

Logs adalah catatan kejadian sederhana yang memberikan informasi tentang apa yang terjadi di aplikasi. Mudah dihasilkan di berbagai bahasa pemrograman.

**Ilustrasi Logs**  
![Logs](https://openobserve.ai/assets/image8_98decc9ee1.png)

### Baggage (Distributed Tracing)

Baggage adalah data key-value yang digunakan untuk mendukung distributed tracing, memungkinkan pelacakan antar microservices.

**Ilustrasi Baggage**  
![Baggage](https://opentelemetry.io/docs/concepts/signals/otel-baggage.svg)

## Propagation

Propagation memastikan traceID dan parent spanID dikirimkan antar microservices melalui header request, memungkinkan distributed tracing.

**Ilustrasi Propagation**  
![Propagation](https://trstringer.com/images/otel-propagation1.png)

## Sampling

Sampling digunakan untuk mengurangi volume data telemetry, menghemat biaya storage dan jaringan, serta fokus pada data yang relevan.

### Head Sampling

-   Dilakukan di level SDK aplikasi.
-   Menggunakan probabilitas untuk memutuskan apakah trace diteruskan ke Collector.
-   Keputusan dibuat oleh aplikasi yang memulai trace (parent).

### Tail Sampling

-   Dilakukan di Collector melalui kebijakan (policy) seperti latensi, status error, atau atribut tertentu.
-   Lebih fleksibel dibandingkan head sampling.

**Ilustrasi Sampling**  
![Sampling](https://opentelemetry.io/docs/concepts/sampling/traces-venn-diagram.svg)

**Contoh**  
![Tail Sampling](https://cdn-ak.f.st-hatena.com/images/fotolife/q/quoll00/20230318/20230318152653.png)

## Kesimpulan

OpenTelemetry adalah solusi standar untuk observabilitas di arsitektur microservices. Dengan mendukung instrumentasi otomatis dan manual, serta menyediakan Collector untuk mengelola data telemetry, OpenTelemetry mempermudah pelacakan error, analisis performa, dan visualisasi sistem. Fitur seperti traces, metrics, logs, baggage, dan sampling memastikan data yang dihasilkan relevan dan efisien, mendukung kebutuhan developer dan tim operasi.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjE4Mzc4NDMsMjEzNzQ3ODA3NiwxNzUwMz
U2MjM2LDE5OTMyMzEwMTIsLTY1NzY2NTkwNiw4MDg1NzcwNzMs
MTg4MDUwODUxMiwtMTUwNzY1Nzk5Niw0MjQ5MDkxNzksLTEwMz
UwOTkxMzQsMTM3MTc3MDg3OCwtNjMxMjcxMzQ0LC0xODAwMDk1
MzA4LDE0NTg3NDE4OTAsLTE4NDA4MzY0MSwxMzUxODczMjc3XX
0=
-->