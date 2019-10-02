I will write essential test cases for normal table view with some data by using TDD.

**TDD** is a technique in which , we write the test case first and try to make it come true.

Let’s start the test.
Create a view controller by name “KittenViewController” and make it first view controller. 

Create unit test class by name “KittenUniTest”

Create first test and copy below code in that case

     func test001_ControllerHasTableView() {
        
        guard let controller = UIStoryboard(name: "Main", bundle: Bundle(for: KittenViewController.self)).instantiateInitialViewController() as? KittenViewController else {
            return XCTFail("Could not instantiate ViewController from main storyboard")
        }
         controller.loadViewIfNeeded()
          XCTAssertNotNil(controller.tableView, "Controller should have a tableview")
    }


This test case (test001_ControllerHasTableView) is getting viewcontroller from main storyboard and finding is this is present or call guard.

         controller.loadViewIfNeeded()
 This line is very important. load View before using the controller and it will initialize the views of controller and make them usable else may be any issue of using between UI component and controller.

          XCTAssertNotNil(controller.tableView, "Controller should have a tableview")
          
This will fail "Value of type 'KittenViewController' has no member 'tableView'” because there is no variable by name tableview in controller.

To make this true, make Tableview in View controller and make IBOutlet


  <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/>      Now run it, its all green Hooray

Now we have controller that have a table view , Time to write the tableview datasource.

Datasource is responsible to provide and manipulate data in table view write below code for testing of data source.


     func test002_TableViewDataSourceIsKittenDataSource() {
       
         guard let controller = UIStoryboard(name: "Main", bundle: Bundle(for: KittenViewController.self)).instantiateInitialViewController() as? KittenViewController else {
            return XCTFail("Could not instantiate ViewController from main storyboard")
        }
         controller.loadViewIfNeeded()
        XCTAssertTrue(controller.tableView.dataSource is KittenDataSource,
                      "TableView's data source should be a KittenDataSource")
    }


<img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/wrong.png" style="width:4%;float:left"/> Test is fial

It will fail because controller doesn’t have any **“KittenDataSource”** 
For this , first create new Unit test by name “KittenDataSourceTests” and create  first test in that class

     func test001_DataSourceHasKittens() {
        let dataSource = KittenDataSource()
 
        XCTAssertEqual(dataSource.kittens.count, 20,
                       "DataSource should have correct number of kittens")
    }

Run the test and it will fail  because there is no   “KittenDataSource” class. so create this class and just write in KittenDataSource

       class KittenDataSource    { }


Run the test, it will fail again with error “Value of type 'KittenDataSource' has no member 'kittens'” 

Create an array In kittendatasource class 

       var kittens = [Kitten]()

Run the test. Fails again with error “Use of unresolved identifier 'Kitten'; did you mean 'listen'?”

No we don’t want to listen :P so create class “Kitten” and copy below line

       class Kitten { }

Fails again  because  kitten does not have any data and we are checking array of length 20.
Time to add some kitten in data source to verify the test. Kitten have name and isAdoptable property.

 Add below lines in  test case 
 
        for number in 0..<20 {
            let kitten = Kitten(name: "Kitten: \(number)")
            dataSource.kittens.append(kitten)
        }

Run it but it will still fail because kitten does not have name property. Add name property in kitten and initialize that. Open **Kitten.swift** file and copy below lines

     var name: String
    init(name: String) {
        self.name = name
    }


 <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/>      Run the test . It is green now .





## Test number of rows

 Now check whether tableview has the correct number of rows or not. For simplicity, make datasource as public variable and initialize that in after class. Also  copy the above input data lines in set up for data
 And copy below lines for  that

     func test002_NumberOfRows() {
        let tableView = UITableView()
        
        let numberOfRows = dataSource.tableView(tableView, numberOfRowsInSection: 0)
        XCTAssertEqual(numberOfRows, 20,
                       "Number of rows in table should match number of kittens")
    }

Run the unit test , it will fail because “Value of type 'KittenDataSource?' has no member 'tableView’”
To resolve this open data source and inherit datasource class fromNSOBject and implement protocol of tableViewdatasource.
Implement important function of tableViewdatasource

And add these functions in KittenDataSource. Kitendatasource file should be like this

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return kittens.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
          return cell
    }
    
 <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/> Run the Test code and it should be green



## Test the number of displayed rows
 
