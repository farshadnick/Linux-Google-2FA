# 1- Install Google Authenticator 
in this Step you need to install Goole Authenticator pam module to Refer it as a Step for SSH Authentication
### Ubuntu/Debian
```
sudo apt update
sudo apt-get install libpam-google-authenticator
```
### Redhat 
```
sudo yum install epel-release
sudo yum update
sudo yum install google-authenticator
```

# 2- Get Your QR Code 
We need to get QR Code  for Connecting Our server to our  Google Authenticator APP on our Cell phone
 **NOTE: Before Generating QR code Login with Your Desire user that want to Connect with**  
```
google-authenticator -t -f -d -w 3 -e 4 -r 10 -R 60 
```
    -t : Use TOTP verification
    -f : Write the configuration to ~/.google_authenticator
    -d : Do not allow reuse of previously used tokens.
    -w 3 : The window size of allowed tokens (By default  expire every 30 sec)
    -e 4 : Generate 4 emergency backup codes
    -r 3 -R 30 : Rate-limit. Allow 10 logins every 60 seconds.
![image](https://github.com/farshadnick/linux-for-devops/assets/88557305/733293b2-466f-422a-9c94-cd349e78695d)

Scan this QR code in Your Google authenticator app on cellphone

![image](https://github.com/farshadnick/linux-for-devops/assets/88557305/fbe5619f-385c-491d-a23e-06e428b3b048)


# 3- It time to Config Your PAM SSH Config 
we need to tell our ssh pam config that Google OTP is required for authentication
somehow we add a step in PAM Config

add config below in **/etc/pam.d/sshd**
```
auth required pam_google_authenticator.so

```
and add challengeable authentication in your ssh config **/etc/ssh/sshd_config**

```
 ChallengeResponseAuthentication yes
 KbdInteractiveAuthentication yes

```
and thats it :) restart ssh service
```
systemctl restart sshd
```

![image](https://github.com/farshadnick/linux-for-devops/assets/88557305/3dbf38a3-e229-4084-be25-8d19a99c78ac)
