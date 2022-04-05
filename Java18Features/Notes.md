




Java 18 has been generally available since 22 March 2022. This release delivers thousands of performance, stability, and security improvements. It also includes enhancements (JEPs) to the platform to improve developer productivity.

You can download [production ready binaries available from Oracle](https://openjdk.java.net/projects/jdk/18/). Other vendor specific version are WIP. 

**NOTE: Oracle JDK 18 is not a long-term support (LTS) release.**

---

## JDK enhancements proposals implemented in Java 18

* **JEP 400: UTF-8 by Default**

    Sets UTF-8 as the default charset of the standard Java APIs. With this change, APIs that depend on the default charset will behave consistently across all implementations, operating systems, locales, and configurations.

* **JEP 408: Simple Web Server**
    
    A command-line tool and API to start a minimal web server that serves static files only. This tool will be useful for prototyping, ad-hoc coding, and testing purposes, particularly in educational contexts.


* **JEP 418: Internet-Address Resolution SPI**
    
    Defines a service-provider interface (SPI) for host name and address resolution, so that java.net.InetAddress can make use of resolvers other than the platform's built-in resolver.

*  **JEP 413: JEP Code Snippets in Java API Documentation**

    Introduces the @snippet tag for JavaDoc’s Standard Doclet to simplify the inclusion of example source code in API documentation.

* **JEP 416: Reimplement Core Reflection with Method Handles** 
    Reimplements java.lang.reflect.Method, Constructor, and Field on top of java.lang.invoke method handles. By making method handles the underlying mechanism for reflection, it reduces the maintenance and development cost of both the java.lang.reflect and java.lang.invoke APIs.

*  Preview of features to be released in future versions 
    * JEP 417: Vector API (Third Incubator)
    Introduces a platform-agnostic, reliable way of clearly and concisely expressing a wide range of vector computations for supported CPU architectures.

    * JEP 419: Foreign Function and Memory API (Second Incubator)
    Greatly simplifies the tools and knowledge required to create Java programs that utilize native. This will allow Java developers access to specialized non-Java libraries. 

    * JEP 420: Pattern Matching for switch (Second Preview)
    Improves Java development productivity by expanding the expressiveness and applicability of switch expressions and statements.  Allowing pattern matching in switch will make expressing complex data-oriented queries more concise and safer.

* Depcrecations
    * JEP 421: Deprecate Finalization for Removal
    Prepares developers for the eventual future removal of the outdated Finalization feature by providing simple tooling to help detect reliance on the deprecated functionality.
    


----    

## Key JEP Walkthrough

### JEP400 - UTF-8 By default

For a long time, Java developers have had to deal with the fact that the standard Java character set varies depending on the operating system and language settings. The default character set was determined during, and depended on the user's locale and default encoding. For example, on Windows the charset was windows-1252, whereas on macOS it was UTF-8. 

With Java 18, **UTF-8 will be default** for all operating systems.

```Java
// Run this code in Linux or MacOS
try (FileWriter fw = new FileWriter("happy-coding.txt");
    BufferedWriter bw = new BufferedWriter(fw)) {
  bw.write("ハッピーコーディング！");
}
```

```Java
// Then Load the file in Windows
try (FileReader fr = new FileReader("happy-coding.txt");
    BufferedReader br = new BufferedReader(fr)) {
  String line = br.readLine();
  System.out.println(line);
} 

```

```Java
//The following output is displayed. That is because Linux and macOS store the file in UTF-8 format, and Windows tries to read it in Windows-1252 format.

ãƒ?ãƒƒãƒ”ãƒ¼ã‚³ãƒ¼ãƒ‡ã‚£ãƒ³ã‚°ï¼?
```


It becomes even more chaotic because newer class library methods do not respect the default character set but always use UTF-8 if no character set is specified. These methods include, for example, Files.writeString(), Files.readString(), Files.newBufferedWriter(), and Files.newBufferedReader().


---

### JEP 408

Almost all modern programming languages allow starting up a rudimentary HTTP server to, for example, quickly test some web functionality. It is recommended for prototyping, testing, and debugging. 

The easiest way to start the provided webserver is the jwebserver command. It starts the server on localhost:8000 and provides a file browser for the current directory.

```sh
$ jwebserver
Binding to loopback by default. For all interfaces use "-b 0.0.0.0" or "-b ::".
Serving /home/sven and subdirectories on 127.0.0.1 port 8000
URL http://127.0.0.1:8000/
```

```java
import java.net.InetSocketAddress;
import java.nio.file.Path;
import com.sun.net.httpserver.SimpleFileServer;
import static com.sun.net.httpserver.SimpleFileServer.OutputLevel;

public class App {
public static void main( String[] args ){
    // Create a simple file server, given the socket address, path and output level
    var server = SimpleFileServer.createFileServer(new InetSocketAddress(8000), Path.of("/home/java"), OutputLevel.VERBOSE);

    // Starting the server
    server.start();

    // A friendly message to get started.
    System.out.println( "We are getting started.. Hello World!" );
  }
}
```

If you are wondering whether you can implement a full-blown production web server using the simple web server APIs, the answer is no. The web server is definitely not intended for that use. It is very rudimentary and has the following limitations:
* The only supported protocol is HTTP/1.1.
* HTTPS is not provided. 
* Only the HTTP GET and HEAD methods are allowed.


--- 

### JEP418 - Internet-Address Resolution SPI


Typically, to find out the IP address(es) for a hostname in Java, we can use `InetAddress.getByName(…)` or `InetAddress.getAllByName(…)`. `InetAddress` inturn uses the operating system's resolver (hosts file and configured DNS servers). 

This hardwiring imepedes prototying and testing activities. Additionaly it makes integration with emerging network protocols harder.

[JDK Enhancement Proposal 418](https://openjdk.java.net/jeps/418) introduces a Service Provider Interface (SPI) to allow the platform's built-in default resolver to be replaced by other resolvers.


---

### JEP 413 - Code snippets in Java API documentation

Developers mostly insert code samples in Javadoc comments using the `@code` annotation. This technique is somewhat limited and requires workarounds. 

As a new approach, JEP 413 proposes using the `@snippet` tag. As noted in the JEP, the tag __"can be used to declare both inline snippets, where the code fragment is included within the tag itself, and external snippets, where the code fragment is read from a separate source file."__

The Javadoc utility now also includes options to specify links, highlight code, and more. See [JEP 413](https://openjdk.java.net/jeps/413) for more details.

```java

/**
 * How to write a text file :
 *
 * <pre><b>try</b> (BufferedWriter writer = Files.<i>newBufferedWriter</i>(path)) {
 *  writer.write(text);
 *}</pre>
 */

```

```java
/**
 * How to write a text file:
 *
 * <pre>{@code try (BufferedWriter writer = Files.newBufferedWriter(path)) {
 *  writer.write(text);
 *}}</pre>
 */
```

Here are two examples of inline snippet with hightlighting features:

```java
/**
 * How to write a text file:
 *
 * {@snippet :
 * try (BufferedWriter writer = Files.newBufferedWriter(path)) {
 *   writer.write(text);
 * }
 * }
 */
```


```java
/**
 *
 * {@snippet :
 * try (BufferedWriter writer = Files.newBufferedWriter(path)) {
 *   writer.write(text);  // @highlight substring="text"
 * }
 * }
 */
```

---

### JEP 416: Reimplement Core Reflection with Method Handles

Reflection allows an executing Java program to examine or "introspect" upon itself, and manipulate internal properties of the program. 

If you have used Java reflection before, you will know that there are more than one way to analyze/modify capabilities of a class at runtume. 
It includes: 
* Core reflection
* Method handles
* Using Native methods

Maintaining all three variants means a considerable effort for the JDK developers. Therefore, as part of [JDK Enhancement Proposal 416](https://openjdk.java.net/jeps/416), it was decided to reimplement the code of the reflection classes java.lang.reflect.Method, Field, and Constructor using method handles and thus reduce the development effort.


---

## Summary

In addition to the JDK Enhancement Proposals and changes to the class libraries presented in this post, there are other change beyond this post's scope. You can find a complete list in the [JDK 18 release notes](https://jdk.java.net/18/release-notes).



---


## References

Java GA announcement
https://jdk.java.net/18/

Downloads
https://openjdk.java.net/projects/jdk/18/

Release notes
https://jdk.java.net/18/release-notes 

Press release
https://www.oracle.com/news/announcement/oracle-releases-java-18-2022-03-22/

Technical Blog
https://blogs.oracle.com/java/post/the-arrival-of-java-18

History
https://en.wikipedia.org/wiki/Java_version_history

Indepth review with examples
https://www.happycoders.eu/java/java-18-features/

Article on Java 18 by Redhat
https://developers.redhat.com/articles/2022/01/27/whats-new-developers-java-18