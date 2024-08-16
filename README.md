# Laravel Simple Auditing

![GitHub Actions](https://github.com/motomedialab/simple-laravel-audit/actions/workflows/main.yml/badge.svg)

A lightweight package to provide the ability to flexibly audit events and actions that happen within
your Laravel application.

## Installation

You can install the package via composer:

```bash
composer require motomedialab/simple-laravel-audit
```

Once you've done this, run your migrations. This will create a table called `audit_logs`.

```
php artisan migrate
```

## Configuration

Out of the box there isn't any requirement to configure the package. It will work with the default settings.

However, if you'd like to customise any options, such as the table name or classes that are utilised, you can publish the config file
and change any of the options. It's designed to be flexible allowing you to change IP address resolution, user ID resolution,
table name and more.

```
php artisan vendor:publish --tag=simple-auditor
```

## Usage

There's a multiple of ways you can use this package. The most common way is to use the `audit` helper function.

### Using the global audit helper

The `audit` helper function is an easy way to quickly log to the audits table. This function takes a string
as the first argument, and an optional array (context) as the second argument. This will only work if you
don't already have a global function called `audit`.

```php
audit('Action performed', ['more_data' => 'Goes here']);
```

### Using the facade

Some people love using Laravel's Facades due to their ease of use and static nature.

```php
// import the facade
use Motomedialab\SimpleLaravelAudit\Facades\SimpleAudit;

// create our audit log
SimpleAudit::audit('Action performed', ['more_data' => 'Goes here']);
```

### Emitting an event

Out of the box, we provide an event that can be emitted to log an audit. This is an alternative usage and
may be more suitable for your application. Again, this event takes a string as the first argument, and an optional
array (context) as the second argument.

```php
// import our AuditableEvent
use Motomedialab\SimpleLaravelAudit\Events\AuditableEvent;

// dispatch the event
event(new AuditableEvent('Action performed', ['more_data' => 'Goes here']));
```

### Using the `AuditableModel` trait on Models

If you have a model that you'd like to be audited on change, you can use the `AuditableModel` trait.
By default, this will record all creations, updates and deletions for this model to the audit log.
This uses Laravel model observers to listen for changes.

```php
use Motomedialab\SimpleLaravelAudit\Traits\AuditableModel;
use Illuminate\Database\Eloquent\Model;

class YourModel extends Model
{
    use AuditableModel;
}
```

#### Customising the `AuditableModel` functionality

If you'd like to expand the functionality of the `AuditableModel` trait, you can override its observer
by configuring the `observer` key in the config file. This will allow you to create your own model observer.

```php
use Motomedialab\SimpleLaravelAudit\Observers\AuditorModelObserver as BaseObserver;

class AuditableObserver extends BaseObserver
{
    // your custom classes here
    // see https://laravel.com/docs/11.x/eloquent#observers for more information
}
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email chris@motocom.co.uk instead of using the issue tracker.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.