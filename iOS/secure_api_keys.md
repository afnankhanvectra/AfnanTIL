## Best Way to save API keys


To save API key in code is risky. Anyone can decompile the code and take that out. To secure the API key we use configure file.


- 1-  Create configure file from new file of Xcode. Be careful don’t click project when making configure file 


<img classes="fancybox fig-10" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1607983501/blog/configure_api_image_1.png" style="width:10%;float:center"
title="Configure image 1"/>

- 2-  Write key in config file like

       SECRET_KEY = "ABCD"

- 3-  Set file in app project . Click on project , go in info  and select configurations 

<img classes="fancybox fig-10" src="https://res.cloudinary.com/dlikzl3m2/image/upload/v1627941983/blog/configure_api_iimage_2.png" style="width:10%;float:center"
title="Configure image 2"/>

- 4- INow add the Secret Key to your info.plist. The SECRET_KEY in our Config.xcconfig acts as a variable which can be accessed by $(SECRET_KEY) on the info.plist.
Like **$(SECRET_KEY)**

- 5- Now use API in code

         Bundle.main.object(forInfoDictionaryKey: "SECRET") as? String




