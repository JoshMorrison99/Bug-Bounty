Web Application Server Backend | Parsing Result | Example
---|---|---
ASP.NET / IIS | All occurrences concatenated with a comma | color=red,blue
ASP / IIS | All occurrences concatenated with a comma | color=red,blue
.NET Core 3.1 / Kestrel | All occurrences concatenated with a comma | color=red,blue
.NET 5 / Kestrel | All occurrences concatenated with a comma | color=red,blue
PHP / Apache | Last occurrence only | color=blue
PHP / Zeus | Last occurrence only | color=blue
JSP, Servlet / Apache Tomcat | First occurrence only | color=red
JSP, Servlet / Oracle Application Server 10g | First occurrence only | color=red
JSP, Servlet / Jetty | First occurrence only | color=red
IBM Lotus Domino | Last occurrence only | color=blue
IBM HTTP Server | First occurrence only | color=red
node.js / express | First occurrence only  | color=red
mod_perl, libapreq2 / Apache | First occurrence only | color=red
Perl CGI / Apache | First occurrence only | color=red
mod_wsgi (Python) / Apache | First occurrence only | color=red
Python / Zope | All occurrences in List data type | color=`[‘red’,’blue’]`
