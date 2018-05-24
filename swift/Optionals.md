# Optional

## Introduction

**Optionals** say either "there is a value, and it equals x" or "there isn't a value at all". We use optional when we are not confirm about values. To declare optional, swift use Question mark “?”. Like
var optionalValue: String?

This declaration is equal to nil .which means no value. To use optional we check for nil. like  

	if  optionalValue ==  nil {  
	 print(“Nil value”)  
	} else {  
 	print(optionalValue!)  
	}
	
## Optional Chaining
Optional chaining is a process for querying and calling properties, methods, and subscripts on an optional that might currently be nil . If the optional contains a value, the property, method, or subscript call succeeds; if the optional is nil , the property, method, or subscript call returns nil. 
  
 	eg: let myTable = MySchool?.getRoom()?.table?

## Optional Binding
We are going to take optional value and we are going to bind it non optional constant. We used If let structure or Guard statement.   

	eg: guard let myTable = MySchool?.getRoom()?.table? else { return }
	
## Forced Unwrapping
When we defined a variable as optional, then to get the value from this variable, we will have to unwrap it. This just means putting an exclamation mark at the end of the variable.  It will crash if optional value is nil.  

	eg: let myTable = (MySchool?.getRoom()?.table)! 
Above code will crash if any of MySchool, getRoom or table is nil

## Automatic Unwrapping
you can declare optional variable using exclamation mark instead of a question mark. It will unwrap automatically and you do not need to unwrap again by using exclamation at the end of variable.    

	var myFriendName : String!
	myFriendName  = “Swift”
	if  myFriendName != nil { 
	print (myFriendName)
	} else {
	print(“No Friend found . forever alone”)
	} 

To finish, here's a poem from 1899 about optionals:  

**Yesterday upon the stair  
 I met a man who wasn’t there  
   He wasn’t there again today  
   I wish, I wish he’d go away**   
<https://en.wikipedia.org/wiki/Antigonish_%28poem%29>

## Links
<https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html>  
<https://www.youtube.com/watch?v=i_UXRcydibQ>>  
<https://www.tutorialspoint.com/swift/swift_optionals.htm>
