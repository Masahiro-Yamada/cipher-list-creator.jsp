# cipher-list-creator.jsp
Tomcat �� NIO/BIO�R�l�N�^�p�� server.xml �ɐݒ肷�� ciphers �̃f�[�^�𐶐����܂��B
## ���C�Z���X
MIT���C�Z���X�Ƃ��܂��B
## �g����
Tomcat �� cipher-list-creator.jsp ��z�u���Ď��s���܂��B
## �Ώۂ̊�
Tomcat + JDK �Ł@NIO/BIO �R�l�N�^��SSL�Ɏg�p���Ă�����B
Tomcat 7 + Java 7 �Ńe�X�g���܂������ATomcat 4.1 + JDK1.4.2 �݊��̃R�[�h�Ƃ��Ă���܂��B
## ���ӎ���
�Í��̃��X�g�͈Í����x���ɂ̓\�[�g����Ă��܂���
## JCE�� ���������x�̊Ǌ��|���V�[�t�@�C��
AES��256�r�b�g�Í����g�p����ɂ́AJCE Unlimited Strength Jurisdiction Policy Files �� [Java SE Downloads](http://www.oracle.com/technetwork/java/javase/downloads/index.html) ����
�_�E�����[�hs��C���X�g�[������K�v������܂��B
## �Q�l
�قƂ�ǂ̃q���g�͈ȉ��̃T�C�g���瓾�����̂ł��B

["SSL/TLS, ciphers, perfect forward secrecy and Tomcat" by Mike Noordermeer on Eveoh blog.](https://blog.eveoh.nl/2014/02/tls-ssl-ciphers-pfs-tomcat/)

[Tomcat wiki : HowTo/SSLCiphers](http://wiki.apache.org/tomcat/HowTo/SSLCiphers)

DHE Ephemeral key �̖��
- [Oracle community : JSSE - How to configure Diffie-Hellmann (DH) parameters for ssl-handhake](https://community.oracle.com/thread/1533751)
- [OpenJDK �� 1024 bit DHE Ephemeral key��L���ɂ���R�~�b�g](http://hg.openjdk.java.net/jdk8/jdk8/jdk/rev/0d5f4f1782e8)
