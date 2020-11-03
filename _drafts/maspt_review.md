A MASPT review including some other thoughts
========================

A discussion I had recently with a LinkedIn connection reminded me to write this review of the Mobile Application Security Pentetration Tester, leading to the eMAPTv1 certification. While it did not turn out as a review per say, I hope you still get some useful tidbits of information out of it. 


## The course
While the course was pretty interesting, I do agree with some of the comments that it is a bit outdated, that is not the focus on this post. Instead of telling what each chapter explains, I want to provide you with some outtakes / scenarios that might be useful when you are doing the course or starting with the assessment of android apps.  
Since the iOS part of the course is not part of the exam, I decided to skip that part for now and go back to it later. 
###	Android security module
The android security module or android architecture chapter provided a main overview of the architecture behind android and how it works. If you want to do a deep-dive on android internals, Jonathan Levin has written Android Internals: A Confectioner's Cookbook, which was recommended to me by a friend. 
I’ve also heard good things about:
-	The android hacker’s handbook
-	Embedded android
-	Android Security Internals.

###	Reversing apks
The introduction of apktool, jd-gui, jadx and other tools that help in the reversing process. After working with them for some time, I switched over to the **Mobile Security Framework** which is listed in the other resources at the bottom of the page. While this framework is easier to use in most cases, also keep the practice of doing the reversing and analysis manually. 
  
###	Rooting the device
In recent versions of android rooting of the device is often done through magisk. Please note that having root on a recent android device still does not allow you to do everything. These permissions can often be managed through Magisk.  This is because the security model of android is a bit different, compared to traditional linux servers. 

###	Proxy configuration
After android 7, the importing of custom or self-signed certificates has been changed. While it is still possible to get it working on android 10 and 11, I got it working through magisk with the certificate store module on a pixel 3a. Furthermore, make sure that your proxy (in my case burp suite pro) is also listening on all interfaces. Instead of the normal 127.0.0.1:8080, for android analysis I used a separate proxy on *:8082. When you have ownership on the network, I found it easiest to define two ssids for testing the apps on the physical device. 
1.	Pentest-wifi
2.	Pentest-wifi-proxy
Then in the android menu, you can set the proxy for the proxy ssid and do not have to switch the proxy on manually every time. 

##
For the course, the chapters on the application fundamentals and the chapter on static code analysis were a good refresher on the android app internals. I recommend you to read them carefully and really understand what is being said there.  


## The exam
While I won’t say to much about the contents of the exam, I recommend catching up on the basics of Java or Kotlin. While I did program in Java a bit in university, it was already a few years ago, and during the exam I realized that I was rustier than I initially thought. 

While it doesn’t happen often that you have to write an app to exploit one or two different apps, it forces you to go back to the drawing board and think more in-depth about the found vulnerabilities compared to doing a security assessment or a pentest report on android apps. In a new revision of the course I would like to see a combination of a penetration test report on the security together with an exploit app.  

I started the exam on a Monday morning, around 1030 am making sure I had my coffee and started analysing the logic of the apps. It didn’t take long to identify the two vulnerabilities in the different apps I had to exploit with my exploit app. By the end of the day I had an app that somewhat worked but it was still missing something. Since I still had 6 days left, I decided to take a break and focus on some other things that I also had to do outside of the exam. That proved fruitful. After finding the missing piece, it didn’t take much time to get the whole exploit app working. 

**Authors note: While the code of my app worked, it was not the prettiest nor was the app the best looking app ever seen. Luckily we were not graded on the UI/UX of the app as far as i know ^^**

After I submitted my exam (the source code and the exploit apk) on Thursday night around 10 pm, I got the results back on Friday saying that I passed the exam. I was amazed at the speed I got my results back. In the official statement it can take up to 30 working days. 

## Other resources that might help you learn about android security
One of the areas I am working in is the assessment of security of android apps based on the the OWASP Mobile Application Verification Standard, a standard with two security grades and a set of requirements on the protections against reverse engineering of android apps. 

### The Mobile Security Testing Guide
For the assessment of the security of android apps, OWASP released a Mobile Security Testing Guide, available at: https://github.com/OWASP/owasp-mstg. The releases tab contains the major release from august 2019, but with the included tools you can also generate a more up to date PDF of this testing guide. 

### Drozer
Outside of the android debug bridge (adb), drozer is a very powerful tool which can help you test and attack android apps. It allows you to interact with the dalvik vm/art vm, apps’ IPC enpoints and the OS of the device. Drozer is available here: https://labs.f-secure.com/tools/drozer/
In order to get it drozer started, you need to install the app on your emulator/or testing device, then enable drozer in the app, forward the right tcp port through adb and then run the drozer console connect command on your test workstation. 

### Frida

### Mobile Security Framework
Mobile Security Framework (MobSF) is an automated security assessment framework capable of performing static and dynamic analysis. It is available here: https://github.com/MobSF/Mobile-Security-Framework-MobSF. It makes the automated scanning very easy, but it also made me overlook some things in the past, so I also recommend doing some manual analysis on the manifest, extracting the app with apktool, and going through the code manually. You can also integrate Frida functionality in Mobsf. 


