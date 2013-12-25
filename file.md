# File System


## Memeriksa eksistensi suatu file/folder

    bool File::exists( string $filename );

Contoh:

    if (File::exists('/document/readme.txt')) {
        echo 'File ditemukan';
    } else {
        echo 'File tidak ditemukan';
    }


## Mendapatkan isi file

    string File::get( string $filename );


## Menulis ke file

    int File::put( string $filename, string $contents );

Contoh:

    File::('readme.txt', 'Ini isi readme.');


## Menghapus file

    bool File::delete( string/array $filename );

Contoh:

    // Menghapus satu file
    File::delete('readme.txt');

    // Menghapus banyak file

    // Opsi #1
    File::delete(array('file1', 'file2', 'file3'));

    // Opsi #2
    File::delete('file1', 'file2', 'file3');


## Mendapatkan nama extension dari suatu file

    string File::extension( string $filename );

Contoh:

    File::extension('gambar1.jpg'); // Output: jpg


## Mendapatkan tipe file/folder

    string File::type( string $filename );

Contoh:

    File::type('images/gambar1.jpg'); // Output: file
    File::type('images/');            // Output: dir


## Mendapatkan ukuran suatu file

    int File::size( string $filename );

Contoh:

    File::size('gambar1.jpg'); // Output: 4096

## Mendapatkan semua file/folder dari suatu folder (rekursif)

## Mendapatkan semua folder dari suatu folder

## Membuat folder baru

## Copy folder

## Menghapus suatu folder

## Mengosongkan suatu folder

    File::cleanDirectory();

## Mencari file dengan pola

    File::glob('**');
