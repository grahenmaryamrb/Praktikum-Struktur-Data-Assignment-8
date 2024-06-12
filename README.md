# <h1 align="center">Laporan Praktikum Modul Priority Queue</h1>
<p align="center">Grahen Maryam Rompas Basiran</p>
<p align="center">2311110062</p>

## Dasar Teori
### PRIORITY QUEUE
Priority queue adalah struktur data abstrak yang mirip dengan antrian (queue) biasa tetapi memiliki fitur tambahan di mana setiap elemen memiliki prioritas yang terkait dengannya. Elemen dengan prioritas lebih tinggi akan diproses lebih dulu dibandingkan dengan elemen dengan prioritas lebih rendah, terlepas dari urutan penyisipannya. Struktur data ini sangat berguna dalam berbagai aplikasi seperti routing jaringan, penjadwalan tugas, dan sistem operasi [1].

### HEAP
Heap adalah struktur data tree berbasis binary yang memenuhi properti heap. Terdapat dua jenis heap yang umum digunakan, yaitu max heap dan min heap.

Max heap:
Dalam max heap, nilai dari setiap node adalah lebih besar atau sama dengan nilai dari kedua anak-anaknya. Dengan kata lain, akar dari pohon selalu memiliki nilai maksimum.

Min heap:
Dalam min heap, nilai dari setiap node adalah lebih kecil atau sama dengan nilai dari kedua anak-anaknya. Dengan kata lain, akar dari pohon selalu memiliki nilai minimum [2].

## Guided
```C++
#include <iostream>
#include <algorithm>
using namespace std;

int H[50];
int heapSize = -1;

int parent (int i){
    return (i-1) / 2;
}

int leftChild (int i){
    return (2 * i) + 1;
}

int rightChild (int i){
    return (2 * i) + 2;
}

void shiftUp (int i){
    while (i > 0 && H[parent(i)] < H[i]){
        swap(H[parent(i)], H[i]);
        i = parent(i);
    }
}

void shiftDown (int i){
    int maxIndex = i;
    int l = leftChild(i);
    if (l <= heapSize && H[l] > H[maxIndex]){
        maxIndex = l;
    }
    int r = rightChild(i);
    if (r <= heapSize && H[r] > H[maxIndex]){
        maxIndex = r;
    }
    if (i != maxIndex) {
        swap(H[i], H[maxIndex]);
        shiftDown(maxIndex);
    }
}

void insert (int p){
    heapSize = heapSize + 1;
    H[heapSize] = p;
    shiftUp(heapSize);
}

int extractMax(){
    int result = H[0];
    H[0] = H[heapSize];
    heapSize = heapSize - 1;
    shiftDown(0);
    return result;
}

void changePriority (int i, int p){
    int oldp = H[i];
    H[i] = p;
    if (p > oldp){
        shiftUp(i);
    } else {
        shiftDown(i);
    }
}

int getMax(){
    return H[0];
}

void remove (int i){
    H[i] = getMax() + 1;
    shiftUp(i);
    extractMax();
}

int main(){
    insert(45);
    insert(20);
    insert(14);
    insert(12);
    insert(31);
    insert(7);
    insert(11);
    insert(13);
    insert(7);

    cout << "Priority Queue: ";
    for (int i = 0; i <= heapSize; ++i){
        cout << H[i] << " ";
    }
    cout << "\n";

    cout << "Node with the maximum priority: " << extractMax() << "\n";
    cout << "Priority queue after extracting maximum: ";
    for (int i = 0; i <= heapSize; ++i){
        cout << H[i] << " ";
    }
    cout << "\n";

    remove(3);

    cout << "Priority queue after removing the element: ";
    for (int i = 0; i <= heapSize; ++i){
        cout << H[i] << " ";
    }
    return 0;
}
```
### Penjelasan
Program diatas merupakan program untuk mengimplementasikan sebuah priority queue menggunakan struktur data heap maksimal atau max heap. 

