# Decorator pattern 

The decorator pattern dynamically modifies the behavior of a core object without changing its existing class definition. It is a behavioral design pattern. This design pattern aims to aid the core object's behavior without changing the code in the main core object. 

##### Dynamic Modifications
Decorators do not simply modify behavior, but they do so dynamically. In other words, they change how objects work during run time instead of compile time.
 A decorator is a replacement for inheritance when we don't want to make too many classes an inheritance.
 
###  Example
 
  	protocol Transporting {
  		func getSpeed() -> Double
  		func getTraction() -> Double
	}  
 
 We create a transport portocol. Now use it in main core class.
 
 	// Core Component
		final class RaceCar: Transporting {
  			private let speed = 10.0
  			private let traction = 10.0
  
  		func getSpeed() -> Double {
    		return speed
  		}
  
  		func getTraction() -> Double {
    	   return traction
  		}
	 }
	 
Now we want to add another tire to our car and change speed and traction according to the tire. for example, we want to add an offroad track tire.

	class OffRoadTireDecorator: Transporting {
     private let transportable: Transporting
  
     init(transportable: Transporting) {
      self.transportable = transportable
      }
  
      func getSpeed() -> Double {
       return transportable.getSpeed() - 3.0
      }
  
      func getTraction() -> Double {
       return transportable.getTraction() + 3.0
     }
    }
    
  
   OffRoadTireDecorator is a wrapper that adds off road tires to a vehicle, causing a change in stats. It accomplishes this in two steps:
   
It calculates the base traction and speed by calling its wrapped object’s getSpeed() and getTraction() methods.

Once it retrieves the default stats, it augments it accordingly and returns a new stat to the caller.

OffroadTiredecorator just wraps around core object and enhance the functionality of the core object.

It is how to use it.

	let defaultRaceCar = RaceCar()
	defaultRaceCar.getSpeed() // 10
	defaultRaceCar.getTraction() // 10

	// Modify Race Car
	let offRoadRaceCar = OffRoadTireDecorator(transportable: defaultRaceCar)
	offRoadRaceCar.getSpeed() // 7
	offRoadRaceCar.getTraction() // 13
	

### Application

* Core object cannot or should not be modified directly
* Avoid modifying legacy code (or third-party frameworks) that are prone to breakage
* Avoid modifying classes marked final as there may be underlying reasons why it should not be subclassed
* Prevent class explosion


### Difference between Strategy and Decorator pattern

The strategy pattern allows you to change the implementation of something used at runtime.

The decorator pattern allows you augment (or add to) existing functionality with additional functionality at run time.

The key difference is in the change vs augment

With the strategy pattern the consumer is aware that the different options exist, whereas with the decorator pattern the consumer would not be aware of the additional functionality.
