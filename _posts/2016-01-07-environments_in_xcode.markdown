---
layout:     post
title:      "Environments in XCode"
date:       2016-01-07
categories: XCode
---

A while ago [Futurice][futurice] published a [blog post][blog-post] on how import it is to consider different environments when developing.

Take a moment to think which environments are important for you when your are building your iOS app. Having to continously improve apps with a huge amount of daily users and high QA standards which have to be met as soon as an app is released to a testing team, I experienced that the least amount of environments you want to have is three:

- __develop__ is the environment you are developing with. Probably you want your app to point to a local server running on your computer. Maybe you want the app to extensively log information you consider valuable and not throw any analytics data to remote servers.

- __test__ is the version you want to give to your Q&A team. Now your app is pointing to a remote server, where you store all the test data. You are probably also interested in some kind of analytics your app is already generating during the test phase. Though you don't want to mix this data with the data of your live version.

- __release__ is the version you will release to the app store. This app is pointing to your  production server. 


Depending on the app you are building these environments can be configured to your specific needs. Maybe you want to have some mechanism on develop so you don't have to login constantly in order to save time during development. Another interesting point for you to consider a test and a release version is, that you don't want your test users to mess up their core data of their app, when they install a new test version on their phone. Well, I think you get the point.  

Setting up your environment with the according build schemes early on during the development phase will save you time later on. Here is one way to setup your environments in XCode:

[blog-post]: http://futurice.com/blog/assets/five-environments-you-cannot-develop-without

[futurice]: http://futurice.com

## Configuration

First of all you have to create a configuration for each environment you want. By default XCode already has Debug and Release when you start a project. In the example below I added one for Test.

![image](/assets/configs.png)

In order to have all environments installed simultaneously on your device you need one provisioning profile with a separate App ID for each. After you have created those in the apple developer portal, setup the product bundle identifier for each configuration, in order XCode to be able to setup and sign your app accordingly.	

![image](/assets/bundleIds.png)

The final step in order to have your app running in the different environments and in order to be able to switch quickly between those, create a scheme for every environment. There are different ways to create a new scheme. Here is one: Go to Product -> Scheme -> Manage Schemes... Here you can select one of your existing schemes and duplicate it by tapping the settings button right to the plus and minus buttons:

![image](/assets/schemes.png)

and make sure your schemes are pointing to the right configuration. You can configure the settings of a scheme, by selecting a scheme in the "Manage Schemes..." window (see above) and clicking the "Edit..." button: 

![image](/assets/scheme.png)

That's basically it, now you have three schemes of your app and you can have all of them installed on your device in parallel with slightly different configurations and their very own core data stack.

## Customisation


Now you can customise the app  the way you want.

###Macros

You can create macros for the different configurations in order to adjust the behaviour of the app for the different environemts:

![image](/assets/macros.png)

Then you can use these macros as follows in your code:

    #ifdef CONFIGURATION_Debug
    // Do some debug stuff, e.g. login user automatically
    #endif

###AppIcon

You can have a separate icon for every environment, so you don't get confused during developing and testing. To do so, create an app icon in your assets folders for every environment:

![image](/assets/appicons.png)

 and set the according configuration to use the right icon:
 
![image](/assets/icons.png)

###Custom Variables

You can have custom variables, representing different values for the different environments. Create a User-Defined Setting in your Build Settings. As an example I created a custom variable called BASE_URL which represents the url of the backend:

![image](/assets/BASE_URLS.png)

In order to be able to use the variable in your code you have to setup another variable in your info.plist to point to the specific user-defined setting:

![image](/assets/baseURLs.png)

And then you can use it as follows inside your application:

`[[[NSBundle mainBundle] infoDictionary] valueForKey:@"baseURL"];`

There are other things you can customize like the name of the app, etc. The described setup helped me a lot having a clean separation between developing new feature and fixing bugs in release.

Do you think I am missing something or a way to do it even better? Please, let me know!