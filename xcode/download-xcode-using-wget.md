# Download Xcode by wget

I was trying to download Xcode via direct link of apple using Google chrome.  But it was failing once and again and showing below error after some downloads.

![Xcode forbidden ](https://res.cloudinary.com/dlikzl3m2/image/upload/v1569423725/install_xcode/image_1.png)
 

This is typical google chrome issue in large files and unfortunately Xcode.xip is huge file (about 7.1 GB) and takes a lot of time. I wasted my hours on this silly issue.

I thought to use another solution for this problem. If you are suffering from same issue, use below solution and it works. 



### 1- Install the home-brew
Open Terminal
Run 

      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


#### 2- install wget
Run below command in terminal to install wget

     brew install wget

### 3- Install and get cookies.txt from google chrome
**Cookies.txt** is an extension of google chrome that save the download link of page in .txt format. Remember it provides different text on every link
Install the cookies extension from below link

    https://chrome.google.com/webstore/detail/cookiestxt/njabckikapfpffapmjgojcnbfjonfjfg?hl=en

Click on cookies extension icon and download the txt file in downloads


### 4- Get the download link of  Xcode
Open the developer site from below link

    https://developer.apple.com/download/more/

Or get the download link of Xcode from below link

    https://stackoverflow.com/questions/10335747/how-to-download-xcode-dmg-or-xip-file

### 5- Update the .txt file with download link
Update the file by using below.(its for Xcode 11 beta ,you can change according to your Xcode download link)

     1) wget -x --load-cookies cookies.txt " https://download.developer.apple.com/Developer_Tools/Xcode_11.1_GM_Seed/Xcode_11.1_GM_Seed.xip"
     
     2) curl --cookie cookies.txt " https://download.developer.apple.com/Developer_Tools/Xcode_11.1_GM_Seed/Xcode_11.1_GM_Seed.xip"
   
    3) aria2c --load-cookies cookies.txt " https://download.developer.apple.com/Developer_Tools/Xcode_11.1_GM_Seed/Xcode_11.1_GM_Seed.xip"


### 6- Run wget file from terminal
Open terminal and navigate to download

Run below command for Xcode 11 or change according to your link

     wget --load-cookies=cookies.txt -c https://download.developer.apple.com/Developer_Tools/Xcode_11.1_GM_Seed/Xcode_11.1_GM_Seed.xip
