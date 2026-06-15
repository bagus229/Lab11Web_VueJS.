# Nama: Bagus aditya hermawan
# Nim: 312410382
# Kelas: I241C
# Matkul: Pemrograman Web 2

## Langkah-Langkah Praktikum 11: VueJS
VueJS merupakan sebuah framework JavaScript yang dirancang khusus untuk membangun antarmuka pengguna (UI) website yang interaktif dan dinamis. VueJS dapat digunakan untuk membangun aplikasi berbasis user interface, seperti halaman web, aplikasi mobile, dan aplikasi desktop.
Framework ini menawarkan keseimbangan dan kemudahan penggunaan dan performa yang kuat. 

Langkah-langkah sebagai berikut:
Menyiapkan library Vuejs dan Axios untuk call API REST. Menggunakan CDN untuk memuat library Vue.
```<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>```.
```<script src="https://unpkg.com/axios/dist/axios.min.js"></script>```

Struktur direktori:
##### ![Gambar 1](ss1/gambar1.png).

Menampilkan data dengan membuat file index.html pada direktori lab8_vuejs. kode sebagai berikut:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Frontend Vuejs</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <link rel="stylesheet" href="assets/css/style.css">
</head>
<body>
    <div id="app">
        <h1>Daftar Artikel</h1>

        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Judul</th>
                    <th>Status</th>
                    <th>Aksi</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(row, index) in artikel">
                    <td class="center-text">{{ row.id }}</td>
                    <td>{{ row.judul }}</td>
                    <td>{{ statusText(row.status) }}</td>
                    <td class="center-text">
                        <a href="#" @click="edit(row)">Edit</a>
                        <a href="#" @click="hapus(index, row.id)">Hapus</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <script src="assets/js/app.js"></script>
</body>
</html>
```
Penjelasan: View ini akan menampilkan data artikel yg ada di praktikum sebelumnya, yakni ci4.

Membuat file app.js.
kode:
```JavaScript
const { createApp } = Vue

// tentukan lokasi API REST End Point
const apiUrl = 'http://localhost/lab11_ci/ci4/public'

createApp({

    data() {

        return {

            artikel: [],

            formData: {
                id: null,
                judul: '',
                isi: '',
                status: 0
            },

            showForm: false,

            formTitle: 'Tambah Data',

            statusOptions: [
                { text: 'Draft', value: 0 },
                { text: 'Publish', value: 1 }
            ]
        }
    },

    mounted() {
        this.loadData()
    },

    methods: {

        // Load data artikel
        loadData() {

            axios.get(apiUrl + '/post')

            .then(response => {
                this.artikel = response.data.artikel
            })

            .catch(error => {
                console.log(error)
            })
        },

        // Tambah data
        tambah() {

            this.showForm = true

            this.formTitle = 'Tambah Data'

            this.formData = {
                id: null,
                judul: '',
                isi: '',
                status: 0
            }
        },

        // Edit data
        edit(data) {

            this.showForm = true

            this.formTitle = 'Ubah Data'

            this.formData = {
                id: data.id,
                judul: data.judul,
                isi: data.isi,
                status: data.status
            }
        },

        // Hapus data
        hapus(index, id) {

            if (confirm('Yakin menghapus data?')) {

                axios.delete(apiUrl + '/post/' + id)

                .then(response => {
                    this.artikel.splice(index, 1)
                })

                .catch(error => {
                    console.log(error)
                })
            }
        },

        // Simpan data
        saveData() {

            // UPDATE
            if (this.formData.id) {

                axios.put(
                    apiUrl + '/post/' + this.formData.id,
                    this.formData
                )

                .then(response => {
                    this.loadData()
                })

                .catch(error => {
                    console.log(error)
                })

            }

            // INSERT
            else {

                axios.post(
                    apiUrl + '/post',
                    this.formData
                )

                .then(response => {
                    this.loadData()
                })

                .catch(error => {
                    console.log(error)
                })
            }

            // Reset form
            this.formData = {
                id: null,
                judul: '',
                isi: '',
                status: 0
            }

            this.showForm = false
        },

        // Status artikel
        statusText(status) {

            if (!status) {
                return 'Draft'
            }

            return status == 1
                ? 'Publish'
                : 'Draft'
        }
    }

}).mount('#app')
```
Fungsinya: 
- loadData
- CREATE Data
- UPDATE Data
- DELETE Data

Menambahkan form tambah dan ubah data pada file index.html dengan kode sebagai berikut:
```html
<button id="btn-tambah" @click="tambah">Tambah Data</button>
            <div class="modal" v-if="showForm">
                <div class="modal-content">
                    <span class="close" @click="showForm = false">&times;</span>
                    <form id="form-data" @submit.prevent="saveData">
                        <h3 id="form-title">{{ formTitle }}</h3>
                        <div><input type="text" name="judul" id="judul" vmodel="formData.judul" placeholder="Judul" required></div>
                        <div><textarea name="isi" id="isi" rows="10" vmodel="formData.isi"></textarea></div>
                        <div>
                            <select name="status" id="status" vmodel="formData.status">
                                <option v-for="option in statusOptions" :value="option.value">
                                    
                                    {{ option.text }}
                                </option>
                            </select>
                        </div>
                        <input type="hidden" id="id" v-model="formData.id">
                        <button type="submit" id="btnSimpan">Simpan</button>
                        <button @click="showForm = false">Batal</button>
                    </form>
                </div>
             </div>
