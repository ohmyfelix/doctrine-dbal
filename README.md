![](https://heatbadger.now.sh/github/readme/contributte/doctrine-dbal/)

<p align=center>
  <a href="https://github.com/contributte/doctrine-dbal/actions"><img src="https://badgen.net/github/checks/contributte/doctrine-dbal/master?cache=300"></a>
  <a href="https://codecov.io/gh/contributte/doctrine-dbal"><img src="https://badgen.net/codecov/c/github/contributte/doctrine-dbal?cache=300"></a>
  <a href="https://packagist.org/packages/nettrine/dbal"><img src="https://badgen.net/packagist/dm/nettrine/dbal"></a>
  <a href="https://packagist.org/packages/nettrine/dbal"><img src="https://badgen.net/packagist/v/nettrine/dbal"></a>
</p>
<p align=center>
  <a href="https://packagist.org/packages/nettrine/dbal"><img src="https://badgen.net/packagist/php/nettrine/dbal"></a>
  <a href="https://github.com/contributte/doctrine-dbal"><img src="https://badgen.net/github/license/contributte/doctrine-dbal"></a>
  <a href="https://bit.ly/ctteg"><img src="https://badgen.net/badge/support/gitter/cyan"></a>
  <a href="https://bit.ly/cttfo"><img src="https://badgen.net/badge/support/forum/yellow"></a>
  <a href="https://contributte.org/partners.html"><img src="https://badgen.net/badge/sponsor/donations/F96854"></a>
</p>

<p align=center>
Website 🚀 <a href="https://contributte.org">contributte.org</a> | Contact 👨🏻‍💻 <a href="https://f3l1x.io">f3l1x.io</a> | Twitter 🐦 <a href="https://twitter.com/contributte">@contributte</a>
</p>

Doctrine DBAL for Nette Framework.

## Versions

| State       | Version | Branch   | Nette  | PHP     |
|-------------|---------|----------|--------|---------|
| dev         | `^0.11` | `master` | `3.2+` | `>=8.2` |
| stable      | `^0.10` | `master` | `3.2+` | `>=8.2` |

Integration of [Doctrine DBAL](https://www.doctrine-project.org/projects/dbal.html) for Nette Framework.

## Contents

- [Installation](#installation)
- [Configuration](#configuration)
  - [Minimal configuration](#minimal-configuration)
  - [Advanced configuration](#advanced-configuration)
  - [Encoding](#encoding)
  - [Replicas](#replicas)
  - [Caching](#caching)
  - [Types](#types)
  - [Debug](#debug)
  - [Middlewares](#middlewares)
  - [Logging](#logging)
  - [Console](#console)
- [Examples](#examples)

## Installation

Install package using composer.

```bash
composer require nettrine/dbal
```

Register prepared [compiler extension](https://doc.nette.org/en/dependency-injection/nette-container) in your `config.neon` file.

```neon
extensions:
  nettrine.dbal: Nettrine\DBAL\DI\DbalExtension
```

For better integration with Nette Framework and Tracy, pass `%debugMode%` to the extension constructor. Learn more in [debug chapter](#debug).

```neon
extensions:
  nettrine.dbal: Nettrine\DBAL\DI\DbalExtension(%debugMode%)
```

> [!NOTE]
> This is just **DBAL**, for **ORM** please use [nettrine/orm](https://github.com/contributte/doctrine-orm).

## Configuration

### Minimal configuration

**PostgreSQL**

```neon
nettrine.dbal:
  connections:
    default:
      driver: pdo_pgsql
      url: "postgresql://user:password@localhost:5432/dbname"
```

**MySQL / MariaDB**

```neon
nettrine.dbal:
  connections:
    default:
      driver: mysqli
      url: "mysql://user:password@localhost:3306/dbname"
```

**SQLite**

```neon
nettrine.dbal:
  connections:
    default:
      driver: pdo_sqlite
      url: "sqlite:///:memory:"
```

### Advanced configuration

Here is the list of all available options with their types.

 ```neon
nettrine.dbal:
  debug:
    panel: <boolean> # optional, it's determined automatically or passed to constructor when extension is registered
    sourcePaths: array<string>

  types: array<string, class-string>
  typesMapping: array<string, class-string>

  connections:
    <name>:
      # Connection
      application_name: <string>
      charset: <string>
      connectstring: <string>
      dbname: <string>
      defaultTableOptions: <array<string, mixed>>
      driver: <'pdo_sqlite', 'sqlite3', 'pdo_mysql', 'mysqli', 'pdo_pgsql', 'pgsql', 'pdo_oci', 'oci8', 'pdo_sqlsrv', 'sqlsrv', 'ibm_db2'>
      driverClass: <string>
      driverOptions: <array<string, mixed>>
      exclusive: <string>
      gssencmode: <string>
      host: <string>
      instancename: <string>
      keepReplica: <bool>
      memory: <bool>
      password: <string>
      path: <string>
      persistent: <bool>
      pooled: <string>
      port: <int>
      primary: <connection-config>
      protocol: <string>
      replica: array<string, connection-config>
      serverVersion: <string>
      service: <string>
      servicename: <string>
      sessionMode: <int>
      ssl_ca: <string>
      ssl_capath: <string>
      ssl_cert: <string>
      ssl_cipher: <string>
      ssl_key: <string>
      sslcert: <string>
      sslcrl: <string>
      sslkey: <string>
      sslmode: <string>
      sslrootcert: <string>
      unix_socket: <string>
      url: <string>
      user: <string>
      wrapperClass: <string>

      # Config
      middlewares: array<string, <service|class-name>>
      resultCache: <service|class-name>
      schemaAssetsFilter: <service|class-name>
      autoCommit: <boolean>
```

For example:

```neon
nettrine.dbal:
  types:
    uuid: Ramsey\Uuid\Doctrine\UuidType

  connections:
    # Short DSN configuration
    default:
      driver: pdo_pgsql
      url: "postgresql://user:password@localhost:5432/dbname?charset=utf8"

    # Explicit configuration
    second:
      driver: pdo_pgsql
      host: localhost
      port: 5432
      user: user
      password: password
      dbname: dbname
      charset: utf8
```

Supported drivers:

- `pdo_mysql`
- `pdo_sqlite`
- `pdo_pgsql`
- `pdo_oci`
- `oci8`
- `ibm_db2`
- `pdo_sqlsrv`
- `mysqli`
- `pgsql`
- `sqlsrv`
- `sqlite3`

> [!TIP]
> Take a look at real **Nettrine DBAL** configuration example at [contributte/doctrine-project](https://github.com/contributte/doctrine-project/blob/f226bcf46b9bcce2f68961769a02e507936e4682/config/config.neon).

### Encoding

You can set encoding for your connection.

```neon
nettrine.dbal:
  connections:
    default:
      charset: utf8
      defaultTableOptions:
        charset: utf8
        collate: utf8_unicode_ci
```

### Replicas

Doctrine DBAL supports read replicas. You can define them in the configuration.

```neon
nettrine.dbal:
  connections:
    default:
      driver: pdo_pgsql
      wrapperClass: Doctrine\DBAL\Connections\PrimaryReadReplicaConnection
      url: "postgresql://user:password@localhost:5432/table?charset=utf8&serverVersion=15.0"
      replica:
        read1:
          url: "postgresql://user:password@read-db1:5432/table?charset=utf8"
        read2:
          url: "postgresql://user:password@read-db2:5432/table?charset=utf8"
```

### Caching

> [!TIP]
> Take a look at more information in official Doctrine documentation:
> - https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/caching.html

A Doctrine DBAL can automatically cache result sets. The feature is optional though, and by default, no result set is cached.
You can enable the result cache by setting the `resultCache` configuration option to an instance of a cache driver.

> [!WARNING]
> Cache adapter must implement `Psr\Cache\CacheItemPoolInterface` interface.
> Use any PSR-6 + PSR-16 compatible cache library like `symfony/cache` or `nette/caching`.

```neon
nettrine.dbal:
  connections:
    default:
      # Create cache manually
      resultCache: App\CacheService(%tempDir%/cache/doctrine/dbal)

      # Use registered cache service
      resultCache: @cacheService
```

If you want to disable cache, you can use provided `NullCacheAdapter`.

```neon
nettrine.dbal:
    connections:
      default:
        resultCache: Nettrine\DBAL\Cache\NullCacheAdapter
```

If you like [`symfony/cache`](https://github.com/symfony/cache) you can use it as well.

```neon
nettrine.dbal:
    connections:
      default:
        # Create cache manually
        resultCache: Symfony\Component\Cache\Adapter\FilesystemAdapter(namespace: dbal, defaultLifetime: 0, directory: %tempDir%/cache/doctrine/dbal)

        # Use registered cache service
        resultCache: @cacheFilesystem
```

If you like [`nette/caching`](https://github.com/nette/caching) you can use it as well. Be aware that `nette/caching` is not PSR-6 + PSR-16 compatible, you need `contributte/psr16-caching`.

```neon
nettrine.dbal:
    connections:
      default:
        resultCache: Contributte\Psr6\CachePool(
          Nette\Caching\Cache(
            Nette\Caching\Storages\FileStorage(%tempDir%/cache)
            doctrine/dbal
          )
        )
```

> [!IMPORTANT]
> You should always use cache for production environment. It can significantly improve performance of your application.
> Pick the right cache adapter for your needs.
> For example from symfony/cache:
>
> - `FilesystemAdapter` - if you want to cache data on disk
> - `ArrayAdapter` - if you want to cache data in memory
> - `ApcuAdapter` - if you want to cache data in memory and share it between requests
> - `RedisAdapter` - if you want to cache data in memory and share it between requests and servers
> - `ChainAdapter` - if you want to cache data in multiple storages

### Types

> [!TIP]
> Take a look at more information in official Doctrine documentation:
> - http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/types.html
> - http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/cookbook/custom-mapping-types.html

Doctrine DBAL supports custom mapping types. You can define your own types in the configuration.

```neon
nettrine.dbal:
  types:
    # https://github.com/ramsey/uuid-doctrine
    uuid: Ramsey\Uuid\Doctrine\UuidType
```

You can also define type mapping for database columns.

```neon
nettrine.dbal:
  types:
    uuid_binary_ordered_time: Ramsey\Uuid\Doctrine\UuidBinaryOrderedTimeType
  typesMapping:
    uuid_binary_ordered_time: binary
```

### Debug

This library provides Tracy panel(s) for debugging queries.

You can enable Tracy panels(s) multiple ways:

1. Autodetect in development mode (`Tracy\Debugger::$productionMode = FALSE`). [Learn more about Nette & Tracy](https://doc.nette.org/en/application/bootstrap).

  ```php
  $configurator->enableTracy(...);
  ```

2. Manually provide `%debugMode%` to the extension.

  ```neon
  extensions:
    nettrine.dbal: Nettrine\DBAL\DI\DbalExtension(%debugMode%)
  ```

3. Manually set `debug.panel` to `true` in NEON configuration.

  ```neon
  nettrine.dbal:
    debug:
      panel: %debugMode%
  ```

![Tracy](.docs/assets/tracy.png)

> [!TIP]
> You can also use `sourcePaths` to backtrace queries to your source code.

```neon
nettrine.dbal:
  debug:
    sourcePaths: [%appDir%]
```

### Middlewares

> [!TIP]
> Take a look at more information in official Doctrine documentation:
> - https://www.doctrine-project.org/projects/doctrine-dbal/en/4.2/reference/architecture.html#middlewares
> - https://github.com/doctrine/dbal/issues/5784
> Since Doctrine v3.6 you have to use middlewares instead of event system.

Middlewares are the way how to extend doctrine library or hook to special events.

```neon
nettrine.dbal:
  connections:
    default:
      middlewares:
        insight: App\InsightMiddleware
```

```php
<?php declare(strict_types=1);

namespace App;

use Doctrine\DBAL\Driver;
use Doctrine\DBAL\Driver\Middleware;

final class InsightMiddleware implements Middleware
{

	public function wrap(Driver $driver): Driver
	{
		return new InsightDriverMiddleware($driver);
	}

}
```

```php
<?php declare(strict_types=1);

namespace App;

use Doctrine\DBAL\Driver\Connection;
use Doctrine\DBAL\Driver\Middleware\AbstractDriverMiddleware;

final class InsightDriverMiddleware extends AbstractDriverMiddleware
{

	public function connect(array $params): Connection
	{
		return new InsightConnection(parent::connect($params));
	}

}
```

### Logging

> [!TIP]
> Take a look at more information in official Doctrine documentation:
> - https://www.doctrine-project.org/projects/doctrine-dbal/en/4.2/reference/architecture.html#logging
> - https://github.com/doctrine/dbal/issues/5784
> Since Doctrine v3.6 you have to use middlewares instead of event system.

Doctrine DBAL supports logging of SQL queries. You can enable logging by setting `logger` middleware.

```neon
nettrine.dbal:
  connections:
    default:
      middlewares:
        # Create logger manually
        logger: Doctrine\DBAL\Logging\Middleware(
            Monolog\Logger(doctrine, [Monolog\Handler\StreamHandler(%tempDir%/doctrine.log)])
        )

        # Use registered logger service
        logger: Doctrine\DBAL\Logging\Middleware(@logger)
```

Unfortunately, `Doctrine\DBAL\Logging\Middleware` provides only basic logger. If you want to measure time or log queries to file, you have to implement your own logger.

```php
<?php declare(strict_types=1);

namespace App;

use Doctrine\DBAL\Driver;
use Doctrine\DBAL\Driver\Middleware;

final class InspectorMiddleware implements Middleware
{
    public function __construct() {
    }

    public function wrap(Driver $driver): Driver
    {
        // Create your custom driver, wrap the original one and return it
        return new InspectorDriver($driver);
    }
}
```

```neon
nettrine.dbal:
  connections:
    default:
      middlewares:
        inspector: App\InspectorMiddleware()
```

> [!TIP]
> Inspiration for custom middleware can be found in Symfony (symfony/doctrine-bridge).
> - https://github.com/symfony/doctrine-bridge/blob/09dbb7c731430335e9ae89ee5054b5f5580c49bf/Middleware/Debug/Middleware.php#L1-L37

### Console

> [!TIP]
> Doctrine DBAL needs Symfony Console to work. You can use `symfony/console` or [contributte/console](https://github.com/contributte/console).

```bash
composer require contributte/console
```

```neon
extensions:
  console: Contributte\Console\DI\ConsoleExtension(%consoleMode%)

  nettrine.dbal: Nettrine\DBAL\DI\DbalExtension
```

Since this moment when you type `bin/console`, there'll be registered commands from Doctrine DBAL.

![Console Commands](.docs/assets/console.png)

## Examples

> [!TIP]
> Take a look at more examples on [contributte.org](https://contributte.org/examples.html).

## Development

See [how to contribute](https://contributte.org) to this package. This package is currently maintained by these authors.

<a href="https://github.com/f3l1x">
    <img width="80" height="80" src="https://avatars2.githubusercontent.com/u/538058?v=3&s=80">
</a>

-----

Consider to [support](https://contributte.org/partners.html) **contributte** development team.
Also thank you for using this package.
