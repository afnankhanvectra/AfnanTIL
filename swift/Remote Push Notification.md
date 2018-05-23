# Remote Push Notification

## Introduction

Remote notification is secure and robust feature to reach user  and perform small task even when users aren’t actively using an app!. Push notification have become more and more powerful and customize. after iOS 10, developer can send video, image, audio and other media file, customize push display, provide action the user can take without opening app.

## How push notification works
Remote notification works with help of Apple Push Notification Service (APNs). On initial launch of your app , the system automatically establish a encrypted connection between your app and APNs. The other half is establish a connection between third party server and APNs. Your web server sends message (device token + payload) to Apple push notification service (APNS) , then APNS routes this message to device whose device token specified in notification. 

![APNS Diagram logo](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Art/remote_notif_simple_2x.png)

## Steps of Push notification
1- **Make push certificate (development and distribution ) from apple portal**.  
	
* Open <https://developer.apple.com>
* Login via account
* Click on Certificates, identifier and profiles
* Click on App IDs in identifiers tab
* Search app from app IDs and open
* Click on edit
* Enable push notification and choose create certificate for development
* For creation certificate, you need to create **CSR** file first. You will find steps to create CSR file there
* Now CSR file for certificates
* Download Development certificate
* repeat that steps for distribution certificate

2- **Install downloaded certificates in keychain by double click**. Make sure in keychain that both certificates have been installed. Development certificates will be named as “Apple development iOS Push services <Bundle id name>”. and distribution as  “Apple Push services <Bundle id name” Don’t enter any password

3-  **Export p12 Files from Certificate. Right Click on installed development certificate** of app  and select Export. Install on desktop file name “aps_development”. Export distribution certificate on desktop and name as “aps”.

4- Make **P12** File
* open desktop in terminal 
* Run command “**openssl pkcs12 -in aps_development.p12 -out aps_development.pem -nodes -clcerts**“ 
* Don’t input any password  and enter
* Run command “**openssl pkcs12 -in aps.p12 -out aps.pem -nodes -clcerts**”  and again don’t input any password
* Run command “**openssl s_client -connect gateway.sandbox.push.apple.com:2195 -cert aps_development.pem -key aps_development.pem**”
* if terminal is letting you write, it means you are on right path else follow above steps
* Run command “**openssl s_client -connect gateway.push.apple.com:2195 -cert aps.pem -key aps.pem**” 
* Again if you can type , you are good 

5-**Upload p12 files on Web server**. In my case i am using one signal.

6- **Enable push notification service in Xcode**. Click on project-> capabilities and enable push service. Also enable background modes -> Remote notification

7- You may need to create ,download and install provision profile again.

Now set up of push notification is ready.  We will learn To integrate one signal for push service.

### Links

<https://www.pubnub.com/docs/ios-objective-c/mobile-gateway#apns-prerequisites-certs>  
<https://www.raywenderlich.com/156966/push-notifications-tutorial-getting-started>  
<https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html>


