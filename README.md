# Spock Reports Extension
*by Renato Athaydes*

## News

* 08th of November 2014 - 1.2.3 released - downgraded minimum Groovy version required to 2.0.8 (benefits some Grails users).
* 03rd of November 2014 - Small feature release (1.2.2): added support for @Unroll-ing variables not only in Spec names,
but also in block texts (given, when, then...)
* 01st of November 2014 - Bug fix release, 1.2.1. Fixed problems with bad-behaving Specification setup methods and
removed usage of some Groovy features to allow running spock-reports with any recent JDK 7 or 8 version.
* 3rd of August 2014 - Release of version 1.2! This release includes full support for @Unroll,
amongst other small improvements.
* 3rd of July 2014 - After a long delay, I have finally published the 1.1 release on a public repo, thanks to @JayStGelais
contribution. So now ``spock-reports`` is available on Bintray's [JCenter](http://jcenter.bintray.com/)!
* 14th of September 2013 - Release of version 1.1 with some minor bug fixes and improvements in the information shown in reports.
Please check the release notes in the file `releases/Release_Notes.txt`.
* Today, the 6th of August 2013, I am proud to release version 1.0 of this project! I have made sure to make it as stable
as possible so that you can start reading proper reports for your Spock specifications without worries.
You can find the jar in the `releases` directory! Download it, place it in your classpath and you're done!
By default, the reports will be saved in the directory `build/spock-reports`, but you can change that if you want,
just check below.
I hope you find this project as useful as I have (I actually developed this out of my own need for it!).
* I wrote a [blog post](http://software.athaydes.com/posts/writingspecificationsthatdoubleastestswithspock) about the motivation behind this project. Please check it out!

## What it is

This project is a global extension for [Spock](https://code.google.com/p/spock/) to create test (or, in Spock terms, Specifications) reports.
Currently, the only available report creator generates a **HTML report** for each Specification, as well as a summary of all Specifications that have been run (index.html).

## Where to find demo reports

I am using [CodePen](http://codepen.io) to design the HTML [feature report](http://codepen.io/renatoathaydes/full/ihGgt), which contains detailed information about each Specification run by Spock, including the examples given (*Where* block) and their results, if any, and the [summary report](http://codepen.io/renatoathaydes/full/mKckz), which summarizes the results of all Specification runs. Click on the links to see the reports used for testing.

If you don't like the styles, you can use your own css stylesheets (see the customization section below). I welcome feedback on how to improve the report looks!

## How to use it 

To enable this Spock extension, you only need to declare a dependency to it (if using Maven, Ivy, Gradle etc) or, in other words, add the jar to the classpath.

In Maven:

Enable the JCenter repository:

```xml
    <repository>
      <id>jcenter</id>
      <name>JCenter Repo</name>
      <url>http://jcenter.bintray.com</url>
    </repository>
```

Add ``spock-reports`` to your ``<dependencies>``:

```xml
<dependency>
  <groupId>com.athaydes</groupId>
  <artifactId>spock-reports</artifactId>
  <version>1.2.3</version>
  <scope>test</scope>
</dependency>
```

In Gradle:

```groovy
repositories {
  jcenter()
}

dependencies {
    testCompile 'com.athaydes:spock-reports:1.2.3'
}
```

If you prefer, you can just download the jar directly from [JCenter](http://jcenter.bintray.com/com/athaydes/spock-reports/).

The only dependencies of this project are on Groovy (version 2.0+) and Spock, but if you're using Spock (version 0.7-groovy-2.0+), you'll already have both!


## Customizing the reports

You can provide custom configuration in a properties file located at the following location (relative to the classpath):

`META-INF/services/com.athaydes.spockframework.report.IReportCreator.properties`

Here's the default properties file:

```properties
# Name of the implementation class of the report creator
# Currently supported classes are:
#   1. com.athaydes.spockframework.report.internal.HtmlReportCreator
com.athaydes.spockframework.report.IReportCreator=com.athaydes.spockframework.report.internal.HtmlReportCreator

# Set properties of the report creator
# For the HtmlReportCreator, the following properties are available
# (the location of the css files is relative to the classpath):
com.athaydes.spockframework.report.internal.HtmlReportCreator.featureReportCss=spock-feature-report.css
com.athaydes.spockframework.report.internal.HtmlReportCreator.summaryReportCss=spock-summary-report.css
# exclude Specs Table of Contents
com.athaydes.spockframework.report.internal.HtmlReportCreator.excludeToc=false

# Output directory (where the spock reports will be created) - relative to working directory
com.athaydes.spockframework.report.outputDir=build/spock-reports

# If set to true, hides blocks which do not have any description
com.athaydes.spockframework.report.hideEmptyBlocks=false
```

Notice that the location of the css file is relative to the classpath!
That means that you have the freedom to place the css files in a separate jar, for example.

The output directory, on the other hand, is relative to the working directory.
For Maven projects which use the defaults, you might want to change it to `target/spock-reports`.

### Properties file location for Grails users

In Grails apps, the properties file has to be placed in `grails-app/conf/META-INF/services`.

So the full path and name for the properties should be:

`grails-app/conf/META-INF/services/com.athaydes.spockframework.report.IReportCreator.properties`

### System properties overrides

The following configuration options can also be overridden by system properties.  These system properties must be set prior to Spock being initialized (which starts this extension).  So you must ensure to set these properties as either JVM arguments or in your own bootstrapping function that in guaranteed to execute before Spock is initialized.  When set *before Spock is initialied*, these system properties will take precedence over values read from config files.  If Spock is initialized before these properties are set then they will have no effect.

`com.athaydes.spockframework.report.IReportCreator`: Set the report creator class to use.
`com.athaydes.spockframework.report.outputDir`: Set the output directory of the generated reports; relative paths are relative to the working directory.
`com.athaydes.spockframework.report.hideEmptyBlocks`: true|false; should blocks with empty text be printed out in report?

Default values are inherited from those described above.