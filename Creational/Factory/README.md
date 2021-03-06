
1. Factory Design Pattern break the tight coupling between source of data and process of data.
2. Factory design pattern is very good when you are dealing with multiple resources and want to implement high level abstraction.
3. In general a "factory" produces objects, it means it is just a centralized place where things are created.

Just like real world factories which creates product , there are different factory patterns .
1. *Simple Factory Pattern*
2. *Factory Method*
3. *Abstract Factory*
 
**Simple Factory Pattern:**

Suppose you have to implement abstraction and the user of your class doesn't need to care about what you've implemented in class definition. He/She just need to worry about the use of your class methods. Imagine, suppose your project needs two database connection

**Restriction:**

1. *Factory class must have a static method(factory method)*
2. *Static method(Factory method) must have to return class intance*
3. *Only one object should be created at one time*


```
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

/**
 * All car should extend this abstract car class
 */
abstract class CarAbstract
{
    protected $types;
 
    public function getTypes() {
        return $this->types;
    }
}
 
/**
 * used to represent a petrol car
 */
class PetrolCar extends CarAbstract
{
    protected $types = 'petrol car </br>';
}
 
/**
 * used to represent a diesel car
 */
class DieselCar extends CarAbstract
{
    protected $types = 'diesel car </br>';
}
 
/**
 * The is the factory which creates car objects
 */
class CarFactory
{
    public static function factory($car) 
    {
        switch ($car) {
            case 'petrol':
                $obj = new PetrolCar();
                break;
            case 'diesel':
                $obj = new DieselCar();
                break;
            default:
                throw new Exception("Car factory could not create car of types '" . $car . "'", 1000);
        }
        return $obj;
    }
}
 
try {
 
    $petrol = CarFactory::factory('petrol'); // object(PetrolCar)#1
    echo $petrol->getTypes(); // petrol car
 
    $diesel = CarFactory::factory('diesel'); // object(DieselCar)#1
    echo $diesel->getTypes(); // diesel car
 
    $hippo = CarFactory::factory('electric'); // This will throw an Exception
 
} catch(Exception $e) {
    echo $e->getMessage(); // CarFactory could not create car of types 'electric'
}
```

**Real Example:**

Lets think about newspaper printing machine which has plate, ink, huge roller that all works together to create each print of your news paper.

**Factory Method:**

Factory Method and simple factory patter are most similar to each other but difference is factory method has multiple factories. You can use factory method if you have large number of product classes and want to group them.

**Restriction**

1: *Each factory you create must implement a factory interface to make them polymorphic*
2: 

```
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

/**
 * All car should extend this abstract car class
 */
abstract class CarAbstract 
{
    protected $types;
    protected $location;
 
    public function getMessage() 
    {
        return '</br>Connecting to '.$this->types.' located in '.$this->location;
    }
}
 
/**
 * used to represent a petrol car
 */
class PetrolCar extends CarAbstract
{
    protected $types = 'petrol car';
    
    public function __construct($location, $type) {
        $this->location = $location;
        $this->types = $type.' '.$this->types;
    }
}
 
/**
 * used to represent a diesel car
 */
class DieselCar extends CarAbstract 
{
    protected $types = 'diesel car';
    
    public function __construct($location, $type) {
        $this->location = $location;
        $this->types = $type.' '.$this->types;
    }
}
 
/**
 * used to represent a Electric car
 */
class ElectricCar extends CarAbstract
{
    protected $types = 'Electric car';
    
    public function __construct($location, $type) {
        $this->location = $location;
        $this->types = $type.' '.$this->types;
    }
}

/**
 * used to represent a LPG Car
 */
class LPGCar extends CarAbstract 
{
    protected $types = 'LPG car';
    
    public function __construct($location, $type) {
        $this->location = $location;
        $this->types = $type.' '.$this->types;
    }
}
 
/**
 * used to represent a all factories should implement this interface
 */
interface CarFactoryInterface 
{
    public static function factory($car);
}
 
/**
 * this should be used to create all cars which are LUXURY
 */
class LuxuryCarFactory implements CarFactoryInterface
{
    public static function factory($car) 
    {
        switch ($car) {
            case 'petrol':
                $obj = new PetrolCar('India','Luxury');
                break;
            case 'diesel':
                $obj = new DieselCar('China','Luxury');
                break;
            default:
                throw new Exception("Luxury car factory could not create car of types '" . $car . "'", 1000);
        }
        return $obj;
    }
}
/**
 * this should be used to create all cars which are SMALL
 */
class NanoCarFactory implements CarFactoryInterface
{
    public static function factory($car) 
    {
        switch ($car) {
            case 'petrol':
                $obj = new PetrolCar('India','Nano');
                break;
            case 'diesel':
                $obj = new DieselCar('China','Nano');
                break;
            default:
                throw new Exception("Nano car factory could not create car of types '" . $car . "'", 1000);
        }
        return $obj;
    }
}

try {
    $luxuryCar = LuxuryCarFactory::factory('petrol');
    echo $luxuryCar->getMessage();
    
    $luxuryCar = LuxuryCarFactory::factory('diesel');
    echo $luxuryCar->getMessage();
    
    $nanoCar = NanoCarFactory::factory('petrol');
    echo $nanoCar->getMessage();
    
    $nanoCar = NanoCarFactory::factory('diesel');
    echo $nanoCar->getMessage();
 
} catch(Exception $e) {
    echo $e->getMessage();
}
```