Write another test case of tableRow. Number of rows to be displayed 

      func test002_NumberOfRows() {
        let tableView = UITableView()
        
        let numberOfRows = dataSource.tableView(tableView, numberOfRowsInSection: 0)
        XCTAssertEqual(numberOfRows, 20,
                       "Number of rows in table should match number of kittens")
    }


 <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/>  It should be passed by using above code

## Test for cell of tableview 

First let's make tableview as global and then copy the below function to check whether label on tableview cell has data or not


     func test003_CellForRow() {
         let cell = dataSource.tableView(tableView, cellForRowAt: IndexPath(row: 0, section: 0))
         XCTAssertEqual(cell.textLabel?.text, "Kitten: 0",
                       "The first cell should display name of first kitten")
    }

Run the test, it will fail because it will not find any cell by name of “Cell” in tableview.

Open the tableView in storyboard and add a cell. Give it identifier. “Cell” and run the code.

It will still fails, because we haven’t register it before using.

Register in starting of cell by this line 

        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "Cell")

Run the code. It will fail because kitten is not showing the name on cell.

Open the class of KittenDataSource and  add  this line in CellForRow 

        cell.textLabel?.text = kittens[indexPath.row].name


<img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/> Run the test . It should be green

## Test for name of kitten

We have tested that kitten should have name that will be set on kitten tableView class. To verify this, make a different unit test by name “KittenTests”
Remove the boilerplate test and add below line in tests

    func test001_KittenHasAName() {
        let kitten = Kitten(name: "Uncle Fester")
        XCTAssertEqual(kitten.name, "Uncle Fester", "Kitten name should be set in initializer")
    }

<img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/>  Run the test and it should work

But there is a problem , we still have one failed case in “KittenUniTest”. That is “XCTAssertTrue failed - TableView's data source should be a KittenDataSource”
 This is because we have tableview but still did not set its datasource.
 
Open storyboard and drag and drop datasource of tableview to controller

Now open kittenviewcontroller and assign datasource to tableview in setter method

    @IBOutlet weak var tableView: UITableView! {
        didSet {
            tableView.dataSource = dataSource
        }
    } 
And initialize kittendatasource on top

        private var dataSource = KittenDataSource()


 <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/>  Run the test it should pass all




## Test for adoption of kitten

Kitten either be adopted or not.. Check by default its not adopted ,Open KittenUniTest controller and write below line for testing

    func test003_KittenIsNotAdoptableByDefault() {
        let kitten = Kitten(name: "Uncle Fester")
        XCTAssertFalse(kitten.isAdoptable,
                       "Kitten should not be adoptable by default")
    }

Run the  test  it will fails on obvious reason. isAdoptable is not found, open kitten class and add in that this property.

    var isAdoptable = false
<img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/> Run test test it should pass

Its better time to global the controller and tableview variable In kitten data test general so that every test case use it.

Copy below Code in **Setup()** of KittenTesrcontroller


  
        guard let vc = UIStoryboard(name: "Main", bundle: Bundle(for: KittenViewController.self)).instantiateInitialViewController() as? KittenViewController else {
            return XCTFail("Could not instantiate ViewController from main storyboard")
        }
        
        controller = vc
        controller.loadViewIfNeeded()
        
        tableView = controller.tableView
        guard let ds = tableView.dataSource as? KittenDataSource else {
            return XCTFail("Controller's table view should have a kitten data source")
        }
        dataSource = ds
        delegate = tableView.delegate



And initialize appropriate variables on top and remove the duplication of code from all test


## Check for table view cell existance

Check whether table view has cells or not. Create another test class for hasCell

     func test004_TableViewHasCells() {
        
        let cell = controller.tableView.dequeueReusableCell(withIdentifier: "Cell")
        XCTAssertNotNil(cell,
                        "TableView should be able to dequeue cell with identifier: 'Cell'")
    }



 <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/>  Run the test , it should be green

