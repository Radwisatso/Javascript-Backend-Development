
# Javascript ES6

### Prerequisite
- Non-primitive data (array and object)
- Functions
- Iteration (looping)
- CLI (Command Line Interface)

### Agenda

- Pass by References
- Built-in Functions
- node `process.argv`
- `module.exports` by CommonJS

## Introduction
Hai developers! Kali ini saya akan berbagi mengenai fitur-fitur yang disediakan oleh Javascript ES6 dan NodeJs. Tentu nya yang kita bahas di sini cukup spesifik, yaitu:

- Mengetahui cara kerja **Pass by References** yang terjadi pada tipe data non primitif
- Penggunaan **Built-in functions** agar memudahkan kita dalam melakukan proses development
- Menggunakan `process.argv` sebagai argumen-argumen yang dikirim melalui command line
- Pemisahan kode menjadi beberapa file menggunakan `module.exports`

## Pass By References
Teman-teman tentu nya perlu mengetahui bahwa terdapat tipe data non-primitif pada Javascript, yaitu `array []` dan `object {}`. Kedua tipe data tersebut salah satu sifat nya memiliki *address*. Jadi anggaplah setiap kita membuat sebuah array, maka tercipta lah *address* dengan nama *array-01* di dalam memory Javascript misalnya. Tetapi sebelum masuk penjelasan Pass by References pada *non-primitive data type*, saya akan coba mencontohkan pada *primitive data type* terlebih dahulu.

Mari kita coba menuliskan kode seperti di bawah ini
```js
// Deklarasikan variabel
let name1 = "Raditya"
let name2 = name1

// Melakukan "Concatination" a.k.a. penggabungan string type

name2 += "WOW"

console.log(name1) // "Raditya"
console.log(name2) // "RadityaWOW"

```
Kita bisa melihat pada kode di atas terdapat *assignment variable* pada `name2` dengan nilai nya dari variabel `name1`. Ketika dilakukan *concatination* pada variabel `name2`, maka nilai nya akan berubah sesuai dengan nilai yang digabungkan. Tetapi *concatination* tersebut tidak mempengaruhi variabel `name1`. Hal ini membuktikan bahwa *primitive data type* tidak bisa melakukan *Pass by Reference*

Sekarang, mari kita lihat jika menggunakan tipe data object
```js
// Deklarasi variabel
let obj1 = { name: "Raditya", age: 100 }
let obj2 = obj1 

// Melakukan prop reassignment pada key "age" dalam variabel obj2
obj2.age = 300

console.log(obj1) // { name: "Raditya", age: 300} > Loh kok bisa? padahal yang diubah obj2 :)
console.log(obj2) // { name: "Raditya", age: 300}
```

Jika diperhatikan kode di atas, variabel `obj2` diassign dengan nilai dari `obj1`. Tetapi ketika melakukan perubahan nilai pada property milik `obj2`, mengapa `obj1` juga ikut berubah? Inilah yang dinamakan **Pass By Reference**

Hal ini terjadi karena kita (mungkin tidak sengaja) melakukan assignment *address* variabel `obj1` kepada `obj2`. Maka kedua nya bisa kita simpulkan memiliki *address* **yang sama!**. 

Misalkan nama *address* nya adalah *object-01* pada variabel `obj1`, maka `obj2` juga memiliki nama alamat yang sama.

#### Bagaimana solusi nya?

Ketika kita ingin membuat sebuah object baru dengan address yang berbeda, ada beberapa cara untuk menerapkannya:

Pertama, kita bisa membuat object baru lagi
```js
// Deklarasi variabel
let obj1 = { name: "Raditya", age: 100 }
let obj2 = { name: obj1.name, age: obj1.age }

// Melakukan prop reassignment pada key "age" dalam variabel obj2
obj2.age = 300

console.log(obj1) // { name: "Raditya", age: 100} > ini tidak berubah a.k.a aman!
console.log(obj2) // { name: "Raditya", age: 300}
```

