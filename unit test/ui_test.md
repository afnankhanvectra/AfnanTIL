## What is UI test


**UI Test** is unit testing for test UI are they working or not. It does not test what’s shaowing on screen and where is it showing. It's just test whether Ui elements are performing perfect like text on label, button click from test and check functionality, view controller change or not.

## How to write simple UI test

Click on 6th icon from left panel and add **“New UI test target”** and name the UI test.
New generated class will create by magical variable “       continueAfterFailure = false”

Because to test the UI is heavy to run so it will finish testing if any test fails 

         XCUIApplication().launch()
 is used to lunch the app 

## Initialize app test :

First lets start with making XCUIApplication as global variable as we will need in every test case. XCUIApplication is like access the application UI in all test.

    var app: XCUIApplication!

Now replace   **XCUIApplication().launch()**
 With below lines 
 
         app = XCUIApplication()
        // UI tests must launch the application that they test. Doing this in setup will make sure it happens for each test method.
        app.launchArguments.append("--uitesting")
        app.launch()

Added “app.launchArguments.append” with key and can capture in Appdelegate’s didFinishLaunchingWithOptions to initialize testing(like create defaut values, empty the database )

if launchOptions(“—uitesting“) {
    configureAppForTesting()
}




## UILabel test:
To test label ,

Set label **access identifier** in storyboard or programmatically
Create test function for this
Label is defined as “static text” in UI testing . To test and verify the text of label , 

    XCTAssertEqual(app.staticTexts.element(matching:.any, identifier: "testingLabel").label, "Test")
Where  testingLabel is access identifier and "Test" is text to test


## Button test:
To check the button click and  its action , we use tap()

         app.buttons["Done"].tap()
Where “Done ” is access identifier
 
Lets test button click and its actions by changing the label text to “Change text”


     XCTAssertEqual(app.staticTexts.element(matching:.any, identifier: "testingLabel").label, "Test")
     app.buttons["ChangetextButton"].tap()
    XCTAssertEqual(app.staticTexts.element(matching:.any, identifier: "testingLabel").label, "Change text")

  app.buttons["ChangetextButton"].tap() will click button aumotally
For more action , follow below blog. for more actions of UI componnets

<https://www.hackingwithswift.com/articles/83/how-to-test-your-user-interface-using-xcode>


## TextFields test:
To use textfield , we first enable textfield by using 

     app.textFields.element.tap()

And to get textfeild use 

        let textField = app.textFields["testingTextField"] // where  testingTextField is accessibility identifier

To write text in textfield 

         textField.typeText("text_user_typed_in")
And to test textfield text

      XCTAssertEqual(textField.value as! String, "text_user_typed_in")



## Controller test
We can not direct check which controller is visible so for this , we change the controller and test any elements from other controller whether it exist or not. Follow below link


<https://stackoverflow.com/questions/33872364/uitesting-xctest-current-viewcontroller-class>



    app.buttons["Secondbutton"].tap() // button to change controller is clicked
         
        wait(for: 1) // wait for 1 second because transaction takes time

        XCTAssert(app.staticTexts["secondText"].exists) // check label on second controller is present or not

## Screenshot via UI testing

Xcode automatically takes screenshots as it runs your UI tests, and automatically deletes them if your tests succeed. But if the tests fails then Xcode will keep the screenshots to help you step through visually and figure out what went wrong.

To take screen if you want , use below code



    let screenshot = app.screenshot()
    let attachment = XCTAttachment(screenshot: screenshot)
    attachment.name = "My Great Screenshot"
    attachment.lifetime = .keepAlways
    add(attachment)
