## We is test driven development(TDD)?

In test driven development , developer write the test cases first that fails then try to make it pass by writing some code. This avoids duplication of code. According to website , test driven development definition is “TDD can be defined as a programming practice that instructs developers to write new code only if an automated test has failed.TDD means “Test Driven Development”. The primary goal of TDD is to make the code clearer, simple and bug-free.”

 In TDD approach, first, the test is developed which specifies and validates what the code will do. Then write the code. 
Because we write tests before real code, sometimes its called “Test First development”.
 



## How to perform TDD Test
Following steps define how to perform TDD test,

1. Add a test.
2. Run all tests and see if any new test fails.
3. Write some code.
4. Run tests and Refactor code.
5. Repeat.

Must remember some points about TDD
 
1. It is neither about design nor about testing.
2. It does not means “write a lot of tests”
 
I will add complete example of TDD in next series

Benefits of Test driven development


##### 1- Early bug notification.

User can find the bug earlier in early test by using automated test. Because we write test first.

##### 2- Better Designed, cleaner and more extensible code.

Because we write short test cases to test and we try to isolate system. so it makes clean design and extensible code.It helps to understand how the code will be used and how it interacts with other modules.

##### 3- Confidence to Refactor

Because we have made test cases before writing code and it must have to be passed so it make more confidence. If you refactor code, there can be possibilities of breaks in the code. So having a set of automated tests you can fix those breaks before release. Proper warning will be given if breaks found when automated tests are used.



##### 4-Good for Developers
Though developers have to spend more time in writing TDD test cases, it takes a lot less time for debugging and developing new features. You will write cleaner, less complicated code.

