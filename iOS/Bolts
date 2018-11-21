**Bolts** is powerful Pods. Bolts is a collection of low-level libraries designed to make developing mobile apps easier. Bolts contains Task. A task is like java script promises. For iOS they have different pods for objective-C and Swift.

Bolt was developed by ***Facebook***. The first component in Bolts is “tasks”, which make organization of complex asynchronous code more manageable. Tasks return either error or cancelled or result .  Result can be of anytime. Simple syntax for bolts Is below:

* In Swift  (Closur approach)
fetch(object).continueWith(Executor.mainThread) { task in 

  // This closure will be executor on the main application's thread
  
}

And for background thread 

     fetch(object)..continue ({ [weak self] (task) -> Any? in 
     return nil 
     })


*  Objective-C
		
		[[self saveAsync:obj] continueWithBlock:^id(BFTask *task) { 
		if (task.isCancelled) 
		}else if (task.error) { 
		// the save failed.
		} else {
		// the object was saved successfully.
		PFObject *object = task.result;
		}
		return ni
		}];

## Create Task
Bolt is collection of tasks. To create a new task we need **‘TaskCompletionSource’**.which is a consumer end of any Task, which gives you an ability to control whether the task is completed/faulted or cancelled. After you create a TaskCompletionSource, you need to call setResult()/setError()/cancel() to trigger its continuations and change its state.

*  In Swift

	func fetch(object: PFObject) -> Task<PFObject> {
	let taskCompletionSource = TaskCompletionSource<PFObject>()
	object.fetchInBackgroundWithBlock() { (object: PFObject?, error: NSError?) in
    if let error = error {
      taskCompletionSource.setError(error)
    } else if let object = object {
      taskCompletionSource.setResult(object)
    } else {
      taskCompletionSource.cancel()
    }
  }
  return taskCompletionSource.task
}

// Objective-C
	
	
	
	-(BFTask *)successAsync { BFTaskCompletionSource *successful = [BFTaskCompletionSource taskCompletionSource];
	[successful setResult:@"The good result."];
	return successful.task;
	}



We can run multiple tasks in paralleled  (like NSOpeationQueue) and can establish dependency between them

**Parallel Tasks:** Different tasks will run together but with return together. You can start multiple operations at once, and use **taskForCompletionOfAllTasks**: to create a new task that will be marked as completed when all of its input tasks are completed. The new task will be successful only if all of the passed-in tasks succeed. Performing operations in parallel will be faster than doing them serially, but may consume more system resources and bandwidth.

* Swift


        let query = PFQuery(className: "Comments")find(query).continueWithTask { task in
         var tasks: [Task<PFObject>] = []
          task.result?.forEach { comment in
        tasks.append(self.deleteComment(comment))
         }
        return Task.whenAll(tasks)
        }.continueOnSuccessWith { task in
         // All comments were deleted
         }


* Objective-C

        PFQuery *query = [PFQuery queryWithClassName:@"Comments"];
        [query whereKey:@"post" equalTo:@123];
        [[[self findAsync:query] continueWithBlock:^id(BFTask *results) {
        // Collect one task for each delete into an array.
        NSMutableArray *tasks = [NSMutableArray array];
         for (PFObject *result in results) {
        // Start this delete immediately and add its task to the list.
         [tasks addObject:[self deleteAsync:result]];
        } 
        // Return a new task that will be marked as completed when all of the deletes are
         // finished.
        return [BFTask taskForCompletionOfAllTasks:tasks];
        }] continueWithBlock:^id(BFTask *task) {
        // Every comment was deleted.
         return nil;
        }];


***Benefit over NSOperation***

* BFTask takes care of managing dependencies for you. Unlike using NSOperation for dependency management, you don't have to declare all dependencies before starting a BFTask. For example, imagine you need to save a set of objects and each one may or may not require saving child objects. With an NSOperation, you would normally have to create operations for each of the child saves ahead of time. But you don't always know before you start the work whether that's going to be necessary. That can make managing dependencies with NSOperation very painful. Even in the best case, you have to create your dependencies before the operations that depend on them, which results in code that appears in a different order than it executes. With BFTask, you can decide during your operation's work whether there will be subtasks and return the other task in just those cases.
* BFTasks release their dependencies. NSOperation strongly retains its dependencies, so if you have a queue of ordered operations and sequence them using dependencies, you have a leak, because every operation gets retained forever. BFTasks release their callbacks as soon as they are run, so everything cleans up after itself. This can reduce memory use, and simplify memory management.
* BFTasks keep track of the state of finished tasks: It tracks whether there was a returned value, the task was cancelled, or if an error occurred. It also has convenience methods for propagating errors. With NSOperation, you have to build all of this stuff yourself.
* BFTasks don't depend on any particular threading model. So it's easy to have some tasks perform their work with an operation queue, while others perform work using blocks with GCD. These tasks can depend on each other seamlessly.
* Performing several tasks in a row will not create nested "pyramid" code as you would get when using only callbacks.
* BFTasks are fully composable, allowing you to perform branching, parallelism, and complex error handling, without the spaghetti code of having many named callbacks.
* You can arrange task-based code in the order that it executes, rather than having to split your logic across scattered callback functions.

 
