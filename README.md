ReservixGuzzleBundle
===================

ReservixGuzzleBundle is a Symfony2 bundle for integrating the [Guzzle PHP library](http://github.com/guzzle/guzzle) in your project.

## Installation

Installation is a quick (I promise!) 4 step process:

1. Download ReservixGuzzleBundle
2. Configure the Autoloader
3. Enable the Bundle
4. Configure the ReservixGuzzleBundle

### Step 1: Download ReservixGuzzleBundle

Ultimately, the GuzzleBundle files should be downloaded to the
`vendor/bundles/Reservix/GuzzleBundle` directory.

This can be done in several ways, depending on your preference. The first
method is the standard Symfony2 method.

**Using the vendors script**

Add the following lines in your `deps` file:

``` ini
[guzzle]
    git=git://github.com/guzzle/guzzle.git
    target=guzzle

[ReservixGuzzleBundle]
    git=git://github.com/reservix/GuzzleBundle.git
    target=bundles/Reservix/GuzzleBundle
```

Now, run the vendors script to download the bundle:

``` bash
$ php bin/vendors install
```

**Using submodules**

If you prefer instead to use git submodules, then run the following:

``` bash
$ git submodule add git://github.com/guzzle/guzzle.git vendor/guzzle
$ git submodule add git://github.com/reservix/GuzzleBundle vendor/bundles/Reservix/GuzzleBundle
$ git submodule update --init
```

### Step 2: Configure the Autoloader

Add the `Guzzle` and `Reservix` namespace to your autoloader:

``` php
<?php
// app/autoload.php
$loader->registerNamespaces(array(
    // ...
    'Guzzle'           => __DIR__.'/../vendor/guzzle/src',
    'Reservix'          => __DIR__.'/../vendor/bundles',
));
```

### Step 3: Enable the bundle

Finally, enable the bundle in the kernel

``` php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Reservix\GuzzleBundle\ReservixGuzzleBundle(),
    );
}
```

### Step 4: Configure the ReservixGuzzleBundle

``` yaml
# app/config/config.yml
reservix_guzzle:
    service_builder:
        configuration_file: "%kernel.root_dir%/config/webservices.xml"
```

And add a Guzzle services configuration file. See the [Guzzle documentation](http://guzzlephp.org/tour/using_services.html#instantiating-web-service-clients-using-a-servicebuilder).

``` xml
<!-- app/config/webservices.xml -->
<?xml version="1.0" ?>
<guzzle>
    <clients>
        <!-- Abstract service to store AWS account credentials -->
        <client name="abstract.aws">
            <param name="access_key" value="12345" />
            <param name="secret_key" value="abcd" />
        </client>
        <!-- Amazon S3 client that extends the abstract client -->
        <client name="s3" class="Guzzle.Aws.S3.S3Client" extends="abstract.aws">
            <param name="devpay_product_token" value="XYZ" />
            <param name="devpay_user_token" value="123" />
        </client>
        <client name="simple_db" class="Guzzle.Aws.SimpleDb.SimpleDbClient" extends="abstract.aws" />
        <client name="sqs" class="Guzzle.Aws.Sqs.SqsClient" extends="abstract.aws" />
        <!-- Unfuddle client -->
        <client name="unfuddle" class="Guzzle.Unfuddle.UnfuddleClient">
            <param name="username" value="test-user" />
            <param name="password" value="my-password" />
            <param name="subdomain" value="my-subdomain" />
        </client>
    </clients>
</guzzle>
```

## License

This bundle is under the MIT license. See the complete license in the bundle:

    Resources/meta/LICENSE