```
Fungsinya: agar pengguna dapat menambahkan data artikel baru dan dapat mengubah artikelnya ketika ada kesalahan saat input.

Membuat file style.css agar halaman web terlihat menarik. kode sebagai berikut:
```css
#app {
    margin: 0 auto;
    width: 900px;
}

table {
    min-width: 700px;
    width: 100%;
}

th {
    padding: 10px;
    background: #5778ff;
    color: #ffffff;
}

tr td {
    border-bottom: 1px solid #eff1ff;
}

tr:nth-child(odd) {
    background-color: #eff1ff;
}

td {
    padding: 10px;
}

.center-text {
    text-align: center;
}

td a {
    margin: 5px;
}

#form-data {
    width: 600px;
}

form input {
    width: 100%;
    margin-bottom: 5px;
    padding: 5px;
    box-sizing: border-box;
}

form select {
    margin-bottom: 5px;
    padding: 5px;
    box-sizing: border-box;
}

form textarea {
    width: 100%;
    margin-bottom: 5px;
    padding: 5px;
    box-sizing: border-box;
}

form div {
    margin-bottom: 5px;
    position: relative;
}

form button {
    padding: 10px 20px;
    margin-top: 10px;
    margin-bottom: 10px;
    margin-right: 10px;
    cursor: pointer;
}

#btn-tambah {
    margin-bottom: 15px;
    padding: 10px 20px;
    cursor: pointer;
    background-color: #3152d6;
    color: #ffffff;
    border: 1px solid #3152d6;
}

#btnSimpan {
    background-color: #3152d6;
    color: #ffffff;
    border: 1px solid #3152d6;
}

.modal {
    display: block;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
    background-color: #fefefe;
    margin: 15% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 600px;
}

