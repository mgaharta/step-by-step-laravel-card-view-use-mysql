# step-by-step-laravel-card-view-use-mysql
step by step laravel card view use mysql

## Data Migrations & Seeding

### 1. make migration file
on terminal
```
php artisan make:model Product -m
```
note : Product is name of page


### 2. Create Schema for input table Product on Mysql

app/database/migrations/...create_product_table.php

```
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateUtilitesTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->increments('id');
            $table->timestamps();
            $table->string('imagePath');
            $table->string('title');
            $table->text('description');
            $table->integer('price');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('products');
    }
}

```


### 3.Create Data Seeder

on terminal
```
php artisan make:seed ProductTableSeeder
```

Create Array
app/Product.php

```
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Utilites extends Model
{
    protected $fillable = ['imagePath', 'title', 'description', 'price'];
}

```

Create Validity Product

app/database/seeds/ProductTableSeeder.php
```
<?php

use Illuminate\Database\Seeder;

class UtilitesTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $product = new \App\Product([
            'imagePath' => 'src/img/koeno.jpg',
            'title' => 'Harry Potter',
            'description' => 'Super cool - at least as a child.',
        ]);
        $product->save();
    }
}

```

### 4. Call ProductTableSeeder
on app/database/seeds/DatabaseSeeder.php
```
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $this->call(ProductTableSeeder::class);
    }
}
```

on terminal run
```
php artisan migrate
```
then
```
php artisan db:seed
```


# Outputing Product Data

### 1. Set Place of view on route

app/Http/routes.php

```
<?php

Route::get('/', function () {
    return view('shop.index');
});
```

note : '/' is the url

### 2. Delete Auth folder
on app/Http/Controllers/Auth

### 3. Make a Product Controller On terminal run

```
php artisan make:controller ProductController
```

type on app\Http\Controllers\ProductController

```
<?php

namespace App\Http\Controllers;

use App\Utilites;
use Illuminate\Http\Request;
use App\Http\Requests;

class ProductController extends Controller
{
    public function getIndex()
    {
        $products = Product::all();
        return view('shop.index', ['products' => $products]);
    }
}
```

### 4. Show Database into card view
type on resources\views\shop\index.blade.php

```
@extends('layouts.master')

@section('title')
	Ngetrip
@endsection

@section('content')
    @if(Session::has('success'))
    <div class="row">
        <div class="col-sm6 col-md-4 col-md-offset-4 col-md-offset-3">
            <div id="charge-message" class="alert alert-success">
                {{ Session::get('success') }}
            </div>
        </div>
    </div>
    @endif
    @foreach($products->chunk(3) as $productChunk)
      <div class="row">
        @foreach($productChunk as $product)
        <div class="col-sm6 col-md-4 card">
        <img class="card-img-top" src="{{ $product->imagePath }}" alt="Card image cap"  class="img-responsive">
        <div class="card-body">
          <h5 class="card-title"></h5>
          <h3>{{ $product->title }}</h3>
          <p class="card-text text-secondary">{{ $product->description }}</p>
          <div class="clearfix">
            <div class="price pull-left font-weight-bold">Rp {{ $product->price }}</div>
            <a href="{{ route('product.addToCart', ['id' => $product->id])}}" class="btn btn-primary pull-right">Go somewhere</a>
          </div>
        </div>
      </div>
        @endforeach
      
    </div>
    @endforeach

@endsection
```


# user-management-laravel-shop-part-3
user-management-laravel-shop-part-3

### 1. Create token at users
```
$table->rememberToken();
```

tun on terminal
```
php artisan migrate:reset
```
```
php artisan migrate
```
```
php artisan db:seed
```