## Test disclosure indicator on adopted kitten
If it's adoptable then show the disclosure indicator
Copy the below code for Check if disclosure indicator is on if adoptable

    func test005_KittenCellHasDisclosureIndicatorWhenAdoptable() {
        guard let dataSource = controller.tableView.dataSource as? KittenDataSource else {
            return XCTFail("Controller's table view should have a kitten data source")
        }
        let kitten = Kitten(name: "Adopt Me")
        kitten.isAdoptable = true
        
        dataSource.kittens.append(kitten)
        
        let cell = dataSource.tableView(controller.tableView, cellForRowAt: IndexPath(row: 0, section: 0))
        XCTAssertEqual(cell.accessoryType, .disclosureIndicator,
                       "Cell should have disclosure indicator for adoptable kitten")

Run the code ,I fail and error is “XCTAssertEqual failed: ("UITableViewCellAccessoryType") is not equal to ("UITableViewCellAccessoryType") - Cell should have disclosure indicator for adoptable kitten”

To resolve this ,open the storyboard and set accessory to “Disclosure indicator”

 <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/> Run the test ,it should be green


## Test hide disclosure indicator on non-adopted kitten


If its not adoptable then should not show disclosure indicator


      functest006_KittenCellDoesNotHaveDisclosureIndicatorWhenNotAdoptable() {
           guard let dataSource = controller.tableView.dataSource as?    KittenDataSource else {
            return XCTFail("Controller's table view should have a    kitten data source")
        }
        
        let kitten = Kitten(name: "Cannot Adopt Me")
        dataSource.kittens.append(kitten)
        
        let cell = dataSource.tableView(controller.tableView, cellForRowAt: IndexPath(row: 0, section: 0))
        XCTAssertEqual(cell.accessoryType, .none,
                       "Cell should have no indicator for a non-adoptable kitten")
    }


Run the test but it will fail with error “XCTAssertEqual failed: ("UITableViewCellAccessoryType") is not equal to ("UITableViewCellAccessoryType") - Cell should have no indicator for a non-adoptable kitten”



Because we have added discourse indicator by default but to handle this, write on datasource file

    cell.accessoryType = ( kittens[indexPath.row].isAdoptable) ? .disclosureIndicator : .none
        
Because we have to check whether its on or off

 <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/> Run the test .it will be working now

  
 
## Check for delegate confromences

If kitten has adoptable then it’s clickable else it’s not clickable . For this we will enable delegate of tableview because delegate has functions of “WillRowselect ” 
  
    func test007_TableViewDelegateIsViewController() {
        XCTAssertTrue(controller.tableView.delegate === controller,
                      "Controller should be delegate for the table view")
    }
 

Run the test. It will fail because “XCTAssertTrue failed - Controller should be delegate for the table view” 
To run the code, drag and drop the delegate of tableview
Run the test and run the code. It will work


## Check clickability on non-adoptable kitten


If kitten is not adoptable, cell should be not clickable. Write the below code for this

     func test008_CannotSelectCellWhenNotAdoptable() {
        let kitten = Kitten(name: "Cannot Adopt Me")
        dataSource.kittens.append(kitten)
        
        XCTAssertNil(delegate.tableView?(tableView, willSelectRowAt: IndexPath(row: 0, section: 0)),
                     "Delegate should not allow cell selection for non-adoptable kittens")
    }

 <img classes="fancybox fig-50" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1569961901/blog/tick.png" style="width:4%;float:left"/>  All Is green


## Check clickability on adoptable kitten


If kitten is adoptable, cell should be clickable. Write the below code for this

    func test009_CanSelectCellWhenAdoptable() {
        let kitten = Kitten(name: "Adopt Me")
        kitten.isAdoptable = true
        dataSource.kittens.append(kitten)
        XCTAssertEqual(delegate.tableView?(tableView, willSelectRowAt: IndexPath(row: 0, section: 0)), IndexPath(row: 0, section: 0),
                       "Delegate should allow cell selection for adoptable kittens")
    }

It fails by reason “XCTAssertEqual failed: ("nil") is not equal to ("Optional([0, 0])") - Delegate should allow cell selection for adoptable kittens”

Because we have not made function for selection of tableview. Write below code in kittenViewController

    func tableView(_ tableView: UITableView, willSelectRowAt indexPath: IndexPath) -> IndexPath? {
        return  dataSource.kittens[indexPath.row].isAdoptable ? indexPath : nil
            
    }

And implement in UITableViewDleegate in kitenViewController

Run the code. It should be all green


## Test on Device

To test on device ,write below code in view controller of kittenViewController


     for num in 0..<100 {
            let kitten = Kitten(name: "Kitten\(num)")
            kitten.isAdoptable = arc4random_uniform(50) % 3 == 0
            dataSource.kittens.append(kitten)
        }
        tableView.reloadData()




This is all test driven development