.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
    cursor: pointer;
}
```

Aktifkan xampp terliebih dahulu. Lalu, jalankan dibrowser untuk melihat hasilnya.
Hasil:
##### ![Gambar 1](ss1/gambar2.png).
##### ![Gambar 1](ss1/gambar3.png).

Penggunaan VueJS mempermudah pembuatan antarmuka Frontend API untuk operasi CRUD secara dinamis. Melalui fitur reaktivitas data dan pengikatan formulir otomatis yang dapat langsung memperbarui tampilan tabel secara real-time tanpa perlu memuat ulang (reload) halaman web.

Pertanyaan dan Tugas
Selesaikan programnya sesuai Langkah-langkah yang ada. Anda boleh melakukan
improvisasi.



## Langkah-Langkah Praktikum 12: VueJS Komponrn dan Routing (Single page Application)
Vue Components merupakan bagian antarmuka pengguna (UI) yang bersifat modular dan dapat digunakan kembali. Dengan memanfaatkan komponen, tampilan aplikasi dapat dipecah menjadi beberapa bagian yang terpisah, seperti Header, Footer, Sidebar, maupun daftar data tertentu. Pendekatan ini membantu menjaga struktur kode tetap rapi, mudah dipelihara, dan lebih terorganisir.
Vue Router adalah library resmi untuk VueJS yang menangani pemindahan halaman di sisi
klien (Client-Side Routing). 

Langkah-Langkah Praktikum
Menambahkan pustaka Vue Router menggunakan CDN. lalu, menambahkan library Vue Router setelah VueJS dan Axios.

```<script src="https://unpkg.com/vue-router@4/dist/vue-router.global.js"></script>```.

Penjelasan: digunakan untuk menghubungkan library Vue Router ke dalam aplikasi Vue.js melalui CDN. dapat menghubungkan komponen ke rute tertentu dan dapat berpindah halaman tanpa harus direfresh.

Membuat folder components pada folder assets/js.
##### ![Gambar 1](ss1/gambar4.png).

#### 1. Membuat File Komponen Halaman Utama
Membuat file Home.js pada folder assets/js/components.
kode sebagai berikut:
```JS
const Home = {
    template: `
         <div class="home-container">
         <h2>Selamat Datang di Portal Admin Artikel</h2>
         <p>Gunakan menu navigasi di atas untuk mengelola data artikel secara real-time
memanfaatkan RESTful API CodeIgniter 4 dan VueJS.</p>
         </div>
     `
};
```
Penjelasan: kode ini berguna sebagai tampilan halaman utama atau Home yang berisi sambutan dan menu navigasi untuk mengatur data artikel secara real-time yang dihubungkan menggunakan Vue Router.

#### 2.  Memindahkan Kode Fitur Artikel Ke Komponen (assets/js/components/Artikel.js)
Memindahkan logika CRUD artikel dari file app.js ke dalam file baru yakni Artikel.js.
```JS
const Artikel = {
    template: `
    <div>

        <h2>Manajemen Data Artikel</h2>

        <button id="btn-tambah" @click="tambah">
            Tambah Data
        </button>

        <div class="modal" v-if="showForm">
            <div class="modal-content">

                <span class="close" @click="showForm = false">
                    &times;
                </span>

                <form id="form-data" @submit.prevent="saveData">

                    <h3>{{ formTitle }}</h3>

                    <div>
                        <input
                            type="text"
                            name="judul"
                            v-model="formData.judul"
                            placeholder="Judul"
                            required
                        >
                    </div>

                    <div>
                        <textarea
                            name="isi"
                            rows="10"
                            v-model="formData.isi"
                            placeholder="Isi Artikel"
                            required
                        ></textarea>
                    </div>

                    <div>
                        <select
                            name="status"
                            v-model="formData.status"
                        >
                            <option
                                v-for="option in statusOptions"
                                :key="option.value"
                                :value="option.value"
                            >
                                {{ option.text }}
                            </option>
                        </select>
                    </div>

                    <input
                        type="hidden"
                        v-model="formData.id"
                    >

                    <button
                        type="submit"
                        id="btnSimpan"
                    >
                        Simpan
                    </button>

                    <button
                        type="button"
                        @click="showForm = false"
                    >
                        Batal
                    </button>

                </form>

            </div>
        </div>

        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Judul</th>
                    <th>Status</th>
                    <th>Aksi</th>
                </tr>
            </thead>

            <tbody>
                <tr
                    v-for="(row, index) in artikel"
                    :key="row.id"
                >
                    <td class="center-text">
                        {{ row.id }}
                    </td>

                    <td>
                        {{ row.judul }}
                    </td>

                    <td>
                        {{ statusText(row.status) }}
                    </td>

                    <td class="center-text">
                        <a
                            href="#"
                            @click.prevent="edit(row)"
                        >
                            Edit
                        </a>

                        |

                        <a
                            href="#"
                            @click.prevent="hapus(index, row.id)"
                        >
                            Hapus
                        </a>
                    </td>
                </tr>
            </tbody>
        </table>

    </div>
    `,

    data() {
        return {
            artikel: [],

            formData: {
                id: null,
                judul: '',
                isi: '',
                status: 0
            },

            showForm: false,

            formTitle: 'Tambah Data',

            statusOptions: [
                {
                    text: 'Draft',
                    value: 0
                },
                {
                    text: 'Publish',
                    value: 1
                }
            ]
        };
    },

    mounted() {
        this.loadData();
    },

    methods: {

        // Load Data
        loadData() {

            axios.get(apiUrl + '/post')

                .then(response => {
                    this.artikel = response.data.artikel;
                })

                .catch(error => {
                    console.log(error);
                });
        },

        // Tambah Data
        tambah() {

            this.showForm = true;

            this.formTitle = 'Tambah Data';

            this.formData = {
                id: null,
                judul: '',
                isi: '',
                status: 0
            };
        },

        // Edit Data
        edit(data) {

            this.showForm = true;

            this.formTitle = 'Ubah Data';

            this.formData = {
                id: data.id,
                judul: data.judul,
                isi: data.isi,
                status: data.status
            };
        },

        // Hapus Data
        hapus(index, id) {

            if (confirm('Yakin menghapus data?')) {

                axios.delete(apiUrl + '/post/' + id)

                    .then(response => {
                        this.artikel.splice(index, 1);
                    })

                    .catch(error => {
                        console.log(error);
                    });
            }
        },

        // Simpan Data
        saveData() {

            // UPDATE
            if (this.formData.id) {

                axios.put(
                    apiUrl + '/post/' + this.formData.id,
                    this.formData
                )

                .then(response => {
                    this.loadData();
                })

                .catch(error => {
                    console.log(error);
                });

            }

            // INSERT
            else {

                axios.post(
                    apiUrl + '/post',
                    this.formData
                )

                .then(response => {
                    this.loadData();
                })

                .catch(error => {
                    console.log(error);
                });
            }

            // Reset Form
            this.formData = {
                id: null,
                judul: '',
                isi: '',
                status: 0
            };

            this.showForm = false;
        },

        // Status Artikel
        statusText(status) {

            if (!status) {
                return 'Draft';
            }

            return status == 1
                ? 'Publish'
                : 'Draft';
        }
    }
};
```
Penjelasan: kode ini berguna untuk perintah CRUD Logika CRUD. Logika ini sebelumnya berada di app.js. lalu, dipindahkan ke artikel.js agar kode lebih terstruktur dan mudah dikelola. File app.js nantinya berguna untuk konfigurasi aplikasi dan routing, sedangkan artikel.js menangani fungsi tambah, tampil, ubah, dan hapus data artikel melalui API.

#### 3. Mengonfigurasi Vue Router pada assets/js/app.js
Mengedit kode di file app.js untuk mendaftarkan rute internal, komponen, dan melakukan mounting aplikasi.
```JS
const { createApp } = Vue;
const { createRouter, createWebHashHistory } = VueRouter;

// endpoint API
const apiUrl = 'http://localhost/lab11_ci/ci4/public';

// routes
const routes = [
    {
        path: '/',
        component: Home
    },
    {
        path: '/artikel',
        component: Artikel
    },
    {
        path: '/about',
        component: About
    }
];

// router
const router = createRouter({
    history: createWebHashHistory(),
    routes
});

// app
const app = createApp({});

app.use(router);

