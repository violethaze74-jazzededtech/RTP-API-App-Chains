# App Chains (Real-Time Personalization)
=========================================
[App Chains](https://sequencing.com/developer-documentation/app-sequencing-and-app-chains) are the easy way to code [Real Time Personalization (RTP)](https://sequencing.com/developer-documentation/what-is-real-time-personalization-rtp) into your app. RTP allows your app to provide app users with a genetically tailored user experience.

App chains consist of an API call that triggers an app hosted by Sequencing.com to perform genetic analysis on your app user's genes. The straightforward, easy-to-use results are then sent back to your app as the API response. Your app can use this information, which is obtained in real-time, to differentiate itself from all other apps by creating a truly personalized user experience.

What types of apps can you personalize with app chains? Any type of app... even a weather app. 
* The open source [Weather My Way +RTP app](https://github.com/SequencingDOTcom/Weather-My-Way-RTP-App/) app differentiates itself from all other weather apps because it uses app chains to provide genetically tailored content to each app user. 
* Experience it yourself using one of the fun sample genetic data files that are available for all apps that use app chains.

**The searchable list of all App Chains, along with additional info and code snippets for each app chain, can be accessed here: [App Chains](https://sequencing.com/app-chains/)**

App chains are free for development use. [Register for a free Sequencing.com account](https://sequencing.com/user/register/)

This repository contains code for App Chains in the following languages:

* Java / Java-Android
* Swift
* Objective-C
* C#/.NET
* PHP
* Perl
* Python


Contents
=========================================
* Introduction
* Installation
* Configuration
* Troubleshooting
* Maintainers
* Contribute

Introduction
=========================================
An app chain is an integration of an API call and an analysis of an app user's genes. Each app chain provides information about a specific trait, condition, disease, supplement or medication. App chains are used to provide genetically tailored content to app users so that the user experience is instantly personalized at the genetic level. This is called [Real Time Personalization (RTP)](https://sequencing.com/developer-documentation/what-is-real-time-personalization-rtp). 

To code Real Time Personalization (RTP) technology into apps, developers may [register for a free account](https://sequencing.com/user/register/) at Sequencing.com. App development with RTP is always free.

Installation
======================================
The code for each App Chain is provided in various coding languages. App developers can cut and paste the code into their apps so that their apps can utilize RTP technology straight out-of-the-box.

Configuration
======================================
There are no strict configurations that have to be performed.

Just drop the source files for an app chain into your project to add Real-Time Personalization to your app.

Code snippets below contain the following three placeholders. Please make sure to replace each of the placeholders with real values:
* ```<your token>``` 
 * replace with the oAuth2 secret obtained from your [Sequencing.com account](https://sequencing.com/api-secret-generator)
  * The code snippet for enabling Sequencing.com's oAuth2 authentication for your app can be found in the [oAuth2 code and demo repo](https://github.com/SequencingDOTcom/oAuth2-code-and-demo)

* ```<chain id>``` 
 * replace with the App Chain ID obtained from the list of [App Chains](https://sequencing.com/app-chains)

* ```<file id>``` 
 * replace with the file ID selected by the user while using your app
  * The code snippet for enabling Sequencing.com's File Selector for your app can be found in the [File Selector code repo](https://github.com/SequencingDOTcom/File-Selector-code)

## Java

AppChains Java API overview

Method  | Purpose | Arguments | Description
------------- | ------------- | ------------- | -------------
`public AppChains(String token, String chainsHostname)`  | Constructor | **token** - security token provided by sequencing.com <br> **chainsHostname** - API server hostname. api.sequencing.com by default | Constructor used for creating AppChains class instance in case reporting API is needed and where security token is required
`public Report getReport(String remoteMethodName, String applicationMethodName, String datasourceId)`  | Reporting API | **remoteMethodName** - REST endpoint name, use "StartApp" <br> **applicationMethodName** - name of data processing routine <br> **datasourceId** - input data identifier <br>

Prerequisites:
* Add Google GSON into your classpath

Adding code to the project:
* Add AppChains.java into your source folder and import (```import com.sequencing.appchains.AppChains.*```) it in your class file.

After that you can start utilizing Reporting API

```java
AppChains chains = new AppChains("<your token>", "api.sequencing.com");
Report result = chains.getReport("StartApp", "<chain id>", "<file id>");

if (result.isSucceeded() == false)
	System.out.println("Request has failed");
else
	System.out.println("Request has succeeded");
		
for (Result r : result.getResults())
{
	ResultType type = r.getValue().getType();
			
	if (type == ResultType.TEXT)
	{
		TextResultValue v = (TextResultValue) r.getValue();
		System.out.println(String.format(" -> text result type %s = %s", r.getName(), v.getData()));
	}
			
	if (type == ResultType.FILE)
	{
		FileResultValue v = (FileResultValue) r.getValue();
		System.out.println(String.format(" -> file result type %s = %s", r.getName(), v.getUrl()));
		v.saveTo("d:/data");
	}
}
```

### Objective-C

AppChains Objective-C API overview

Method  | Purpose | Arguments | Description
------------- | ------------- | ------------- | -------------
`-(instancetype)initWithToken:(NSString *)token` | Constructor | **token** - security token provided by sequencing.com | 
`-(instancetype)initWithToken:(NSString *)token withHostName:(NSString *)hostName`  | Constructor | **token** - security token provided by sequencing.com <br> **hostName** - API server hostname. api.sequencing.com by default | Constructor used for creating AppChains class instance in case reporting API is needed and where security token is required
`- (void)getReportWithRemoteMethodName:(NSString *)remoteMethodName             withApplicationMethodName:(NSString *)applicationMethodName                      withDatasourceId:(NSString *)datasourceId withSuccessBlock:(void (^)(Report *result))success withFailureBlock:(void (^)(NSError *error))failure`  | Reporting API | **remoteMethodName** - REST endpoint name, use "StartApp" <br> **applicationMethodName** - name of data processing routine <br> **datasourceId** - input data identifier <br> <br> **success** - callback executed on success operation<br> **failure** - callback executed on operation failure

Adding code to the project:
* Add AppChains.h, AppChains.m into your source folder and import AppChains in your Objective-C source file (```#import "AppChains.h"```).

After that you can start utilizing Reporting API

```objectivec
[appChains getReportWithRemoteMethodName:@"StartApp"
               withApplicationMethodName:@"<chain id>"
                        withDatasourceId:@"<file id>"
                        withSuccessBlock:^(Report *result) {
                               NSArray *arr = [result getResults];
                               for (Result *obj in arr) {
                                    ResultValue *frv = [obj getValue];

                                    if ([frv getType] == kResultTypeFile) {
                                        [(FileResultValue *)frv saveToLocation:@"/tmp/"];
                                    }
                                }
                            } withFailureBlock:^(NSError *error) {
                                NSLog(@"Error occured: %@", [error description]);
                            }];
```

### Swift

AppChains Swift API overview

Method  | Purpose | Arguments | Description
------------- | ------------- | ------------- | -------------
`init(token: String, chainsHostname: String)`  | Constructor | **token** - security token provided by sequencing.com <br> **chainsHostname** - API server hostname. api.sequencing.com by default | Constructor used for creating AppChains class instance in case reporting API is needed and where security token is required
`func getReport(remoteMethodName: String, applicationMethodName: String, datasourceId: String) -> ReturnValue<Report>`  | Reporting API | **remoteMethodName** - REST endpoint name, use "StartApp" <br> **applicationMethodName** - name of data processing routine <br> **datasourceId** - input data identifier <br>

Prerequisites:
* Swift v1 compatible compiler

Adding code to the project:
* Add AppChains.swift into your source folder.

After that you can start utilizing Reporting API

```swift
let chains = AppChains(token: "<your token>", chainsHostname: "api.sequencing.com")

let report:ReturnValue<Report> = chains.getReport("StartApp", applicationMethodName: "<chain id>", datasourceId: "<file id>")

if let r = report.value
{
    for x:Result in r.results
        
    {
        let type:ResultType = x.value.type;
        switch (type)
        {
            case ResultType.TEXT:
                let v:TextResultValue = x.value as TextResultValue
                println(String(format: " -> text result type %@ = %@", x.name, v.data));
            break;
            case ResultType.FILE:
                let v:FileResultValue = x.value as FileResultValue
                println(String(format: " -> text result type %@ = %@", x.name, v.url));
                v.saveTo("/tmp")
            break;
        }
    }
}
else
{
    println("Error occured: " + report.error!)
}
```

## C# ##

AppChains C# API overview

Method  | Purpose | Arguments | Description
------------- | ------------- | ------------- | -------------
`public AppChains(string token, string chainsUrl, string beaconsUrl)`  | Constructor | **token** - security token provided by sequencing.com <br> **chainsUrl** - API server hostname. api.sequencing.com by default <br> **beaconsUrl** - beacons API server hostname. https://beacon.sequencing.com by default | Constructor used for creating AppChains class instance in case reporting API is needed and where security token is required
`public Report GetReport(string applicationMethodName, string datasourceId)`  | Reporting API | **applicationMethodName** - name of data processing routine <br> **datasourceId** - input data identifier <br>

Prerequisites:
* Add Newtonsoft.Json and RestSharp nuget packages into your project

Adding code to the project:
* Add SQAPI folder with all files into your project

After that you can start utilizing Reporting API:

```csharp
var chains = new AppChains("<your token>", "https://api.sequencing.com/v1", "https://beacon.sequencing.com/");

Report result = chains.GetReport("Chain9", "FILE:80599");
if (result.Succeeded == false)
    Console.WriteLine("Request has failed");
else
    Console.WriteLine("Request has succeeded");
foreach (Result r in result.getResults())
{
    ResultType type = r.getValue().getType();

    if (type == ResultType.TEXT)
    {
        var v = (TextResultValue) r.getValue();
        Console.WriteLine(" -> text result type {0} = {1}", r.getName(), v.Data);
    }

    if (type == ResultType.FILE)
    {
        var v = (FileResultValue) r.getValue();
        Console.WriteLine(" -> file result type {0} = {1}", r.getName(), v.Url);
        v.saveTo("<your token>", ".\\");
    }
}
```

## PHP

AppChains PHP API overview

Method  | Purpose | Arguments | Description
------------- | ------------- | ------------- | -------------
`public function __construct($token = null, $chainsHostname = null)`  | Constructor | **token** (optional) - security token provided by sequencing.com <br> **chainsHostname** (optional) - API server hostname. api.sequencing.com by default | Use both arguments for creating chains instance in case reporting API is required
`public function getReport($remoteMethodName, $applicationMethodName, $datasourceId)`  | Reporting API | **remoteMethodName** - REST endpoint name, use "StartApp" <br> **applicationMethodName** - name of data processing routine <br> **datasourceId** - input data identifier <br>

Prerequisites:
* PHP 5.x and higher

Adding code to the project:
* Add AppChains.php into your source folder and import (```require_once("AppChains.php");```) it in your PHP file.

After that you can start utilizing Reporting API

```php
$chains = new AppChains("<your token>", "api.sequencing.com");
$chainsResult = $chains->getReport("StartApp", "<chain id>", "<file id>");
	
if ($chainsResult->isSucceeded())
	echo "Request has succeeded\n";
else
	echo "Request has failed\n";
	
foreach ($chainsResult->getResults() as $r)
{
	$type = $r->getValue()->getType();
		
	switch ($type)
	{
		case ResultType::TEXT:
			$v = $r->getValue();
			echo sprintf("-> text result type %s = %s\n", $r->getName(), $v->getData());
		break;
		case ResultType::FILE:
			$v = $r->getValue();
			echo sprintf(" -> file result type %s = %s\n", $r->getName(), $v->getUrl());
			$v->saveTo("d:\data");
		break;
	}
}
```

### Python

AppChains Python API overview

Method  | Purpose | Arguments | Description
------------- | ------------- | ------------- | -------------
`def __init__(self, token=None, hostname=None)`  | Constructor | **token** (optional) - security token provided by sequencing.com <br> **hostname** (optional) - API server hostname. api.sequencing.com by default | Use both arguments for creating chains instance in case reporting API is required
`def getReport(self, remote_method_name, app_method_name, source_id)`  | Reporting API | **remote_method_name** - REST endpoint name, use "StartApp" <br> **app_method_name** - name of data processing routine <br> **source_id** - input data identifier <br>

Prerequisites:
* Python 2.7.x

Adding code to the project:
* Add AppChains.py into your source folder and import AppChains (```from AppChains import AppChains```) in your Python file.

After that you can start utilizing Reporting API

```python
self.chains = AppChains('<your token>', 'api.sequencing.com')
chains_result = self.chains.getReport('StartApp', '<chain id>', '<file id>')
if chains_result.isSucceeded():
    print('Request has succeeded')
    else:
        print('Request has failed')
        for r in chains_result.getResults():
            file_type = r.getValue().getType()
            v = r.getValue()
            if file_type == 'TEXT':
                print('-> text result type {} = {}'.format(
                    r.getName(), v.getData()))
            elif file_type == 'FILE':
                print(' -> file result type {} = {}'.format(
                    r.getName(), v.getUrl()
                ))
                v.saveTo('/tmp')
```

### Perl

AppChains Perl API overview

Method  | Purpose | Arguments | Description
------------- | ------------- | ------------- | -------------
`sub new`  | Constructor | **1st arg** (optional) - security token provided by sequencing.com <br> **2nd arg** (optional) - API server hostname. api.sequencing.com by default | Use both arguments for creating chains instance in case reporting API is required
`sub getReport`  | Reporting API | **1st arg** - REST endpoint name, use "StartApp" <br> **2nd arg** - name of data processing routine <br> **3rd arg** - input data identifier <br>

Prerequisites:
* Perl 5.10.x

Adding code to the project:
* Add AppChains.pm into your source folder and import AppChains in your Perl file (```use AppChains;```).

Adding code to the project:
* Add AppChains.h, AppChains.m into your source folder and import AppChains in your Objective-C source file (```#import "AppChains.h"```).

After that you can start utilizing Reporting API

```perl
my $chains = AppChains->new("<your token>", "api.sequencing.com");
my $chainsResult = $chains->getReport("StartApp", "<chain id>", "<file id>");

if ($chainsResult->isSucceeded()) {
	print "Request has succeeded\n";
} else {
	print "Request has failed\n";
}

foreach (@{$chainsResult->getResults()})
{
	my $type = $_->getValue()->getType();
	
	if ($type == ResultType->TEXT)
	{
		my $v = $_->getValue();
		print sprintf("-> text result type %s = %s\n", $_->getName(), $v->getData());
	}
	elsif ($type == ResultType->FILE)
	{
		my $v = $_->getValue();
		print sprintf(" -> file result type %s = %s\n", $_->getName(), $v->getUrl());
		$v->saveTo("/tmp");
	}
}
```

Troubleshooting
======================================
Each app chain code should work straight out-of-the-box without any configuration requirements or issues. 

Other tips

* Ensure that the following three placeholders have been substituted with real values:

1. ```<your token>```
  * replace with the oAuth2 secret obtained from your [Sequencing.com account](https://sequencing.com/api-secret-generator)
   * The code snippet for enabling Sequencing.com's oAuth2 authentication for your app can be found in the [oAuth2 code and demo repo](https://github.com/SequencingDOTcom/oAuth2-code-and-demo)
2. ```<chain id>```
  * replace with the App Chain ID obtained from the list of [App Chains](https://sequencing.com/app-chains)
3. ```<file id>```
  * replace with the file ID selected by the user while using your app. 
   * The code snippet for enabling Sequencing.com's File Selector for your app can be found in the [File Selector code repo](https://github.com/SequencingDOTcom/File-Selector-code)
   
* [Developer Documentation](https://sequencing.com/developer-documentation/)

* [oAuth2 guide](https://sequencing.com/developer-documentation/oauth2-guide/)

* Review the [Weather My Way +RTP app](https://github.com/SequencingDOTcom/Weather-My-Way-RTP-App/), which is an open-source weather app that uses Real-Time Personalization to provide genetically tailored content

* Confirm you have the latest version of the code from this repository.

Maintainers
======================================
The codebase is actively maintained by [Sequencing.com](https://sequencing.com/). Please email the Sequencing.com bioinformatics team at gittaca@sequencing.com if you require any more information or just to say hola.

Contribute
======================================
We encourage you to passionately fork us. If interested in updating the master branch, please send us a pull request. If the changes contribute positively, we'll let it ride.
