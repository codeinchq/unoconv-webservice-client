# Unoconv web service client

This PHP 7 library is a (very) simple client for [zrrrzzt/tfk-api-unoconv](https://github.com/zrrrzzt/tfk-api-unoconv) and in particular its [Docker implementation](https://github.com/zrrrzzt/docker-unoconv-webservice) (also available on [Docker hub](https://hub.docker.com/r/zrrrzzt/docker-unoconv-webservice)). The library is fully [PSR-7](https://www.php-fig.org/psr/psr-7/) compatible and uses [Guzzle](https://github.com/guzzle/guzzle) as its HTTP client.

## Usage

```php
<?php
use CodeInc\UnoconvClient\UnoconvClient;

$client = new UnoconvClient('http://localhost:3000');
$client->getFormats(); // lists supported formats (calls /unoconv/formats)
$client->getFormats('graphics'); // list supported format for the graphics type (calls /unoconv/formats/graphics)
$client->getHealth(); // returns the API server's uptime (calls /healthz)
$client->getVersions(); // returns all versions of installed dependencies lookup (calls /unoconv/versions)

// converts a document
$responseStream = $client->convert($sourceStream, 'pdf');
```

### To convert a local file
```php
<?php
use CodeInc\UnoconvClient\UnoconvClient;
use GuzzleHttp\Psr7\LazyOpenStream;
use function GuzzleHttp\Psr7\copy_to_string;

$client = new UnoconvClient('http://localhost:3000');
$localFilePath = '/path/to/my/document.docx';
$localFileStream = new LazyOpenStream($localFilePath);
$responseStream = $client->convert($localFileStream, 'pdf');
```

### To convert a local file and display the result
```php
<?php
use CodeInc\UnoconvClient\UnoconvClient;
use GuzzleHttp\Psr7\LazyOpenStream;
use function GuzzleHttp\Psr7\copy_to_string;

$client = new UnoconvClient('http://localhost:3000');
$localFilePath = '/path/to/my/document.docx';
$localFileStream = new LazyOpenStream($localFilePath);
$responseStream = $client->convert($localFileStream, 'pdf');

header('Content-Type: application/pdf');
echo $responseStream;
```

## Installation

This library is available through [Packagist](https://packagist.org/packages/codeinc/unoconv-webservice-client) and can be installed using [Composer](https://getcomposer.org/):

```bash
composer require codeinc/unoconv-webservice-client
```


## License

The library is published under the MIT license (see [`LICENSE`](LICENSE) file).