## Laravel Vendor Cleanup Command

### Remove tests & documentation from the vendor dir

#### Linux only and requires the exec() command!
Require this package in your composer.json:

    "barryvdh/laravel-vendor-cleanup": "dev-master"

And add the ServiceProvider to the providers array in app/config/app.php

    'Barryvdh\VendorCleanup\VendorCleanupServiceProvider',

You can now remove all the docs/tests/examples/build scripts throught artisan

    php artisan vendor-cleanup

You should change your composer file to use the `dist` version, without the .git history.
You can configure your composer.json to do this, and cleanup after updating:

    "scripts":{
        "post-update-cmd":[
            "php artisan vendor-cleanup",
            "php artisan optimize"
        ]
    },
    "config": {
        "preferred-install": "dist"
    },

Note: If you switch from prefer-source (the default composer setting for non-stable packages), you should delete your
vendor dir and re-download all packages, without the .git history (and greatly reducing filesize).
This will also prevent questions about changed files, because packages with .git history are checked for changes.

When distributing

You can add your own rules, when you publish the config file.

    'commands' => array(
       'name/package' => 'CHANGELOG* phpunit.xml* tests docs',
    ),

This wil look for `CHANGELOG* phpunit.xml* tests docs` in vendor/name/package and delete them. So this is the same as running:
`cd /path/to/vendor/name/package && rm -rf CHANGELOG* phpunit.xml* tests docs`

If the package is commonly used, please make a PR to add the command to src/Barryvdh/VendorCleanup/VendorCleanupCommand.php

Most of the cleanup commands are based on the SensioDistributionBundle build script;
https://github.com/sensio/SensioDistributionBundle/blob/master/Resources/bin/build.sh


### License

The Laravel Vendor Cleanup Command is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)