# Namespaces dan PSR-4 Autoloading dengan Composer

Tutorial singkat ini menjelaskan cara memahami dan menggunakan **Namespaces**, serta mengimplementasikan **PSR-4 Autoloading** menggunakan **Composer**.

---

## 1. Apa Itu Namespace?

**Namespace** adalah cara untuk mengorganisasi kode di PHP agar menghindari konflik nama. Namespace memungkinkan Anda menggunakan nama yang sama untuk kelas, fungsi, atau konstanta di tempat yang berbeda.

### Contoh Penggunaan:
```
<?php
namespace App\Models;

class User {
    public function __construct() {
        echo "Halo dari App\\Models\\User!";
    }
}
```

### Mengakses kelas User:
```
<?php
require 'src/Models/User.php';
use App\Models\User;
$user = new User();
```

## 2. Apa Itu PSR-4 Autoloading?
PSR-4 adalah standar autoloading yang memetakan namespace ke struktur direktori. Dengan PSR-4, Anda tidak perlu menulis require secara manual.

## 3. Cara Implementasi PSR-4 Autoloading dengan Composer

### Langkah 1: Inisialisasi Composer
Buat proyek baru dan inisialisasi Composer:
```
composer init
```

### Langkah 2: Tambahkan Autoload di composer.json
Tambahkan aturan autoload untuk namespace App:
```
{
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
}
```

### Langkah 3: Buat Struktur Direktori
Buat file dan folder sesuai namespace:
```
mkdir -p src/Models
touch src/Models/User.php
```
Isi file src/Models/User.php:
```
<?php
namespace App\Models;

class User {
    public function __construct() {
        echo "Halo dari App\\Models\\User!";
    }
}
```

### Langkah 4: Jalankan Composer Dump-Autoload
Jalankan perintah berikut untuk menghasilkan file autoload:
```
composer dump-autoload
```

### Langkah 5: Gunakan di Proyek Anda
Buat file index.php di root proyek:
```
<?php
require 'vendor/autoload.php';

use App\Models\User;

$user = new User();
```

Jalankan dengan:
```
php index.php
```
Output:
```
Halo dari App\Models\User!
```
Dengan langkah-langkah ini, Anda dapat menggunakan Namespaces dan PSR-4 Autoloading secara efisien dalam proyek PHP Anda. Selamat mencoba!
