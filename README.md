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

## 3. Algoritma Traversal Graf (Penelusuran Graf)

Traversal graf adalah proses mengunjungi setiap vertex dalam graf secara terstruktur dan memastikan tidak ada vertex yang dikunjungi lebih dari sekali (menghindari perulangan tanpa akhir akibat adanya siklus/loop pada graf). Ada dua algoritma utama yang sangat fundamental:

**A. Breadth-First Search (BFS)**
BFS adalah algoritma penelusuran yang bekerja secara melebar. Algoritma ini mengunjungi semua vertex tetangga terdekat terlebih dahulu sebelum melangkah ke level yang lebih dalam.
- **Analogi Nyata:** Seperti riak air yang menyebar secara melingkar saat Anda menjatuhkan batu ke dalam kolam tenang, atau seperti proses pelacakan kontak erat suatu virus.
- **Struktur Data Pendukung:** BFS menggunakan struktur data **Queue (Antrean - FIFO: First In First Out)** untuk melacak vertex mana yang harus dikunjungi berikutnya, serta sebuah array/vector boolean `visited` untuk mencatat vertex yang sudah pernah dikunjungi.
- **Langkah Kerja:**
  1. Masukkan vertex awal ke dalam _Queue_ dan tandai sebagai `visited`.
  2. Selama _Queue_ tidak kosong, keluarkan vertex dari depan antrean (sebut saja vertex $u$).
  3. Periksa semua tetangga dari vertex $u$. Jika tetangga tersebut belum ditandai `visited`, masukkan ke dalam Queue dan tandai sebagai `visited`.
  4. Ulangi langkah ini hingga _Queue_ kosong.
  
**B. Depth-First Search (DFS)**
DFS adalah algoritma penelusuran yang bekerja secara mendalam. Algoritma ini berjalan menelusuri satu jalur sedalam mungkin hingga mencapai titik buntu, lalu melakukan backtrack (kembali mundur) untuk mencoba jalur alternatif yang belum dieksplorasi.
- **Analogi Nyata:** Seperti Anda mencari jalan keluar di dalam labirin (maze). Anda akan terus berjalan menyusuri satu lorong sampai mentok, lalu kembali ke persimpangan terakhir untuk mencoba lorong lainnya.
- **Struktur Data Pendukung:** DFS secara alami menggunakan struktur data **Stack (Tumpukan - LIFO: Last In First Out)**. Dalam praktiknya, DFS paling sering diimplementasikan menggunakan fungsi Rekursi karena tumpukan fungsi memori komputer secara otomatis bertindak sebagai _Stack_.
- **Langkah Kerja:**
  1. **Masukkan vertex awal ke dalam _Queue_ dan tandai sebagai `visited`.**
  2. Cari tetangga dari vertex saat ini yang belum dikunjungi.
  3. Secara rekursif, panggil fungsi DFS untuk vertex tetangga tersebut.
  4. Jika semua tetangga dari vertex saat ini sudah dikunjungi, fungsi akan selesai dan otomatis mundur (_backtrack_) ke vertex sebelumnya.

---

## 4. Jalur Terpendek (Shortest Path)

Masalah jalur terpendek (*Shortest Path Problem*) bertujuan untuk mencari rute dengan total bobot terkecil atau jumlah langkah paling sedikit dari satu vertex (sumber) ke vertex lainnya dalam graf.

### A. Kasus Graf Tanpa Bobot (Unweighted Graph)
Jika graf tidak memiliki bobot, jalur terpendek murni diukur dari **jumlah lompatan / jalur (edges) yang paling sedikit**. Untuk kasus ini, algoritma **BFS adalah solusi yang paling optimal dan tepat**. Karena BFS menjelajah secara level-by-level, vertex yang pertama kali ditemukan oleh BFS dari titik sumber dijamin merupakan jalur terpendek menuju titik tersebut.

### B. Kasus Graf Berbobot (Weighted Graph)
Jika graf memiliki bobot (misalnya bobot menunjukkan jarak dalam kilometer), jumlah lompatan paling sedikit belum tentu menghasilkan jarak terkecil. Kita membutuhkan algoritma cerdas yang mempertimbangkan nilai bobot tersebut. Algoritma yang paling terkenal dan menjadi standar industri adalah **Algoritma Dijkstra**.

#### Algoritma Dijkstra
Dijkstra digunakan untuk mencari jalur terpendek dari satu titik sumber (*Single-Source Shortest Path*) ke semua titik lainnya pada graf berbobot, dengan syarat **semua bobot harus bernilai positif** (tidak boleh bernilai negatif).

