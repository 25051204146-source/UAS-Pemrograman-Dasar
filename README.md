#include <iostream>
#include <fstream>
#include <string>

using namespace std;

struct mahasiswa{
    string nama;
    int nim;
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

void input_mahasiswa(mahasiswa*mhs_daftar, int&jumlah){
    if(jumlah == 100){
        cout<<"Sistem Sudah Penuh!"<<endl;
        return;
    }
    mahasiswa*mhs=&mhs_daftar[jumlah];
    cout<<"Masukkan nama: ";
    getline(cin,mhs->nama);
    cout<<"Masukkan NIM: ";
    cin>>mhs->nim;
    cout<<"Masukkan nilai Tugas: ";
    cin>>mhs->nilai_tugas;
    cout<<"Masukkan nilai UTS: ";
    cin>>mhs->nilai_uts;
    cout<<"Masukkan nilai UAS: ";
    cin>>mhs->nilai_uas;
    hitung_rata_dan_grade(mhs);
    jumlah++;
    cout<<"Data Mahasiswa Berhasil Ditambahkan!"<<endl;
    cin.ignore();
}

void tampilkan_mahasiswa(mahasiswa*mhs_daftar, int jumlah){
    if (jumlah == 0){
        cout<<"Tidak Ada Data Mahasiswa di Dalam Sistem!"<<endl;
        return;
    }
}
