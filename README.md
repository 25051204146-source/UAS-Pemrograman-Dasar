#include <iostream>
#include <fstream>
#include <string>
using namespace std;

struct Kurir {
    string idKurir;
    string namaKurir;
    string areaTugas;
    int jumlahPaket;
};

void inputKurir(Kurir *k) {
    cin.ignore();
    cout << "ID : ";
    getline(cin, k->idKurir);
    cout << "Nama : ";
    getline(cin, k->namaKurir);
    cout << "Area : ";
    getline(cin, k->areaTugas);
    cout << "Jumlah Paket: ";
    cin >> k->jumlahPaket;
}
void tampilkanKurir(Kurir *k, int index) {
    cout << "Kurir #" << index + 1 << ": "
         << k->idKurir << " - "
         << k->namaKurir << " - "
         << k->areaTugas << " - "
         << k->jumlahPaket << " paket" << endl;
}
void loadDataOtomatis(Kurir kurir[], int &jumlah) {
    ifstream file("data_kurir.txt"); // if stream membaca data file

    if (!file) {
        cout << "Data loaded from 'data_kurir.txt' (0 records found)\n\n"<< endl;
        jumlah = 0;
        return;
    }

    jumlah = 0;
    while (!file.eof()) {
        Kurir k;
        getline(file, k.idKurir);
        if (k.idKurir == "") break;

        getline(file, k.namaKurir);
        getline(file, k.areaTugas);
        file >> k.jumlahPaket;
        file.ignore();

        kurir[jumlah] = k;
        jumlah++;
    }

    cout << "Data loaded from 'data_kurir.txt' (" << jumlah << " records found)\n\n";
    file.close();
}

void saveDataOtomatis(Kurir kurir[], int jumlah) { //ini bagian menambah data, ofstreamnya
    ofstream file("data_kurir.txt");

    for (int i = 0; i < jumlah; i++) {
        file << kurir[i].idKurir << "\n";
        file << kurir[i].namaKurir << "\n";
        file << kurir[i].areaTugas << "\n";
        file << kurir[i].jumlahPaket << "\n";
    }

    cout << "Data saved to 'data_kurir.txt' (" << jumlah << " records)\n";
    file.close();
}

int main() {
    Kurir data[100];
    int jumlah = 0;
    loadDataOtomatis(data, jumlah);

    int pilihan;

    do {
        cout << "SISTEM DATA KURIR - AUTO SAVE/LOAD\n";
        cout << "===================================\n";
        cout << "1. Tambah Kurir\n";
        cout << "2. Tampilkan Semua Kurir\n";
        cout << "3. Keluar\n";
        cout << "Pilihan: ";
        cin >> pilihan;

        cout << endl;

        if (pilihan == 1) {
            inputKurir(&data[jumlah]); //panggil,nambah
            jumlah++;
            cout << "Data kurir berhasil ditambahkan!" << endl;
            cout << endl;
        }
        else if (pilihan == 2) {
            if (jumlah == 0) {
                cout << "Belum ada data kurir."<< endl;
                cout << endl;
            } else {
                cout << "DAFTAR KURIR (" << jumlah << " data): " << endl;
                for (int i = 0; i < jumlah; i++) {
                    tampilkanKurir(&data[i], i);
                }
                cout << endl;
            }
        }
        else if (pilihan == 3) {
            saveDataOtomatis(data, jumlah);
            cout << "Terima kasih!" << endl;
        }

    } while (pilihan != 3);

    return 0;
}
