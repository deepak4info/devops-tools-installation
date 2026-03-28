SSL Certificate? 

create key 

openssl genrsa -out ssl-2026.key 2048                      ------------------ ssl-2026 --- is key name you can chang it. 

it create key then after 
Generate CSR (Certificate Signing Request) 

 openssl req -new -key ./ssl-2026.key -out ./ssl-2026.csr 


------Generate Self-Signed Certificate----------- 

 openssl x509 -req -days 365 \
-in ssl-2026.csr \
-signkey ssl-2026.key \

 
