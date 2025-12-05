#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>  // Tambahkan header ini untuk setw

using namespace std;

struct mahasiswa {
    string nama;
    int nim;
    double nilai_tugas;
    double nilai_uts;
    double nilai_uas;
    double rata;
    char grade;
};

void hitung_rata_dan_grade(mahasiswa* mhs){
    mhs->rata = (mhs->nilai_tugas + mhs->nilai_uts + mhs->nilai_uas) / 3;
    if(mhs->rata >= 86){
        mhs->grade = 'A';
    }else if(mhs->rata >= 71){
        mhs->grade = 'B';
    }else if(mhs->rata >= 56){
        mhs->grade = 'C';
    }else if(mhs->rata >= 41){
        mhs->grade = 'D';
    }else{
        mhs->grade = 'E';
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
    cin >> mhs->nim;
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
    cout << "\n=== Daftar Mahasiswa ===\n";
    // Header tabel dengan kolom tambahan untuk nilai-nilai
    cout << left << setw(5) << "No." 
         << setw(20) << "Nama" 
         << setw(10) << "NIM" 
         << setw(10) << "Tugas" 
         << setw(10) << "UTS" 
         << setw(10) << "UAS" 
         << setw(15) << "Rata-rata" 
         << setw(5) << "Grade" << endl;
    cout << string(85, '-') << endl;  // Garis pemisah yang lebih panjang
    for (int i = 0; i < jumlah; i++) {
        cout << left << setw(5) << (i + 1) 
             << setw(20) << mhs_daftar[i].nama 
             << setw(10) << mhs_daftar[i].nim 
             << setw(10) << fixed << setprecision(2) << mhs_daftar[i].nilai_tugas 
             << setw(10) << fixed << setprecision(2) << mhs_daftar[i].nilai_uts 
             << setw(10) << fixed << setprecision(2) << mhs_daftar[i].nilai_uas 
             << setw(15) << fixed << setprecision(2) << mhs_daftar[i].rata 
             << setw(5) << mhs_daftar[i].grade << endl;
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
    if (jumlah == 0) {
        file << "Tidak ada data mahasiswa.\n";
        file.close();
        cout << "Data berhasil disimpan!\n";
        return;
    }
    // Header kolom
    file << left << setw(5) << "No." << " | " << setw(20) << "Nama" << endl;
    file << string(30, '-') << endl;  // Garis pemisah
    for (int i = 0; i < jumlah; i++) {
        file << left << setw(5) << (i + 1) << " | " << setw(20) << mhs_daftar[i].nama << endl;
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
    string line;
    // Lewati header dan garis pemisah
    getline(file, line);  // Header
    getline(file, line);  // Garis pemisah
    while (getline(file, line)) {
        if (line.empty()) continue;
        // Parse line: "No. | Nama"
        size_t pos = line.find(" | ");
        if (pos != string::npos) {
            string no_str = line.substr(0, pos);
            string nama = line.substr(pos + 3);
            // Hapus spasi di akhir nama jika ada
            nama.erase(nama.find_last_not_of(" \t") + 1);
            mahasiswa* mhs = &mhs_daftar[jumlah];
            mhs->nama = nama;
            // Karena file baru hanya nama, set nilai default atau biarkan kosong
            mhs->nim = 0;  // Default
            mhs->nilai_tugas = 0.0;
            mhs->nilai_uts = 0.0;
            mhs->nilai_uas = 0.0;
            hitung_rata_dan_grade(mhs);
            jumlah++;
        }
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