1. Program mendefinisikan array `H` untuk menyimpan elemen-elemen di dalam heap dan variabel `heapSize` untuk melacak ukuran heap.

2. Program mendefinisikan beberapa fungsi bantu seperti `parent`, `leftChild`, dan `rightChild` untuk mendapatkan indeks dari simpul induk, anak kiri, dan anak kanan dari simpul tertentu.

3. Fungsi `shiftUp` dan `shiftDown` digunakan untuk mempertahankan properti heap. Fungsi `shiftUp` memperbaiki heap dengan memindahkan suatu simpul ke atas jika nilai simpul tersebut lebih besar dari nilai simpul induknya. Fungsi `shiftDown` memperbaiki heap dengan memindahkan suatu simpul ke bawah jika nilai simpul tersebut lebih kecil dari salah satu nilai simpul anaknya.

4. Fungsi `insert` digunakan untuk menambahkan elemen baru ke dalam heap. Fungsi ini menambahkan elemen baru ke akhir array, kemudian memanggil `shiftUp` untuk mempertahankan properti heap.

5. Fungsi `extractMax` digunakan untuk menghapus dan mengembalikan elemen dengan prioritas tertinggi (akar heap). Fungsi ini menukar akar dengan elemen terakhir di array, menghapus elemen terakhir, dan memanggil `shiftDown` untuk mempertahankan properti heap.

6. Fungsi `changePriority` digunakan untuk mengubah prioritas suatu elemen di dalam heap. Fungsi ini memanggil `shiftUp` atau `shiftDown` bergantung pada apakah prioritas baru lebih besar atau lebih kecil dari prioritas sebelumnya.

7. Fungsi `getMax` mengembalikan elemen dengan prioritas tertinggi (akar heap) tanpa menghapusnya dari heap.

8. Fungsi `remove` digunakan untuk menghapus elemen pada indeks tertentu dari heap. Fungsi ini mengubah prioritas elemen menjadi lebih besar dari semua elemen lain menggunakan `getMax` dan `shiftUp`, kemudian menghapus elemen tersebut dengan `extractMax`.

9. Pada `main`, program mendemonstrasikan penggunaan fungsi-fungsi tersebut dengan menambahkan beberapa elemen ke dalam heap, mengekstrak elemen dengan prioritas tertinggi, dan menghapus elemen pada indeks tertentu.

Seluruh operasi pada heap memiliki kompleksitas waktu O(log n), di mana n adalah jumlah elemen di dalam heap. Ini karena pada operasi `shiftUp` dan `shiftDown`, setiap level pada heap hanya diakses satu kali.

