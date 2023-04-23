# Tying it all together

## Deploy the YouTube Bot App

### Create IAM User 
1. Create new IAM user
2. Attach **AmazonS3FullAccess** policy
3. Create new Security credential **Access Key** to send programmatic calls to AWS from youre app

### Create S3 Bucket 
1. Create an S3 Bucket

### Create the EC2 instance 

1. Create an EC2 instance, as follows:
    1. **Ubuntu** AMI.
    2. **t2.micro** instance type (or equivalent micro type from another generation).
    3. Choose your existed key-pair (create if needed).
    4. All other configurations are default
2. Your instance should have a public ip4v address. Connect to your instance via SSH by click on **Connect** button in the instance page, then **SSH Client**, follow the instructions there.

   
### Deploy the app

1. Within the terminal session of your instance, install app dependencies:
   ```shell
   sudo apt-get update -y  
   sudo apt-get install nodejs npm ffmpeg -y
   ```
2. Get the app code by
   ```shell
   git clone https://github.com/ofekb/youtubeBot.git
   ```

4. Update the index.js file with your IAM user Security credential Access Key
   ```javascript
   const s3 = new AWS.S3({ region: AWS_REGION, accessKeyId: 'YOURKEY', secretAccessKey: 'YOURSECRET' });
   ```

3. Then install NodeJs dependencies by
   ```shell
   cd /youtubeBot
   npm install
   ```
4. Create `.env` file containing the following environment variables. Change `<your-bucket-name>` to your S3 bucket name.
   ```shell
   echo "BUCKET_NAME=<your-bucket-name>" >> .env
   ```

5. Redirect port 80 to port 3000 using iptable
   ```shell
   $sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 3000﻿
   //Varify the port has redirect successfully
   $sudo iptables -t nat -L 
   ```

5. Run the app by `npm start`, and test it. 