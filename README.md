Repository Design Pattern

1. Trait <br/>
untuk membuat fungsi yang dilakukan berulang kali (seperti return format json yang sama).

2. Interface<br/>
Sebelum itu, bikin validasi di Request/UserRequest (php artisan make:request UserRequest)
Dalam interface, tentukan method2 yang wajib untuk digunakan beserta parameternya.

3. Repository<br/>
Repository is a layer for communicating with the controller and the data, in this case interacting with the database.

cth <br/>
 public function getAllUsers()<br/>
    {<br/>
        try { <br/>
            $users = User::all(); <br/>
            return $this->success("All Users", $users); <br/>
        } catch(\Exception $e) { <br/>
            return $this->error($e->getMessage(), $e->getCode()); <br/>
        } <br/>
    } <br/>

4. Refactor Controller <br/>
cth <br/>
  public function index()<br/>
    { <br/>
        return $this->userInterface->getAllUsers(); <br/>
    } <br/>

5. Registering a new Provider <br/>
app/providers/RepositoryServiceProvider.php <br/><br/>
<?php <br/>

namespace App\Providers; <br/>

use Illuminate\Support\ServiceProvider; <br/>

class RepositoryServiceProvider extends ServiceProvider <br/>
{ <br/>
    public function register() <br/>
    { <br/>
        // Register Interface and Repository in here <br/>
        // You must place Interface in first place <br/>
        // If you dont, the Repository will not get readed. <br/>
        $this->app->bind( 
            'App\Interfaces\UserInterface',
            'App\Repositories\UserRepository'
        );
    }<br/>
}
