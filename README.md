# cipher-list-creator.jsp Ver.2.0
Creates ciphers list for server.xml of Tomcat NIO/BIO connector
## License
Licensed by MIT License.
## How to use
put cipher-list-creator.jsp into your Tomcat ,and execute it.
## Target Environment
Tomcat + JDK using NIO/BIO connector for SSL.
Tested on Tomcat 7 + Java 7 / Tomcat 8 + Java 8, but written as Tomcat 4.1 + JDK1.4.2 compatible.
## Notice
Cipher list is sorted by recommended order. Next Tomcat will support suite ordering at Tomcat 8.0.21 / 7.0.61 , see [bug 55988](https://bz.apache.org/bugzilla/show_bug.cgi?id=55988).

## JCE Unlimited Strength Jurisdiction Policy Files
To enable 256 bit AES ciphers, install JCE Unlimited Strength Jurisdiction Policy Files from [Java SE Downloads](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
## See also
Almost of hints are here. 

["SSL/TLS, ciphers, perfect forward secrecy and Tomcat" by Mike Noordermeer on Eveoh blog.](https://blog.eveoh.nl/2014/02/tls-ssl-ciphers-pfs-tomcat/)

[Tomcat wiki : HowTo/SSLCiphers](http://wiki.apache.org/tomcat/HowTo/SSLCiphers)

Detail of DHE Ephemeral key problem is here.
- [Oracle community : JSSE - How to configure Diffie-Hellmann (DH) parameters for ssl-handhake](https://community.oracle.com/thread/1533751)
- [OpenJDK : Commit to enable 1024 bit DHE Ephemeral key](http://hg.openjdk.java.net/jdk8/jdk8/jdk/rev/0d5f4f1782e8)

## History
- Ver.1.0 : first release
- Ver.2.0 : Sorting cipher list is supported (issue 2)


