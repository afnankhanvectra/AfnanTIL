## Notification service extension + one signal 
### Problem

Normal notification has limit of 4K. You can not send a heavy image , video and gif in that. For this purpose we use Notification service extension. 

### Introduction
A UNNotificationServiceExtension object provides the entry point for a Notification Service app extension, which lets you customize the content of a remote notification before it is delivered to the user.  You can change notification content, text before delivered to user.

I used notification extension with one signal. That makes life easier. You don’t have to handle images, handle in receiving. Onesignal is free and easy to use for developers.

### Steps
- 1- Make notification extension  by File -> New -> Target ->  Notification Service Extension
- 2- In Project setting , in general of Service extension set provision profile and deployment target . must greater than 10.2
- 3-  install pod of one signal in target of service  like 
target 'OneSignalNotificationServiceExtension' do
  pod 'OneSignal', '>= 2.7.1', '< 3.0'
end
- 4- In sending attach attachment with static url 
            "ios_attachments": ["image" : "https://cdn.pixabay.com/photo/2017/01/16/15/17/hot-air-balloons-1984308_1280.jpg"],

- 5- In NotificationService.swift change code of 

        override func didReceive(_ request: UNNotificationRequest, withContentHandler contentHandler: @escaping (UNNotificationContent) -> Void) 
         and add  
         if let bestAttemptContent = bestAttemptContent {
            OneSignal.didReceiveNotificationExtensionRequest(self.receivedRequest, with: self.bestAttemptContent)
            contentHandler(bestAttemptContent)
        }

- 6- Change implementation of      
     
         override func serviceExtensionTimeWillExpire() and add bottom line 
         if let contentHandler = contentHandler, let bestAttemptContent =  bestAttemptContent {
            OneSignal.serviceExtensionTimeWillExpireRequest(self.receivedRequest, with: self.bestAttemptContent)
            contentHandler(bestAttemptContent)
        }
- 7- Make different provision profile for Notification service and install in system

***NOTE::*** Always send static image url. Notification extension don’t allow redirect. Even you can not send cloudinary url as wloudinary use Amazon storage. I would recommend to use amazon storage for sending.
