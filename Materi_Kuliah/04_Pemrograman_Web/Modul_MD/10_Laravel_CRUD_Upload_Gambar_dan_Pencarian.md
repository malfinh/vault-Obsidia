# 10. Tutorial Praktis Laravel: CRUD, Unggah Gambar, dan Pencarian Data

Navigasi: Modul Sebelumnya: [[09_Framework_Laravel_dan_Arsitektur_MVC]] | [[Konsep_Pemrograman_Web]] | Modul Berikutnya: [[11_Laravel_Autentikasi_Middleware_dan_Cheatsheet]]

---

Modul ini berisi panduan praktis langkah-demi-langkah membangun aplikasi pengolahan data **Student Management System** lengkap dengan fitur **CRUD (Create, Read, Update, Delete)**, **Unggah Berkas Gambar**, serta **Pencarian Data**.

---

## 10.1. Langkah 1: Membuat Model dan Migration

Jalankan perintah Artisan berikut untuk membuat Model `Student` beserta berkas Migration tabelnya sekaligus:

```bash
php artisan make:model Student -m
```

Edit berkas migration yang dibuat di `database/migrations/xxxx_create_students_table.php`:

```php
public function up(): void
{
    Schema::create('students', function (Blueprint $table) {
        $table->id();
        $table->string('name')->nullable();
        $table->string('email')->nullable();
        $table->string('image')->nullable(); // Menyimpan nama file gambar
        $table->timestamps();
    });
}
```

Jalankan migrasi untuk membuat tabel `students` secara fisik di database:

```bash
php artisan migrate
```

Edit berkas Model `app/Models/Student.php` untuk mengizinkan *mass assignment*:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Student extends Model
{
    protected $fillable = ['name', 'email', 'image'];
}
```

---

## 10.2. Langkah 2: Pendaftaran Route di `routes/web.php`

Daftarkan rute kueri untuk seluruh fitur CRUD dan Pencarian:

```php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\HomeController;

// Display & Form Upload
Route::get('/', [HomeController::class, 'index'])->name('student.index');
Route::post('/upload', [HomeController::class, 'upload'])->name('student.upload');

// Delete & Search
Route::get('/delete/{id}', [HomeController::class, 'delete'])->name('student.delete');
Route::get('/search', [HomeController::class, 'search'])->name('student.search');

// Update Data
Route::get('/update_view/{id}', [HomeController::class, 'update_view'])->name('student.update_view');
Route::post('/update/{id}', [HomeController::class, 'update'])->name('student.update');
```

---

## 10.3. Langkah 3: Membuat Logika CRUD & Upload di Controller

Buka `app/Http/Controllers/HomeController.php` dan tambahkan method-method berikut:

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Student;

class HomeController extends Controller
{
    // 1. READ: Menampilkan Seluruh Data
    public function index()
    {
        $data = Student::all();
        return view('view', compact('data'));
    }

    // 2. CREATE & UPLOAD: Menyimpan Data & Unggah Gambar
    public function upload(Request $request)
    {
        $student = new Student;
        $student->name = $request->name;
        $student->email = $request->email;

        // Pemrosesan File Gambar
        $image = $request->file('image');
        if ($image) {
            // Buat nama gambar unik berbasis timestamp
            $imagename = time() . '.' . $image->getClientOriginalExtension();
            
            // Pindahkan file gambar ke folder public/student/
            $image->move(public_path('student'), $imagename);
            
            $student->image = $imagename;
        }

        $student->save();
        return redirect()->back()->with('success', 'Data siswa dan gambar berhasil diunggah!');
    }

    // 3. DELETE: Menghapus Data berdasarkan ID
    public function delete($id)
    {
        $data = Student::findOrFail($id);
        
        // Hapus file gambar fisik di folder jika ada
        if ($data->image && file_exists(public_path('student/' . $data->image))) {
            unlink(public_path('student/' . $data->image));
        }

        $data->delete();
        return redirect()->back()->with('success', 'Data berhasil dihapus!');
    }

    // 4. SEARCH: Fitur Pencarian Data
    public function search(Request $request)
    {
        $search = $request->search;

        // Pencarian nama ATAU email yang cocok dengan keyword
        $data = Student::where('name', 'like', '%' . $search . '%')
                       ->orWhere('email', 'like', '%' . $search . '%')
                       ->get();

        return view('view', compact('data'));
    }

    // 5. UPDATE: Menampilkan Form Edit & Simpan Pembaruan
    public function update_view($id)
    {
        $student = Student::findOrFail($id);
        return view('update_view', compact('student'));
    }

    public function update(Request $request, $id)
    {
        $student = Student::findOrFail($id);
        $student->name = $request->name;
        $student->email = $request->email;

        $image = $request->file('image');
        if ($image) {
            // Hapus gambar lama jika ada
            if ($student->image && file_exists(public_path('student/' . $student->image))) {
                unlink(public_path('student/' . $student->image));
            }

            $imagename = time() . '.' . $image->getClientOriginalExtension();
            $image->move(public_path('student'), $imagename);
            $student->image = $imagename;
        }

        $student->save();
        return redirect()->route('student.index')->with('success', 'Data berhasil diperbarui!');
    }
}
```

