> [!IMPORTANT]
> _**JasperReports Library 7 coming soon!**_
> 
> Work is in progress on a separate branch that will introduce
> major refactoring of the library, which is needed for the [Jakarta Migration](https://blogs.oracle.com/javamagazine/post/transition-from-java-ee-to-jakarta-ee).
> The changes help improving the dependency management by splitting the library into multiple optional artifacts (`*.jar` files) depending on
> the functionality they provide. Deprecated code has been removed and the backward compatibility of serialized/compiled `*.jasper` report template
> files has been deliberately broken.

## JasperReports Library 7.0.0 Change Log
- removal of the Ant build system and replacing it with a Maven build system;
- deprecated code removed;
- deliberately breaking backward compatibility of serialized/compiled `*.jasper` report template files, mostly because of historical 
deprecated serialization code removal/cleanup mentioned above (source `*.jrxml` report templates need to be recompiled to `*.jasper` using the new version of the library);
- extracting various optional extension JAR artifacts from the the core library JAR artifact to allow the Jakarta Migration of certain of these optional features while also
introducing better third party Maven dependency management of these artifacts; 
- backward compatibility of source JRXML report templates is still guaranteed, but the built-in JRXML writer does not write back to
versions older than 5.5.2;

# JasperReports® - Free Java Reporting Library

The **JasperReports Library** is the world's most popular open source reporting engine. 
It is entirely written in Java and it is able to use data coming from any kind of data source and 
produce pixel-perfect documents that can be viewed, printed or exported in a variety of document 
formats including HTML, PDF, Excel, OpenOffice, MS Word and other.

_**Documentation:**_
- [JasperReports Ultimate Guide](https://jasperreports.sourceforge.net/JasperReports-Ultimate-Guide-3.pdf)
- [Samples Reference](https://jasperreports.sourceforge.net/sample.reference.html)
- [Schema Reference](https://jasperreports.sourceforge.net/schema.reference.html)
- [Configuration Reference](https://jasperreports.sourceforge.net/config.reference.html)
- [Functions Reference](https://jasperreports.sourceforge.net/function.reference.html)
- [FAQ](http://community.jaspersoft.com/wiki/jasperreports-library-faqs)
- [API (Javadoc)](https://jasperreports.sourceforge.net/api/index.html)
- [Discussion Forums](https://community.jaspersoft.com/project/jasperreports-library/answers)
<br/>
<br/>

**Jaspersoft® Studio** - *report designer for the JasperReports Library*

The report templates for the *JasperReports Library* are XML files which can be edited using a powerful,
open source, Eclipse-based report designer called [Jaspersoft Studio](https://www.jaspersoft.com/products/jaspersoft-studio).
Using *Jaspersoft Studio*, reports can be built out of any data source and can have their look and feel 
formatted for printing or on-screen reading, or can be deployed to a *JasperReports Server* instance, 
*JasperReports IO* repository or to a custom application using the *JasperReports Library* implementation
and exported to a wide range of output document formats.
<br/>
<br/>

**JasperReports Server** - *reporting and analytics server*

[JasperReports Server](https://www.jaspersoft.com/products/jasperreports-server) is a stand-alone and embeddable 
reporting server. It provides reporting and analytics that can be embedded into a web or mobile application as well as 
operate as a central information hub for the enterprise by delivering mission critical information on a real-time or 
scheduled basis to the browser, mobile device, or email inbox in a variety of file formats. *JasperReports Server* is 
optimized to share, secure, and centrally manage your Jaspersoft reports and analytic views.
<br/>
<br/>

**JasperReports Web Studio** - *web-based version of the desktop Jaspersoft® Studio*

[JasperReports Web Studio](https://www.jaspersoft.com/products/jasperreports-web-studio) is a new web visual designer that 
creates and edits report templates for the *JasperReports® Library* reporting engine and the whole *Jaspersoft®* family of 
products that use the open-source library to produce dynamic content and rich data visualizations.
<br/>
<br/>

**JasperReports IO** - *reporting and data visualization in a world of cloud, microservices, and DevOps*

[JasperReports IO](https://www.jaspersoft.com/products/jasperreports-io) is a RESTful reporting and data 
visualization service built on *JasperReports Library*, designed for generating reports and data visualizations in 
modern software architectures. Just as the *JasperReports Library* offers a `Java API` to leverage a powerful and high 
quality reporting engine inside Java applications, *JasperReports IO* offers a `REST API` to leverage the same reporting 
engine from any other software development platform.
<br/>
<br/>

## Building the JasperReports Library Project

The JasperReports Library project consists of one core JAR artifact and a number for optional extension JAR artifacts.

The build system relies entirely on the [Maven](https://maven.apache.org/) build tool.

Building the core JAR artifact and the optional extension JAR artifacts can be performed from root folder of the project
using the `pom-all.xml` file as follows:

    mvn clean install source:jar javadoc:jar -f pom-all.xml

Building the JAR artifacts using a JDK version later than 1.8 requires the suppression of a Java enforcer plugin,
while building having local non-committed Git modifications requires the suppression of the build number plugin check as follows:

    mvn clean install source:jar javadoc:jar -f pom-all.xml -Denforcer.skip -Dmaven.buildNumber.doCheck=false

The project has a separate artifact for tests under the `/tests`, which can be run using the following command:

    mvn clean test

The project documentation consists of schema reference, configuration reference, samples reference, functions reference and the aggregated Javadoc.
It can be all generated using the following command in the `/docs` folder of the project:

    mvn clean exec:exec@docs

The generated documentation is to be found under the `/docs/target/docs` folder of the project.

## Running the samples

The project shows and documents many of its features using a set of samples which are themselves Maven projects that can be
run from command line. Some of these samples make use of a demo HSQLDB data base that needs to be started before the respective
reports are run.

Starting the HSQLDB demo database containing demo data is done using the following command in the `/demo/hsqldb` folder of the project:

    mvn exec:java

The samples are each in a separate subfolder in the /demo/samples folder of the project and can be run either individually or all
at once. To run an individual sample, the following command should be run in the respective sample folder:

    mvn clean compile exec:java
    
This `exec:java` Maven goal is calling the `test()` method of the sample application main class. Calling individual methods from the sample application main class can be performed
using a command like the following:

    mvn exec:java -Dexec.args=pdf

which calls the `pdf()` method of the sample application main class to export the reports to PDF. Multiple methods can be called in sequence like this:

    mvn exec:java -Dexec.args="pdf xls"
    
Viewing the source JRXML report design is done using the following command:

    mvn exec:java@viewDesign
    
The compiled `*.jasper` report template file can also be viewed using something like:

    mvn exec:java@viewDesign -Dexec.args=target/reports/I18nReport.jasper 
 
The generated `*.jrprint` report file can be viewed using either:

    mvn exec:java@view
    
or a more explicit command like:

    mvn exec:java@view -Dexec.args=target/reports/I18nReport.jrprint

which would also work for XML exported report files:

    mvn exec:java@view -Dexec.args=target/reports/I18nReport.jrpxml

Some samples use additional services that need to be started before the actual sample reports are run.
To make sure all required steps are performed in right order, an alternate way to run the sample is as follows:

    mvn clean compile exec:exec@all

This command is also the one that can be used to run all the samples at once, but it needs to be launched from the `/demo/samples` folder:

    mvn clean compile exec:exec@all

