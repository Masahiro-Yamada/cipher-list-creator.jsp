<%--
This code is licenced by MIT License
All hints are described in following:

"SSL/TLS, ciphers, perfect forward secrecy and Tomcat" by Mike Noordermeer on Eveoh blog.
  https://blog.eveoh.nl/2014/02/tls-ssl-ciphers-pfs-tomcat/

Tomcat wiki : HowTo/SSLCiphers 
 http://wiki.apache.org/tomcat/HowTo/SSLCiphers

Detail of DHE Ephemeral key problem is here:
  https://community.oracle.com/thread/1533751
  http://hg.openjdk.java.net/jdk8/jdk8/jdk/rev/0d5f4f1782e8

---------------------------------------------------------------------------------
The MIT License (MIT)

Copyright (c) 2015 Masahiro YAMADA

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
---------------------------------------------------------------------------------
--%><%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    session="false"
    import="java.util.HashMap"
    import="javax.net.ssl.SSLServerSocketFactory"
%><%!
    /** DHE ephemeral key size is weak or secure */
    private static final boolean hasWeakDHE;
    static {
        boolean isJava5_6_7 = false;
        try {
            //Temporal is available on Java8 or later.
            Class.forName("java.time.temporal.Temporal"); //$NON-NLS-1$
            //Java 8 or later
        } catch (ClassNotFoundException e) {
            //Java 7 or older
            try {
                //StringBuilder is available on Java5 or later.
                Class.forName("java.lang.StringBuilder"); //$NON-NLS-1$
                //Now current Java VM is Java 5/6/7
                /*
                 *========== IMPORTANT NOTE ========================================================
                 * JSSE shipped with Sun/Oracle Java 5 / 6 / 7 uses INSECURE ephemeral DHE key size.
                 * see:    https://community.oracle.com/thread/1533751
                 *         http://hg.openjdk.java.net/jdk8/jdk8/jdk/rev/0d5f4f1782e8
                 *==================================================================================
                 */
                 isJava5_6_7 = true;
            } catch (ClassNotFoundException e2) {
                //J2SE 1.4.2 or older
            }
        }
        hasWeakDHE = isJava5_6_7;
    }