## Unguided
### 1. Modifikasi guided diatas yang mana heap dikonstruksi secara manual berdasarkan user.
```C++
#include <iostream>
#include <algorithm>
using namespace std;

// deklarasi ukuran maksimal H dan deklarasi heapSize
int H[50];
int heapSize = -1;

// fungsi void untuk dapatkan parent node
int parent(int i) {
    return (i - 1) / 2;
}

// fungsi void untuk dapatkan leftChiled node dari indeks i
int leftChild(int i) {
    return ((2 * i) + 1);
}

// fungsi void untuk dapatkan rightChild node dari indeks i
int rightChild(int i) {
    return ((2 * i) + 2);
}

// fungsi void untuk lakukan operasi shift up pada elemen dengan indeks i (elemen digerakkan ke atas heap)
void shiftUp(int i) {
    while (i > 0 && H[parent(i)] < H[i]) {
        swap(H[parent(i)], H[i]);
        i = parent(i);
    }
}

// fungsi void untuk lakukan operasi shift down pada elemen dengan indeks i (elemen digerakkan ke bawah heap)
void shiftDown(int i) {
    // deklarasi maxIndex
    int maxIndex = i;
    int l = leftChild(i);
    // jika leftChild (l) lebih kecil dari heapSize dan H[l] > H[maxindex]
    if (l <= heapSize && H[l] > H[maxIndex]) {
        maxIndex = l;
    }
    int r = rightChild(i);
    // jika rightChild (r) lebih kecil dari heapSize dan H[r] > H[maxindex]
    if (r <= heapSize && H[r] > H[maxIndex]) {
        maxIndex = r;
    }
    // jika indeks tidak sama dengan maxIndex
    if (i != maxIndex) {
        swap(H[i], H[maxIndex]);
        shiftDown(maxIndex);
    }
}

// fungsi void untuk tambahkan elemen baru 
void insert(int p) {
    heapSize = heapSize + 1;
    H[heapSize] = p;
    shiftUp(heapSize);
}

// fungsi void untuk ekstrak (keluarkan) elemen maksimal dimana nilainya paling besar
int extractMax() {
    int result = H[0];
    H[0] = H[heapSize];
    heapSize = heapSize - 1;
    shiftDown(0);
    return result;
}

// fungsi void untuk ubah priority elemen
void changePriority(int i, int p) {
    int oldp = H[i];
    H[i] = p;
    if (p > oldp) {
        shiftUp(i);
    } else {
        shiftDown(i);
    }
}

// mendapatkan elemen maksimum dari heap
int getMax() {
    return H[0];
}

// fungsi void untuk hapus elemen dari heap
void remove(int i) {
    H[i] = getMax() + 1;
    shiftUp(i);
    extractMax();
}

// fungsi void untuk atur ukuran elemen dan isi elemen di dalam heap oleh input user
void addHeap(int size) {
    for (int i = 0; i < size; i++) {
        int element;
        cout << "Enter element " << (i + 1) << ": ";
        cin >> element;
        insert(element);
    }
}

// main program (program utama)
int main() {
    // deklarasi a
    int a;
    // user input ukuran elemen (banyak elemen yang diinginkan)
    cout << "Enter the size of elements: ";
    cin >> a;
    // memanggil fungsi addHeap
    addHeap(a);

    // tampilkan priority queue dari elemen yang telah diinput user
    cout << "Priority Queue: ";
    for (int i = 0; i <= heapSize; ++i) {
        cout << H[i] << " ";
    }
    cout << "\n";

    // tampilkan node yang memiliki maximum priority dengan memanggil fungsi extractMax
    cout << "Node with maximum priority: " << extractMax() << "\n";

    // tampilkan priority queue setelah ekstrak elemen maximum"
    cout << "Priority queue after extracting maximum: ";
    for (int i = 0; i <= heapSize; ++i) {
        cout << H[i] << " ";
    }
    cout << "\n";

    // deklarasi index dan newPriority
    int indeks, newPriority;
    // user input index elemen yang ingin diubah
    cout << "Enter the Index of the priority you want to change: ";
    cin >> indeks;
    // user input new priority untuk menggantikan elemen pada index yang telah diinput
    cout << "Enter the new priority: ";
    cin >> newPriority;
    // memanggil fungsi changePriority untuk ubah elemen
    changePriority(indeks, newPriority);

    // tampilkan Priority queue setelah elemen diperbarui
    cout << "Priority queue after priority change: ";
    for (int i = 0; i <= heapSize; ++i) {
        cout << H[i] << " ";
    }
    cout << "\n";

    // deklarasi removeElement
    int removeElement;
    // user input indeks dari elemen yang ingin dihapus
    cout << "Enter the Indeks of the element you want to remove: ";
    cin >> removeElement;
    // panggil fungsi remove untuk hapus elemen
    remove(removeElement);

    // tampilkan Priority Queue setelah elemen dihapus
    cout << "Priority queue after removing the element: ";
    for (int i = 0; i <= heapSize; ++i) {
        cout << H[i] << " ";
    }
    cout << "\n";

    return 0;
}
```
#### Output:
![Screenshot]
### Penjelasan
Program diatas mengimplementasikan struktur data Priority Queue (Antrian Prioritas) menggunakan konsep Heap Biner (Binary Heap).

