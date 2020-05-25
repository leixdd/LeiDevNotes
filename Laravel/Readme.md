# Laravel Dev Notes 

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




 * **Tip for Validations with numbers, letters and spaces**
   
   ```php
       //AppServiceProvider.php
      
       \Validator::extend('alpha_spaces', function($attr, $value, $parameters, $validator) {
            return preg_match('/^[a-zA-Z\s]+$/', $value);
        });

        \Validator::extend('alpha_num_spaces', function($attr, $value, $parameters, $validator) {
            return preg_match('/^[a-zA-Z0-9\s]+$/', $value);
        });
   ```
   After that create a validation message on resource/lang/validation.php
   
   
 * **Laravel Queue Systemd**
 
 ```
 
  # Laravel queue worker using systemd
  # ----------------------------------
  #
  # /lib/systemd/system/queue.service
  #
  # run this command to enable service:
  # systemctl enable queue.service

  [Unit]
  Description=Laravel queue worker

  [Service]
  User=nginx
  Group=nginx
  Restart=always
  ExecStart=/usr/bin/nohup /usr/bin/php /var/www/html/laravel-project/artisan queue:work --daemon

  [Install]
  WantedBy=multi-user.target
 
 ```
 
 * **Laravel VPS Deployment (Ubuntu)**

  1. Clone the Project
  2. Run Composer install and npm install
  3. Set permission

      ```
          sudo chown -R my-user:www-data /path/to/your/laravel/root/directory
          sudo find /path/to/your/laravel/root/directory -type f -exec chmod 664 {} \;    
          sudo find /path/to/your/laravel/root/directory -type d -exec chmod 775 {} \;
          sudo chgrp -R www-data storage bootstrap/cache
          sudo chmod -R ug+rwx storage bootstrap/cache
      ```

  4. Run laravel deployment commands

      ```
          composer install --optimize-autoloader --no-dev
          php artisan config:cache
          php artisan route:cache
          php artisan view:cache
      ```
 
   