app.mount('#app');
```
Penjelasan: Kode tersebut digunakan untuk mengatur navigasi halaman menggunakan Vue Router. Selain itu, kode ini juga mendefinisikan endpoint API, membuat rute untuk halaman Home, Artikel, dan About yang dikonfigurasi menggunakan Hash History agar navigasi antarhalaman dapat berjalan tanpa memuat ulang halaman. lalu menjalankan aplikasi pada elemen HTML dengan id app.

#### 4. Memodifikasi Master Layout pada index.html
Menyesuaikan kode agar menyediakan menu navigasi menggunakan <router-link> dan tempat penampung halaman dinamis menggunakan <router-view>.
```JS
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Frontend VueJS</title>

    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="https://unpkg.com/vue-router@4/dist/vue-router.global.js"></script>

    <link rel="stylesheet" href="assets/css/style.css">
</head>
<body>

    <div id="app">
        <header>
            <h1>Daftar Artikel</h1>

            <nav class="nav-menu">
                <router-link to="/">Beranda</router-link> |
                <router-link to="/artikel">Kelola Artikel</router-link>
                <router-link to="/about">About</router-link>
            </nav>
        </header>

        <main>
            <router-view></router-view>
        </main>
    </div>

    <!-- Components -->
    <script src="assets/js/components/Home.js"></script>
    <script src="assets/js/components/Artikel.js"></script>
    <script src="assets/js/components/About.js"></script>

    <!-- Main App -->
    <script src="assets/js/app.js"></script>

</body>
</html>
```
Penjelasan: Menu navigasi seperti halaman Beranda, Kelola Artikel, dan About menggunakan router-link, sedangkan router-view berfungsi menampilkan halaman komponen sesuai rute yang dipilih.

#### 5. Tambahan CSS Menarik pada assets/css/style.css
Agar tampilan menu terlihat rapi dan menarik.
```css
.nav-menu {
 padding: 10px;
 background: #eff1ff;
 border-radius: 5px;
 margin-bottom: 15px;
}
.nav-menu a {
 text-decoration: none;
 color: #3152d6;
 font-weight: bold;
 padding: 5px 10px;
}
/* Style otomatis saat route aktif */
.router-link-exact-active {
 background-color: #3152d6;
 color: #ffffff !important;
 border-radius: 3px;
}
.home-container {
 padding: 20px;
 border: 1px solid #eff1ff;
 background: #fafafa;
}
```
Hasil:
##### ![Gambar 1](ss1/gambar5.png).
##### ![Gambar 1](ss1/gambar6.png).

Pertanyaan	dan	Tugas
1. Selesaikan semua langkah praktikum di atas.
2. Tambahkan satu rute baru (/about) beserta komponen About.js baru yang berisi profil singkat Anda (Nama, NIM, Kelas, dan Foto/Avatar). Masukkan tautan rutenya ke dalam menu navigasi atas pada index.html.
```html
<nav class="nav-menu">
                <router-link to="/">Beranda</router-link> |
                <router-link to="/artikel">Kelola Artikel</router-link>
                <router-link to="/about">About</router-link>
            </nav>
```
Penjelasan: Menu navigasi ditambahkan pada html supaya halaman about dan halaman terhubung.
```JS
const routes = [
    {
        path: '/',
        component: Home
    },
    {
        path: '/artikel',
        component: Artikel
    },
    {
        path: '/about',
        component: About
    }
];
```
Penjelasan: Menambahkan route agar dapat berpindah ke halaman about.

```JS
const About = {
    template: `
    <div class="about-container">

        <div class="profile-card">

            <div class="profile-header">
                <img
                    src="assets/img/avatar.jpg"
                    alt="Foto Profil"
                    class="profile-img"
                >

                <div class="profile-title">
                    <h2>Bagus Aditya</h2>
                    <p>Perograman Web 2</p>
                </div>
            </div>

            <div class="profile-info">
                <table>
                    <tr>
                        <td>Nama</td>
                        <td>Bagus Aditya Hermawan</td>
                    </tr>
                    <tr>
                        <td>NIM</td>
                        <td>312410382</td>
                    </tr>
                    <tr>
                        <td>Kelas</td>
                        <td>I241C</td>
                    </tr>
                </table>
            </div>

        </div>

    </div>
    `
};
```
Penjelasan: halaman about ini berisikan profil: Nama, NIM, Kelas, dan Foto/Avatar.

```css
.about-container {
    margin-top: 20px;
}

.profile-card {
    background: #eef0fa;
    border-radius: 8px;
    padding: 25px;
}

.profile-header {
    display: flex;
    align-items: center;
    gap: 20px;
    margin-bottom: 25px;
}

.profile-img {
    width: 120px;
    height: 120px;
    border-radius: 8px;
    object-fit: cover;
    border: 2px solid #3b5bdb;
}

.profile-title h2 {
    margin: 0;
    color: #3b5bdb;
}

.profile-title p {
    margin-top: 5px;
    color: #666;
}

.profile-info table {
    width: 100%;
    border-collapse: collapse;
}

.profile-info td {
    padding: 12px;
    border-bottom: 1px solid #d6daf0;
}

