## Factory Design Pattern 


### Introduction 

**Factory design pattern** is most common creational pattern.
In Factory pattern, we create object without exposing the creation logic to the client and refer to newly created object using a common interface.

#### When we use

Factory pattern can come in handy when you want to reduce the dependency of a class on other classes. On the other hand, it encapsulates the object creation process and users can simply pass in parameters to a generic factory class without knowing how the objects are actually being created. Also it gives you a clean code.

#### How to use 

For factory pattern we need 3 components.

 1. Base class  like shape In swift , we use protocol to implement factory design pattern. 	
2. subclasses of base  like triangle , square, we can make class of protocol
3. Decision making class where we check parameter and take decision to create appropriate object of base class from child class and return to caller

#### Example  of Factory pattern
For creation design pattern:

First create the base  protocol for general definition of class . 

    //MARK: - URL Body Protocol
     protocol URLBodyProtocol : class {
        func getSyncDataInJson(fromDataObject dataObject : Any) -> JSON

     }

Make subclasses of base class.

 **GPS Body paramter sub class**
 
    class GPSBodyParameter : URLBodyProtocol {
        func getSyncDataInJson(fromDataObject dataObject: Any) -> JSON {
         // Custom implementation
       }
     }

 **Health Form paramter sub class**

     class HealthFormBodyParameter : URLBodyProtocol {
        func getSyncDataInJson(fromDataObject dataObject: Any) -> JSON {
     // Custom implementation
   
        }
    }

 **Safety Form paramter sub class**

     class SafetyFormBodyParameter : URLBodyProtocol {
        func getSyncDataInJson(fromDataObject dataObject: Any) -> JSON {
     // Custom implementation

       }
    }


**Make choice enum**

      public enum syncDataType: String {
    
    case gpsMobileLocation = "Mobile GPS Location Data"
    case healthForm = "Mobile Health Form Submissions"
    case safetyForm = "Mobile Safety Form Submissions"
     
}

Now the Decesion making class

    class URLBodyHandler {
    
      static func  getBodyParamter(ofType syncType : String) -> String {
        
        let  urlBodyProtocol : URLBodyProtocol?
        
        if syncType == syncDataType.gpsMobileLocation.rawValue {
            urlBodyProtocol = GPSBodyParameter()
        } else if syncType == syncDataType.healthForm.rawValue {
            urlBodyProtocol = HealthFormBodyParameter()
        } else if syncType == syncDataType.safetyForm.rawValue {
            urlBodyProtocol = SafetyFormBodyParameter()
        } 
            
        }
        // let gpsBodyParameter : GPSBodyParameter = GPSBodyParameter()
        return urlBodyProtocol!.getBodyString(ofType: syncType, fromDataObject: syncDataObject)
        
       }
    }


And to call this function ti get dynamic subclass at run time

     let gpsformString =  URLBodyHandler.getBodyParamter(ofType: syncDataType.gpsMobileLocation.rawValue)



