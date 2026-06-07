# Panduan Lengkap Teori Graf

Selamat datang di panduan komprehensif mengenai **Graf (Graph)**. Dokumen ini dirancang untuk membawa Anda memahami konsep graf dari dasar (pengertian) hingga implementasi algoritma tingkat lanjut seperti *Shortest Path* dan *Minimum Spanning Tree*. Seluruh materi disajikan secara mendalam dengan pendekatan yang natural, runtut, dan mudah dipahami.

---

## 1. Pengertian Graf (Graph)

Dalam dunia ilmu komputer dan matematika, **Graf** adalah sebuah struktur data non-linear yang digunakan untuk menggambarkan hubungan atau relasi antar objek. Objek-objek tersebut direpresentasikan sebagai titik, sedangkan hubungan antar objek tersebut direpresentasikan sebagai garis.

Secara formal, graf didefinisikan sebagai pasangan set $G = (V, E)$, di mana:
- **$V$ (Vertex / Node):** Adalah kumpulan titik atau entitas dalam graf. Contoh objek nyata: nama kota, pengguna media sosial, atau komputer dalam jaringan.
- **$E$ (Edge / Jalur):** Adalah kumpulan garis atau hubungan yang menghubungkan sepasang Vertex. Contoh nyata: jalan raya yang menghubungkan dua kota, hubungan pertemanan, atau kabel jaringan.

### Karakteristik dan Jenis Graf

Berdasarkan karakteristik jalurnya, graf dibagi menjadi beberapa jenis utama:

1. **Berdasarkan Arah Jalur (Direction):**
   - **Undirected Graph (Graf Tidak Berarah):** Jalur antara dua vertex tidak memiliki arah. Jika Vertex A terhubung dengan Vertex B, maka Anda bisa berjalan dari A ke B sekaligus dari B ke A. Hubungan ini bersifat simetris (seperti pertemanan di Facebook).
   - **Directed Graph / Digraph (Graf Berarah):** Jalur memiliki arah spesifik yang ditunjukkan oleh anak panah. Jika ada jalur dari A ke B, belum tentu ada jalur dari B ke A (seperti sistem *follower* di Instagram atau X/Twitter).

2. **Berdasarkan Bobot (Weight):**
   - **Unweighted Graph (Graf Tanpa Bobot):** Semua jalur memiliki nilai atau kedudukan yang sama. Hubungan hanya berupa "ada" atau "tidak ada".
   - **Weighted Graph (Graf Berbobot):** Setiap jalur memiliki nilai/bobot (*weight*) tertentu. Bobot ini bisa merepresentasikan jarak fisik, biaya finansial, waktu tempuh, atau kapasitas bandwidth.

---

## 2. Representasi Graf: Adjacency List (Daftar Keketanggaan)

Untuk memproses graf di dalam kode pemrograman, kita perlu merepresentasikannya ke dalam struktur data tertentu. Ada dua metode yang paling populer: *Adjacency Matrix* (Matriks Keketanggaan) dan *Adjacency List* (Daftar Keketanggaan). 

Dalam panduan ini, kita akan berfokus pada **Adjacency List** karena jauh lebih efisien dalam penggunaan memori, terutama untuk graf yang bersifat *sparse* (graf yang jumlah jalurnya relatif sedikit dibanding total vertex yang ada).

### Cara Kerja Adjacency List
Konsepnya sangat sederhana: Kita membuat sebuah array atau list berukuran sebesar jumlah Vertex. Setiap indeks pada list tersebut mewakili sebuah Vertex, dan di dalam indeks tersebut terdapat daftar (list/vector) berisi Vertex-vertex lain yang terhubung langsung dengannya.

Mari kita ambil contoh graf tidak berarah dan tanpa bobot yang sederhana dengan 4 buah vertex (0, 1, 2, 3) dan bentuk hubungan sebagai berikut:
- Vertex 0 terhubung dengan 1 dan 2
- Vertex 1 terhubung dengan 0, 2, dan 3
- Vertex 2 terhubung dengan 0 dan 1
- Vertex 3 terhubung dengan 1

### Implementasi Kode Adjacency List (C++)

Berikut adalah implementasi kode yang bersih dan terstruktur dalam bahasa C++ untuk merepresentasikan contoh graf di atas:

```cpp
#include <iostream>
#include <vector>

using namespace std;

// Kelas untuk merepresentasikan Graf menggunakan Adjacency List
class Graph {
private:
    int numVertices; // Menyimpan jumlah total vertex dalam graf
    vector<vector<int>> adjList; // Vector dari vector untuk menyimpan daftar tetangga

public:
    // Konstruktor untuk inisialisasi graf dengan jumlah vertex tertentu
    Graph(int vertices) {
        numVertices = vertices;
        adjList.resize(vertices); // Mengatur ukuran vector sesuai jumlah vertex
    }

    // Fungsi untuk menambahkan jalur (edge) antar vertex (Undirected Graph)
    void addEdge(int src, int dest) {
        // Karena graf tidak berarah, tambahkan dest ke list src, dan sebaliknya
        adjList[src].push_back(dest);
        adjList[dest].push_back(src);
    }

    // Fungsi untuk menampilkan representasi Adjacency List ke layar
    void printGraph() {
        cout << "=== REPRESENTASI ADJACENCY LIST ===\n";
        for (int i = 0; i < numVertices; i++) {
            cout << "Vertex " << i << " terhubung dengan: [ ";
            for (int neighbor : adjList[i]) {
                cout << neighbor << " ";
            }
            cout << "]\n";
        }
    }
};

int main() {
    // Membuat objek graf dengan 4 vertex (0, 1, 2, 3)
    Graph g(4);

    // Menambahkan jalur sesuai contoh skenario
    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 2);
    g.addEdge(1, 3);

    // Menampilkan graf ke layar
    g.printGraph();

    return 0;
}
```
OUTPUT:
```
=== REPRESENTASI ADJACENCY LIST ===
Vertex 0 terhubung dengan: [ 1 2 ]
Vertex 1 terhubung dengan: [ 0 2 3 ]
Vertex 2 terhubung dengan: [ 0 1 ]
Vertex 3 terhubung dengan: [ 1 ]
```

Penjelasan Kode Langkah demi Langkah:
1. `vector<vector<int>> adjList;:` Kita menggunakan struktur data vector dinamis dari pustaka C++. Baris luar bertindak sebagai baris vertex utama, dan elemen di dalamnya bertindak sebagai daftar tetangga dinamis.
2. `adjList.resize(vertices);:` Fungsi ini mengalokasikan ruang memori awal di memori agar kita bisa mengakses indeks dari 0 sampai vertices - 1 tanpa terjadi error out of bounds.
3. `void addEdge(int src, int dest):` Fungsi ini menerima dua parameter, yaitu titik asal (`src`) dan titik tujuan (`dest`). Karena contoh ini adalah graf tidak berarah, kita melakukan dua operasi `push_back:` memasukkan `dest` ke dalam list milik `src`, dan memasukkan `src` ke dalam list milik `dest`. Jika Anda ingin membuat graf berarah, Anda cukup menghapus baris kedua (`adjList[dest].push_back(src);`).
4. `printGraph():` Kita menggunakan perulangan bersarang (nested loop). Perulangan pertama menyusuri setiap vertex utama, dan perulangan kedua (berbasis range-based for loop) mencetak seluruh tetangga yang terhubung dengan vertex tersebut.
---

