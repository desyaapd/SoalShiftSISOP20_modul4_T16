# SoalShiftSISOP20_modul4_T16

1. I Gede Arimbawa Putra Teja Wardana - 05311840000045
2. Desya Ananda Puspita Dewi - 05311840000046

# Soal Shift Modul 4
  Di suatu perusahaan, terdapat pekerja baru yang super jenius, ia bernama jasir. Jasir baru bekerja selama seminggu di perusahan itu, dalam waktu seminggu tersebut ia selalu terhantui oleh ketidak amanan dan ketidak efisienan file system yang digunakan perusahaan tersebut. Sehingga ia merancang sebuah file system yang sangat aman dan efisien. Pada segi keamanan, Jasir telah menemukan 2 buah metode enkripsi file. 

  Pada mode enkripsi pertama, nama file-file pada direktori terenkripsi akan dienkripsi menggunakan metode substitusi. 

  Sedangkan pada metode enkripsi yang kedua, file-file pada direktori terenkripsi akan di-split menjadi file-file kecil. Sehingga orang-orang yang tidak menggunakan filesystem rancangannya akan kebingungan saat mengakses direktori terenkripsi tersebut. 

  Untuk segi efisiensi, dikarenakan pada perusahaan tersebut sering dilaksanakan sinkronisasi antara 2 direktori, maka jasir telah merumuskan sebuah metode agar filesystem-nya mampu mengsingkronkan kedua direktori tersebut secara otomatis. Agar integritas filesystem tersebut lebih terjamin, maka setiap command yang dilakukan akan dicatat kedalam sebuah file log.

*(catatan: filesystem berfungsi normal layaknya linux pada umumnya)*

*(catatan: mount source (root) filesystem adalah direktori /home/[user]/Documents)*

Berikut adalah detail filesystem rancangan jasir:

### 1.	Enkripsi versi 1:
----------------------------------
a.	Jika sebuah direktori dibuat dengan awalan `“encv1_”`, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v1.

b.	Jika sebuah direktori di-rename dengan awalan `“encv1_”`, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v1.

c.	Apabila sebuah direktori terenkripsi di-rename menjadi tidak terenkripsi, maka isi adirektori tersebut akan terdekrip.

d.	Setiap pembuatan direktori terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log berupa file.

e.	Semua file yang berada dalam direktori ter enkripsi menggunakan caesar cipher dengan key.

	9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M.b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_ND|jO

