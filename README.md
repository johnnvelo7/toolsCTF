# toolsCTF
some of my scripts used for playing CTF

# EPT1911 - RE Writeup

**Task Description:**

![ept1911](Picture1.png)

After unzipping the file, I was given only 1 tool to work with which was the ***KeyGen.exe***

![ept1911](Picture2.png)

![ept1911](Picture3.png)

A weird key generator tool with title ***“Razor”***. Doing some light ***OSINT*** we find that razor is an actual tool used for keygen. 

![ept1911](Picture4.png)

![ept1911](Picture5.png)

We also find that it’s a *Norwegian demo group* since 1986.

**Time to Check the file!**

I see that it’s a tool compiled using ***.NET***. This is good news since we can use ***dnSpy*** to see the source code of the program.

![ept1911](Picture6.png)

![ept1911](Picture7.png)

*The source code*

![ept1911](Picture8.png)

*This checks if the computer is part of the domain contoso.com, then prints the flag.*

![ept1911](Picture9.png)

*The encoded part of the flag.*

**In summary**

This program checks if the machine is part of *contoso.com* domain. If it is, it will print *“EPT{“* along with the strings decoded from the *“Settings.Default.encpw”* code. For each character in encpw, it adds *42* and converts the character back to text. 

Knowing what to do next, I used this python script to decode the array of strings from *“encpw”* code back to text, as well as adding 42 in it.

![ept1911](Picture10.png)

![ept1911](Picture11.png)

**SIKE**

I got this flag, but it was wrong. I was stuck because I didn’t know if I was missing something, or if the server was having problems. That’s why I contacted the guys from the event and got a tip to make sure if I have the *“complete”* flag. Hmmmm…

I then went back to the source code and found the crucial part that I missed. 

![ept1911](Picture12.png)

This was the part of the code which adds *‘!’* to the end of the string, completing the flag.

![ept1911](Picture13.png)

After running the new script, I finally got the correct ***flag!***

![ept1911](Picture14.png)




<html>
<head>
<style type="text/css">
body { color: #bbbbbb; font-family: Segoe UI, Verdana, Arial, Helvetica, sans-serif; font-size: 11px; }
h2 { font-family: Segoe UI, Verdana, Arial, Helvetica, sans-serif; }
h3 { font-family: Segoe UI, Verdana, Arial, Helvetica, sans-serif; font-size: 11px; }
p { margin: 0 0 10px 0; }
a { color: #bddefc; }
</style>
</head>
<body>
<h2>Cross-origin resource sharing: arbitrary origin trusted</h2>
<hr>
<table cellpadding="0" cellspacing="0">
<tr><td width=57px>Severity:  </td><td>Information</td></tr>
<tr><td>Confidence:  </td><td>Certain</td></tr>
<tr><td>URL:  </td><td><https://digital.ntcm.com.ph/CompanyController/register</td>></tr>
</table>
<hr>
<h3>Issue detail</h3>
The application implements an HTML5 cross-origin resource sharing (CORS) policy for this request that allows access from any domain.<br><br>The application allowed access from the requested origin <strong><https://ngvjzhqyybup.com</strong>><br><br>If the application relies on network firewalls or other IP-based access controls, this policy is likely to present a security risk.<br><br>Since the Vary: Origin header was not present in the response, reverse proxies and intermediate servers may cache it. This may enable an attacker to carry out cache poisoning attacks.

<br><br>
<h3>Issue background</h3>
<p>An HTML5 cross-origin resource sharing (CORS) policy controls whether and how content running on other domains can perform two-way interaction with the domain that publishes the policy. The policy is fine-grained and can apply access controls per-request based on the URL and other features of the request.</p><p>
Trusting arbitrary origins effectively disables the same-origin policy, allowing two-way interaction by third-party web sites. Unless the response consists only of unprotected public content, this policy is likely to present a security risk.</p>
<p>If the site  specifies the header Access-Control-Allow-Credentials: true, third-party sites may be able to carry out privileged actions and retrieve sensitive information. Even if it does not, attackers may be able to  bypass any IP-based access controls by proxying through users'  browsers.</p>
<h3>Issue remediation</h3>
<p>Rather than using a wildcard or programmatically verifying supplied origins, use a whitelist of trusted domains.</p>

<br><br>
<h3>References</h3>
<ul><li><a href="https://portswigger.net/web-security/cors ">Web Security Academy: Cross-origin resource sharing (CORS)</a></li><li><a href="https://portswigger.net/research/exploiting-cors-misconfigurations-for-bitcoins-and-bounties ">Exploiting CORS Misconfigurations</a></li></ul>
<h3>Vulnerability classifications</h3>
<ul><li><a href="https://cwe.mitre.org/data/definitions/942.html ">CWE-942: Overly Permissive Cross-domain Whitelist</a></li></ul>
</body>
</html>
