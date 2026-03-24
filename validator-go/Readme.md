

# ✅ Validator Golang

Library **Validator Golang** digunakan untuk memvalidasi data pada bahasa Go secara efisien, konsisten, dan aman. melalui pendekatan berbasis *tag*, developer dapat menulis kode validasi yang ringkas, mudah dibaca, dan memiliki performa tinggi.
  

---

  

## 📚 Table of Contents

  

1. [Pendahuluan](#-pendahuluan)

2. [Arsitektur dan Komponen Utama](#-arsitektur-dan-komponen-utama)

3. [Instalasi](#-instalasi)

4. [Contoh Penggunaan](#-contoh-penggunaan)

5. [Kustom Validasi](#-kustom-validasi)

6. [Error Handling](#-error-handling)

7. [Troubleshooting](#-troubleshooting)

8. [Referensi](#-referensi)

  

---

  

## 🧩 Pendahuluan

  

Validasi adalah proses penting dalam pengembangan aplikasi untuk memastikan data yang diproses sudah benar, lengkap, dan sesuai dengan format sistem.

Dengan validasi yang baik, kesalahan data dapat dicegah lebih awal sebelum sampai ke tahap penyimpanan atau logika bisnis.

  

### 🔍 Kapan Validasi Dilakukan

  

Validasi umumnya dilakukan pada beberapa tahap berikut:

-  **Input Request User** — memverifikasi data dari pengguna (_Verification_).

-  **Business Logic** — memastikan role dan otorisasi sesuai (_Role Business Access Control_).

-  **Database Layer** — menjaga *constraint* seperti `UNIQUE` dan `NOT NULL`.

  

### 🎯 Mengapa Validasi Diperlukan

  

- Memastikan data sesuai dengan format dan aturan sistem.

- Meningkatkan keamanan dengan mencegah *injection* atau data berbahaya.

- Menjaga konsistensi dan integritas data.

- Mendeteksi kesalahan input lebih awal.

  

---

  

## ⚙️ Arsitektur dan Komponen Utama

  

### 🔸 Manual Validation

  

- Umumnya menggunakan banyak *if statements*.

- Kurang efisien jika jumlah aturan validasi meningkat.

- Sulit untuk dikelola dan rawan human error.

  

### 🔸 Library Validation (Validator Package)

  

- Menggunakan tag untuk mendefinisikan aturan validasi.

- Thread-safe dan berperforma tinggi.

- Memiliki banyak *baked-in rules* yang siap pakai.

- Mendukung cache internal untuk optimasi performa.

  

### 📦 Komponen Utama

  


| Komponen           | Deskripsi                                                |
|--------------------|----------------------------------------------------------|
| **Tag-Based Rules** | Menentukan aturan validasi langsung di struct tag.       |
| **Thread Safe**     | Aman digunakan dalam environment *concurrent*.           |
| **Caching System**  | Menyimpan hasil parsing tag agar validasi lebih cepat.   |
| **Custom Validator**| Mendukung pembuatan aturan validasi sendiri.             |


  

---

  

## 💻 Instalasi

  

Gunakan `go get` untuk menambahkan dependensi ke proyek Anda:

  

```bash

go  get  github.com/go-playground/validator/v10

```

  

Import pada file Go Anda:

  

```go

import  "github.com/go-playground/validator/v10"

```

  

Inisialisasi validator:

  

```go

validate := validator.New()

```
atau
```go

validate := validator.New(validator.WithRequiredStructEnabled())
```
  

---

  

## 🚀 Contoh Penggunaan

  

### 1️⃣ Validasi Struct Sederhana

  

```go

package main



import (

    "fmt"

    "github.com/go-playground/validator/v10"

)



type User struct {

    Name string `validate:"required"`

    Email string `validate:"required,email"`

    Age int `validate:"gte=18,lte=65"`

}



func main() {

    validate: = validator.New()



        user: = User {

        Name: "John Doe",

        Email: "john.doe@example.com",

        Age: 17,

    }



        err: = validate.Struct(user)

    if err != nil {

        fmt.Println("Validation failed:", err)

    } else {

        fmt.Println("Validation success!")

    }

}
```

  

---

  

## 🧠 Kustom Validasi

  

Anda dapat membuat aturan validasi sendiri menggunakan fungsi kustom:

  

```go

validate.RegisterValidation("even", func(fl validator.FieldLevel) bool {

            return fl.Field().Int() % 2 == 0

})

```

  

Penggunaan dalam struct:

  

```go

type Number struct {

    Value int `validate:"even"`

}

```

  

---

  

## ⚡ Error Handling

  

Ketika validasi gagal, `validator` akan mengembalikan error yang dapat diproses lebih lanjut:

  

```go
if err: = validate.Struct(user);
err != nil {

    for _, err: = range err.(validator.ValidationErrors) {

        fmt.Printf("Field '%s' failed on the '%s' tag\n", err.Field(), err.Tag())

    }

}

```

contoh lainnya dalam bentuk function:
```
// GetErrors returns the validation errors from a validator error.
func GetErrors(err error) validator.ValidationErrors {
    if err == nil {
        return nil
    }
    if errs, ok: = err.(validator.ValidationErrors);
    ok {
        return errs
    }
    return nil
}

// GetErrorsFullStr returns the full validation errors from a validator error.
func GetErrorsFullStr(err error) string {
    if err == nil {
        return ""
    }

    var errors validator.ValidationErrors
    errors, ok: = err.(validator.ValidationErrors)
    if !ok {
        return ""
    }

    if len(errors) == 0 {
        return ""
    }

    var result = make([] string, len(errors))
    for i, e: = range errors {
        result[i] = fmt.Sprintf("%s: %s", e.Field(), e.Tag())
    }
    return strings.Join(result, ", ")
}

// GetErrorFirstStr returns the first validation error from a validator error.
func GetErrorFirstStr(err error) string {
    if err == nil {
        return ""
    }

    var errors validator.ValidationErrors
    errors, ok: = err.(validator.ValidationErrors)
    if !ok {
        return ""
    }

    if len(errors) == 0 {
        return ""
    }

    errArr: = errors[0]
    return fmt.Sprintf("%s: %s", errArr.Field(), errArr.Tag())
}
```

  

Output:

```

Field 'Age' failed on the 'gte' tag

```

  

---

  

## 🩵 Troubleshooting

  


| Masalah                        | Penyebab Umum                           | Solusi                                                        |
|--------------------------------|------------------------------------------|---------------------------------------------------------------|
| `panic: interface conversion`  | Error tidak di-cast ke `ValidationErrors` | Gunakan `err.(validator.ValidationErrors)`                    |
| Tag tidak berfungsi            | Salah penulisan tag                      | Periksa ejaan, gunakan huruf kecil (`required`, bukan `Required`) |
| Validator tidak berjalan       | Struct tidak diekspor                    | Pastikan field diawali huruf kapital                          |


  

---

  

## 📚 Referensi

  

- 📘 [Go Playground Validator v10 Documentation](https://pkg.go.dev/github.com/go-playground/validator/v10)

- 🧱 [Source Code di GitHub](https://github.com/go-playground/validator)

- 🔍 [Tag Built-in List](https://pkg.go.dev/github.com/go-playground/validator/v10#section-readme)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkwODMwNzE3MiwyMDYyOTU5NzEzLC0xNz
g2MjY0MjE2LDE2ODIwNDUyMDIsLTI1NjM1MzcxMiw3MTE3MDI3
OTJdfQ==
-->