.profile-info td:first-child {
    font-weight: bold;
    width: 120px;
    color: #3b5bdb;
}
```
Penjelasan: agar tampilan halaman about lebih menarik.


3. Lakukan pengujian perpindahan halaman menu (Beranda, Kelola Artikel, dan About) dan pastikan browser tidak melakukan hard-reload (SPA bekerja).
##### ![Gambar 1](ss1/gambar10.png).
##### ![Gambar 1](ss1/gambar11.png).
Tambah artikel:
##### ![Gambar 1](ss1/gambar7.png).
Edit artikel:
##### ![Gambar 1](ss1/gambar8.png).
Halaman about:
##### ![Gambar 1](ss1/gambar9.png).

## Langkah-Langkah Praktikum 13: VueJS	Autentikasi	dan	Navigation Guards	(SPA Security)
Navigation Guards merupakan fitur yang dimiliki oleh Vue router dengan memiliki fungsi  router.beforeEach() yang bertindak sebagai interceptor perpindahan rute yang akan memeriksa status login pengguna sebelum diizinkan rute tersebut ditampilkan di layar browser.

Langkah-langkah Praktikum
#### Tahap 1: PEMBUATAN API ENDPOINT LOGIN (SISI BACKEND CI4) 
Menyediakan endpoint REST API di CodeIgniter 4 sebelum ke konfogurasi. berfungsi untuk memvalidasi pengguna yang dikirim dari aplikasi frontend.

Langkah 1.1: Membuat Auth Controller
Membuat Controller pada app/Controllers/Api/Auth.php. kode seperti berikut:

```php
<?php
namespace App\Controllers\Api;
use CodeIgniter\RESTful\ResourceController;
use App\Models\UserModel;

class Auth extends ResourceController
{
    protected $format = 'json';

    public function login()
    {
        $username = $this->request->getVar('username');
        $password = $this->request->getVar('password');
        $model = new UserModel();

        $user = $model->where('username', $username)
            ->orWhere('useremail', $username)
            ->first();

        if ($user) {
            if ($password === $user['userpassword'] || password_verify($password, $user['userpassword'])) {
                return $this->respond([
                    'status' => 200,
                    'error' => null,
                    'messages' => 'Login Berhasil',
                    'data' => [
                        'id' => $user['id'],
                        'username' => $user['username'],
                        'token' => base64_encode("TOKEN-SECRET-" . $user['username'])
                    ]
                ], 200);
            }
        }

        return $this->failUnauthorized('Username atau Password yang Anda masukkan salah.');
    }
}
```
Penjelasan: API login memverifikasi username/email dan password pengguna. Jika data cocok, sistem mengembalikan informasi pengguna beserta token autentikasi. Jika tidak cocok, API akan mengirimkan pesan "Username atau Password yang Anda masukkan salah.".

Langkah 1.2: Mendaftarkan Route API Login
Menambahkan kode berikut pada file Routes.php.
```php
$routes->post('api/login', 'Api\Auth::login');
```
Penjelasan: request POST ke endpoint untuk api/login, sistem akan menjalankan method login() pada controller Auth yang berada di folder Api.

#### Tahap 2: PENGEMBANGAN INTEGRASI FRONTEND (SISI VUEJS SPA) 
Menambahkan Login.js pada folder components. struktur sebagai berikut:
##### ![Gambar 1](ss1/gambar12.png).

Langkah 2.1: Membuat Komponen Login (assets/js/components/Login.js)
Menambahkan kode login pada file Login.js sebagai berikut:
```php
const Login = {
    template: `
        <div class="login-container">
            <div class="login-box">
                <h2>Form Login Admin</h2>
                <form @submit.prevent="handleLogin">
                    <div class="form-group">
                        <label>Username / Email</label>
                        <input type="text" v-model="username" placeholder="Masukkan username" required>
                    </div>
                    <div class="form-group">
                        <label>Password</label>
                        <input type="password" v-model="password" placeholder="Masukkan password" required>
                    </div>
                    <button type="submit" class="btn-login">Masuk Aplikasi</button>
                </form>
                <p v-if="errorMessage" class="error-msg">{{ errorMessage }}</p>
            </div>
        </div>
    `,
    data() {
        return {
            username: '',
            password: '',
            errorMessage: ''
        }
    },
    methods: {
        handleLogin() {
            axios.post(apiUrl + '/api/login', {
                username: this.username,
                password: this.password
            })
            .then(response => {
                if (response.data.status === 200) {
                    localStorage.setItem('isLoggedIn', 'true');
                    localStorage.setItem('userToken', response.data.data.token);
                    const appInstance = this.$root;
                    appInstance.isLoggedIn = true;
                    this.$router.push('/artikel');
                }
            })
            .catch(error => {
                if (error.response && error.response.data.messages) {
                    this.errorMessage = error.response.data.messages;
                } else {
                    this.errorMessage = 'Terjadi kesalahan jaringan atau server.';
                }
            });
        }
    }
};
```
Penjelasan: Vue.js yang menampilkan form login untuk pengguan sesuai pada database. Pengguna memasukkan username/email dan password, kemudian data dikirim ke API menggunakan Axios. Jika login berhasil, token dan status login disimpan di localStorage, lalu pengguna diarahkan ke halaman Artikel. Jika login gagal, sistem akan menampilkan pesan kesalahan yang sesuai.

Langkah 2.2: Mengonfigurasi Proteksi Rute dan Guards pada assets/js/app.js
Mendaftarkan komponen Login, tambahkan properti meta: {
requiresAuth: true } pada rute artikel, serta buat fungsi kontrol beforeEach.
```php
const { createApp } = Vue;
const { createRouter, createWebHashHistory } = VueRouter;