---

## 10.4. Langkah 4: Membuat Antarmuka Tampilan Blade (`view.blade.php`)

Buat berkas `resources/views/view.blade.php` untuk menampilkan form masukan, pencarian, dan tabel data:

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>Manajemen Data Siswa</title>
    <style>
        table, th, td { border: 1px solid #ccc; border-collapse: collapse; padding: 8px; }
        .form-box { margin-bottom: 20px; padding: 15px; background: #f9f9f9; border-radius: 5px; }
    </style>
</head>
<body>

    <h2>Aplikasi Data Siswa</h2>

    {{-- Form Pencarian Data --}}
    <div class="form-box">
        <form action="{{ route('student.search') }}" method="GET">
            <input type="text" name="search" placeholder="Cari nama atau email..." required>
            <input type="submit" value="Cari Data">
            <a href="{{ route('student.index') }}">Reset</a>
        </form>
    </div>

    {{-- Form Upload Data Siswa Baru --}}
    <div class="form-box">
        <h3>Tambah Siswa Baru</h3>
        <form action="{{ route('student.upload') }}" method="POST" enctype="multipart/form-data">
            @csrf
            <div>
                <label>Nama Siswa:</label><br>
                <input type="text" name="name" required>
            </div>
            <div>
                <label>Email Siswa:</label><br>
                <input type="email" name="email" required>
            </div>
            <div>
                <label>Foto Profil:</label><br>
                <input type="file" name="image">
            </div><br>
            <button type="submit">Unggah Data</button>
        </form>
    </div>

    {{-- Tabel Menampilkan Data Siswa --}}
    <h3>Daftar Siswa Terdaftar</h3>
    <table>
        <thead>
            <tr>
                <th>Nama</th>
                <th>Email</th>
                <th>Foto</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody>
        @forelse($data as $student)
            <tr>
                <td>{{ $student->name }}</td>
                <td>{{ $student->email }}</td>
                <td>
                    @if($student->image)
                        <img src="{{ asset('student/' . $student->image) }}" height="80">
                    @else
                        <i>Tidak ada foto</i>
                    @endif
                </td>
                <td>
                    <a href="{{ route('student.update_view', $student->id) }}">Edit</a> | 
                    <a href="{{ route('student.delete', $student->id) }}" 
                       onclick="return confirm('Yakin ingin menghapus data ini?')">Hapus</a>
                </td>
            </tr>
        @empty
            <tr>
                <td colspan="4">Data tidak ditemukan.</td>
            </tr>
        @endforelse
        </tbody>
    </table>

</body>
</html>
```

---

Navigasi: Modul Sebelumnya: [[09_Framework_Laravel_dan_Arsitektur_MVC]] | [[Konsep_Pemrograman_Web]] | Modul Berikutnya: [[11_Laravel_Autentikasi_Middleware_dan_Cheatsheet]]
