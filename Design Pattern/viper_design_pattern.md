## What Is Viper

Viper Design pattern is part of design architecture design patterns.It  is application of  clean architecture in iOS.It is based on “Single Responsibility Principle”. The word VIPER is a backronym for View, Interactor, Presenter, Entity, and Routing. Clean Architecture divides an app’s logical structure into distinct layers of responsibility. This makes it easier to isolate dependencies (e.g. your database) and to test the interactions at the boundaries between layers:
	Insert image of VIPER


### Part of VIPER

* **View**: The responsibility of the view is to send the user actions to the presenter and shows whatever the presenter tells it.
* **Interactor:** This is the backbone of an application as it contains the business logic.
* **Presenter:** Its responsibility is to get the data from the interactor on user actions and after getting data from the interactor, it sends it to the view to show it. It also asks the router/wireframe for navigation.
* **Entity:** It contains basic model objects used by the Interactor.
* **Router:** It has all navigation logic for describing which screens are to be shown when. It is normally written as a wireframe.
We use another class  by name “Protocols” where we put all protocol needed for communications

### Why We use Viper

Viper has many benefits . Because it supports “ Single Responsibility principle ” and clean architecture like

#### Testability
The loosely coupled modules ensure that testing is carried out easily and separately. 

#### Easy to iterate on
The codebase is familiar to most developers. Files are smaller, logic is clearer and the overall stability and flexibility are higher.

VIPER is a step forward to ensure that the SOLID code principles can be further applied to iOS apps as well. The beauty of this architecture is that it stays true to the theory behind its implementation.

## Example of Viper

Create project by using Single View application
Create required classes for use

1. Protocol class  - It will contain all protocol for communication between components
2. Entity class - It wil contains basic model object class
3. View Controller Class -  this class is responsible for handling UI components
4. Interactor -  This class is most important . It handles all business logic of app. 
5. Presentor - this class is resposnible for communication/mediator between Interactor and view  
6. Router -  Router class is responsible for handling screen shown and basic initialization of components


Open Project protocol class and initialize 5 protocol we use for communication
   
     protocol PresentorToViewProtocol : class {
     }

    protocol InteractorToPresentorProtocol  : class {
    }

    protocol PresentorToInteractorProtocol : class {
    }
    
     protocol ViewToPresentroProtocol : class {
     }
     protocol PresentorToRouteProtocol : class {

    }


Now time to create our simple view. I am using a tableView and Button to fetch some data from free online Json Placeholder Site .

(See project for View)



Time to implement functionality for fetching data from server. I am using [Json Place holder api](https://jsonplaceholder.typicode.com/todos). Free open source rest api 

Make entity modeler fetching data.  This API provides list of TODO list . So make entity file  by “TodoObject”  .for fetching data ,  we will use **Alamofire**  .install the pod of Alamofire by  

     pod 'Alamofire' 
     
  
  We need to get data by Rest API on “Get data button” click. Make method in  “**ViewToPresentroProtocol**”  to ask from presenter  about new Todo List.
  In  ViewToPresentroProtocol class write

        func getTodoList()

Open the presenter class that implements “**ViewToPresentroProtocol**” and implement this function. 
Now presentor will call Interactor for business logic/ call rest API. Because presentor implements  ViewToPresentroProtocol  so make interactor object in protocol and use here. Write below line in  **ViewToPresentroProtocol**

    var interactor: PresentorToInteractorProtocol?

Implement business logic in Interactor. Interaction will implement “**PresentorToInteractorProtocol**” Make function in that protocol so that presentor can talk with it.
In **PresentorToInteractorProtocol** protocol make 

    func fetchTodoList() 
And implement in Interactor

After fetching Todo list, interactor will send update to presentor , So get object of presentor protcol. In  ***PresentorToInteractorProtocol** make presentor protocol object for delegate

    var presenter: InteractorToPresentorProtocol? {get set}

Now implement business logic of getting Todo List from server. I am using Alamofire. For code , After getting result we need to send back to presentor to inform that we have got result. Make 2 delegate function in “**InteractorToPresentorProtocol**” 

      func todoListFetched(todoList: [TodoObject])
      func todoListFetchedFailed()
Interactor code should look like this


func fetchTodoList() {
         
        Alamofire.request(URL).response { response in
            if(response.response?.statusCode == 200){
                guard let data = response.data else { return }
                do {
                    let decoder = JSONDecoder()
                    let newsResponse = try decoder.decode([TodoObject].self, from: data)
                     
                    self.presenter?.todoListFetched(todoList: newsResponse)
                } catch let error {
                    print(error)
                }
            }
            else {
                self.presenter?.todoListFetchedFailed()
            }
        }
        
    }
    


Now implement Interactor to presentor protocol in Presentor class and implement that methods to get response. 
Meanwhile in getTodoList() of presentor class , call interactor?.fetchTodoList() for getting data from Interactor

Everything is looking fine so far without any visualtion. Now time to initialize router that will handle the routing and initialing


In project router implement “**PresentorToRouteProtocol**”
Create module for initialization in this protocol. It will be static because router is shared among all components

    static func createModule(forViewController view: UIViewController )  

Initialize all components in this function . It will look like this

static func createModule(forViewController view: UIViewController) {
        
        let presenter: ViewToPresentroProtocol & InteractorToPresentorProtocol = ProjectPresentor()
                   let interactor: PresentorToInteractorProtocol = ProjectInteractor()
                   let router: PresentorToRouteProtocol = ProjectRouter()
                   
                   (view as! ViewController).presenter = presenter
               
               presenter.view = view as? PresentorToViewProtocol
                   presenter.router = router
                   presenter.interactor = interactor
                   interactor.presenter = presenter
             }
    
    
If  (view as! ViewController).presenter = presenter makes error .. initialize  presentor in ViewController class

    var presenter: ViewToPresentroProtocol?


Now create module in **ViewDidLoad** of controller


    override func viewDidLoad() {
        super.viewDidLoad()
        ProjectRouter.createModule(forViewController: self)
     }


It  will initialize all components for eachother.
On GetData button click, call presentor for todo list

After debugging , we have got result from API. And passed too presentor . Now pass result from presenter to View . Create these 2 functions in  “**PresentorToViewProtocol**” for handling results.

    func showTodoList(todoList: [TodoObject])
    func showError()


 In presentor , where implementing the **InteractorToPresentorProtocol**, send back the fetched result to view
 
    func todoListFetched(todoList: [TodoObject]) {
        view?.showTodoList(todoList: todoList)
    }
    
    func todoListFetchedFailed() {
        view?.showError()
    }


View controller will implement “**PresentorToViewProtocol**” . Add functionality in delegates method like show error pop up on error

    extension ViewController :  PresentorToViewProtocol {
    func showTodoList(todoList: [TodoObject]) {
         
    }
    
    func showError() {
         
        let alert = UIAlertController(title: "Alert", message: "Problem Fetching Todo list", preferredStyle: UIAlertController.Style.alert)
        alert.addAction(UIAlertAction(title: "Okay", style: UIAlertAction.Style.default, handler: nil))
        self.present(alert, animated: true, completion: nil)
        
       }
     }


Get the result in **func showTodoList(todoList: [TodoObject])**  and show on tableView


Done! . You can download [demo project](https://github.com/afnankhanvectra/Viper-Arcitecture)  for help
