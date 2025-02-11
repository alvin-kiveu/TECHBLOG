
```bash
composer require laravel/horizon
```

Then publish and migrate the Horizon configuration:

```bash
php artisan horizon:install
```

```bash
php artisan migrate
```

```bash
php artisan make:migration create_horizon_jobs_table
```

```bash
php artisan make:migration create_horizon_monitoring_table
```

Start Horizon:

```bash
php artisan horizon
```

Visit http://your-app.test/horizon 
