{% assign spec_type=page.spec_type %}
{% if spec_type == 'html' %}
{% assign html=true %}
{% assign md=false  %}
{% assign spec_type_desc = 'HTML' %}
{% assign ext='html' %}
{% elsif spec_type == 'markdown' %}
{% assign html=false %}
{% assign md=true    %}
{% assign spec_type_desc = 'Markdown' %}
{% assign ext='md'    %}
{% endif %}

{% assign fixture_language=page.fixture_language %}
{% if fixture_language == 'java' %}
{% assign java=true %}
{% assign csharp=false  %}
{% assign fixture_language_desc = 'Java' %}
{% elsif fixture_language == 'csharp' %}
{% assign java=false %}
{% assign csharp=true %}
{% assign fixture_language_desc = 'C#' %}
{% endif %}

_This page shows the integrations for __{{ fixture_language_desc }}__._  Click the toggle buttons above to choose other options.

* TOC
{:toc}

{% if java %}
# IDEs

## IntelliJ IDEA

The excellent [Concordion support](https://plugins.jetbrains.com/plugin/7978?pr=idea) plugin for IDEA provides support such as autocompletion, 2-way navigation between specifications and fixtures, run test from specification, surround with Concordion command, go to declaration and renaming.

This plugin supports both HTML and Markdown format specifications.

## Eclipse

The [concordion-eclipse](https://code.google.com/archive/p/concordion-eclipse/) plugin provides content assist and validation for Concordion specifications, integrated into Eclipse's HTML and Web Page (Source) editors.

This plugin currently supports HTML format specifications (not Markdown). It has not been updated with the Concordion 2.0 commands yet, so will show these as errors, but still works well.

----

# Build tools

## Gradle

Concordion tests can be run using the standard test task. A typical build script is:

~~~groovy
apply plugin: 'java'

repositories {
    mavenCentral()
}

dependencies {
    testCompile "org.concordion:concordion:x.y.z"
}

test {
    testLogging.showStandardStreams = true   // display test output on console
    systemProperties['concordion.output.dir'] = "$reporting.baseDir/spec"  // write output to build/reports/spec
    outputs.upToDateWhen { false }   // force tests to run even if code hasn't changed
}
~~~

where _x.y.z_ is replaced by the Concordion version, for example 2.0.0.

If you have created a suite of specifications using the run command, you may want to run only your main fixture class by adding this line inside the `test` block:

~~~groovy
    include '**/MainFixture.*' 
~~~

where <i>MainFixture.java</i> is the fixture class corresponding to the main index page.

(This is not strictly necessary since Concordion will cache test results so that tests are not run multiple times within a single test run. The number of tests reported will be less if you only run the main fixture class.)


## Maven

Concordion tests can be run using the surefire plugin. For example:

~~~xml
<build>
    <plugins>
        ......
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.19.1</version>
            <configuration>
                <systemPropertyVariables>
                    <property>
                        <name>concordion.output.dir</name>
                        <value>target/concordion</value>
                    </property>
                </systemPropertyVariables>
            </configuration>
        </plugin>
        ......
    </plugins>
</build>
~~~

If you have created a suite of specifications using the run command, you may want to run only your main fixture class by adding these lines inside the `configuration` element:

~~~xml
                <includes>
                    <include>**/MainFixture.java</include>
                </includes>
~~~

where <i>MainFixture.java</i> is the fixture class corresponding to the main index page.

(This is not strictly necessary since Concordion will cache test results so that tests are not run multiple times within a single test run. The number of tests reported will be less if you only run the main fixture class.)

There is also a [maven-concordion-reporting-plugin](https://github.com/bassman5/maven-concordion-reporting-plugin) which allows you to incorporate Concordion reports into a Maven site report in the project reports section.

----

# Build servers

## Jenkins

The [HTML Publisher plugin](https://wiki.jenkins-ci.org/display/JENKINS/HTML+Publisher+Plugin) can publish the Concordion results, maintain a per-build history, and let you download the output as a zip file.

__Note:__ as of Jenkins 1.641 / 1.625.3, the new [Content Security Policy](https://wiki.jenkins-ci.org/display/JENKINS/Configuring+Content+Security+Policy) results in Concordion reports no longer being fully functional in Jenkins, with the error `"Refused to apply inline style because it violates the following Content Security Policy directive: "style-src 'self'"`. Concordion reports use inline CSS and Javascript.

While we investigate the issue further, the only workaround is to restore the security vulnerability. To do this, relax the policy to allow `script-src 'unsafe-inline'` and `style-src 'unsafe-inline'` by appending `-Dhudson.model.DirectoryBrowserSupport.CSP=\" sandbox; default-src 'none'; img-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; script-src 'unsafe-inline';\"` to the value of the JAVA_ARGS variable in the Jenkins startup script (/etc/default/jenkins on Linux) and restarting Jenkins. Note that we are still investigating this further - see the [discussion](https://groups.google.com/d/msg/concordion/RSp92D2CNuc/nwYW4yqvEQAJ).
    
{% elsif csharp %}

# Test Runners

## Standalone NUnit Runners

NUnit provides [different runners](http://nunit.org/index.php?p=runningTests&r=2.6.4), which can be used to run your Concordion.NET tests.

### NUnit GUI runner

1. [Download]({{site.baseurl}}/download/{{ page.fixture_language }}/{{ page.spec_type }}) Concordion.NET.
2. Download and [install NUnit version 2.6.4](http://www.nunit.org/index.php?p=installation&r=2.6.4).
3. Copy Concordion.NUnit.dll from the tools directory of your Concordion.NET package into the addin directory of your NUnit installation (&lt;NUnit-installation-path&gt;\bin\addins).
  When updating Concordion.NET, make sure you update this DLL.
4. Load your tests with the NUnit GUI runner.
  Tip: You can open your Visual Studio solutions and/or projects in NUnit, when you activate the [IDE Support Settings - Visual Studio](http://nunit.org/index.php?p=settingsDialog&r=2.6.4).
5. Select the Concordion.NET test you want to run in the tree view and press the Run button.

The GUI Runner will show the test results, for example:

![Display of test results]({{ site.baseurl }}/img/integration-nunit-result.jpg)

The Concordion output specifications can be found in the temp directory (on Windows defined by the environment variable %Temp%, which by default points to C:\Users\&lt;your-windows-user&gt;\AppData\Local\Temp). Concordion.NET can also be [configured]({{site.baseurl}}/coding/{{ page.fixture_language }}/{{ page.spec_type }}#configuration-options) to use a different output directory. 


### Console runner
NUnit provides a [command line client](http://nunit.org/index.php?p=nunit-console&r=2.6.4) for test execution, which can be useful for the automation of test execution and integration into other systems (e.g. build systems). You can run your Concordion.NET tests with the NUnit console runner as follows:

1. [Download]({{site.baseurl}}/download/{{ page.fixture_language }}/{{ page.spec_type }}) Concordion.NET
2. Download and [install NUnit version 2.6.4](http://www.nunit.org/index.php?p=installation&r=2.6.4)
3. Copy Concordion.NUnit.dll from the tools directory of your Concordion.NET package into the addin directory of your NUnit installation (&lt;NUnit-installation-path&gt;\bin\addins).
  When updating Concordion.NET, make sure you update this DLL.
4. In the command line window, [run the nunit-console application](http://nunit.org/index.php?p=nunit-console&r=2.6.4) and pass in the DLL containing your Concordion.NET specifications and tests.

### Debugging Concordion.NET tests

You can debug your Concordion.NET tests as you would for standard NUnit tests, as described in this [debugging NUnit tests article](http://erraticdev.blogspot.co.at/2012/01/running-or-debugging-nunit-tests-from.html).

-----

## NUnit runners integrated in Visual Studio

There are several ways to run Concordion.NET automated tests within Visual Studio (e.g. TestDriven.NET, ReSharper, Test Adapter, etc.).

### TestDriven.NET

You can use [TestDriven.NET](http://testdriven.net) to execute Concordion.NET tests within Visual Studio.

1. [Download]({{site.baseurl}}/download/{{ page.fixture_language }}/{{ page.spec_type }}) Concordion.NET
2. Install TestDriven.NET
3. Copy Concordion.NUnit.dll from the tools directory of your Concordion.NET package into the addin directory of the latest NUnit version in the installation directory of TestDriven.NET (&lt;testdriven.net-installation-path&gt;\NUnit\2.6\addins).
4. To run a test, use the "Run Test(s)" command of TestDriven.NET on your Concordion.NET fixture class ([http://testdriven.net/quickstart.aspx](http://testdriven.net/quickstart.aspx)).

When you run Concordion.NET tests with TestDriven.NET in Visual Studio, you should see an output similar to:

~~~console
------ Test started: Assembly: Kickstart.Spec.dll ------ 
Processed specifications : C:\concordion-test\Kickstart\Spec\HelloWorld\HelloWorld.html
1 passed, 0 failed, 0 skipped, took 0,82 seconds (NUnit 2.6.4).
~~~

### NUnit Test Adapter

Since version 2012, Visual Studio supports [3rd party test runners](http://blogs.msdn.com/b/visualstudioalm/archive/2012/03/08/what-s-new-in-visual-studio-11-beta-unit-testing.aspx) to execute their tests right inside Visual Studio. This allows the NUnit Test Adapter for Visual Studio to run Concordion.NET tests.

Please note that at the time of writing an [issue of the NUnit Test Adapter](https://github.com/nunit/nunit-vs-adapter/issues/9) prevents the execution of Concordion.NET tests based on the NUnit addin. Until the issue is fixed by the NUnit team, you will need to adapt your fixture to extend `ExecutableSpecification` as follows:

~~~csharp
using Concordion.Runners.NUnit;
using NUnit.Framework;

namespace Kickstart.Spec.HelloWorld
{
    [TestFixture]
    public class HelloWorldTest : ExecutableSpecification
...
~~~

When you want to use the Test Runner, make sure to remove the `[assembly: RequiredAddin("ConcordionNUnitAddin")]` from the projects AssemblyInfo.cs. Otherwise the issue in the NUnit Test Adapter prevents your Concordion.NET tests being found by the NUnit Test Adapter.

To run Concordion.NET tests without the need for addins, you have to derive your test fixtures from the Concordion.Runners.NUnit.ExecutableSpecification class and annotate it with the standard NUnit [TestFixture] annotation.

### Resharper

You can run Concordion.NET acceptance tests within Visual Studio with [ReSharper](https://www.jetbrains.com/resharper/)

1. [Download]({{site.baseurl}}/download/{{ page.fixture_language }}/{{ page.spec_type }}) Concordion.NET
2. Install ReSharper
3. Copy Concordion.NUnit.dll from the tools directory of the Concordion.NET package into the addin directory of your ReSharper installation (&lt;resharper-installation-path&gt;\Bin\addins\)
    * Using specific NUnit installation: If you aren't using the build-in NUnit, but your specified NUnit installation (ReSharper -&gt; Options ... -&gt; Tools -&gt; Unit Testing -&gt; NUnit), you have to copy Concordion.NUnit.dll into the used NUnit installation (&lt;nunit-installation-path&gt;\bin\addins\).
    * Make sure you use NUnit version 2.6.4 in any setup.
4. Run your Concordion.NET acceptance tests with ReSharper
    * To run a single test directly from the editor: Click on the ReSharper testing icon next to the line of your fixture class definition and select the Run or Debug option in the context menu.
    * To run multiple tests: Right click on the element containing the tests of interest in the Solution Explorer and select either the Run Unit Tests or Debug Unit Tests option in the context menu.

When running the automated tests with ReSharper you can see the progress and results in the Unit Test Sessions window:

![ReSharper Unit Test Session]({{ site.baseurl }}/img/integration-resharper-test-session.jpg)
{% endif %}