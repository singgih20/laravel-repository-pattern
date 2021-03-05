Repository Design Pattern

1. Trait
untuk membuat fungsi yang dilakukan berulang kali (seperti return format json yang sama).

2. Interface
Sebelum itu, bikin validasi di Request/UserRequest (php artisan make:request UserRequest)
Dalam interface, tentukan method2 yang wajib untuk digunakan beserta parameternya.

3. Repository
Repository is a layer for communicating with the controller and the data, in this case interacting with the database.

cth 
 public function getAllUsers()
    {
        try {
            $users = User::all();
            return $this->success("All Users", $users);
        } catch(\Exception $e) {
            return $this->error($e->getMessage(), $e->getCode());
        }
    }

4. Refactor Controller
cth
  public function index()
    {
        return $this->userInterface->getAllUsers();
    }

5. Registering a new Provider
app/providers/RepositoryServiceProvider.php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;

class RepositoryServiceProvider extends ServiceProvider
{
    public function register()
    {
        // Register Interface and Repository in here
        // You must place Interface in first place
        // If you dont, the Repository will not get readed.
        $this->app->bind(
            'App\Interfaces\UserInterface',
            'App\Repositories\UserRepository'
        );
    }
}
