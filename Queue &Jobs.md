## Queue & Jobs in Laravel

1. Set Up the Queue Driver
Ensure your Laravel application is configured to use a queue driver. For testing purposes, you can use the database driver. Update the .env file:

```env
QUEUE_CONNECTION=database
```

Run the migrations to create the necessary table:

```bash
php artisan queue:table
php artisan migrate
```


2. Create a Job
Generate a new job class:

```bash
php artisan make:job TestQueueJob
```

This creates a file in app/Jobs. Edit it to include the logic you want to test. For example:

```php
namespace App\Jobs;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;
use Illuminate\Support\Facades\Log;

class TestQueueJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public $data;

    /**
     * Create a new job instance.
     */
    public function __construct($data)
    {
        $this->data = $data;
    }

    /**
     * Execute the job.
     */
    public function handle()
    {
        Log::info('Queue job executed with data: ' . $this->data);
    }
}
```


3. Dispatch the Job

You can dispatch the job in a controller, command, or route for testing purposes. For example, add this in web.php:

```php
use App\Jobs\TestQueueJob;

Route::get('/test-queue', function () {
    TestQueueJob::dispatch('Testing Laravel Queue');
    return 'Job dispatched!';
});

```
Visit /test-queue in your browser to dispatch the job.

4. Run the Queue Worker

Start the queue worker in your terminal:

```bash
php artisan queue:work
```

The job will be picked up and executed. You can check the storage/logs/laravel.log file to confirm the job execution.

5. Monitor the Queue

Use the following command to check pending jobs:

```bash
php artisan queue:failed
```

If a job fails, you can retry it:

```bash
php artisan queue:retry all
```


6. Testing in a Unit Test

You can test a queue in your test suite using Laravel's built-in queue fake:

```php
use Illuminate\Support\Facades\Queue;
use Tests\TestCase;
use App\Jobs\TestQueueJob;

class QueueTest extends TestCase
{
    public function test_queue_job_dispatched()
    {
        Queue::fake();

        // Dispatch the job
        TestQueueJob::dispatch('Test Data');

        // Assert the job was pushed
        Queue::assertPushed(TestQueueJob::class, function ($job) {
            return $job->data === 'Test Data';
        });
    }
}
```



To check if Supervisor is running on Ubuntu, follow these steps:

1. Check the Supervisor Service Status

Run the following command to check the status of the Supervisor service:

```bash
sudo systemctl status supervisor
```
If Supervisor is running, the output will include Active: active (running).

If it is not running, you might see inactive or failed.

2. Check if Supervisor is Enabled at Startup

Ensure that Supervisor is set to start on boot:

```bash
sudo systemctl is-enabled supervisor
```

The output should be enabled. If it's not, enable it with:

```bash
sudo systemctl enable supervisor
```


3. Check Running Processes Managed by Supervisor

To see all processes managed by Supervisor:

```bash
sudo supervisorctl status
```


This will display a list of all programs configured in Supervisor along with their current status (e.g., RUNNING, STOPPED, etc.).

4. Verify Supervisor Version
To ensure Supervisor is installed and accessible, check its version:

```bash
supervisord --version
```

If you receive a version number, Supervisor is installed correctly.

5. Restart Supervisor if Necessary
If Supervisor is not running or needs a restart, you can start/restart it with:

```bash
sudo systemctl start supervisor
sudo systemctl restart supervisor
```

6. Check Logs for Troubleshooting
If Supervisor is not working as expected, inspect the logs:

```bash
sudo tail -f /var/log/supervisor/supervisord.log
```

This log file provides details on errors and issues related to Supervisor.

Following these steps will help you verify and manage the Supervisor process on an Ubuntu system.






2. Program Configuration Files:

The program-specific configuration files are often located in:

```bash
cd /etc/supervisor/conf.d/
```

Ensure file is writabel

```bash
sudo chmod -R 775 /var/www/html/myapp
```

Start/Restart the Worker: If it's already running, restart the worker with:

```bash
sudo supervisorctl restart laravel-worker:*
```