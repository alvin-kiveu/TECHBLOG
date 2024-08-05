To create a cron job for your Laravel application running on Ubuntu, follow these steps:

Create a Laravel Command:
First, you'll need a Laravel command that you want to run. You can create one using Artisan.

bash
Copy code
php artisan make:command YourCommandName
This command will create a new file in app/Console/Commands/YourCommandName.php. Edit this file to define the functionality of your command.

php
Copy code
namespace App\Console\Commands;

use Illuminate\Console\Command;

class YourCommandName extends Command
{
    protected $signature = 'your:command';
    protected $description = 'Description of your command';

    public function __construct()
    {
        parent::__construct();
    }

    public function handle()
    {
        // Your logic here
    }
}
Register the Command:
Add the command to the $commands array in app/Console/Kernel.php.

php
Copy code
protected $commands = [
    \App\Console\Commands\YourCommandName::class,
];
Define the Schedule:
Open app/Console/Kernel.php and add the command to the schedule in the schedule method.

php
Copy code
protected function schedule(Schedule $schedule)
{
    $schedule->command('your:command')->daily(); // Adjust frequency as needed
}
You can use different scheduling options like hourly(), weekly(), etc., depending on how often you want the command to run.

Set Up the Cron Job:
Edit the crontab file using the following command:

bash
Copy code
crontab -e
Add the following line to the crontab to run the Laravel scheduler every minute:

bash
Copy code
* * * * * php /path-to-your-laravel-app/artisan schedule:run >> /dev/null 2>&1
Replace /path-to-your-laravel-app with the path to your Laravel application.

Save and Exit:
Save the crontab file and exit the editor. The cron job is now set up and will trigger Laravel's task scheduler every minute.

By following these steps, your Laravel command will run at the specified intervals as defined in your schedule method. If you need to debug or check if the cron job is running correctly, you can check the Laravel logs or set up logging within your command.



Test the Command:
Manually test the command to ensure it works:

bash
Copy code
php /var/www/html/TandaoRadius/artisan send:deleteexpiredusers
This will run the command and execute the deleteExpiredUser method. Check the output and logs to ensure it works as expected.