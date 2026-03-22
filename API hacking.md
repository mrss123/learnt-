# Application Program Interface 
#whatis : it is simply a program that functions as a waiter but for a system 
	resicve a request from a user and and take it to back-end program and vice versa 
#benefits : make a data retrivial very easy 
#drawbacks : can be manipulated if the it is no configured properly

## what and where to look
- over permissioned  End point 
- Return too much information
- Access unauthorithed information 
- expose logic flaws 
# most known ATTACKS 
## BROKEN OBJECT LEVEL AUTHORIZATION (BOLA)
#whatis : simply the AUTH is broken which leads to User A can access User B data
	this can cause a very significant data loss 

#steps_to_identify_vul :
	the attacker would intentionaly send a wrong or unintended request to see what type of error would cause 
	but this might actually work some times 
		**example**: on COIN BASE--> a platform to market bitcoin and ethrum and other staff to 
			the hacker sent deposited a dollar and wanted to buy etherium and he did then same hacker wanted to sell that ethrium and cash out the dollar and here is the thing 
			on the logic he sent ethrium identifyer as a bitcoin and sent it the logic was not present so the $1000 ethrium got sold as $43000 bitcoin on the platform
## BROKEN AUTHENTICATION
#whatis : weak or poor authentication on our APIs. Could be no CAPTCHA, no two factor authentication, brute forcing, credential stuffing and these types of things.
API breaches are happening because APIs are left completely unsecured with no authentication
**example**: 
	once duolingo API leaks a user information (a ton of them) by only typing a user email address only on the url or curl or burpsuite using a authenticated cookie 

## Broken Object Property Level Authorization
#whatis : mass assignment through API. like reading changing others data or owns data in un holy way, or changing object element , or API could send too much info (unnecessary)
**example**: 
	Venmo, the payment platform. It's a social platform, so on their homepage they had a feature that would show you the last 10 or 20 transactions in real time, but anonymized data, naturally. So it might have my first name, but not my last name or account details.
	But as you can see here this attacker found the API powering that feature of the homepage. It was a public API, no authorization required. So they could query it to their heart's content. But here's the key. When they queried that API directly, as opposed to viewing it through the UI, the API did not return anonymized data. It returned to the whole record. First name and last name and so forth.
	And so they wrote a script and harvested over 200 million transactions out of the platform in full, complete detail
## Unrestricted Resource Consumption
#whatis : happens when the API do not restrict how much data can recieve and validate them all in single time 
this will led to bruteforeing the API, DOS, DDOS
**example**: sending lots of email to validate to a OTP or MFA - this will cause the server to respond for each one of them which eventually end up to crush or to take a lot of memory to validate those emails 
## Broken Function Level Authorization

#whatis : abuse of API functionality to improper modify objects
	CRUD (**passive:** GET **active** : PUT AND DELETE )
	**EXAMPLE** : 
		privilege escalation (from normal user to premium or to admin )
		delete  an invoice 
		delete a debt 
## Unrestricted Access to sensitive Business Flows 
#whatis : abuse of a legitimate business flow 
	making the system do things that is not intended to do 
## server side request forgery
#whatis : tricking a server to go where it really should't gp 
	exploiting URL inputs to make a request to malicious or 3rd party server  
## server mis-configuration 
#whatis hardening unnecessary service 
	use of bots to scan detect and exploit mis-configuration 
## Improper Inventory Management
- awareness and visibility
#whatis : when a hacker found out there are different versions of API that have no same security protocol as the current one the attacker will try to exploit it 
