# Installation

```bash
$ composer require florianv/symfony-swap php-http/message php-http/guzzle6-adapter
```

Update the dependency by running:

```bash
$ php composer.phar update florianv/swap-bundle
```

Enable the Bundle in the kernel:

``` php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Florianv\SwapBundle\FlorianvSwapBundle(),
    );
}
```

# Configuration

## Builtin providers

### Yahoo Finance

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        yahoo_finance: ~
```

### Google Finance

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        google_finance: ~
```

### European Central Bank

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        european_central_bank: ~
```

### National Bank of Romania

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        national_bank_of_romania: ~
```

### Open Exchange Rates

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        open_exchange_rates:
            # Your app id
            app_id: secret

            # True if your AppId is an enterprise one. Defaults to false
            enterprise: true
```

### Xignite

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        xignite:
            # Your API token
            token: secret
```

### WebserviceX

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        webservicex: ~
```

### Central Bank of the Republic of Turkey

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        central_bank_of_republic_turkey: ~
```

You can register multiple providers, they will be called in chain. In this example the Yahoo Finance is
the first one and Google Finance is the second one:

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        yahoo_finance: ~
        google_finance: ~
```

## Custom providers

In order to add your custom providers implementing `Swap\ProviderInterface`, you can tag them as `florianv_swap.provider`:

```xml
<service id="acme_demo.provider.custom" class="Acme\DemoBundle\Provider\Custom">
    <tag name="florianv_swap.provider" />
</service>
```

## Providers priority

A priority attribute is available in tags and `config.yml` to sort providers.

```yaml
# app/config/config.yml
florianv_swap:
    providers:
        yahoo_finance:
            priority: 10
        google_finance:
            priority: 0
```

```xml
<service id="acme_demo.provider.custom" class="Acme\DemoBundle\Provider\Custom">
    <tag name="florianv_swap.provider" priority="5" />
</service>
```

In this case, the order of providers will be:

- yahoo_finance
- acme_demo.provider.custom
- google_finance

## Custom HTTP adapter

You can define your own HTTP adapter. It has to inherit from `Ivory\HttpAdapter\HttpAdapterInterface`.

```yaml
# app/config/config.yml
florianv_swap:
    http_adapter: my_service
```

## Cache

Currently only [Doctrine Cache](https://github.com/doctrine/cache) is supported as cache implementation.

### Lifetime

You must specify a lifetime for your cache entries:

```yaml
# app/config/config.yml
florianv_swap:
    cache:
        ttl: 3600 # seconds
```

### Provider

You can use a doctrine service id:

```yaml
# app/config/config.yml
florianv_swap:
    cache:
        doctrine: my_apc_service
```

or one of the implemented providers (`apc`, `apcu`, `array`, `xcache`, `wincache`, `zenddata`)

```yaml
# app/config/config.yml
florianv_swap:
    cache:
        doctrine: apcu
```

# Usage

The Swap service is available in the container:

```php
/** @var \Swap\SwapInterface $swap */
$swap = $this->get('florianv_swap.swap');
```

For more information about how to use it, please consult the [Swap documentation](https://github.com/florianv/swap).
