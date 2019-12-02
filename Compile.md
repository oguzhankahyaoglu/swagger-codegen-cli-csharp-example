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

# Step 3 - Checkout swagger codegen source
Checkout https://github.com/swagger-api/swagger-codegen to a folder.

# Step 4 - Compile CLI
Run *mvn clean package* command in swagger directory. It is going to take a while, we will be using *modules\swagger-codegen-cli\target\swagger-codegen-cli.jar* file after compilation.

# Step 5 - CSharp client generation
After getting swagger-codegen-cli.jar file, here is a default batch script for compiling a csharp library:

**set JAVA_OPTS=%JAVA_OPTS% -XX:MaxPermSize=256M -Xmx1024M -DloggerPath=conf/log4j.properties
set ags=generate -t RELATIVE_TEMPLATE_DIRECTORY -i SWAGGER_JSON_FILE_URL -l csharp -o OUTPUT_DIRECTORY -DpackageName=
- OUTPUT_LIBRARY_NAME: -DtargetFramework=v5.0
java %JAVA_OPTS% -jar %executable% %ags%**

- RELATIVE_TEMPLATE_DIRECTORY: Relative mustache folder containing templates, *mine was ..\modules\swagger-codegen\src\main\resources\csharp*
- SWAGGER_JSON_FILE_URL: swagger.json file location or url to be used
- OUTPUT_DIRECTORY: Output folder of the generated source file
- OUTPUT_LIBRARY_NAME: Output project name of the generated source code, *mine was Swagger.MyApi*

# Bonus Step - Compilation to DLL
If you use csharp language instead of csharp-dotnet2, generated output contains *build.bat* file which builds generated source into a DLL file. Instead of copying the source code to your project, you can directly reference this dll. 
