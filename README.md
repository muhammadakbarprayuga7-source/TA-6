#include <iostream>
#include <string>
using namespace std;

#define SIZE 10  

struct Pengemudi {
    string id;
    string nama;
    string lokasi;
    bool aktif; 
};

class HashTable {
private:
    Pengemudi table[SIZE];
    bool occupied[SIZE]; 

    int hashFunc(string id) {
        int hash = 0;
        for (char c : id)
            hash += c;
        return hash % SIZE;
    }

public:
    HashTable() {
        for (int i = 0; i < SIZE; i++)
            occupied[i] = false;
    }

    void insertData(string id, string nama, string lokasi, bool aktif) {
        int index = hashFunc(id);
        int start = index;

        while (occupied[index]) {
            index = (index + 1) % SIZE;
            if (index == start) {
                cout << "Tabel penuh! Tidak dapat menambahkan data.\n";
                return;
            }
        }

        table[index] = {id, nama, lokasi, aktif};
        occupied[index] = true;
        cout << "Data pengemudi " << nama << " berhasil ditambahkan.\n";
    }

    void searchData(string id) {
        int index = hashFunc(id);
        int start = index;

        while (occupied[index]) {
            if (table[index].id == id) {
                cout << "=== Data Pengemudi Ditemukan ===\n";
                cout << "ID: " << table[index].id << endl;
                cout << "Nama: " << table[index].nama << endl;
                cout << "Lokasi: " << table[index].lokasi << endl;
                cout << "Status: " << (table[index].aktif ? "Aktif" : "Tidak Aktif") << endl;
                return;
            }
            index = (index + 1) % SIZE;
            if (index == start)
                break;
        }
        cout << "Data pengemudi dengan ID " << id << " tidak ditemukan.\n";
    }

    void updateStatus(string id, bool aktif) {
        int index = hashFunc(id);
        int start = index;

        while (occupied[index]) {
            if (table[index].id == id) {
                table[index].aktif = aktif;
                cout << "Status pengemudi " << table[index].nama 
                     << " diperbarui menjadi " 
                     << (aktif ? "Aktif" : "Tidak Aktif") << ".\n";
                return;
            }
            index = (index + 1) % SIZE;
            if (index == start)
                break;
        }
        cout << "Pengemudi tidak ditemukan.\n";
    }

    void tampilkanSemua() {
        cout << "\n=== Daftar Pengemudi ===\n";
        for (int i = 0; i < SIZE; i++) {
            if (occupied[i]) {
                cout << i << ". " << table[i].nama << " ("
                     << table[i].id << ") - " 
                     << (table[i].aktif ? "Aktif" : "Tidak Aktif")
                     << " - Lokasi: " << table[i].lokasi << endl;
            }
        }
    }
};

int main() {
    HashTable ht;

    ht.insertData("DR001", "Budi", "Jakarta Selatan", true);
    ht.insertData("DR002", "Andi", "Depok", true);
    ht.insertData("DR003", "Sinta", "Bekasi", false);

    cout << endl;
    ht.searchData("DR002");

    cout << endl;
    ht.updateStatus("DR002", false);

    cout << endl;
    ht.tampilkanSemua();

    return 0;
}


