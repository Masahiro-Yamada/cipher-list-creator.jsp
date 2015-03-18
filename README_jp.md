# cipher-list-creator.jsp
Tomcat の NIO/BIOコネクタ用に server.xml に設定する ciphers のデータを生成します。
## ライセンス
MITライセンスとします。
## 使い方
Tomcat に cipher-list-creator.jsp を配置して実行します。
## 対象の環境
Tomcat + JDK で　NIO/BIO コネクタをSSLに使用している環境。
Tomcat 7 + Java 7 でテストしましたが、Tomcat 4.1 + JDK1.4.2 互換のコードとしてあります。
## 注意事項
暗号のリストは暗号強度順にはソートされていません
## JCEの 無制限強度の管轄ポリシーファイル
AESの256ビット暗号を使用するには、JCE Unlimited Strength Jurisdiction Policy Files を [Java SE Downloads](http://www.oracle.com/technetwork/java/javase/downloads/index.html) から
ダウンロードしてインストールする必要があります。
## 参考
ほとんどのヒントは以下のサイトから得たものです。

["SSL/TLS, ciphers, perfect forward secrecy and Tomcat" by Mike Noordermeer on Eveoh blog.](https://blog.eveoh.nl/2014/02/tls-ssl-ciphers-pfs-tomcat/)

[Tomcat wiki : HowTo/SSLCiphers](http://wiki.apache.org/tomcat/HowTo/SSLCiphers)

DHE Ephemeral key の問題
- [Oracle community : JSSE - How to configure Diffie-Hellmann (DH) parameters for ssl-handhake](https://community.oracle.com/thread/1533751)
- [OpenJDK で 1024 bit DHE Ephemeral keyを有効にするコミット](http://hg.openjdk.java.net/jdk8/jdk8/jdk/rev/0d5f4f1782e8)