Misal kan ada file bernama “kelincilucu.jpg” dalam directory FOTO_PENTING, dan key yang dipakai adalah 10

	“encv1_rahasia/FOTO_PENTING/kelincilucu.jpg” => “encv1_rahasia/ULlL@u]AlZA(/g7D.|_.Da_a.jpg

*Note : Dalam penamaan file ‘/’ diabaikan, dan ekstensi tidak perlu di encrypt.*

f.	Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lainnya yang ada didalamnya.

### 2.	Enkripsi versi 2:
----------------------------------
a.	Jika sebuah direktori dibuat dengan awalan “encv2_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v2.

b.	Jika sebuah direktori di-rename dengan awalan “encv2_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v2.

c.	Apabila sebuah direktori terenkripsi di-rename menjadi tidak terenkripsi, maka isi direktori tersebut akan terdekrip.

d.	Setiap pembuatan direktori terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log berupa file.

e.	Pada enkripsi v2, file-file pada direktori asli akan menjadi bagian-bagian kecil sebesar 1024 bytes dan menjadi normal ketika diakses melalui filesystem rancangan jasir. Sebagai contoh, file File_Contoh.txt berukuran 5 kB pada direktori asli akan menjadi 5 file kecil yakni: File_Contoh.txt.000, File_Contoh.txt.001, File_Contoh.txt.002, File_Contoh.txt.003, dan File_Contoh.txt.004.

f.	Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lain yang ada didalam direktori tersebut (rekursif).

### 3.	Sinkronisasi direktori otomatis:
----------------------------------
Tanpa mengurangi keumuman, misalkan suatu directory bernama dir akan tersinkronisasi dengan directory yang memiliki nama yang sama dengan awalan sync_ yaitu sync_dir. Persyaratan untuk sinkronisasi yaitu:

a.	Kedua directory memiliki parent directory yang sama.

b.	Kedua directory kosong atau memiliki isi yang sama. Dua directory dapat dikatakan memiliki isi yang sama jika memenuhi:
    
i.	Nama dari setiap berkas di dalamnya sama.

  ii.	Modified time dari setiap berkas di dalamnya tidak berselisih lebih dari 0.1 detik.

c.	Sinkronisasi dilakukan ke seluruh isi dari kedua directory tersebut, tidak hanya di satu child directory saja.

d.	Sinkronisasi mencakup pembuatan berkas/directory, penghapusan berkas/directory, dan pengubahan berkas/directory.

Jika persyaratan di atas terlanggar, maka kedua directory tersebut tidak akan tersinkronisasi lagi.
Implementasi dilarang menggunakan symbolic links dan thread.

### 4.	Log system:
----------------------------------
a.	Sebuah berkas nantinya akan terbentuk bernama `"fs.log"` di direktori *home* pengguna `(/home/[user]/fs.log)` yang berguna menyimpan daftar perintah system call yang telah dijalankan.

b.	Agar nantinya pencatatan lebih rapi dan terstruktur, log akan dibagi menjadi beberapa level yaitu `INFO` dan `WARNING`.

c.	Untuk log level `WARNING`, merupakan pencatatan log untuk syscall `RMDIR` dan `UNLINK`.

d.	Sisanya, akan dicatat dengan level `INFO`.

e.	Format untuk logging yaitu:

```
[LEVEL]::[yy][mm][dd]-[HH]:[MM]:[SS]::[CMD]::[DESC ...]
```

- LEVEL	: Level logging
- yy	: Tahun dua digit
- mm	: Bulan dua digit
- dd	: Hari dua digit
- HH	: Jam dua digit
- MM	: Menit dua digit
- SS	: Detik dua digit
- CMD	: System call yang terpanggil
- DESC	: Deskripsi tambahan (bisa lebih dari satu, dipisahkan dengan ::)

Contoh format logging nantinya seperti:
```
INFO::200419-18:29:28::MKDIR::/iz1
INFO::200419-18:29:33::CREAT::/iz1/yena.jpg
INFO::200419-18:29:33::RENAME::/iz1/yena.jpg::/iz1/yena.jpeg
```


## Penyelesaian:
**Source Code: [ssfs.c](https://github.com/desyaapd/SoalShiftSISOP20_modul4_T16/blob/master/ssfs.c)**

### __Soal 1 :__ 
Merupakan program untuk mengenkripsi sebuah file dengan nama yang diawali dengan `encv1_`. Sehingga sebuah file yang memiliki nama dengan awalan `envc1_` akan terenkripsi. Dan apabila file tersebut di ubah atau di `rename`, maka isi file tersebut akan terdekripsi.

```bash
int main(int argc, char *argv[])
{
	umask(0);
	return fuse_main(argc, argv, &_oper, NULL);
}
```
Untuk membuat sebuah fuse, maka akan ada pemanggilan fungsi fuse_main. Kemudian, di dalam parameter ketiga, kita akan memanggil fungsi &_oper
```bash
static const struct fuse_operations _oper = {
	.getattr	= _getattr, //mendapatkan atribut/path file
	.access		= _access,
	.readlink	= _readlink, //membaca target dari symbolik link
	.readdir	= _readdir, //membaca direktori
	.mkdir		= _mkdir, //membuat direktori
	.symlink	= _symlink,
	.unlink		= _unlink, //meremove file
	.rmdir		= _rmdir, //meremove direktori
	.rename		= _rename, //mengubah nama file/direktori
	.link	        = _link,
	.chmod		= _chmod, //mengubah bit perisinan file
	.chown		= _chown, //mengubah owner dari sekelompok file
	.truncate	= _truncate, //mengedit/mengecilkan size sebuah file
	.utimens	= _utimens,
	.open	        = _open, //membuka file/direktori
	.create 	= _create,
	.read		= _read, //membaca file/direktori
	.write		= _write, //menulis ke sebuah file
	.statfs		= _statfs,
	.release	= _release,
};
```
Setalah itu, kita mendefinisikan direktori mana yang akan di mount, dan kita juga mendeklarasikan variable kode yang merupakan key dari caesar cipher
```bash
static const char *dirpath = "/home/desyaapd/Documents";
static const char *key = "9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M.b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_ND|jO";
```

Untuk fungsi decrypt, terdapat `isEncrypt` pada argumen yang akan merubah fungsi ini menjadi enkripsi dari path yang dituju jika `isEncyrpt` memiliki nilai atau (path, 1)
```bash
void decrypt(char *path, int isEncrypt) {
  char *cursor = path;
  while (cursor-path < strlen(path)) {
    char *ptr = strchr(key, *cursor);
    if (ptr != NULL) {
      int index = (ptr-key+strlen(key)-10)%strlen(key);
      if (isEncrypt) index = (ptr-key+strlen(key)+10)%strlen(key);
      *cursor = *(key+index);
    }
    cursor++;
  }
}
```
Fungsi ini akan mengenkrip/dekrip sesuai dengan kondisi yang sedang berjalan, `int index = (ptr-key+strlen(key)-10)%strlen(key)` index disini akan ditambah dengan panjang key yang akan di proses, kemudian 10 merupakan banyaknya karakter yang di shift dengan panjang key cursor akan di increment

Untuk mendefinisikan fungsi dekrip, kita harus mengetahui terlebih dahulu fila dan path dari file yang akan di enktipsi/atau dekripsi
```bash
void getDirAndFile(char *dir, char *file, char *path) {
  char buff[1000];
  memset(dir, 0, 1000);
  memset(file, 0, 1000);
  strcpy(buff, path);
  char *token = strtok(buff, "/");
  while(token != NULL) {
    sprintf(file, "%s", token);
    token = strtok(NULL, "/");
    if (token != NULL) {
      sprintf(dir, "%s/%s", dir, file);
    }
```
Fungsi ini akan membagi path menjadi token _directory_ dan _file_ berdasarkan fungsi `token = strtok(NULL, "/")`. while disini berfungsi untuk mengambil setiap token sampai ke buffer file terakhir yang telah di definisikan dengan menggunakan `memset(file, 0, 1000)`

Untuk mendukung kedua fungsi sebelumnya, kita membuat fungsi system call yang mengatur 3 keperluan tertentu sesuai permintaan soal yaitu:
- `mkdir` : pembuatan directory yang telah ditentukan
- `create` : proses rename pada directory sesuai dengan nama yang telah ditentukan
- `write` : perubahan atau penambahan dalam penyimpanan database/log file
```bash
void changePath(char *fpath, const char *path, int isWriteOper, int isFileAsked) {
  char *ptr = strstr(path, "/encv1_");
  int state = 0;
  if (ptr != NULL) {
    if (strstr(ptr+1, "/") != NULL) state = 1;
  }
```
Fungsi `void changePath` akan melakukan pencarian nama file yang memiliki`"encv1_"`. Kemudian file tersebut di pointer, 

- jika tidak memiliki `"encv1_"` maka `if (ptr != NULL)` dan `int state = 0` 
- jika iya `if (strstr(ptr+1, "/") != NULL)` dan `state = 1`
```bash
  char fixPath[1000];
  memset(fixPath, 0, sizeof(fixPath));
```
Fungsi untuk mendefinisikan buffer yang merupakan tempat penyimpanan processed path dari 3 (tiga) keperluan yang berbeda `(1.0, 0.1 dan 0.0)`.
```bash
  if (ptr != NULL && state) {
    ptr = strstr(ptr+1, "/");
    char pathEncvDirBuff[1000];
    char pathEncryptedBuff[1000];
    strcpy(pathEncryptedBuff, ptr);
    strncpy(pathEncvDirBuff, path, ptr-path); //copy path ke EncvDirBuff
```
Di dalam fungsi `fixPath` terdapat satu kondisi yang akan bekerja untuk kondisi `1.0` dan `0.1` yang mendefinisikan dan mengisi buffer.
```bash
    if (isWriteOper) {
      char pathFileBuff[1000];
      char pathDirBuff[1000];
      getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
      decrypt(pathDirBuff, 0);
      sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
```
Pada kondisi `1.0`, `if (isWriteOper)` yang memiliki nilai, `char pathFileBuff[1000]` akan mendefinisikan file path dan `char pathDirBuff[1000]` akan mendefinisikan directory path kemudian menjalankan fungsi `getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff)` yang digunakan untuk mengembalikan path dari file dan directory sesuai dengan path yang telah tersimpan di dalam `pathEncryptedBuff` 

Dilanjutkan dengan proses `decrypt(pathDirBuff, 0)` pendeskripsian dan menyimpannya di dalam buffer `fixPathsprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff)` dengan format "encv1 _, directory path, file path".
```bash
    } else if (isFileAsked) {
      char pathFileBuff[1000];
      char pathDirBuff[1000];
      char pathExtBuff[1000];
      getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
      char *whereIsExtension = strrchr(pathFileBuff, '.');
      if (whereIsExtension-pathFileBuff <= 1) {
        decrypt(pathDirBuff, 0);
        decrypt(pathFileBuff, 0);
        sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
      } else {
        char pathJustFileBuff[1000];
        memset(pathJustFileBuff, 0, sizeof(pathJustFileBuff));
        strcpy(pathExtBuff, whereIsExtension);
        snprintf(pathJustFileBuff, whereIsExtension-pathFileBuff+1, "%s", pathFileBuff);
        decrypt(pathDirBuff, 0);
        decrypt(pathJustFileBuff, 0);
        sprintf(fixPath, "%s%s/%s%s", pathEncvDirBuff, pathDirBuff, pathJustFileBuff, pathExtBuff);
      }
```
Untuk kondisi lainnya yakni:
- hanya kondisi `0.1` `else if (isFileAsked)` yang memiliki nilai, maka `char pathFileBuff[1000]` akan mendefinisikan file path, `char pathDirBuff[1000]` akan mendefinisikan directory path serta `char pathExtBuff[1000]` akan mendefinisikan ekstensi dari file tersebut. 

Kemudian, file path dan directory path akan disimpan terlebih dahulu ke dalam encrypted buffer menggunakan fungsi getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);. Setelah melakukan penyimpanan path ke dalam buffer, kita akan melakukan char *whereIsExtension = strrchr(pathFileBuff, '.'); pencarian ekstensi dari file melalui path yang telah ditentukan dengan melakukan pengecekan karakter ekstensi. 

Apabila `if (whereIsExtension-pathFileBuff <= 1)` tidak ditemukan ekstensi sesuai dengan path yang ditentukan atau ditemukan ekstensi sesuai dengan path yang ditentukan sejumlah 1 karakter 
- maka akan dilakukan proses pendeskripsian` decrypt(pathDirBuff, 0)` pada directory path dan `decrypt(pathFileBuff, 0)` pada file path. 
- hasil deskripsi disimpan ke dalam fixPath `sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff)` dengan format "encv1 _ , directory path, file path"

Apabila `else` ditemukan ekstensi sesuai dengan path yang ditentukan sejumlah lebih dari 1 karakter
- maka akan dilakukan penyimpanan ekstensi dari file tersebut ke dalam file path dan dilanjutkan dengan proses pendeskripsian `decrypt(pathDirBuff, 0)` pada directory path dan `decrypt(pathFileBuff, 0)` pada file path
- hasil deskripsi tersebut disimpan ke dalam fixPath `sprintf(fixPath, "%s%s/%s%s", pathEncvDirBuff, pathDirBuff, pathJustFileBuff, pathExtBuff)` dengan format "__encv1 _ , directory path, file path, ekstensi".
```bash
    } else {
      decrypt(pathEncryptedBuff, 0);
      sprintf(fixPath, "%s%s", pathEncvDirBuff, pathEncryptedBuff);
    }
  } else {
    strcpy(fixPath, path);
  }
  if (strcmp(path, "/") == 0) {
    sprintf(fpath, "%s", dirpath);
  } else {
    sprintf(fpath, "%s%s", dirpath, fixPath);
  }
}
```
Fungsi tersebut akan bekerja pada kondisi `0.0`,`1.0`, dan pada kondisi `0.1` tidak memiliki nilai, maka `decrypt(pathEncryptedBuff, 0)` akan terjadi proses pendeskripsian pada isi buffer tersebut dan `sprintf(fixPath, "%s%s", pathEncvDirBuff, pathEncryptedBuff)` disimpan di dalam `fixPath` dengan format "encv1 _ , decrypt path".


------------------------

### __Soal 4 :__
Kita membuat sebuah log dimana ia akan berisi semua system call yang telah dijalankan dan akan tersimpan di dalam `fs.log`. terdapat ketentuan dimana setiap system call `rmdir` dan `unlink` akan emanmpilkan `WARNING`

```bash
static const char *logpath = "/home/desyaapd/fs.log";
```
Line tersebut merupakan log path untuk menyimpan nama path file yang nantinya digunakan untuk membuat file fs.log
```bash
void logFile(char *level, char *cmd, int res, int lenDesc, const char *desc[]) {
  FILE *f = fopen(logpath, "a");
  time_t t;
  struct tm *tmp;
  char timeBuff[100];

  time(&t);
  tmp = localtime(&t);
  strftime(timeBuff, sizeof(timeBuff), "%y%m%d-%H:%M:%S", tmp);

  fprintf(f, "%s::%s::%s::%d", level, timeBuff, cmd, res);
  for (int i = 0; i < lenDesc; i++) {
    fprintf(f, "::%s", desc[i]);
  }
  fprintf(f, "\n");

  fclose(f);
}
```
Didalam fungsi `logFile`, akan mengatur format logging yang sesuai dengan ketentuan log file pada soal. Dimana fungsi dari 
- `level` untuk menunjukkan level dari system call
- `cmd` menunjukkan system call yang terpanggil
- `res` menyimpan status dari file
- `lenDesc` menunjukkan panjang file path
- `desc[]` menunjukkan absolute file path

Kemudian ` FILE *f = fopen(logpath, "a")` merupakan proses pembuatan log file sesuai direktori path. Dalam format logging, akan terdapat waktu pemanggilan system call dengan `strftime(timeBuff, sizeof(timeBuff), "%y%m%d-%H:%M:%S", tmp)`

Setelah itu format penulisan atau tampilan dari logging ini ` fprintf(f, "%s::%s::%s::%d", level, timeBuff, cmd, res)`


## Output

**Nomor 1**

- file setelah terenkripsi

![capture](https://github.com/desyaapd/SoalShiftSISOP20_modul4_T16/blob/master/image/enkrip.PNG) 

- file setelah di dekrip dengan cara _rename_

![capture](https://github.com/desyaapd/SoalShiftSISOP20_modul4_T16/blob/master/image/dekrip.PNG) 

**Nomor 4**

log system akan terupdate setiap system call di jalankan dengna membuka `fs.log`

![capture](https://github.com/desyaapd/SoalShiftSISOP20_modul4_T16/blob/master/image/log1.PNG)
![capture](https://github.com/desyaapd/SoalShiftSISOP20_modul4_T16/blob/master/image/log2.PNG)
![capture](https://github.com/desyaapd/SoalShiftSISOP20_modul4_T16/blob/master/image/log3.PNG) 


## Kendala

- Soalnya terlalu ambigu mas, jadi kita bingung yang benar bagaimana
- Karena soalnya panjang, dan kata-katanya susah untuk dipahami, sehingga tidak semua soal dikerjakan