Kedua, kita bisa menggunakan `spread operator`. Tujuannya agar semua property pada `obj1` tersalin dan menempel pada object baru
```js
// Deklarasi variabel
let obj1 = { name: "Raditya", age: 100 }
let obj2 = { ...obj1 } // spread operator

// Melakukan prop reassignment pada key "age" dalam variabel obj2
obj2.age = 300

console.log(obj1) // { name: "Raditya", age: 100} > ini tidak berubah a.k.a aman!
console.log(obj2) // { name: "Raditya", age: 300}
```

Ketiga, kita bisa menggunakan built-in function bernama [structuredClone()](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone). 

*ps: function ini masih memiliki performance issue jika merujuk dari komunitas Javascript*
```js
// Deklarasi variabel
let obj1 = { name: "Raditya", age: 100 }
let obj2 = structureClone(obj1) // structuredClone

// Melakukan prop reassignment pada key "age" dalam variabel obj2
obj2.age = 300

console.log(obj1) // { name: "Raditya", age: 100} > ini tidak berubah a.k.a aman!
console.log(obj2) // { name: "Raditya", age: 300}
```

## Built-in Functions
Sekarang, kita akan membahas beberapa built-in functions yang seringkali digunakan untuk melakukan proses CRUD (*Create, Read, Update, Delete*)

- find()
- filter()
- split()
- join()
- forEach()
- map()
- slice()
- includes()

Optional:
- sort()

### find()
Penggunaan `find()` sudah sesuai dengan nama nya sendiri, yaitu untuk mencari. Mencari apa? Mencari sesuatu dari **kumpulan data (array)** sesuai dengan apa yang kita minta a.k.a. *condition*. `find()` akan mengembalikan **satu nilai** berupa nilai yang dicari sesuai kondisinya. Berikut contohnya:
```js
let arr = [
    {
        name: 'Raditya'
    },
    {
        name: 'Budi',
    },
    {
        name: 'Tono'
    }
]

const foundName = arr.find((element) => element.name === 'Budi')

console.log(foundName) // { name: 'Budi' }
```
Cara bekerja `find()` sangat mirip dengan iterasi `for`, karena pada dasar nya `find()` adalah iterasi juga. Perlu diketahui bahwa `find()` bekerja dari data paling kiri/atas. Jadi jika ada data yang nilai nya duplikat, `find()` hanya akan **mengambil data yang pertama kali ketemu**.

### filter()
Penggunaan `filter()` juga sesuai dengan nama nya sendiri, yaitu untuk menyaring kumpulan data menjadi **kumpulan data baru** sesuai dengan kondisi yang diminta. Perbedaannya dengan `find()` hanya terletak dari bentuk dan jumlah data yang dikembalikan. Jika `find()` mengembalikan satu data, maka `filter()` mengembalikan **lebih dari satu data**. Berikut contoh nya:
```js
let arr = [
    {
        name: 'Raditya',
        age: 20
    },
    {
        name: 'Budi',
        age: 30
    },
    {
        name: 'Tono',
        age: 10
    }
]

const foundName = arr.filter((element) => element.age > 15)

console.log(foundName) 
/*
[
    {
        name: 'Raditya',
        age: 20
    },
    {
        name: 'Budi',
        age: 30
    }
]
*/
```

### split()

`split()` digunakan pada sebuah data yang bertipe *string* dan mengubah nya menjadi sebuah *array*. Arti dari function itu sendiri yaitu memisahkan satu kesatuan *string* dipecah menjadi beberapa *string* yang berkumpul dalam 1 (satu) *array* sesuai dengan karakter pembatas yang diminta. Berikut contohnya:
```js
let names = 'Raditya;Budi;Tono'

// cerita nya string di atas akan dipisah berdasarkan semicolon (;)
let splittedNames = names.split(';')

console.log(splittedNames) // ['Raditya', 'Budi', 'Tono']
```

### join()

Built-in function ini adalah kebalikan dari `split()`, menggabungkan elemen-elemen *string* di dalam sebuah *array* dengan karakter pemisah yang dikirimkan. Berikut contohnya:
```js
let names = ['Raditya', 'Budi', 'Tono']

// cerita nya array of string di atas akan digabungkan kembali menggunakan semicolon (;)
let joinedNames = names.join(';'

console.log(joinedNames) // 'Raditya;Budi;Tono'

```

