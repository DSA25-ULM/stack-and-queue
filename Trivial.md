# Penjelasan Stack dan Queue Menggunakan `std::deque` di C++ STL

## 1. Mengapa Stack dan Queue di STL Menggunakan deque

Dalam C++ Standard Template Library (STL), baik `stack` maupun `queue` secara default menggunakan `std::deque` sebagai kontainer dasar. Hal ini karena `deque`:

- Memiliki efisiensi tinggi untuk operasi di kedua ujung (depan dan belakang)

- Memberikan akses acak ke elemen (tidak seperti list yang hanya bisa traversal sekuensial)

- Lebih fleksibel daripada `vector`, terutama untuk operasi `push_front` dan `pop_front`

```
std::stack<int> st;     // Secara default menggunakan std::deque<int>
std::queue<int> q;      // Juga menggunakan std::deque<int>
```

## 2. Mekanisme Internal `std::deque` - Metode Hybrid

`std::deque` adalah singkatan dari __double-ended queue__, dan implementasinya menggunakan metode __hybrid__: kombinasi array dan linked list.

### Struktur Internal:

- Elemen disimpan dalam blok-blok kecil (misalnya 512 byte per blok)

- Terdapat array indeks (pointer table) yang menunjuk ke masing-masing blok

- Tiap blok memiliki beberapa elemen, dan alokasi blok tidak harus berdekatan di memori (seperti linked list)

### Diagram Struktur

```
Index Table (Pointer ke tiap blok)
+--------+--------+--------+--------+
| idx[0] | idx[1] | idx[2] | idx[3] |
+---|----+---|----+---|----+---|----+
    |        |        |        |
    v        v        v        v

 Blok-blok memori (tiap blok punya beberapa elemen)
+------------+   +------------+   +------------+   +------------+
|  10  20  - |   |  30  40  - |   |  50  60  - |   |  70  80  - |
+------------+   +------------+   +------------+   +------------+
```

> Tanda `-` menandakan slot kosong dalam blok.

Operasi `push_back(90)`

```
+------------+   +------------+   +------------+   +------------+   +------------+
|  10  20  - |   |  30  40  - |   |  50  60  - |   |  70  80  - |   |  90  -   - |
+------------+   +------------+   +------------+   +------------+   +------------+
```

Operasi `pop_front()`

```
+------------+   +------------+   +------------+   +------------+   +------------+
|  x  20  -  |   |  30  40  - |   |  50  60  - |   |  70  80  - |   |  90  -   - |
+------------+   +------------+   +------------+   +------------+   +------------+
```

## 3. Keunggulan dan Kompleksitas Operasi


| Operasi      | Kompleksitas     | Keterangan      |
| Operasi         | Kompleksitas | Keterangan                                |
|-----------------|--------------|--------------------------------------------|
| Akses Elemen    | O(1)         | Sama seperti array (`[]` atau `at()` )     |
| `push_back`     | O(1)         | Tambah elemen di akhir                     |
| `push_front`    | O(1)         | Tambah elemen di depan                     |
| `pop_back`      | O(1)         | Hapus elemen dari belakang                 |
| `pop_front`     | O(1)         | Hapus elemen dari depan                    |
| Insert tengah   | O(n)         | Kurang efisien untuk random insert/delete  |


## 4. Ringkasan

- `std::deque` menggabungkan efisiensi array dan fleksibilitas linked list

- Cocok untuk stack dan queue karena bisa push/pop dari dua arah

- Struktur internal berupa array of blocks membuat alokasi memori efisien dan scalable

- Jika digunakan untuk mengimplementasikan `stack` dan `queue`, `deque` memberikan trade-off terbaik antara kecepatan dan fleksibilitas dibanding `vector` atau `list`.