// endpoint API
const apiUrl = 'http://localhost/lab11_ci/ci4/public';

// routes
const routes = [
    {
        path: '/',
        component: Home
    },
    {
        path: '/login',
        component: Login
    },
    {
        path: '/artikel',
        component: Artikel,
        meta: { requiresAuth: true }
    },
    {
        path: '/about',
        component: About
    }
];

// router
const router = createRouter({
    history: createWebHashHistory(),
    routes
});

// navigation guard
router.beforeEach((to, from, next) => {
    const isAuthenticated = localStorage.getItem('isLoggedIn') === 'true';
    if (to.matched.some(record => record.meta.requiresAuth) && !isAuthenticated) {
        alert('Akses Ditolak! Anda harus login terlebih dahulu.');
        next('/login');
    } else {
        next();
    }
});

// app
const app = createApp({
    data() {
        return {
            isLoggedIn: false
        }
    },
    mounted() {
        this.isLoggedIn = localStorage.getItem('isLoggedIn') === 'true';
    },
    methods: {
        logout() {
            if (confirm('Apakah Anda yakin ingin keluar aplikasi?')) {
                localStorage.removeItem('isLoggedIn');
                localStorage.removeItem('userToken');
                this.isLoggedIn = false;
                this.$router.push('/');
            }
        }
    }
});

app.use(router);
app.mount('#app');
```
Penjelasan: Kode ini menerapkan navigation guard untuk membatasi akses ke halaman tabel Artikel hanya untuk pengguna yang sudah login. mengelola status autentikasi pengguna melalui localStorage dan menmbah fitur untuk logout dan mengarahkan ke halamana utama.

Langkah 2.3: Menyesuaikan Tata Letak Dinamis pada index.html
Menambahkan direktif v-if / v-else pada menu navigasi bagian atas.
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Frontend VueJS</title>

    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="https://unpkg.com/vue-router@4/dist/vue-router.global.js"></script>

    <link rel="stylesheet" href="assets/css/style.css">
</head>
<body>

    <div id="app">
        <header>
            <h1>Daftar Artikel</h1>

            <nav class="nav-menu">
                <router-link to="/">Beranda</router-link> |
                <router-link to="/artikel">Kelola Artikel</router-link> |
                <router-link to="/about">About</router-link> |
                <router-link v-if="!isLoggedIn" to="/login">Login</router-link>
                <a v-else href="#" @click.prevent="logout">Logout</a>
            </nav>
        </header>

        <main>
            <router-view></router-view>
        </main>
    </div>

    <script src="assets/js/components/Home.js"></script>
    <script src="assets/js/components/Artikel.js"></script>
    <script src="assets/js/components/About.js"></script>
    <script src="assets/js/components/Login.js"></script>
    <script src="assets/js/app.js"></script>

</body>
</html>
```
Penjelasan: Menu navigasi untuk berpindah antarhalaman yang nantinya akan ditampilkan pada elemen <router-view> agar lebih dinamis.

Langkah 2.4: Menambahkan Desain Antarmuka Form pada assets/css/style.css
```css
.login-container {
     display: flex;
     justify-content: center;
     align-items: center;
     padding: 40px 0;
}
.login-box {
     width: 350px;
     padding: 25px;
     border: 1px solid #ccc;
     border-radius: 8px;
     background-color: #ffffff;
     box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}
.login-box h2 {
     margin-top: 0;
     margin-bottom: 20px;
     text-align: center;
     color: #333;
}
.form-group {
     margin-bottom: 15px;
}
form-group label {
     display: block;
     margin-bottom: 5px;
     font-weight: bold;
}
.form-group input {
     width: 100%;
     padding: 8px;
     border: 1px solid #ccc;
     border-radius: 4px;
     box-sizing: border-box;
}
.btn-login {
     width: 100%;
     padding: 10px;
     background-color: #3152d6;
     color: white;
     border: none;
     border-radius: 4px;
     font-weight: bold;
     cursor: pointer;
}
.btn-login:hover {
     background-color: #203ca3;
}
.error-msg {
     color: red;
     font-size: 14px;
     text-align: center;
     margin-top: 15px;
}
```
Penjelasan: Agar tampilan form login terlihat lebih rapih.

Pertanyaan	dan	Tugas
1. Selesaikan seluruh pengerjaan kode pemrograman sistem autentikasi API di atas.
2. Jalankan pengujian skenario kontrol keamanan berikut pada browser:
- Skenario A (Kondisi Terkunci): Bersihkan penyimpanan local storage
browser (atau dalam keadaan belum login). Tekan menu navigasi "Kelola
Artikel". Amati jalannya aplikasi, apakah sistem berhasil menolak akses secara
langsung, memunculkan alert, dan melempar halaman tampilan ke form login.
Hasil:
##### ![Gambar 1](ss1/gambar13.png).
##### ![Gambar 1](ss1/gambar14.png).
Penjelasan: Ketika pengguan mengakses halaman tabel artikel tanpa login dahulu, dengan otomatis sistem menolak akses dan memunculkan alert. Setelah alert muncul sistem akan melempar ke halaman form login.

