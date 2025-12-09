#include <iostream>
#include <fstream>
#include <string>

using namespace std;

struct mahasiswa {
    string nama;
    string nim;
    double nilai_tugas;
    double nilai_uts;
    double nilai_uas;
    double rata;
    char grade;
};

void hitung_rata_dan_grade(mahasiswa* mhs){
mhs->rata=(mhs->nilai_tugas+mhs->nilai_uts+mhs->nilai_uas)/3;
    if(mhs->rata>=86){
        mhs->grade='A';
    }else if(mhs->rata>=71){
        mhs->grade='B';
    }else if(mhs->rata>=56){
        mhs->grade='C';
    }else if(mhs->rata>=41){
        mhs->grade='D';
    }else{
        mhs->grade='E';
    }
}
void input_mahasiswa(mahasiswa* mhs_daftar, int& jumlah) {
    if (jumlah >= 100) {
        cout << "Sistem sudah penuh!" << endl;
        return;
    }
    mahasiswa* mhs = &mhs_daftar[jumlah];
    cin.ignore(); 
    cout << "Masukkan Nama: ";
    getline(cin, mhs->nama);
    cout << "Masukkan NIM: ";
    getline(cin,mhs->nim);
    cout << "Masukkan Nilai Tugas: ";
    cin >> mhs->nilai_tugas;
    cout << "Masukkan Nilai UTS: ";
    cin >> mhs->nilai_uts;
    cout << "Masukkan Nilai UAS: ";
    cin >> mhs->nilai_uas;
    hitung_rata_dan_grade(mhs);
    jumlah++;
    cout << "Data berhasil ditambahkan!\n";
}
void tampilkan_mahasiswa(mahasiswa* mhs_daftar, int jumlah) {
    if (jumlah == 0) {
        cout << "Tidak Ada Data Mahasiswa di Dalam Sistem!" << endl;
        return;
    }
    cout << "\n==================================\n";
    cout << "======== Daftar Mahasiswa ========\n";
    cout << "==================================\n";
    for (int i = 0; i < jumlah; i++) {
        cout << i + 1 << ". " << mhs_daftar[i].nama 
             << " | NIM: " << mhs_daftar[i].nim
             << " | Rata-rata Nilai: " << mhs_daftar[i].rata
             << " | Grade: " << mhs_daftar[i].grade << endl;
    }
}
void edit_mahasiswa(mahasiswa* mhs_daftar, int jumlah) {
    if (jumlah == 0) {
        cout << "Tidak ada data!" << endl;
        return;
    }
    int index;
    cout << "Masukkan nomor data yang ingin diedit (1-" << jumlah << "): ";
    cin >> index;
    if (index < 1 || index > jumlah) {
        cout << "Nomor tidak valid!" << endl;
        return;
    }
    mahasiswa* mhs = &mhs_daftar[index - 1];
    cin.ignore();
    cout << "Nama baru: ";
    getline(cin, mhs->nama);
    cout << "NIM baru: ";
    cin >> mhs->nim;
    cout << "Nilai Tugas baru: ";
    cin >> mhs->nilai_tugas;
    cout << "Nilai UTS baru: ";
    cin >> mhs->nilai_uts;
    cout << "Nilai UAS baru: ";
    cin >> mhs->nilai_uas;
    hitung_rata_dan_grade(mhs);
    cout << "Data berhasil diedit!\n";
}
void simpan_ke_file(mahasiswa* mhs_daftar, int jumlah) {
    ofstream file("data_mahasiswa.txt");
    for (int i = 0; i < jumlah; i++) {
        file << "--------------------------------------" << endl;
        file << "Nama       : " <<mhs_daftar[i].nama << endl;
        file << "NIM        : " <<mhs_daftar[i].nim << endl;
        file << "Nilai Tugas: " <<mhs_daftar[i].nilai_tugas << endl;
        file << "Nilai UTS  : " <<mhs_daftar[i].nilai_uts << endl;
        file << "Nilai UAS  : " <<mhs_daftar[i].nilai_uas << endl;
        file << "Grade      : " <<mhs_daftar[i].grade << endl;
    }
    file.close();
    cout << "Data berhasil disimpan!\n";
}
void muat_dari_file(mahasiswa* mhs_daftar, int& jumlah) {
    ifstream file("data_mahasiswa.txt");
    if (!file.is_open()) {
        cout << "File tidak ditemukan!\n";
        return;
    }
    jumlah = 0;
    while (!file.eof()) {
        mahasiswa* mhs = &mhs_daftar[jumlah];
        getline(file, mhs->nama);
        if (mhs->nama == "") break; 
        file >> mhs->nim;
        file >> mhs->nilai_tugas;
        file >> mhs->nilai_uts;
        file >> mhs->nilai_uas;
        file.ignore();
        hitung_rata_dan_grade(mhs);
        jumlah++;
    }
    file.close();
    cout << "Data berhasil dimuat!\n";
}
int main() {
    mahasiswa daftar[100];
    int jumlah = 0;
    int pilihan;
    do {
        cout << "\n=== MENU SIAMI ===\n";
        cout << "1. Tambah Mahasiswa\n";
        cout << "2. Tampilkan Data\n";
        cout << "3. Edit Data\n";
        cout << "4. Simpan ke File\n";
        cout << "5. Muat dari File\n";
        cout << "6. Keluar\n";
        cout << "Pilih: ";
        cin >> pilihan;
        switch (pilihan) {
            case 1: input_mahasiswa(daftar, jumlah); break;
            case 2: tampilkan_mahasiswa(daftar, jumlah); break;
            case 3: edit_mahasiswa(daftar, jumlah); break;
            case 4: simpan_ke_file(daftar, jumlah); break;
            case 5: muat_dari_file(daftar, jumlah); break;
            case 6: cout << "Keluar\n"; break;
            default: cout << "Pilihan tidak valid!\n";
        }

    } while (pilihan != 6);
    return 0;
}
