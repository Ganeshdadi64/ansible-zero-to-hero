CONNECTING TO AWS EC2 (UBUNTU)

==============================
METHOD 1: USING PUBLIC KEY (RECOMMENDED)
==============================

Step 1: Launch EC2 Instance
- Go to AWS Console → EC2 → Launch Instance
- Choose Ubuntu AMI
- Select instance type
- Create or select a Key Pair (.pem file)
- Download the .pem file
- Launch the instance
- Note the Public IP address

Step 2: Give Permission to PEM File (On Local Machine)
chmod 400 your-key.pem

Step 3: Connect to EC2 Using SSH
ssh -i your-key.pem ubuntu@<EC2-PUBLIC-IP>

Example:
ssh -i mykey.pem ubuntu@3.110.25.10

Step 4 (Optional): Copy Your Public Key to EC2
ssh-copy-id -f -o "IdentityFile your-key.pem" ubuntu@<EC2-PUBLIC-IP>

Explanation:
- ssh-copy-id → Copies your public key to remote server
- -f → Force copy
- -o "IdentityFile your-key.pem" → Use this private key
- ubuntu → Default username for Ubuntu AMI


==============================
METHOD 2: USING PASSWORD (NOT RECOMMENDED)
==============================

Step 1: Connect Using PEM Key First
ssh -i your-key.pem ubuntu@<EC2-PUBLIC-IP>

Step 2: Set Password for Ubuntu User
sudo passwd ubuntu

Step 3: Enable Password Authentication
sudo nano /etc/ssh/sshd_config.d/60-cloudimg-settings.conf

Find:
PasswordAuthentication no

Change to:
PasswordAuthentication yes

Save and exit.

Step 4: Restart SSH Service
sudo systemctl restart ssh

Step 5: Login Using Password
ssh ubuntu@<EC2-PUBLIC-IP>


==============================
IMPORTANT NOTES
==============================
- Always prefer Public Key authentication.
- Password login is less secure.
- Do not open port 22 to 0.0.0.0/0 in production.
- Keep your .pem file safe.
