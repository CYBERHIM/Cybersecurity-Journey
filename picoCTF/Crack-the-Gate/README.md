# Cybersecurity-Journey

Detailed walkthroughs and solutions for CTF challenges, focusing on web exploitation, network security and others .





**picoCTF: "Crack the Gate" Solution**





**Objective**

Bypass a secure login portal using source code analysis and manual HTTP header manipulation.  





**1. Initial Reconnaissance**



  I performed reconnaissance by viewing the page source (Ctrl+U). Tucked away in an HTML comment was a jumbled string of letters—a "developer breadcrumb". Developers often leave notes like this during testing. 

  

**2. Decoding the "Backdoor"**



The string was encoded with ROT13, a substitution cipher that replaces a letter with the 13th letter after it in the alphabet.



**Encoded:** _ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf"_



**Decoded:** _NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"_   



**3. Executing the Attack**



A standard browser login form doesn't allow custom HTTP Headers. To trigger the bypass, I used **curl** to craft a custom request: 



**Bash**



_curl -X POST http://amiable-citadel.picoctf.net:53491/login \

-H "Content-Type: application/json" \

-H "X-Dev-Access: yes" \

-d '{"email":"ctf-player@picoctf.org", "password":"password123"}_'



**4. Result**



The server's logic saw the X-Dev-Access: yes header and skipped the password check. I immediately received the success response and the flag.  



**5. Key Takeaways**



**-Audit Public Code**: Always check public-facing source code for comments or debug paths.  



**-Remove Backdoors:** Any temporary testing functionality must be explicitly removed before production.  Security Through Obscurity: Encoding a note (like ROT13) is NOT security; if a vulnerability is visible, it is exploitable.  



**-Security Through Obscurity:** Encoding a note (like ROT13) is NOT security; if a vulnerability is visible, it is exploitable
