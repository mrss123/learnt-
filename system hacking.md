windows authentication
	security accessible manger database 
		using hash
	NTLM
		storing data from hash in to its own 
			request
			challenge 
			answer
			confirm / denial
	kerberos 

# if one password is once hashed it can never be reversed to the former form
attackers use this database to get the password

types of attack
1. non electronic attack
	1. social engineering
	2. shoulder serving
	3. dumpster deriving (from trash that is written to get info real)
2. active online attack
	1. dictionary attack
	2. brute force attack
	3. rule based attack
	4. password guessing
	5. . default password
	6. trojans 
	7. spyware 
	8. keylogger 
	9. hash injection (i dentifiying the hash and using that password to login)
	10. pass the hash
	11. internal mono log attack
	12. cracking kerberos attask 
	13. pass the ticket 
3. passive online attack
	1. wire sniffing
		1. must be physical
4. offline attack
	1. rainbow table attack
	2. distributed network attack 
	3. password salting
		1. creating new hash by just adding new char on the password


# seclist-master from github

