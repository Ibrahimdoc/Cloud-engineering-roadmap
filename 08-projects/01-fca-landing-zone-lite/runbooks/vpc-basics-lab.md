
# VPC Basics Lab

## What I did 
   - Created a custom VPC (10.0.0.0/16)
   - Added public subnet (10.0.1.0/24) and private subnet (10.0.2.0/24)
   - Attached internet Gateway and set up public route table with 0.0.0.0/0 ---> IGW 
   - Launched EC2 inpublic subnet, installed nginx, opened HTTP port in security group
   - Verified website was reachable (http://...)

## Issues encountered 
   - Forgot to associate public subnet with correct route table ----> no internet 
   - Forgot to add HTTP rule to security group ----> site not loading 
   - Fixed both, then everything worked.

## Key commands 
   - ssh -i ~/.ssh/my-ssh.key.pem ec2-user@<ip>
   - sudo yum install -y nginx 
   - sudo systemctl start nginx
   - sudo systemctl status nginx 




