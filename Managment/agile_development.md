# Agile development

In agile development , we create user stories. User stories are like how users will interact and what will be the process. We use VSTS for this. 
User story has 3 main parts, who ,what , why, like:
As a product manager, I want different translation of my app so that other language person can use it.

User story should complete a user flow, like complete sign up of user or complete login of user. So here should be 2 different user stories, one or login and one for sign up

## User story:

One user story belongs to one feature and we create task for this user story
User story must contain acceptance criteria. Tasks are dependent on this acceptance criteria. A user story will complete if all acceptance criteria are done 

Ideally one user story should complete in one scrum (2 weeks scrum).
User story has difficulty points(how much difficult it is). This Points are in Fibonacci series like 1,2,3,5, 8,13,21
Where 1 is easiest and 21 is hardest
Ideal point for one user story is 5 and 8 
If user story is too much difficult like points are greater than 8, We can break it in other user stories
At a time , only user story should run.

## Development:

Create the tasks of these user stories. Because concepts are clear ,now create the tasks of this user story. Every task should Start with the and it should contains
Title of tasks 
Description of task. Mainly It describe it simple English that why is this necessary

- Progress (To do, in progress, done)
- Difficult level (1,2,3 here 1 - easy , 2 = medium,3 = hard)
- Estimated time 
- Remaining hours
- Spend hours
- Is blocked ( if it blocked by some other task like waiting of design, waiting of accounts etc)
- Task type  like design, research, development etc  
- Relationship of task . Is this task child of any task (user story itself is a task so task should be child of user story), or sibling of any etc

It’s better to write the steps in bullet form that you are doing it.

If task is done , then mention immediate manager that it has been done

## Scrum meeting:

Ideally scrum meeting should take 10 minutes. Team lead or manager will conduct this meeting and every individual will answer the question below:

1. What have u done today . Post the task number with some detail
2. What will you do tomorrow (if in morning, today). Just some detail, task number is not important
3. Is there any blockage, any problem , discuss
4. How far we are from our target like complete the sprint on time

## Sprint:

Sprint is the main part of scrum and agile.  One sprint is consist of 2 weeks and ideally should cover 2 user stories but it can vary.
There should be sprint meeting in morning of sprint Monday. Product manager will create sprint of this 2 weeks and describe what he wants and some description. 
Team lead will create tasks from this sprint.
Basically sprints part of “EPIC” and epic contains long term goal of product. Sprint is derived from there.
 
Sprint has user stories and user stories has tasks. You can view the tasks progress in chart and chart should follow some decline. 
If the developer fails to complete sprint on time (within 2 weeks) , then move user stories in the next sprint. Don’t delay the sprint it should be 2 weeks longer, Choice easiest task in next sprint and try to complete the older one as well. 


## Designing:

Designer pick a user story and create task of lo-fi of design
Lo-fi is like rough design, it could be on page, on paint or design without color
It will approve by product manager and developer can start work on lo-fi
Designer will Start to work on hi-fi design and will get approval from product manager
Designer will provide assets. And other data in one-drive. It’s better that designer create folder by name of user story and upload all assets there. Later developer can pick from there. It will save  time and cost.
In testing mood , designer will test design of app. He will pick the mistakes of fonts, color, alignment etc

## Testing:

Testing is quite different in agile development. Steps are below
There are 2 types of test cases. Adhoc testing. That is done in every sprint and regression testing and complete app testing that is done before app release.
Adhoc testing steps are below

1. Developer will complete user story froths side and commit code and give branch name to tester
2. Tester will download code from branch
3. Tester will create spreadsheet of testing and write all test cases
4. Test cases are driven from user acceptance criteria . For easiness, one acceptance criteria is one test case.
5. Write down the steps to  verify the test case
6. Must test on lates 2 operating system
7. Run the test case  according to steps and write it if it fails or pass
8. If it fails ,

   i. create bug on VSTS
   
   ii -  Assign to developer with production step , screen shot
   
   iii- Developer will resolve the big and commit as. “Done not verified”
   
   Iv- Tester will check it again , and verified the bug

	 
### Complete app testing
Adhoc testing is just for one user stories. Bugs are produced when many features are tested together and apply integration testing.
Bugs process is similar as mentioned above



## Git:

Ever developer will make a branch by name of feature.
Ideally ,one project should contains 3 types branches

1. **development branch:**  It will create for every feature and developer keep push code here
2. **Stage branch:** It's between development and release. It's like developer has released form his side and product manager and other persons are testing from this branch. Test  flight build will create from this branch
3. **Release branch :** Its for release,  Build of App Store and Google play will create and upload from this branch.

#### Naming of branch : 

Naming of branch is more important because it effects on project and its quite difficult to resolve  the merge issue
Must take care of something

1. All letter must be small
2. if name has different words, use hyphen(-) for this, never use space

Development branch name :  development/feature-name
Stage branch name :              stage/feature-name
Release branch name  :         release/feature name


## Database:

There should be 3 databases.

1. Development database :  Could change , erase many times
2. Sage database:  fro family and friends. Recommendation is to not erase but you could do sometimes if really necessary.
3. Production database. Never try to play with it. It’s hazardous. If it’s needed at any cost , must use migration
