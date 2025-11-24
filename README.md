
# Dokumentasi Basis Data Sistem Informasi Rumah Sakit

Dokumen ini menjelaskan struktur basis data serta relasi antar tabel

Tujuan dokumentasi ini: - Menjelaskan fungsi setiap tabel - Menjelaskan
bagaimana antar tabel saling terhubung - Menyajikan contoh struktur
migration

## CLI Boiler plate yang digunakan
- DCU
- DCM NamaModel
- DCI
- DCM NamaModel
- DCP pesan

## Daftar Model

-   RumahSakit
-   Poliklinik
-   Pasien
-   Dokter
-   Obat
-   JadwalPraktek
-   Kunjungan
-   Resep

## Relasi Antar Tabel

Pasien 1 --- N Kunjungan Dokter 1 --- N Kunjungan Dokter 1 --- N
JadwalPraktek Poliklinik 1 --- N Dokter

Kunjungan 1 --- N Resep Obat 1 --- N Resep

## Alur Data Utama

1.  Pasien mendaftar atau datang ke poliklinik.
2.  Sistem mencatat kunjungan pasien, termasuk dokter dan jadwal
    praktek.
3.  Dokter melakukan pemeriksaan dan menentukan diagnosa.
4.  Jika diperlukan, dokter membuat resep obat.
5.  Obat direkam dalam tabel Resep dan dihubungkan ke Kunjungan.

## Contoh Migration

``` php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('kunjungans', function (Blueprint $table) {
            $table->id();
            $table->foreignId('pasien_id')->constrained();
            $table->foreignId('dokter_id')->constrained();
            $table->foreignId('jadwal_id')->constrained('jadwal_prakteks');
            $table->text('keluhan')->nullable();
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('kunjungans');
    }
};
```