- **Cara Kerja Dijkstra:**
  1. Inisialisasi jarak dari titik sumber ke dirinya sendiri dengan nilai `0`, dan jarak ke seluruh vertex lainnya dengan nilai tak hingga (`infinity`).
  2. Buat sebuah *Priority Queue* (Antrean Berprioritas) untuk menyimpan pasangan `(jarak, vertex)`. Atur agar antrean ini selalu mengeluarkan elemen dengan jarak terkecil terlebih dahulu (Min-Heap).
  3. Masukkan titik sumber ke dalam *Priority Queue*.
  4. Ambil vertex dengan jarak terkecil dari *Priority Queue* (sebut saja vertex $u$).
  5. Lakukan proses **Relaxation (Relaksasi)** pada seluruh tetangga dari vertex $u$ (sebut saja tetangga tersebut adalah $v$). Proses relaksasi memeriksa: *Apakah jarak menuju $v$ lewat $u$ lebih pendek daripada jarak menuju $v$ yang tercatat saat ini?*
     $$\text{Jika } \text{jarak}[u] + \text{bobot}(u, v) < \text{jarak}[v]$$
     Maka perbarui nilai `jarak[v]` dengan hasil yang baru dan masukkan `(jarak[v], v)` ke dalam *Priority Queue*.
  6. Ulangi proses ini sampai seluruh simpul dieksplorasi.

---

## 5. Minimum Spanning Tree (MST) - Algoritma Prim

### Apa itu Spanning Tree?
Sebelum masuk ke algoritma, mari pahami istilahnya terlebih dahulu. **Spanning Tree (Pohon Rentang)** adalah sub-graf dari sebuah graf berarah/tidak berarah yang menghubungkan **semua vertex** bersama-sama tanpa membentuk satu pun **siklus (cycle)**. Sebuah graf dengan $V$ buah vertex akan memiliki Spanning Tree dengan tepat $V - 1$ buah edge (jalur).

**Minimum Spanning Tree (MST)** adalah Spanning Tree yang memiliki **total bobot jalur paling minimum** di antara semua kemungkinan Spanning Tree yang bisa dibentuk dari graf tersebut. 

- **Aplikasi Nyata:** Pembangunan jaringan kabel listrik atau pipa air antar rumah di suatu kompleks. Kita ingin semua rumah terhubung satu sama lain (tanpa ada rumah yang terisolasi), namun dengan total panjang kabel atau pipa seminimal mungkin untuk menghemat anggaran.

### Algoritma Prim
Algoritma Prim adalah algoritma jenis *greedy* yang membangun MST dengan cara menumbuhkan pohon satu per satu vertex demi vertex dari suatu titik awal.

#### Logika Kerja Alami Algoritma Prim:
Bayangkan Anda sedang membangun jaringan jalan antar kota. Anda mulai dari satu kota sembarang. Kota ini sekarang masuk ke dalam area "Aman" (sudah terhubung). Dari kota tersebut, Anda melihat semua proyek jalan yang mengarah ke kota-kota tetangga yang belum terhubung. Anda pasti akan memilih membangun jalan yang **paling murah atau pendek**. 

Setelah jalan tersebut dibangun, kota tetangga tersebut sekarang ikut masuk ke dalam area "Aman". Proses ini diulang kembali dengan melihat semua pilihan jalan termurah keluar dari seluruh kota yang sudah aman menuju kota yang belum aman.

#### Langkah-Langkah Algoritma Prim secara Detail:
1. Sediakan sebuah array boolean `inMST` berukuran $V$ untuk menandai apakah suatu vertex sudah masuk ke dalam komponen MST atau belum. Inisialisasi semuanya dengan nilai `false`.
2. Sediakan array `key` untuk menyimpan bobot minimum untuk mencapai suatu vertex dari luar komponen MST. Inisialisasi titik awal dengan `0` dan titik lainnya dengan nilai tak hingga (`infinity`).
3. Buat sebuah *Priority Queue* (Min-Heap) untuk mengambil secara cepat jalur dengan bobot terkecil. Masukkan pasangan `(0, titik_awal)` ke dalam antrean.
4. Selama *Priority Queue* tidak kosong:
   - Ambil vertex dengan nilai `key` terkecil dari antrean, sebut saja vertex $u$.
   - Jika vertex $u$ sudah berada di dalam MST (`inMST[u] == true`), abaikan dan lanjutkan ke iterasi berikutnya (menghindari duplikasi).
   - Masukkan vertex $u$ ke dalam komponen MST dengan mengubah `inMST[u] = true`.
   - Telusuri semua tetangga dari vertex $u$ (sebut saja tetangga tersebut adalah $v$). Jika $v$ belum masuk MST (`inMST[v] == false`) dan bobot jalur $(u, v)$ lebih kecil dari nilai `key[v]` saat ini, maka:
     - Perbarui `key[v]` menjadi bobot jalur $(u, v)$.
     - Masukkan pasangan `(key[v], v)` ke dalam *Priority Queue*.
