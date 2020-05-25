#Laravel VPS Deployment (Ubuntu)

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
