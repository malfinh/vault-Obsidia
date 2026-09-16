# 7. Integrasi REST API, Retrofit 2, dan Konsumsi API Publik

Navigasi: Modul Sebelumnya: [[06_Penyimpanan_Data_Shared_File_dan_SQLite_Room]] | [[Konsep_Pengembangan_Aplikasi_Bergerak]] | Modul Berikutnya: [[08_Sensor_dan_Fitur_Perangkat_Hardware]]

---

Aplikasi mobile modern bertindak sebagai klien yang terhubung ke server backend untuk mengambil dan mengirimkan data secara *real-time* melalui **RESTful API (Application Programming Interface)**.

---

## 7.1. Keterkaitan dengan Pemrograman Web (Web vs Mobile API Consumption)

Konsep konsumsi API pada aplikasi mobile pada dasarnya identik dengan pemrosesan API pada pengembangan web yang dipelajari pada [[04_Pemrograman_Web/Modul_MD/04_jQuery_dan_AJAX]]:

```mermaid
flowchart TD
    subgraph Web_Client ["1. Klien Web Front-End"]
        JS["Browser JavaScript (Fetch API / Axios / AJAX)"]
    end

    subgraph Mobile_Client ["2. Klien Mobile Native / Cross-Platform"]
        Android["Aplikasi Android (Retrofit 2 / OkHttp)"]
        FlutterApp["Aplikasi Flutter (http package / Dio)"]
    end

    subgraph Server_Backend ["3. Server REST API Tersentralisasi"]
        Backend["Server Web (Laravel REST API / Node.js Express)"]
        DB[(Database MySQL / PostgreSQL)]
    end

    JS -->|HTTP GET/POST (JSON)| Backend
    Android -->|HTTP GET/POST (JSON)| Backend
    FlutterApp -->|HTTP GET/POST (JSON)| Backend
    Backend <--> DB
```

| Parameter | Konsumsi API di Web | Konsumsi API di Mobile |
| :--- | :--- | :--- |
| **Klien Utama** | Peramban Web (Chrome, Firefox, Safari) | Perangkat Android (Kotlin) / iOS (Swift) / Flutter |
| **HTTP Client Library** | `fetch()`, `axios`, `$.ajax()` | **`Retrofit 2`**, `OkHttp` (Android), `http` (Flutter) |
| **Parsing Data** | Otomatis diparse sebagai Objek JS | Menggunakan Converter seperti **`Gson`** atau **`Moshi`** |
| **Keamanan Network** | Diatur oleh aturan CORS pada browser | Diatur oleh Network Security Config & SSL Pinning |

---

## 7.2. Pengenalan Pustaka Retrofit 2

**Retrofit** adalah pustaka HTTP Client tipe terstruktur (*type-safe*) buatan Square untuk Android dan Java/Kotlin. Retrofit mengonversi API REST menjadi antarmuka (*interface*) Kotlin menggunakan anotasi HTTP.

---

## 7.3. Langkah-demi-Langkah Implementasi Retrofit CRUD (JSONPlaceholder API)

Berikut adalah panduan praktis mengonsumsi REST API publik dari [JSONPlaceholder](https://jsonplaceholder.typicode.com/posts):

### Step 1: Tambahkan Dependensi di `build.gradle (Module: app)`

```groovy
dependencies {
    // Retrofit 2 & Gson Converter
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.squareup.okhttp3:logging-interceptor:4.11.0'
}
```

Pastikan izin internet terpasang di `AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### Step 2: Buat Model Data Class (DTO)

```kotlin
import com.google.gson.annotations.SerializedName

data class PostModel(
    @SerializedName("userId") val userId: Int,
    @SerializedName("id") val id: Int,
    @SerializedName("title") val title: String,
    @SerializedName("body") val body: String
)
```

### Step 3: Buat Antarmuka `ApiService` dengan Anotasi HTTP

```kotlin
import retrofit2.Call
import retrofit2.http.*

interface ApiService {

    // 1. READ: Ambil seluruh daftar Post
    @GET("posts")
    fun getAllPosts(): Call<List<PostModel>>

    // 2. READ Detail: Ambil Post berdasarkan ID
    @GET("posts/{id}")
    fun getPostById(@Path("id") id: Int): Call<PostModel>

    // 3. CREATE: Tambah Post Baru
    @POST("posts")
    fun createPost(@Body newPost: PostModel): Call<PostModel>

    // 4. UPDATE: Perbarui Post berdasarkan ID
    @PUT("posts/{id}")
    fun updatePost(@Path("id") id: Int, @Body post: PostModel): Call<PostModel>

    // 5. DELETE: Hapus Post berdasarkan ID
    @DELETE("posts/{id}")
    fun deletePost(@Path("id") id: Int): Call<Void>
}
```

### Step 4: Buat Singleton Client `ApiClient`

```kotlin
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

object ApiClient {
    private const val BASE_URL = "https://jsonplaceholder.typicode.com/"

    val instance: ApiService by lazy {
        val retrofit = Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()

        retrofit.create(ApiService::class.java)
    }
}
```

### Step 5: Eksekusi Request Asinkronus di Activity / ViewModel

```kotlin
import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Pemanggilan API GET
        ApiClient.instance.getAllPosts().enqueue(object : Callback<List<PostModel>> {
            override fun onResponse(call: Call<List<PostModel>>, response: Response<List<PostModel>>) {
                if (response.isSuccessful) {
                    val posts = response.body()
                    posts?.forEach { post ->
                        Log.d("API_SUCCESS", "Title: ${post.title}")
                    }
                }
            }

            override fun onFailure(call: Call<List<PostModel>>, t: Throwable) {
                Log.e("API_ERROR", "Gagal memuat data API: ${t.message}")
            }
        })
    }
}
```

---

Navigasi: Modul Sebelumnya: [[06_Penyimpanan_Data_Shared_File_dan_SQLite_Room]] | [[Konsep_Pengembangan_Aplikasi_Bergerak]] | Modul Berikutnya: [[08_Sensor_dan_Fitur_Perangkat_Hardware]]