5. Proses selesai setelah kita mengulang sebanyak $V$ kali atau ketika antrean kosong, menghasilkan pohon terhubung dengan total bobot minimum.

### Simulasi Kode C++ untuk Algoritma Prim

Berikut adalah contoh program lengkap implementasi Algoritma Prim untuk mencari total bobot Minimum Spanning Tree. Kita menggunakan struktur data berpasangan `pair<int, int>` di mana elemen pertama adalah bobot dan elemen kedua adalah vertex tujuan.

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

// Definisikan tipe data pair untuk kemudahan pembacaan kode
// pair.first -> Bobot Jalur, pair.second -> Vertex Tujuan
typedef pair<int, int> pii;

class PrimGraph {
private:
    int V; // Jumlah Vertex
    vector<vector<pii>> adj; // Adjacency list berbobot

public:
    // Konstruktor
    PrimGraph(int V) {
        this->V = V;
        adj.resize(V);
    }

    // Fungsi menambah jalur berbobot (Graf Tidak Berarah)
    void addEdge(int u, int v, int weight) {
        adj[u].push_back(make_pair(weight, v));
        adj[v].push_back(make_pair(weight, u));
    }

    // Fungsi utama menghitung Minimum Spanning Tree dengan Algoritma Prim
    void calculateMST(int startVertex) {
        // Priority queue untuk memilih jalur dengan bobot terkecil (Min-Heap)
        // Format di dalam priority_queue: pair<bobot, vertex>
        priority_queue<pii, vector<pii>, greater<pii>> pq;

        // Array untuk menyimpan nilai minimum (key) untuk terhubung ke MST
        vector<int> key(V, 1e9); // 1e9 diasumsikan sebagai nilai tak hingga (infinity)

        // Array penanda apakah vertex sudah masuk dalam komponen MST
        vector<bool> inMST(V, false);

        // Variabel untuk menjumlahkan total bobot akhir dari MST
        int totalMSTWeight = 0;

        // Memulai dari vertex acuan yang ditentukan pengguna
        pq.push(make_pair(0, startVertex));
        key[startVertex] = 0;

        cout << "=== PROSES PEMBENTUKAN JALUR MST (PRIM) ===\n";

        while (!pq.empty()) {
            // Mengambil vertex dengan bobot terkecil
            int u = pq.top().second;
            int weight = pq.top().first;
            pq.pop();

            // Jika vertex sudah masuk MST, lewati untuk mencegah siklus
            if (inMST[u] == true) {
                continue;
            }

            // Masukkan vertex ini ke dalam MST
            inMST[u] = true;
            totalMSTWeight += weight;
            
            if (u != startVertex) {
                cout << "Jalur ditambahkan ke MST membawa Vertex " << u 
                     << " dengan bobot jalur: " << weight << "\n";
            }

            // Periksa semua tetangga dari vertex u
            for (auto neighbor : adj[u]) {
                int v = neighbor.second;
                int weight_uv = neighbor.first;

                // Jika v belum masuk MST dan bobot baru ini lebih kecil dari nilai key lama milik v
                if (inMST[v] == false && key[v] > weight_uv) {
                    key[v] = weight_uv;
                    pq.push(make_pair(key[v], v));
                }
            }
        }

        cout << "\n=========================================\n";
        cout << "Total Bobot Akhir Minimum Spanning Tree = " << totalMSTWeight << "\n";
        cout << "=========================================\n";
    }
};

int main() {
    // Membuat Graf dengan 5 Vertex (diindeks dari 0 sampai 4)
    PrimGraph g(5);

    // Menambahkan jalur berbobot sesuai skenario contoh graf
    g.addEdge(0, 1, 2); // Jalur 0-1 bobot 2
    g.addEdge(0, 3, 6); // Jalur 0-3 bobot 6
    g.addEdge(1, 2, 3); // Jalur 1-2 bobot 3
    g.addEdge(1, 3, 8); // Jalur 1-3 bobot 8
    g.addEdge(1, 4, 5); // Jalur 1-4 bobot 5
    g.addEdge(2, 4, 7); // Jalur 2-4 bobot 7
    g.addEdge(3, 4, 9); // Jalur 3-4 bobot 9

    // Hitung MST dimulai dari Vertex 0
    g.calculateMST(0);

    return 0;
}
```
OUTPUT:
```
=== PROSES PEMBENTUKAN JALUR MST (PRIM) ===
Jalur ditambahkan ke MST membawa Vertex 1 dengan bobot jalur: 2
Jalur ditambahkan ke MST membawa Vertex 2 dengan bobot jalur: 3
Jalur ditambahkan ke MST membawa Vertex 4 dengan bobot jalur: 5
Jalur ditambahkan ke MST membawa Vertex 3 dengan bobot jalur: 6

=========================================
Total Bobot Akhir Minimum Spanning Tree = 16
=========================================
```
