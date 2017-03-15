# KSCrashFrameWork
framework for KSCrash

# 项目地址: 
https://github.com/kstenerud/KSCrash

# How to Use KSCrash

1. Add the framework to your project (or add the KSCrash project as a dependency)

2. Add the following system frameworks & libraries to your project:
    libc++.dylib
    libz.dylib
    MessageUI.framework (iOS only)
    SystemConfiguration.framework
    
3. Add the flag "-ObjC" to Other Linker Flags in your Build Settings

4. Add the following to your [application: didFinishLaunchingWithOptions:] method in your app delegate:

```
#import <KSCrash/KSCrash.h>
// Include to use the standard reporter.
#import <KSCrash/KSCrashInstallationStandard.h>
// Include to use Quincy or Hockey.
#import <KSCrash/KSCrashInstallationQuincyHockey.h>
// Include to use the email reporter.
#import <KSCrash/KSCrashInstallationEmail.h>
// Include to use Victory.
#import <KSCrash/KSCrashInstallationVictory.h>

- (BOOL)application:(UIApplication*) application didFinishLaunchingWithOptions:(NSDictionary*) launchOptions
{
KSCrashInstallationStandard* installation = [KSCrashInstallationStandard sharedInstance];
installation.url = [NSURL URLWithString:@"http://put.your.url.here"];

// OR:

KSCrashInstallationQuincy* installation = [KSCrashInstallationQuincy sharedInstance];
installation.url = [NSURL URLWithString:@"http://put.your.url.here"];

// OR:

KSCrashInstallationHockey* installation = [KSCrashInstallationHockey sharedInstance];
installation.appIdentifier = @"PUT_YOUR_HOCKEY_APP_ID_HERE";

// OR:

KSCrashInstallationEmail* installation = [KSCrashInstallationEmail sharedInstance];
installation.recipients = @[@"some@email.address"];

// Optional (Email): Send Apple-style reports instead of JSON
[installation setReportStyle:KSCrashEmailReportStyleApple useDefaultFilenameFormat:YES]; 

// Optional: Add an alert confirmation (recommended for email installation)
[installation addConditionalAlertWithTitle:@"Crash Detected"
                                 message:@"The app crashed last time it was launched. Send a crash report?"
                               yesAnswer:@"Sure!"
                                noAnswer:@"No thanks"];

// OR:

KSCrashInstallationVictory* installation = [KSCrashInstallationVictory sharedInstance];
installation.url = [NSURL URLWithString:@"https://put.your.url.here/api/v1/crash/<application key>"];

[installation install];
    …
}

// last: 
[installation sendAllReportsWithCompletion:^(NSArray *filteredReports, BOOL completed, NSError *error)
{
 // Stuff to do when report sending is complete
}];
```
