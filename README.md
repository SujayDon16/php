<?php
// assessment_8.php

// Composer Integration
// Composer is used to include the "slim/slim" package for creating REST APIs.
// Run `composer require slim/slim` in your terminal to include this package.

require 'vendor/autoload.php';

use Slim\Factory\AppFactory;
use Psr\Http\Message\ResponseInterface as Response;
use Psr\Http\Message\ServerRequestInterface as Request;

// Namespaces
namespace App\Utils {
    /**
     * Utility class to handle string operations.
     */
    class StringUtils
    {
        /**
         * Validate email format using regular expressions.
         *
         * @param string $email
         * @return bool
         */
        public static function validateEmail($email)
        {
            $pattern = '/^[\w\.-]+@[\w\.-]+\.\w+$/';
            return preg_match($pattern, $email) === 1;
        }
    }
}

// Create Slim App
$app = AppFactory::create();

// Define a route to validate email
$app->get('/validate-email/{email}', function (Request $request, Response $response, array $args) {
    $email = $args['email'];
    $isValid = \App\Utils\StringUtils::validateEmail($email);
    $result = ['email' => $email, 'valid' => $isValid];
    
    $response->getBody()->write(json_encode($result));
    return $response->withHeader('Content-Type', 'application/json');
});

// Define a route to fetch content (dummy implementation for demonstration)
$app->get('/fetch-content', function (Request $request, Response $response) {
    // Example content for demonstration
    $content = ['message' => 'This is a dummy content fetch.'];
    
    $response->getBody()->write(json_encode($content));
    return $response->withHeader('Content-Type', 'application/json');
});

// Run the app
$app->run();

// Magic Method Example
namespace App\MagicExample {

    class MagicClass
    {
        private $data = [];

        /**
         * Magic method __set to handle dynamic property setting.
         *
         * @param string $name
         * @param mixed $value
         */
        public function __set($name, $value)
        {
            $this->data[$name] = $value;
        }

        /**
         * Magic method __get to handle dynamic property retrieval.
         *
         * @param string $name
         * @return mixed
         */
        public function __get($name)
        {
            return $this->data[$name] ?? null;
        }
    }
}

// Example Usage
$magicClass = new \App\MagicExample\MagicClass();
$magicClass->foo = "bar";
echo "Dynamic property 'foo': " . $magicClass->foo . "\n";
?>