- Skenario B (Kondisi Login Terautentikasi): Buka form login, masukkan data akun pengguna yang valid sesuai isi database user Anda. Amati apakah sistem berhasil memvalidasi kredensial ke database backend melalui Axios, membawa Anda masuk ke halaman tabel artikel, serta merubah tombol menu login atas menjadi link "Logout".
Hasil:
##### ![Gambar 1](ss1/gambar14.png).
##### ![Gambar 1](ss1/gambar15.png).
##### ![Gambar 1](ss1/gambar16.png).
##### ![Gambar 1](ss1/gambar17.png).
##### ![Gambar 1](ss1/gambar18.png).
Penjelasan: Membuka form login. lalu pengguna memasukaan username/email dan password. setelah saya amati sistem dapat berhasil memvalidasi kredensial melalui axios. kemudian, sistem mengarahkan ke halaman tabel artikel. pada menu navigasi link login telah terubah menjadi link logout. ketika logout sistem mengirim alert apakah akan keluar aplikasi?. setelah keluar, sistem mengarahkan ke halaman utama.

3. Terapkan pengaman rute serupa (meta: { requiresAuth: true }) untuk komponen halaman About.js (profil mahasiswa) yang telah dibuat pada Praktikum 12, sehingga menu tersebut ikut terproteksi dari pengguna luar.
Kode:
```php
 {
        path: '/about',
        component: About,
        meta: { requiresAuth: true }
    }
```
Hasil:
##### ![Gambar 1](ss1/gambar13.png).
Penjelasan: Pada hasil tersebut ketika saya mencoba mengakses about tanpa login dahulu. sistem dengan secara langsung menolak akses dan diarahkan ke halaman form login. setelah login berhasil baru halaman about baru dapat diakses.

#### Penjelasan Analisis Ringkas Dari Alur Kerja router.beforeEach Dan Axios HTTP Post
1. Analisis router.beforeEach
Ketika sistem berjalan router.beforeEach berfungsi memeriksa status login yang tersimpan di localStorage. Jika pengguna mencoba mengakses halaman tersebut tetapi belum login, sistem akan menampilkan pesan peringatan dan mengarahkan pengguna ke halaman Login. router.beforeEach berfungsi sebagai mekanisme keamanan untuk membatasi akses ke halaman tertentu tanpa login.
2. Analisis Axios HTTP POST
Saya mencoba memasukkan data username dan password. lalu data dikirim, setelah data dikirim, server akan melakukan proses validasi. Jika login berhasil, aplikasi menyimpan status login dan token ke localStorage, kemudian mengarahkan pengguna ke halaman Artikel. Tetapi ketika saya mencoba validasi gagal, sistem menampilkan pesan kesalahan yang diterima dari server. Dari kerja sistem tersebut Axios berperan sebagai penghubung antara frontend Vue.js dan backend API dalam proses autentikasi pengguna.



## Langkah-Langkah Praktikum 14: Keamanan API, Autentikasi Token, dan Axios Interceptors
#### Tahap 1: MENGAMANKAN ENDPOINT API (SISI BACKEND CI4)
Langkah 1.1: Membuat API Auth Filter
Membuat file filter baru di folder  app/Filters/ dengan nama ApiAuthFilter.php.
```php
<?php
namespace App\Filters;
use CodeIgniter\Filters\FilterInterface;
use CodeIgniter\Http\RequestInterface;
use CodeIgniter\Http\ResponseInterface;
use Config\Services;

class ApiAuthFilter implements FilterInterface
{
    public function before(RequestInterface $request, $arguments = null)
    {
        // 1. Ambil data Header Authorization dari request klien
        $authHeader = $request->getServer('HTTP_AUTHORIZATION');

        if (!$authHeader) {
            $response = Services::response();
            $response->setStatusCode(401);
            return $response->setJSON([
                'status' => 401,
                'error' => 401,
                'messages' => 'Akses Ditolak. Token tidak ditemukan pada request!'
            ]);
        }

        // 2. Ekstrak string token
        $token = null;
        if (preg_match('/Bearer\s(\S+)/', $authHeader, $matches)) {
            $token = $matches[1];
        }

        // 3. Validasi Token
        if (!$token || empty($token)) {
            $response = Services::response();
            $response->setStatusCode(401);
            return $response->setJSON([
                'status' => 401,
                'error' => 401,
                'messages' => 'Sesi Token tidak valid atau kedaluwarsa!'
            ]);
        }
    }

    public function after(RequestInterface $request, ResponseInterface $response, $arguments = null)
    {
        // Tidak diperlukan aksi setelah request diproses
    }
}
```
Penjelasan: kode ini berfungsi untuk melindungi akses yang tidak mempunyai token. sistem memeriksa autentikasi token pada header Authorization sebelum memproses request. kalau token tidak valid sistem akan mengembalikan dengan respon 401.

Langkah 1.2: Mendaftarkan Filter API
Menambahkan daftar class pada Filters.php.
```php
'apiauth' => \App\Filters\ApiAuthFilter::class,
```
Penjelasan: class ini berfungsi memvalidasi toke pada route tertentu sebelum mengakses endpoint API.

Langkah 1.3: Menerapkan Filter ke Route Artikel
Menerapkan filter apiauth pada app/Config/Routes.php.
```php
$routes->post('post', 'Api\\Post::create', ['filter' => 'apiauth']);
$routes->put('post/(:segment)', 'Api\\Post::update/$1', ['filter' => 'apiauth']);
$routes->delete('post/(:segment)', 'Api\\Post::delete/$1', ['filter' => 'apiauth']);
```
Penjelasan: kode route tersebut untuk melindungi operasi CUD data post dari pengguna yang tidak memiliki token autentikasi valid.