**Abstract factory**


```
<?php

error_reporting(E_ALL);
ini_set('display_errors', 1);

/*
 * CarFactory classes
 */

abstract class AbstractCarFactory {

    abstract function makeDieselCar();

    abstract function makePetrolCar();
}

class TataCarFactory extends AbstractCarFactory {

    private $companyName = "Tata";

    function makeDieselCar() {
        return new TataDieselCar;
    }

    function makePetrolCar() {
        return new TataPetrolCar;
    }

}

class SkodaCarFactory extends AbstractCarFactory {

    private $companyName = "Skoda";

    function makeDieselCar() {
        return new SkodaDieselCar;
    }

    function makePetrolCar() {
        return new SkodaPetrolCar;
    }

}

/*
 *   Car classes
 */
abstract class AbstractCar {

    abstract function getEngine();

    abstract function getCompany();
}

abstract class AbstractPetrolCar extends AbstractCar {

    private $type = "Petrol";

}

class TataPetrolCar extends AbstractPetrolCar {

    private $engine;
    private $company;

    function __construct() {
        $this->engine = '1197 cc';
        $this->company = 'Tata Korea';
    }

    function getEngine() {
        return $this->engine;
    }

    function getCompany() {
        return $this->company;
    }

}

class SkodaPetrolCar extends AbstractPetrolCar {

    private $engine;
    private $company;

    function __construct() {
        $this->engine = '1396 cc';
        $this->company = 'Skoda Korea';
    }

    function getEngine() {
        return $this->engine;
    }

    function getCompany() {
        return $this->company;
    }

}

abstract class AbstractDieselCar extends AbstractCar {

    private $type = "Diesel";

}

class TataDieselCar extends AbstractDieselCar {

    private $engine;
    private $company;
    private static $oddOrEven = 'odd';

    function __construct() {
        //alternate between 2 engins
        if ('odd' == self::$oddOrEven) {
            $this->engine = '2694 cc';
            $this->company = 'Tata India';
            self::$oddOrEven = 'even';
        } else {
            $this->engine = '2500 cc';
            $this->company = 'Tata China';
            self::$oddOrEven = 'odd';
        }
    }

    function getEngine() {
        return $this->engine;
    }

    function getCompany() {
        return $this->company;
    }

}

class SkodaDieselCar extends AbstractDieselCar {

    private $engine;
    private $company;

    function __construct() {
        //alternate randomly between 2 cars
        mt_srand((double) microtime() * 10000000);
        $rand_num = mt_rand(0, 1);

        if (1 > $rand_num) {
            $this->engine = '3694 cc';
            $this->company = 'Skoda India';
        } else {
            $this->engine = '3500 cc';
            $this->company = 'Skoda China';
        }
    }

    function getEngine() {
        return $this->engine;
    }

    function getCompany() {
        return $this->company;
    }

}

//Execution

echo '############# TESTING TataCarFactory ###############'."<br/>";
$carFactoryInstance = new TataCarFactory;
$dieselCarOne = $carFactoryInstance->makeDieselCar();
echo 'first diesel engine: ' . $dieselCarOne->getEngine()."<br/>";
echo 'first diesel company: ' . $dieselCarOne->getCompany()."<br/>";

$dieselCarTwo = $carFactoryInstance->makeDieselCar();
echo 'second diesel Engine: ' . $dieselCarTwo->getEngine()."<br/>";
echo 'second diesel Company: ' . $dieselCarTwo->getCompany()."<br/>";

$petrolCar = $carFactoryInstance->makePetrolCar();
echo 'Petrol Engine: ' . $petrolCar->getEngine()."<br/>";
echo 'Petrol Company: ' . $petrolCar->getCompany()."<br/>";
echo "<br/>";

echo '########## TESTING SkodaCarFactory ###############'."<br/>";
$carFactoryInstance = new SkodaCarFactory;
$dieselCarOne = $carFactoryInstance->makeDieselCar();
echo 'first diesel engine: ' . $dieselCarOne->getEngine()."<br/>";
echo 'first diesel company: ' . $dieselCarOne->getCompany()."<br/>";

$dieselCarTwo = $carFactoryInstance->makeDieselCar();
echo 'second diesel Engine: ' . $dieselCarTwo->getEngine()."<br/>";
echo 'second diesel Company: ' . $dieselCarTwo->getCompany()."<br/>";

$petrolCar = $carFactoryInstance->makePetrolCar();
echo 'Petrol Engine: ' . $petrolCar->getEngine()."<br/>";
echo 'Petrol Company: ' . $petrolCar->getCompany()."<br/>";

/*
OutPut:
############# TESTING TataCarFactory ###############
first diesel engine: 2694 cc
first diesel company: Tata India
second diesel Engine: 2500 cc
second diesel Company: Tata China
Petrol Engine: 1197 cc
Petrol Company: Tata Korea

########## TESTING SkodaCarFactory ###############
first diesel engine: 3694 cc
first diesel company: Skoda India
second diesel Engine: 3694 cc
second diesel Company: Skoda India
Petrol Engine: 1396 cc
Petrol Company: Skoda Korea
*/
?>
```