%><!DOCTYPE html>
<html>
<head>
<meta charset="charset=UTF-8">
<title>Cipher list</title>
<script>
</script>
</head>
<body>
<%
    if (request.getQueryString() == null) {
%>
    <form action="<%= response.encodeURL(request.getRequestURI()) %>">
        <table border=1>
            <caption>Cipher Options</caption>
            <tr><th>DHE</th>
                <td>
                    <select name="DHE">
                        <option value="Auto">Auto (use result of version detection)</option>
                        <option value="Disable">Disable</option>
                        <option value="Enable">Enable</option>
                    </select>
                </td>
            </tr>
            <tr><th>DH (optional)</th>
                <td>
                    <select name="DH">
                        <option value="Disable">Disable</option>
                        <option value="Enable">Enable</option>
                    </select>
                </td>
            </tr>
            <tr><th>ECDH (optional)</th>
                <td>
                    <select name="ECDH">
                        <option value="Disable">Disable</option>
                        <option value="Enable">Enable</option>
                    </select>
                </td>
            </tr>
            <tr><th>AES 256bit (optional)</th>
                <td>
                    <select name="AES256">
                        <option value="Disable">Disable</option>
                        <option value="Enable">Enable</option>
                    </select>
                </td>
            </tr>
            <tr><th>Support of Old Client</th>
                <td>
                    <select name="legacy">
                        <option value="">None</option>
                        <option value="3DES">Triple DES</option>
                        <option value="RC4">RC4-SHA</option>
                    </select>
                </td>
            </tr>
        </table>
        <input type="submit" value="go">
    </form>
<%
    } else {
%>
    <h1>Selected ciphers</h1>
    <p>
<%       
        final String paramDHE = request.getParameter("DHE");
        final String paramECDH = request.getParameter("ECDH");
        final String paramDH = request.getParameter("DH");
        final String paramAES256 = request.getParameter("AES256");
        final String paramLegacy = request.getParameter("legacy");
        //DHE
        boolean disableDHE = hasWeakDHE;
        if (paramDHE != null) {
            if (paramDHE.equals("Disable")) {
                disableDHE = true;
            } else if (paramDHE.equals("Enable")) {
                disableDHE = false;
            }
        }
        //ECDH
        final boolean disableECDH = (paramECDH == null) || !paramECDH.equals("Enable");
        //DH
        final boolean disableDH = (paramDH == null) || !paramDH.equals("Enable");
        //AES256
        final boolean disableAES256 = (paramAES256 == null) || !paramAES256.equals(paramAES256);
        //Legacy
        final String xpCompatibleCipherName;
        if (paramLegacy == null) {
            xpCompatibleCipherName = null;
        } else if (paramLegacy.equals("3DES")) {
            xpCompatibleCipherName = "SSL_RSA_WITH_3DES_EDE_CBC_SHA";
        } else if (paramLegacy.equals("RC4")) {
            xpCompatibleCipherName = "SSL_RSA_WITH_RC4_128_SHA";
        } else {
            xpCompatibleCipherName = null;
        }

       
        final String[] availableCiphers = ((SSLServerSocketFactory)SSLServerSocketFactory.getDefault()).getSupportedCipherSuites();
        boolean first = true;
        StringBuffer wk = new StringBuffer();
        boolean aes256Found = false;
        for(int i = 0; i < availableCiphers.length; i++) {
            final String cipherName = availableCiphers[i];
            //Check AES256 avilable or not ()
            if (cipherName.contains("_AES_256_")) {
                aes256Found = true;
            }
           
            //1.Disable weak ciphers
            if (cipherName.contains("_NULL_")) continue;
            if (cipherName.contains("_EXPORT")) continue; // EXPORT and EXPORT1024
            if (cipherName.contains("_DES_")) continue; // Single DES
            if (cipherName.contains("_RC4_")||cipherName.contains("_3DES_")) {//RC4 , Triple DES
                //Enable when legacy support is needed
                if (!cipherName.equals(xpCompatibleCipherName)) continue;
            }
           
            //2.Disable weak hash key MD5
            if (cipherName.contains("_MD5")) continue;
            //3.Disable ANON Server Authentification
            if (cipherName.contains("_anon_")) continue;
       
            //4.Not used Key Exchange
            if (cipherName.contains("_DSS_")) continue; // DSS (not needed for RSA certificate)
            if (cipherName.contains("_ECDSA_")) continue; // ECDSA (not needed for RSA certificate)
            if (cipherName.contains("_KRB5_")) continue; // KRB5 (not used)
            //5.Skip dummy cipher
            if (cipherName.equals("TLS_EMPTY_RENEGOTIATION_INFO_SCSV")) continue;
   
            //6.Optional Cipher
            if (disableAES256 && cipherName.contains("_AES_256_")) continue;
            //7.Optional Key Exchange
            if (disableDHE && cipherName.contains("_DHE_")) continue;
            if (disableECDH && cipherName.contains("_ECDH_")) continue;
            if (disableDH && cipherName.contains("_DH_")) continue;
           
            if (first) {
                wk.append("cipher=\"");
                first = false;
            } else {
                wk.append(',');
            }
            wk.append(cipherName);
%>
    <%= cipherName %><br>
<%
        }
        if (first) {
            wk.append("no available cipher");
        } else {
            wk.append('\"');
        }
%>
    </p>
    <h1>server.xml config:</h1>
    <code><%= wk.toString() %></code>
    <p><a href="<%= response.encodeURL(request.getRequestURI()) %>">back</a>
    <% if ((!disableAES256) && !aes256Found) {%>
    <h1>AES256</h1>
    AES256 is not supported :  install the JCE Unlimited Strength Jurisdiction Policy Files from
    <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html">Java SE download page</a>.
    <% } %>
<%
    }
%>
</body>
</html>