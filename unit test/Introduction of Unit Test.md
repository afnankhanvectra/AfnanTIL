### What is unit test

Unit test is development side testing that is done by developer to test the different units(components). Of app independently. 
Basic requirements for unit testing is Components should be ideally independent. Its like we test car’s part in different component like check the oil, check the gear system and headlights. If everything is working perfect, car should run well.
 We isolate functions in unit test and test independent.  Although it’s not ideally possible, we use. “Dependency injection” for this sometimes.

### Why unit test

Unit test is necessary for many reasons. 

   **1- Developer side testing :**
Unit testing is more common testing from developers and developer get confidence about app. If every component is running without bug then whole  product should run perfect.

  **2- Fast :**  It is falser because it helps to find the bug faster. You could easily find defects in failed unit cases.
  
  **3- TDD :**  Test driven development is test the app before development.  Verify the unit test before code development and it will give clear idea to developer that what should he writes. 
   
  **4- Confident by adding new feature :**  Because app is decided into components, adding new feature should not effect on previous added feature. We can test this modularity easily in independent system.


### Disadvantages of unit test


**1- More code :** Most common quote is “adding a new line in code is adding a bug in code“. We ad multiple lines in code and almost we have to do double work. Write code for development and write code for testing. 

 **2- Change in unit testing :** A backend service  makes some changing in return data, we have to do on 2 places. If we are using mocking in unit testing, it could be thrice. So change the existing feature, return output, calculation effects on unit testing that is time taking. 
 
**3- difficult to isolate :** Ideal unit test should be independent from each other but its really difficult to do. Although mocking, dependency injection works sometimes, but it is real difficulty.


### Example of unit test

**1-** Open the Xcode and click on 6th option from left panel 

![Image 1](https://res.cloudinary.com/dlikzl3m2/image/upload/v1568915466/unittest/image_1.png)

 
**2-** Click on “+” on left  bottom. And select “New unit test target”

**3-** create the unit test. It will make a folder by unit test name with unittest.swit file  and info.plist

**4-** Open UnitTestTests.swift file . It will have 4 basic functions. For the time being we will focus on two function
setUp() and teardown() remove other function

**setUp()** in start of every test. When test start. We initialize data here

**tearDown()** is called every time when test end




### Make function 

**1-** Import project files in test unit. Just write blow line
@testable import projectname
It will import complete project automatic and you can access any class for test

**2-** Create the variable of viewController or swift file that are being tested and name it. “sut (system under test)”

       var sut : ViewController?
    
**3-** If it is view controller than initialise like below

       sut = UIStoryboard(name: "Main", bundle: nil)
            .instantiateInitialViewController() as? ViewController
            
In set up and use super.setup() before that. Because this functions call each and every time for unit test,  this variable will initialize every time

    override func setUp() {
        super.setUp()
        sut = UIStoryboard(name: "Main", bundle: nil)
            .instantiateInitialViewController() as? ViewController
     }

**4-**  Dealloc this variable in “teardown” because its called every time for deallocation

    override func tearDown() {
        sut = nil
        super.tearDown()
     }

Now set up is ready for testing. Check it by using “**CTRL+U**”. For testing. If everything works fine, diamond in. Gutter should be green

**5-** Crate testing function. Name must start from test and Xcode will  identify that it is testing function
Like testGuess(){
}

**6-** Write some code in that and test whether it is working.if diamond is green then it is working
Testing function has 3 major parts

**1. Given :** Give the testing data like variables , urls 

**2.  When :** Execute the code being tested. Call the function of controller, API calls or other work

**3. Then :**  Check the result. Use different XCAssert to check the result and verify the test

         

### Debugging the test

  For debugging the test , 
  
**1-** click on second last option in left panel and click on left bottom button “+”
**2-** Click on plus button and select “Test Failure breakpoint”

![Image 1](https://res.cloudinary.com/dlikzl3m2/image/upload/v1568915466/unittest/image_2.png)

If test fails , it will give more detail in  bottom



### Unit test for asynchronous  operation

We have tested unit test on simple operation ,lets taste in async code like API cal. Its  very necessary for  environment like test the API calls whether are they working or not.

I use this test personally to check every morning whether all API’s are working correctly or not and returning  correct data.

To test that asynchronous operations behave as expected, you create one or more expectations within your test, and then fulfill those expectations when the asynchronous operation completes successfully. 
Your test method waits until all expectations are fulfilled or a specified timeout expires.

 Lets start this testing
In asynchronous operation , usually we use dependency injection

**1-** import the project and create SUT as we have done in simple 
unit testing, initialize in set up and dealloc in teardown

**2-** create the testing function (must start from “**test**” word)

**3-** On given , create the url 
 let url =
            URL(string: "https://itunes.apple.com/search?media=music&entity=song&term=abba")

On when , check expectation by Ising promise. We use expectation

        let promise = expectation(description: "Status code: 200")

in when call the api for service and get the result
Every expectation must fulfill

     let dataTask = sut.dataTask(with: url!) { data, response, error in
            // then
            if let error = error {
                XCTFail("Error: \(error.localizedDescription)")
                return
            } else if let statusCode = (response as? HTTPURLResponse)?.statusCode {
                if statusCode == 200 {
                    // 2
                    promise.fulfill()
                } else {
                    XCTFail("Status code: \(statusCode)")
                }
            }
        }
        dataTask.resume()
        // 3
        wait(for: [promise], timeout: 5)
    }

                XCTFail("Error: \(error.localizedDescription)")
          }
 Will fail if result does not show up according to requirements

This code has 3 major components

**1. expectation(description:):** Returns an XCTestExpectation object, stored in promise. The description parameter describes what you expect to happen.

**2. promise.fulfill():** Call this in the success condition closure of the asynchronous method’s completion handler to flag that the expectation has been met.

**3. wait(for:timeout:):** Keeps the test running until all expectations are fulfilled, or the timeout interval ends, whichever happens first.





We can improve this by using block of failur and success handler if timeout takes more time. 

 



 
