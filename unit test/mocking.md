Mocking the object.

There are two types of mocking.

1. Partial mocking
2. Full mocking



Partial mocking is where you take a class and ask it to behave as normal, except you want to override certain functionality. This is useful for unit testing services who communicate with other parts of your application. By overriding the behaviour that would call the other part of your application you can test your service in isolation.
Another example would be when a component would communicate with a database driver. By mocking the part that would communicate with the driver, you can test that part of the application without having to have a database.

Full mocking is the change the behavior of class
 Partial mocking is done by mock some function while full mocking is done by using “protocol”(abstract).
 Below is the best site for this


See unit test project for unit testing


http://iosunittesting.com/snippets/
