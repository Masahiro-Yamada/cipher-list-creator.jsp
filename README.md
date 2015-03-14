# cipher-list-creator.jsp
Creates ciphers list for server.xml of Tomcat NIO/BIO connector
## License
Licensed by MIT License.
## How to use
put cipher-list-creator.jsp into your Tomcat ,and execute it.
## Target Environment
Tomcat + JDK using NIO/BIO connector for SSL.
Tested on Tomcat 7 + Java7, but written as Tomcat 4.1 + JDK1.4.2 compatible.
## Restrictions
Order of cipher names is **not sorted** by cipher strength.
## See also
Almost of hints of this is from 

["SSL/TLS, ciphers, perfect forward secrecy and Tomcat" by Mike Noordermeer on Eveoh blog.](https://blog.eveoh.nl/2014/02/tls-ssl-ciphers-pfs-tomcat/)

[Tomcat wiki : HowTo/SSLCiphers](http://wiki.apache.org/tomcat/HowTo/SSLCiphers)

Detail of DHE Ephemeral key problem is here.
  [Oracle community : JSSE - How to configure Diffie-Hellmann (DH) parameters for ssl-handhake](https://community.oracle.com/thread/1533751)
  [OpenJDK : Commit to enable 1024 bt DHE Ephemeral key](http://hg.openjdk.java.net/jdk8/jdk8/jdk/rev/0d5f4f1782e8)


