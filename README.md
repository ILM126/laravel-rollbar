Laravel Rollbar
===============

[![Build Status](http://img.shields.io/travis/jenssegers/laravel-rollbar.svg)](https://travis-ci.org/jenssegers/laravel-rollbar) [![Coverage Status](http://img.shields.io/coveralls/jenssegers/laravel-rollbar.svg)](https://coveralls.io/r/jenssegers/laravel-rollbar)

Rollbar error monitoring integration for Laravel projects. This library adds a listener to Laravel's logging component. Laravel's session information will be sent in to Rollbar, as well as some other helpful information such as 'environment', 'server', and 'session'.

![rollbar](https://d37gvrvc0wt4s1.cloudfront.net/static/img/features-dashboard1.png?ts=1361907905)

Installation
------------

Install using composer:

    composer require jenssegers/rollbar

Add the service provider to the `'providers'` array in `config/app.php`:

    Jenssegers\Rollbar\RollbarServiceProvider::Class,

Configuration
-------------

This package supports configuration through the services configuration file located in `config/services.php`. All configuration variables will be directly passed to Rollbar:

    'rollbar' => array(
        'access_token' => 'your-rollbar-token',
        'level' => 'debug',
    ),

The level variable defines the minimum log level at which log messages are sent to Rollbar. For development you could set this either to `debug` to send all log messages, or to `none` to sent no messages at all. For production you could set this to `error` so that all info and debug messages are ignored.

Usage
-----

To automatically monitor exceptions, simply use the `Log` facade in your error handler in `app/Exceptions/Handler.php`:

    public function report(Exception $e)
    {
        \Log::error($e);

        return parent::report($e);
    }

For Laravel 4 installations, this is located in `app/start/global.php`:

    App::error(function(Exception $exception, $code)
    {
        Log::error($exception);
    });

Your other log messages will also be sent to Rollbar:

    \Log::debug('Here is some debug information');

### Context informaton

You can pass user information as context like this:

    \Log::error('Something went wrong', [
        'person' => ['id' => 123, 'username' => 'John Doe', 'email' => 'john@doe.com']
    ]);

Or pass some extra information:

    \Log::warning('Something went wrong', [
        'download_size' => 3432425235
    ]);
