## Injection III

In processing to develop the application , we need to run the app again and again . it takes a long time , sometimes we are testing a function in which we need to just change a value to notice the result. To make this process more easy and make an app where we need not to run again and again, we use injectionIII.

To use the tol ,first download the “InjectionIII” from mac store from 
[App Store Injection ](https://itunes.apple.com/app/injectioniii/id1380446739?mt=12)


Install the application in mac and run it . Injection tool should show in tool bar.
Now in the project , go into the build setting . search “Other linker flags” , in debug of “Other linker flags” click + sign and write “-Xlinker -interposable”

 add following to your application delegate's applicationDidFinishLaunching:

     #if DEBUG
	Bundle(path: "/Applications/InjectionIII.app/Contents/Resources/iOSInjection.bundle")?.load()
    #endif


Now run the app , it will ask for the project directory , Navigate to the project directory which contains the ".xcodeproj” file.
Now it's connected to injection. If it fails to connect, you can reconnect by clicking on the toolbar and connecting from there.

In classes write below function 

      @objc func injected() {
     /// any work you want to check on save , could be any function liek below
     changeValue()
       }
 
     func changeValue(){
        testngLabel.text = "113X"
        }
 
 
**injected()** is called every time you save the file and it will call the code inside it. 
Now run the app , change the value  of text in changeValue() 
and save files. 


## Links
<https://github.com/johnno1962/InjectionIII>  
