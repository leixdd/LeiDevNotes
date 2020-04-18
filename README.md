# LeiDevNotes

  * **Problem: Laravel QueryException**
      > SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes
      
       - **Solution**
         
         Place 
         ```php  
         \Schema::defaultStringLength(191); 
         ``` 
         inside of boot method of AppServiceProvider.
         Then run 
         
         > php artisan migrate 
         
         again
         
       - **Code**
         ```php
            <?php

              namespace App\Providers;

              use Illuminate\Support\ServiceProvider;

              class AppServiceProvider extends ServiceProvider
              {
                  /**
                   * Register any application services.
                   *
                   * @return void
                   */
                  public function register()
                  {
                      //
                  }

                  /**
                   * Bootstrap any application services.
                   *
                   * @return void
                   */
                  public function boot()
                  {
                      //other code
                       \Schema::defaultStringLength(191); // it will adjust the default string length whenever we use the Schema Facade
                  }
              }

         ```