1. Program menyediakan beberapa fungsi dasar untuk operasi pada Heap Biner, seperti `insert`, `extractMax`, `changePriority`, `getMax`, dan `remove`.

2. Fungsi `insert` digunakan untuk menambahkan elemen baru ke dalam Priority Queue dengan cara memasukkannya ke dalam Heap Biner dan mempertahankan sifat heap.

3. Fungsi `extractMax` digunakan untuk mengambil dan menghapus elemen dengan prioritas tertinggi dari Priority Queue.

4. Fungsi `changePriority` digunakan untuk mengubah nilai prioritas suatu elemen dalam Priority Queue.

5. Fungsi `getMax` digunakan untuk mendapatkan nilai elemen dengan prioritas tertinggi tanpa menghapusnya dari Priority Queue.

6. Fungsi `remove` digunakan untuk menghapus elemen pada indeks tertentu dari Priority Queue.

7. Program juga menyediakan fungsi `addHeap` yang memungkinkan pengguna untuk memasukkan sejumlah elemen ke dalam Priority Queue.

8. Di dalam fungsi `main`, program meminta pengguna untuk memasukkan jumlah elemen yang ingin ditambahkan ke dalam Priority Queue, kemudian menampilkan isi Priority Queue tersebut.

9. Program juga memungkinkan pengguna untuk mengekstrak elemen dengan prioritas tertinggi, mengubah prioritas suatu elemen, dan menghapus elemen pada indeks tertentu dari Priority Queue.

10. Setelah setiap operasi, program menampilkan isi Priority Queue yang telah diperbarui.

## Kesimpulan
#### Hasil praktikum: 
Pada kesempatan praktikum ini, saya belajar priority queue dan heap. Priority queue adalah struktur data abstrak yang mirip dengan antrian (queue) biasa tetapi memiliki fitur tambahan di mana setiap elemen memiliki prioritas yang terkait dengannya. Sedangkan Heap adalah struktur data tree berbasis binary yang memenuhi properti heap. Terdapat dua jenis heap yang umum digunakan, yaitu max heap dan min heap.

#### Pelajaran yang didapat
1. Priority Queue (Antrian Prioritas) adalah struktur data abstrak yang mirip dengan antrian biasa, tetapi dengan fitur tambahan berupa prioritas yang terkait dengan setiap elemennya. Elemen dengan prioritas lebih tinggi akan diproses terlebih dahulu dibandingkan elemen dengan prioritas lebih rendah.

2. Heap adalah struktur data tree berbasis binary yang memenuhi properti heap. Terdapat dua jenis heap yang umum digunakan, yaitu max heap dan min heap.

3. Dalam max heap, nilai dari setiap node adalah lebih besar atau sama dengan nilai dari kedua anak-anaknya, sehingga akar dari pohon selalu memiliki nilai maksimum.

4. Dalam min heap, nilai dari setiap node adalah lebih kecil atau sama dengan nilai dari kedua anak-anaknya, sehingga akar dari pohon selalu memiliki nilai minimum.

5. Priority Queue dan Heap memiliki keterkaitan, di mana implementasi Priority Queue dapat dilakukan secara efisien menggunakan struktur data Heap, baik max heap maupun min heap.

6. Penggunaan Priority Queue dan Heap sangat berguna dalam berbagai aplikasi, seperti routing jaringan, penjadwalan tugas, dan sistem operasi, di mana diperlukan penanganan elemen-elemen dengan prioritas yang berbeda-beda.

Dengan demikian, pemahaman tentang Priority Queue dan Heap, serta hubungan di antara keduanya, sangat penting dalam bidang ilmu komputer dan pemrograman untuk memecahkan berbagai masalah terkait penanganan data dengan prioritas.


## Referensi
[1] M. Herlihy, “Priority queues”, 2019. 

[2] Robert Lafore, "Data Structures and Algorithms in Java", halaman 148-149", 2017.