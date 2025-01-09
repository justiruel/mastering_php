# Tutorial Object-Oriented Programming (OOP) di PHP

Tutorial ini mencakup dasar-dasar dan konsep lanjutan dalam **Object-Oriented Programming (OOP)** di PHP, termasuk inheritance, polymorphism, traits, interfaces, abstract classes, magic methods, serta dependency injection.

---

## 1. Dasar-Dasar OOP di PHP

OOP adalah paradigma pemrograman berbasis objek. Komponen utama OOP:
- **Kelas**: Blueprint atau cetak biru untuk objek.
- **Objek**: Instance dari kelas.
- **Properti**: Variabel dalam kelas.
- **Metode**: Fungsi dalam kelas.

### Contoh:
```
<?php
class User {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function sayHello() {
        return "Hello, " . $this->name;
    }
}

$user = new User("John");
echo $user->sayHello(); // Output: Hello, John
```

## 2. Konsep Lanjutan OOP
### a. Inheritance (Pewarisan)
Memungkinkan satu kelas mewarisi properti dan metode dari kelas lain.

```
<?php
class Animal {
    public function eat() {
        return "Eating...";
    }
}

class Dog extends Animal {
    public function bark() {
        return "Barking...";
    }
}

$dog = new Dog();
echo $dog->eat();  // Output: Eating...
echo $dog->bark(); // Output: Barking...
```

### b. Polymorphism
Kemampuan objek untuk memiliki banyak bentuk melalui metode override.

```
<?php
class Animal {
    public function sound() {
        return "Some sound...";
    }
}

class Dog extends Animal {
    public function sound() {
        return "Bark";
    }
}

$animal = new Dog();
echo $animal->sound(); // Output: Bark
```

### c. Traits
Digunakan untuk berbagi metode di beberapa kelas.

```
<?php
trait Logger {
    public function log($message) {
        echo $message;
    }
}

class User {
    use Logger;
}

$user = new User();
$user->log("Logging message..."); // Output: Logging message...
```

### d. Interfaces
Mendefinisikan kontrak tanpa implementasi. Kelas harus mengimplementasikan metode yang didefinisikan dalam interface.

```
<?php
interface Animal {
    public function makeSound();
}

class Cat implements Animal {
    public function makeSound() {
        return "Meow";
    }
}

$cat = new Cat();
echo $cat->makeSound(); // Output: Meow
```

### e. Abstract Classes
Kelas abstrak tidak dapat diinstansiasi dan bisa memiliki metode abstrak.

```
<?php
abstract class Animal {
    abstract public function sound();
}

class Dog extends Animal {
    public function sound() {
        return "Bark";
    }
}

$dog = new Dog();
echo $dog->sound(); // Output: Bark
```

## 3. Magic Methods
Magic methods adalah metode khusus di PHP yang diawali dengan __.

Contoh Magic Methods:
```
<?php
class Magic {
    private $data = [];

    public function __construct($name) {
        echo "Constructor: Hello, $name\n";
    }

    public function __get($key) {
        return $this->data[$key] ?? null;
    }

    public function __set($key, $value) {
        $this->data[$key] = $value;
    }

    public function __call($name, $arguments) {
        return "Calling method '$name' with arguments: " . implode(", ", $arguments);
    }
}

$magic = new Magic("John");
$magic->age = 30;
echo $magic->age . "\n"; // Output: 30
echo $magic->someMethod("arg1", "arg2"); // Output: Calling method 'someMethod' with arguments: arg1, arg2
```

## 4. Dependency Injection (DI) dan Service Containers
### Apa itu Dependency Injection?
DI adalah pola desain di mana objek menerima dependensi mereka dari luar, bukan membuatnya sendiri.

Contoh:
Tanpa DI:

```
<?php
class Database {
    public function connect() {
        return "Connecting to database...";
    }
}

class User {
    private $db;

    public function __construct() {
        $this->db = new Database(); // Hard-coded dependency
    }
}
```

Dengan DI:

```
<?php
class Database {
    public function connect() {
        return "Connecting to database...";
    }
}

class User {
    private $db;

    public function __construct(Database $db) {
        $this->db = $db;
    }

    public function fetchUser() {
        return $this->db->connect() . " Fetching user data...";
    }
}

$db = new Database();
$user = new User($db);
echo $user->fetchUser(); // Output: Connecting to database... Fetching user data...
```

### Apa itu Service Container?
Service container adalah alat untuk mengelola dependensi dalam aplikasi. Framework seperti Laravel menggunakan service container untuk DI.
