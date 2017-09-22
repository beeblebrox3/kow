# KOW
Another PHP framework. I created this when I was studying about middlewares and PHP-DI and I came up with this... framework.

# How it works
It's just a middleware pipeline with PSR-7 and PSR-15 (draft yet) that has a router-dispatcher-ish middleware to simulate MVC.

# Want to contribute?
Just fork and pull request

# Install
As it's just a study framework (at least, for now), just clone the repo.

# Configuration
## Middleware
Just add a line to the `config\middlewares.php` with your new middleware. Should implement the PSR-15 interface (or extends the `AbstractMiddleware`).

### Example
```php
<?php
namespace Katapoka\Kow\Middlewares;

use Interop\Http\Middleware\DelegateInterface;
use Psr\Http\Message\RequestInterface;
use Psr\Http\Message\ResponseInterface;

class ShowErrosMiddleware extends AbstractMiddleware
{

    /**
     * Process an incoming client or server request and return a response,
     * optionally delegating to the next middleware component to create the response.
     *
     * @param RequestInterface $request
     * @param DelegateInterface $delegate
     *
     * @return ResponseInterface
     */
    public function process(RequestInterface $request, DelegateInterface $delegate) {
        ini_set('display_errors', 1);
        ini_set('display_startup_errors', 1);
        error_reporting(E_ALL);

        return $delegate->process($request);
    }
}
```

`di.php` file:

```php
<?php

return [
    \Katapoka\Kow\Middlewares\ShowErrosMiddleware::class,
    // ...
];
```

## DI Injection
I'm using the PHP-DI framework to inject. Just configure the `config\di.php` and add the function/how to retrieve the instance of your dependency.  
It has autowire annotation-based injection, just annontate your property with `@Inject`.  
If injecting a concrete class instead of an interface, there's no need to configure the `config\di.php` to this dependency.  