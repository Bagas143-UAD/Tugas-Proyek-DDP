#include <iostream>
#include <fstream>
#include <iomanip>

using namespace std;

const int MAX_ITEMS = 100;

// Record untuk menyimpan data barang
struct Item {
    int id;
    string nama;
    double harga;
    int stock;
};

// Array untuk menyimpan barang
Item items[MAX_ITEMS];
int itemCount = 0;

// Fungsi untuk menambahkan barang ke dalam sistem
void input() {
    if (itemCount >= MAX_ITEMS) {
        cout << "\nGagal menambahkan barang, kapasitas penuh!\n";
        return;
    }

    cout << "\n--- Tambah Barang ---\n";
    items[itemCount].id = itemCount + 1;
    cout << "Nama barang: ";
    cin.ignore();
    getline(cin, items[itemCount].nama);
    cout << "Harga barang: ";
    cin >> items[itemCount].harga;
    cout << "Stok barang: ";
    cin >> items[itemCount].stock;

    itemCount++;
    cout << "Barang berhasil ditambahkan!\n";
}

// Fungsi untuk menampilkan daftar barang
void tampilan() {
    if (itemCount == 0) {
        cout << "\nTidak ada barang yang tersedia.\n";
        return;
    }

    cout << "\n--- Daftar Barang ---\n";
    cout << left << setw(5) << "ID" << setw(20) << "Nama" << setw(10) << "Harga" << setw(10) << "Stok" << endl;
    for (int i = 0; i < itemCount; i++) {
        cout << left << setw(5) << items[i].id << setw(20) << items[i].nama << setw(10) << items[i].harga << setw(10) << items[i].stock << endl;
    }
}

// Fungsi untuk menyimpan data barang ke file
void simpan() {
    ofstream file("itemsa.txt");
    if (!file) {
        cout << "\nGagal membuka file untuk menyimpan data.\n";
        return;
    }

    for (int i = 0; i < itemCount; i++) {
        file << items[i].id << "," << items[i].nama << "," << items[i].harga << "," << items[i].stock << endl;
    }

    file.close();
    cout << "\nData barang berhasil disimpan ke file!\n";
}

// Fungsi untuk memuat data barang dari file
void tutup() {
    ifstream file("itemsa.txt");
    if (!file) {
        cout << "\nTidak ada file data yang ditemukan.\n";
        return;
    }

    itemCount = 0;
    while (!file.eof() && itemCount < MAX_ITEMS) {
        file >> items[itemCount].id;
        file.ignore();
        getline(file, items[itemCount].nama, ',');
        file >> items[itemCount].harga;
        file.ignore();
        file >> items[itemCount].stock;
        file.ignore();
        itemCount++;
    }

    file.close();
    cout << "\nData barang berhasil dimuat dari file!\n";
}

// Fungsi untuk melakukan transaksi
void jual() {
    if (itemCount == 0) {
        cout << "\nTidak ada barang yang tersedia untuk dijual.\n";
        return;
    }

    int id, quantity;
    tampilan();
    cout << "\nMasukkan ID barang yang ingin dibeli: ";
    cin >> id;
    cout << "Masukkan jumlah yang ingin dibeli: ";
    cin >> quantity;

    if (id < 1 || id > itemCount) {
        cout << "\nID barang tidak valid.\n";
        return;
    }

    if (items[id - 1].stock < quantity) {
        cout << "\nStok barang tidak mencukupi.\n";
        return;
    }

    items[id - 1].stock -= quantity;
    double total = quantity * items[id - 1].harga;
    cout << "\nTransaksi berhasil! Total harga: Rp" << total << endl;
}

int main() {
    tutup();

    int choice;
    do {
        cout << "\n--- Sistem Manajemen Toko Elektronik ---\n";
        cout << "1. Tambah Barang\n";
        cout << "2. Lihat Barang\n";
        cout << "3. Jual Barang\n";
        cout << "4. Simpan dan Keluar\n";
        cout << "Pilihan Anda: ";
        cin >> choice;

        switch (choice) {
            case 1:
            	system("cls");
                input();
                break;
            case 2:
            	system("cls");
                tampilan();
                break;
            case 3:
            	system("cls");
                jual();
                break;
            case 4:
            	system("cls");
                tutup();
                cout << "\nTerima kasih telah menggunakan sistem ini!\n";
                break;
            default:
                cout << "\nPilihan tidak valid.\n";
        }
    } while (choice != 4);

    return 0;
}
