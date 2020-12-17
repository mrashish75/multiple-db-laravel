# multiple-db-laravel

Multiple Databases in Laravel 



Step 1:- Setup Laravel 

# composer create-project --prefer-dist laravel/laravel multiple-db


Step 2:- Create Multiple DB

# Create Multiple DB in mysql server
Login to Mysql Server then Create Databases-

CREATE DATABASE `multipal-db1`;
CREATE DATABASE `multipal-db2`;

Step 3:- Changes in .env files

copy and past with rename variables like 

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=multipal-db1
DB_USERNAME=root
DB_PASSWORD=

RDF_DB_CONNECTION=mysql
RDF_DB_HOST=127.0.0.1
RDF_DB_PORT=3306
RDF_DB_DATABASE=multipal-db2
RDF_DB_USERNAME=root
RDF_DB_PASSWORD=

Note:- RDF_ is name use second database variable name


Step 4:- Changes in config/database.php Files  

looking at Database Connections -

There is a multiple array of vairiable, value getting from env files

Just Copy mysql array like that :-

		'mysql' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],  

Rename Connection name with .env prefix variables  -

		'RDF_DB' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('RDF_DB_HOST', '127.0.0.1'),
            'port' => env('RDF_DB_PORT', '3306'),
            'database' => env('RDF_DB_DATABASE', 'forge'),
            'username' => env('RDF_DB_USERNAME', 'forge'),
            'password' => env('RDF_DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ], 

Step 5:- Use Model With Separated Databases

		/**
		*SomeModel use database of RDF_Db (other DB)
		*/
		class SomeModel extends Model
		{
		    protected $connection='RDF_DB';
		    protected $table='tablesname';
	    }

		/**
		*SomeModel2 use database of mysql (default DB)
		*/
	    class SomeModel2 extends Model
		{
		    protected $table='tablesname';
	    }
	    
Step 6:- Use Separated Databases by Eloquent ORM
		
		Exp. 1->
		$sql="select * from tablesname  where id=? ";

        DB::connection('RDF_DB')->select($sql,[
            $id,
        ]);

		Exp 2->
        DB::connection('RDF_DB')
        		->table('tablesname')
                ->count();

Step 7: Dynamice Model DB changes Connection

		$someModel = new SomeModel;

        $someModel->setConnection('mysql2');

        $something = $someModel->find(1);

		Other Aternet Options
			$user = User::on(‘RDF_DB’)->find(1);
			User::on(‘another-db’)->find(1);
