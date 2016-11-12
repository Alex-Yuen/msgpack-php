# Msgpack for PHP
[![Build Status](https://secure.travis-ci.org/msgpack/msgpack-php.png)](https://travis-ci.org/msgpack/msgpack-php)

This extension provide API for communicating with MessagePack serialization. 

MessagePack is a binary-based efficient object serialization library.
It enables to exchange structured objects between many languages like JSON.
But unlike JSON, it is very fast and small.

## Requirement
- PHP 5.0 +

## Install

### Install from PECL
Msgpack is an PECL extension, thus you can simply install it by:
````
pecl install msgpack
````
### Compile Msgpack from source
````
$/path/to/phpize
$./configure 
$make && make install
````

### Example
```php
<?php
$data = array(0 => 1, 1 => 2, 2 => 3);
$msg = msgpack_pack($data);
$data = msgpack_unpack($msg);
```

### Example Advanced
```php
<?php
$data = array(0 => 1, 1 => 2, 2 => 3);
$packer = new \MessagePack(false);
$packed = $packer->pack($data);

$unpacker = new \MessagePackUnpacker(false);
$unpacker->feed($packed);
$unpacker->execute();
$unpacked = $unpacker->data();
$unpacker->reset();

```

### Example Advanced Streaming
```php
<?php
$data1 = array(0 => 1, 1 => 2, 2 => 3);
$data2 = array("a" => 1, "b" => 2, "c" => 3);

$packer = new \MessagePack(false);
$packed1 = $packer->pack($data1);
$packed2 = $packer->pack($data2);

$unpacker = new \MessagePackUnpacker();
$buffer = "";
$nread = 0;

//Simulating streaming data :)
$buffer .= $packed1;
$buffer .= $packed2;

while(true) {
   if($unpacker->execute($buffer, $nread)) {
       $msg = $unpacker->data();
       
       var_dump($msg);
       
       $unpacker->reset();
       $buffer = substr($buffer, $nread);
       $nread = 0;
       if(!empty($buffer)) {
            continue;
       }
   }
   break;
}

```

# Resources
 * [msgpack](http://msgpack.org/)