### forEach()

`forEach()` adalah built-in function khusus tipe data *array* yang cara bekerja nya sama denga iterasi `for loop`. Built-in function ini melakukan iterasi pada sebuah *array* dan **tidak mengembalikan apapun**. Tentu nya, kita juga bisa mengakses *index* dan *array* nya itu sendiri Berikut contohnya:
```js
let names = ['Raditya', 'Budi', 'Tono']

names.forEach((name) => {
    console.log(name)
})

/*
output:

Raditya
Budi
Tono
*/
```

Contoh jika menggunakan *index*:
```js
let names = ['Raditya', 'Budi', 'Tono']

names.forEach((name, index) => {
    console.log(name + "-" + index)
})

/*
output:

Raditya-0
Budi-1
Tono-2
*/
```

### map()

Built-in function `map()` sangat mirip dengan `forEach()`. Akan tetapi `map()` sifat nya **mengembalikan sesuatu** berupa *array* baru (dan mungkin data yang dimodifikasi). Kita juga bisa menciptakan struktur data yang baru dari `map()`. Berikut contohnya:
```js
let names = ['Raditya', 'Budi', 'Tono']

let result = names.map((name) => {
    return name + 'HELLO'
})

console.log(result)
/*
[
    'RadityaHELLO',
    'BudiHELLO',
    'TonoHELLO'
]
*/
```
Contoh bila kita mengubah nya menjadi tipe data *object* per elemennya:
```js
let names = ['Raditya', 'Budi', 'Tono']

let result = names.map((name) => {
    return {
        name: name + 'HELLO'
    }
})

console.log(result)
/*
[
    { name: 'RadityaHELLO' },
    { name: 'BudiHELLO' },
    { name: 'TonoHELLO' }
]
*/
```

### slice()

Coming soon

### includes()

Coming soon

## node `process.argv`
#### Penting: Wajib menginstall nodejs terlebih dahulu
Tujuan dari penggunaan `process.argv` ialah agar kita bisa mengirimkan nilai (*value*) melalui CLI (*Command Line Interface*). Jadi harapannya dengan ini kita dapat mengirimkan argumen yang sifat nya dinamis melalui CLI. Berikut contoh sebelum menggukanan `process.argv`:
```js
// app.js
let num1 = 1
let num2 = 2

let result = num1 + num2

console.log(result) // 3

// jalankan perintah: node app.js
```
akan tetapi jika kita menggunakan `process.argv` menjadi seperti ini:
```js
// app.js
let num1 = Number(process.argv[2]) // perlu dilakukan convert type Number, karena semua argumen argv adalah string
let num2 = Number(process.argv[3])

let result = num1 + num2

console.log(result) // 3

// jalankan perintah: node app.js 1 
```

#### How to use it?

Coba jalankan kode ini
```js
// app.js
console.log(process.argv)

// jalankan perintah: node app.js

/*
output:

[
    *path node terinstall*,
    *path node dijalankan*
]
*/
```
Kita bisa melihat hasil nya bahwa `process.argv` mengembalikan sebuah data bertipe *array* dengan jumlah 2 (dua) elemen. Elemen pertama adalah *path* node terinstall dan elemen kedua adalah *path* node dijalankan. Lalu di mana kah argumen baru akan diterima? Argumen baru akan diterima pada elemen ketiga dan seterusnya alias *index* kedua. Berikut contohnya:
```js
// app.js
console.log(process.argv)

// jalankan perintah: node app.js Raditya "Hello World"

/*
output:

[
    *path node terinstall*,
    *path node dijalankan*,
    'Raditya',
    'Hello World'
]
*/
```
Tentu nya kita juga bisa menghilangkan kedua *path* di elemen pertama dan kedua menggunakan built-in function `slice()`. Berikut contohnya:
```js
// app.js
console.log(process.argv.slice(2)) // kita ingin menghilangkan dua elemen pertama

// jalankan perintah: node app.js Raditya "Hello World"

/*
output:

[
    *path node terinstall*,
    *path node dijalankan*,
    'Raditya',
    'Hello World'
]
*/
```