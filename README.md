# swagger-codegen-cli-csharp-example
Example project in depth detail for Swagger Codegen CLI tool; generating client projects with the same output of https://editor.swagger.io

# Step 1 - Install JDK
JDK must be installed in order to use this client tool. After installing JDK, installation path (like  *C:\Program Files\Java\jdk1.8.0_221* ) must be added to PATH environment variable.

# Step 2 - Install Maven
Maven must be installed before compiling CLI. You have to follow steps at these links:
- https://maven.apache.org/download.cgi
- https://maven.apache.org/install.html
- https://maven.apache.org/run.html

Maven must be added to PATH environment variable too.

# Step 3 - Checkout swagger-codegen-cli jar file
```
mvn dependency:copy -Dartifact=io.swagger:swagger-codegen-cli:2.2.2 -DoutputDirectory=. -Dmdep.stripVersion=true
```
To try v3 codegen tool:
```
mvn dependency:copy -Dartifact=io.swagger.codegen.v3:swagger-codegen-cli:3.0.11 -DoutputDirectory=. -Dmdep.stripVersion=true
```

# Step 4 - CSharp client generation
After getting swagger-codegen-cli.jar file, here is a default batch script for compiling a csharp library:
```
SET APIURL=SWAGGER_JSON_FILE_URL
SET OUTPUT_PATH=OUTPUT_DIRECTORY
SET PACKAGE_NAME=OUTPUT_LIBRARY_NAME
set executable=swagger-codegen-cli.jar
set JAVA_OPTS=%JAVA_OPTS% -XX:MaxPermSize=256M -Xmx1024M -DloggerPath=conf/log4j.properties
set ags=generate -i %APIURL% -l csharp -o %OUTPUT_PATH% -DpackageName=%PACKAGE_NAME% -DoptionalProjectFile=true
java %JAVA_OPTS% -jar %executable% %ags%

```
- SWAGGER_JSON_FILE_URL: swagger.json file location or url to be used
- OUTPUT_DIRECTORY: Output folder of the generated source file
- OUTPUT_LIBRARY_NAME: Output project name of the generated source code, *mine was Swagger.MyApi*

# Bonus Step - Compilation to DLL
If you use csharp language instead of csharp-dotnet2, generated output contains *build.bat* file which builds generated source into a DLL file. Instead of copying the source code to your project, you can directly reference this dll. 
To generate DLL file, you can use the batch file. However, there is a tls issue for the default bat which can be replaced with the batch commands below:

```
@echo off
SET CSCPATH=%SYSTEMROOT%\Microsoft.NET\Framework\v4.0.30319
if not exist ".\nuget.exe" powershell -Command "[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;(new-object System.Net.WebClient).DownloadFile('https://dist.nuget.org/win-x86-commandline/latest/nuget.exe', '.\nuget.exe')"
.\nuget.exe install src\IO.Swagger\packages.config -o packages
if not exist ".\bin" mkdir bin
copy packages\Newtonsoft.Json.10.0.3\lib\net45\Newtonsoft.Json.dll bin\Newtonsoft.Json.dll
copy packages\JsonSubTypes.1.2.0\lib\net45\JsonSubTypes.dll bin\JsonSubTypes.dll
copy packages\RestSharp.105.1.0\lib\net45\RestSharp.dll bin\RestSharp.dll
%CSCPATH%\csc  /reference:bin\Newtonsoft.Json.dll;bin\JsonSubTypes.dll;bin\RestSharp.dll;System.ComponentModel.DataAnnotations.dll  /target:library /out:bin\IO.Swagger.dll /recurse:src\IO.Swagger\*.cs /doc:bin\IO.Swagger.xml

```