#### Tahap 2: IMPLEMENTASI AXIOS INTERCEPTORS (SISI VUEJS FRONTEND)
Langkah 2.1: Menambahkan Konfigurasi Interceptor Global pada assets/js/app.js
Menambahkan konfigurasi Interceptor Axios pada file app.js.
```php
const { createApp } = Vue;
const { createRouter, createWebHashHistory } = VueRouter;

const apiUrl = 'http://localhost/lab11_ci/ci4/public';

axios.interceptors.request.use(
    (config) => {
        const token = localStorage.getItem('userToken');
        if (token) {
            config.headers['Authorization'] = 'Bearer ' + token;
        }
        return config;
    },
    (error) => {
        return Promise.reject(error);
    }
);

axios.interceptors.response.use(
    (response) => {
        return response;
    },
    (error) => {
        if (error.response && error.response.status === 401) {
            alert('Sesi Anda telah berakhir atau Token tidak sah. Silakan login kembali.');
            localStorage.clear();
            window.location.href = '#/login';
            window.location.reload();
        }
        return Promise.reject(error);
    }
);

const routes = [
    {
        path: '/',
        component: Home
    },
    {
        path: '/login',
        component: Login
    },
    {
        path: '/artikel',
        component: Artikel,
        meta: { requiresAuth: true }
    },
    {
        path: '/about',
        component: About,
        meta: { requiresAuth: true }
    }
];

const router = createRouter({
    history: createWebHashHistory(),
    routes
});

router.beforeEach((to, from, next) => {
    const isAuthenticated = localStorage.getItem('isLoggedIn') === 'true';
    if (to.matched.some(record => record.meta.requiresAuth) && !isAuthenticated) {
        alert('Akses Ditolak! Anda harus login terlebih dahulu.');
        next('/login');
    } else {
        next();
    }
});

const app = createApp({
    data() {
        return {
            isLoggedIn: false
        }
    },
    mounted() {
        this.isLoggedIn = localStorage.getItem('isLoggedIn') === 'true';
    },
    methods: {
        logout() {
            if (confirm('Apakah Anda yakin ingin keluar aplikasi?')) {
                localStorage.removeItem('isLoggedIn');
                localStorage.removeItem('userToken');
                this.isLoggedIn = false;
                this.$router.push('/');
            }
        }
    }
});

app.use(router);
app.mount('#app');
```
Penjelasan: Axios Interceptor akan menyisipkan token autentikasi dengan otomatis ke setiap request API, menghapus sesi login ketika logout dan megarahkan ke halaman login. sistem membatasi hak akses ke halaman tertentu seperti Artikel dan About kepada pengguna yang belum login.

Pertanyaan dan Tugas
1. Terapkan seluruh langkah pembuatan kode pengamanan Token-Based Authentication
di atas baik pada backend (CI4) maupun frontend (VueJS).

2. Lakukan simulasi pengujian pembobolan database menggunakan aplikasi REST Client
seperti Postman atau Insomnia:
- Tembak URL input data artikel backend Anda (http://localhost/labci4/public/post) menggunakan metode POST tanpa menyertakan token di bagian headers.
- Amati hasil respon JSON yang diberikan oleh CodeIgniter 4. Pastikan statusnya mengembalikan error HTTP 401 Unauthorized.

Jawab:
##### ![Gambar 1](ss1/gambar19.png).
Penjelasan singkat: ketika URL di tembak hasil respon menunjukkan status berhasil dikembalikan dengan pesan error HTTP 401 Unauthorized

3. Buka aplikasi web utama lewat browser, lakukan login, lalu coba lakukan manipulasi data artikel (Tambah/Ubah/Hapus). Berkat Axios Interceptors, proses manipulasi data harus berjalan sukses karena token dikirim secara terselubung di latar belakang sistem.

Jawab:
##### ![Gambar 1](ss1/gambar20.png).
Penjelasan singkat: ketika melakukan manipulasi data artikel dan membuka tab network sebelum dijalankan. hasilnya menunjukkan bahwa token berhasil dikirim secara terselubung dilatar belakang sistem seperti yang ada pada gambar diatas

4. Tuliskan kesimpulan akhir dari hasil analisis Anda mengenai perbedaan mendasar fungsi perlindungan keamanan antara Vue Router Navigation Guards (sisi klien) dan CodeIgniter Filters (sisi server).
Jawab:
Dari hasil uji coba tersebut dapat disimpulkan bahwa Vue Router Navigation Guards hanya melindungi tampilan antarmuka pada browser, sehingga masih bisa dibypass dengan mengakses URL endpoint API secara langsung menggunakan Postman. Sedangkan CodeIgniter Filters melindungi data pada sisi server dengan memvalidasi token setiap request masuk, sehingga ketika mencoba tanpa token yang valid server langsung menolak request dengan response 401 Unauthorized tanpa menyentuh database. Menurut saya dari kedua sistem tersebubt lebih bagus diterapkan bersamaan. Karena Navigation Guards berguna untuk kenyamanan antarmuka dan CodeIgniter Filters berguna sebagai pelindung sistem.
