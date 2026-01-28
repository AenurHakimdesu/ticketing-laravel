# Guide Book Project Magang BKPSDM

## **Langkah-langkah Git**

### 1. **Clone repository**
```bash
git clone https://github.com/AenurHakimdesu/ticketing-laravel.git
```
### 2. **Buat env**
Jalankan di terminal local vscode
```bash
copy .env.example .env
``` 
### 3. **Install Composser**
agar command artisan bisa berjalan
```bash
composer install
```
### 4. **Generate Application Key**
membuat kunci enkripsi aplikasi agar aplikasi bisa berjalan
```bash
php artisan key:generate
```
### 5. **Konfigurasi env**
```bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=ticketing_app
DB_USERNAME=root
DB_PASSWORD=
```
### 6. **Jalankan migrasi**
```bash
php artisan migrate
```
lalu ketik 
```bash
yes
```
### 7. **Migrasi seeder**
```bash
php artisan db:seed
```
### 8. **Install Laravel Breeze**
```bash
composer require laravel/breeze --dev
```
Setelah itu, jalankan perintah instalasi Breeze:
```bash
php artisan breeze:install
```
Which Breeze stack would you like to install?
```bash
blade
```
would you like dark support
```bash
yes
```
Which testing framework do you prefer?
```bash
1
```
### 9. **Install Dependency Frontend**
```bash
npm install
```
Setelah proses instalasi selesai, jalankan build frontend
```bash
npm run build
```
jika tidak bisa jalankan npm
```bash
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```
### 10. **setting route**
```bash
<?php

use App\Http\Controllers\User\OrderController;
use App\Http\Controllers\Admin\EventController as AdminEventController;
use App\Http\Controllers\User\EventController as UserEventController;
use App\Http\Controllers\User\HomeController;
use App\Http\Controllers\Admin\HistoriesController;
use App\Http\Controllers\Admin\TiketController;
use App\Http\Controllers\Admin\EventController;
use App\Http\Controllers\Admin\CategoryController;
use App\Http\Controllers\Admin\DashboardController;
use App\Http\Controllers\ProfileController;
use Illuminate\Support\Facades\Route;

Route::get('/', [HomeController::class, 'index'])->name('home');

// Events
Route::get('/events/{event}', [UserEventController::class, 'show'])->name('events.show');

//Orders
Route::get('/orders', [OrderController::class, 'index'])->name('orders.index');
Route::get('/orders/{order}', [OrderController::class, 'show'])->name('orders.show');
Route::post('/orders', [OrderController::class, 'store'])->name('orders.store');

Route::middleware('auth')->group(function () {
    Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
    Route::patch('/profile', [ProfileController::class, 'update'])->name('profile.update');
    Route::delete('/profile', [ProfileController::class, 'destroy'])->name('profile.destroy');

    
    Route::middleware('admin')->prefix('admin')->name('admin.')->group(function () {
        Route::get('/', [DashboardController::class, 'index'])->name('dashboard');
        
        // Category Management
        Route::resource('categories', CategoryController::class);

        // Event Management
        Route::resource('events', EventController::class);

        // Tiket Management 
        Route::resource('tickets', TiketController::class);
    
        // Histories
        Route::get('/histories', [HistoriesController::class, 'index'])->name('histories.index');
        Route::get('/histories/{id}', [HistoriesController::class, 'show'])->name('histories.show');
        });
});

require __DIR__.'/auth.php';
```
### 10. **setting ...\app\Http\Controllers\Auth\AuthenticatedSessionController.php**
```bash
<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use App\Http\Requests\Auth\LoginRequest;
use Illuminate\Http\RedirectResponse;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Illuminate\View\View;

class AuthenticatedSessionController extends Controller
{
    /**
     * Display the login view.
     */
    public function create(): View
    {
        return view('auth.login');
    }

    /**
     * Handle an incoming authentication request.
     */
    public function store(LoginRequest $request): RedirectResponse
    {
            $request->authenticate();
            $request->session()->regenerate();
            
            // Redirect based on user role
            $user = Auth::user();
            if ($user->role === 'admin') {
                    return redirect()->intended('/admin');
            } else {
                    return redirect()->intended('/');
            }
    }

    /**
     * Destroy an authenticated session.
     */
    public function destroy(Request $request): RedirectResponse
    {
        Auth::guard('web')->logout();

        $request->session()->invalidate();

        $request->session()->regenerateToken();

        return redirect('/');
    }
}

```
### 11. **refresh**
```bash
php artisan optimize:clear
```
### 12. **Jalankan server**
```bash
php artisan serve
```
