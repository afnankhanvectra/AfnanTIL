## Create Cocoapods



To create Cocoapods we need 4 major steps. We can include Unit test as 5th step.


1. Set up Xcode project and necessary targets
2. Link to Github
3. Implement the pod
4. Publish the pod


#### Set up Xcode project and necessary targets

Before creating pod, must check whether same name pod is exist on 
[Cocoapods.org](https://cocoapods.org/). Newly created pod name must be unique.  After deciding the unique name , we can go ahead. 


##### Create a Cocoa Touch Framework Xcode project



Under the hood, a CocoaPods library is a Cocoa Touch framework library. Launch Xcode and create a new project, choose Cocoa Touch Framework .



(Attach photo)



And give the name of cocoa touch library. It should be unique .. for my project I am using **“MAKTestPod”**



After creatingg your project should look like this 

(Attach photo 2)




##### Prepare the targets



We have created cocoa touch library for work. Now create application to test . Select MAKTestPod project from the project navigator, then choose File > New > Target…, select Single View App as the template and click on Next . Enter the **“MAKTestPodExample”** and click next.



##### Link to Github

 

Go to Github and create a new project called **MAKTestPod** , keep other settings as default and click on Create repository , an empty Github repository is now created.





Now copy the url in SourceTree and download project.Copy the old created project in that folder and must delete .git file of inner folder. Now check the source tree, there should be some files to upload . Upload them. Must upload the branch to master.

Now check the Github . It should contain the project. 

Create read me with some dummy data. Then create License file with MIT License. You can copy from other project. Now we have link to github Time to go ahead 



### Implement the pod

As we mentioned earlier, all the implementation is going to happen in the MAKTestPod target. In the project navigator, right click on the MAKTestPod target and select New File... Create some demo file here and some demo code. I am writing below code in that file (MathCalculation.swift) > Must checking while creating file that it is in  MAKTestPod.  Not in example project 


 

    public  class MathCalculation {
     let name = "MathCalculation"
       public init() {
     }

      open func add(a: Int, b: Int) -> Int {
         return a + b
     }
  
    open func sub(a: Int, b: Int) -> Int {
         return a - b
      }
     }

Now build the project . It should build. Now use the newly created file in example code.

Import cocoa touch project in View controller file of example project



     import MAKTestPod



In View controller’s ViewDidload



    let math = MathCalculation.init()
    let number  = math.add(a:12 , b:20)
     print("number is \(number)")
 
 
### Publish the pod

Now we have worked on project. Time of last and most important step; publish the pod


First create pod Spec file   that defines our pod, e.g., where to find the source. Drag the project in terminal and write below command


     pod spec create MAKTestPod
 
Edit the **MAKTestPod.podspec** file:



Edit the summary . Should write something about app

Change the description by tag of “spec.description “ Should different from summary

Change the spec.homepage  . 

     spec.homepage     = "https://github.com/afnankhanvectra/MAKTestPod

Change the license. Add the  file License where spec file is presented. And reference that 

      spec.license= { :type => "MIT", :file => "LICENSE" }
  
Change the spec author 

     spec.author = { "afnan khan" => "afnan.khan@irsavideo.com" }



Include deployment target and platform details



    spec.platform     = :ios
    # spec.platform     = :ios, "5.0"
    spec.ios.deployment_target = "13.3"

    spec.swift_version = "5.0"
    
   Include the source file path to download the pod It is most necessary line
   
     spec.source        = { :git => "https://github.com/afnankhanvectra/MAKTestPod.git", :tag => "#{spec.version}" }





Change the source_files and exclude_files Classes name

    spec.source_files  = "MAKPodTest/**/*.{h,m,swift}"
    spec.exclude_files = "Classes/Exclude"


 

Check if MAKTestPod.podspec is correct, fix issues if there is any Run below command for validationMAKTestPod

    > pod lib lint



If it pass validation “MAKTestPod passed validation.” message will come .

Now push the code on Github
  
Start to distribute our library
 
Tag the correct version

    git tag 0.0.1
    git push origin 0.0.1

 Then push it!
  
    pod trunk push



**Congratulations!** You just published the CocoaPods library to the open-source.


HelpFul links

<https://medium.com/flawless-app-stories/create-your-own-cocoapods-library-da589d5cd270)>
