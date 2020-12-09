## Property wrapper

Property wrapper is new feature of Swift language that was introduced in Swift 5. When dealing with properties that represent some form of state, it’s very common to have some kind of associated logic that gets triggered every time that a value is modified. For example , we want to validate some properties or save automatically in user default or other data storage when any change record in property.

In those kinds of situations, Swift 5’s property wrappers feature can be incredibly useful, as it enables us to attach such behaviors and logic directly to our properties themselves — which often opens up new opportunities for code reuse and generalization. 

A **property wrapper** is essentially a type that wraps a given value in order to attach additional logic to it . It is implemented by either using Class or Struct by annotating it with the **@propertyWrapper** attribute.Besides that, the only real requirement is that each property wrapper type should contain a stored property called wrappedValue, which tells Swift which underlying value that’s being wrapped. 

For example we want to uppercase every character of name .  Then it be implement like this


    @propertyWrapper struct UpperCaseString {
    var wrappedValue: String {
        didSet { wrappedValue = wrappedValue.uppercased() }
    }

    init(wrappedValue: String) {
        self.wrappedValue = wrappedValue.uppercased()
     }
    }

And use this wrapper like this

      struct User {
         @UpperCaseString var firstName: String
         @UpperCaseString var lastName: String
      }


Now whenever we change the value of first name or last name, it will uppercase that string automatically.


#### Custom Value initialization


In  this example , I am increasing 1 number on each call and passing key for user default as parameter
  

     @propertyWrapper
     struct ChangeableValue {
     private var number: Int
    private var keyname : String
     
    init(propertyName : String) {
        self.number = 0
        self.keyname  = propertyName
    }
     var wrappedValue: Int {
        get {
             let value = UserDefaults.standard.integer(forKey: keyname)
            UserDefaults.standard.set(value + 1, forKey: keyname)
             return value
         }
       }
    }


this program is increasing one value after every call . To use this 

    @ChangeableValue(propertyName: "increamentValue") var increamentValue : Int



Where property names  is key for saving

**Note:** , we can’t make property wrapper variable in function (I don’t know the reason) . We have tomato it global and defined on top



