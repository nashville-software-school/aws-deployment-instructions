# Deploying Your Capstones with Amazon Web Services

### Table of Contents
1. [Amazon Web Services](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#amazon-web-services-aws)
2. [Why Deploy on AWS?](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#why-deploy-on-aws)
3. [Getting Started](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#getting-started)
4. [AWS Elastic Beanstalk and the AWS Elastic Beanstalk CLI](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#aws-elastic-beanstalk-and-the-aws-elastic-beanstalk-cli)
5. [Deploying Your React Application with AWS Elastic Beanstalk](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#deploying-your-react-application-with-aws-elastic-beanstalk)
6. [Deploying Your Django Application with AWS Elastic Beanstalk](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#deploying-your-django-application-with-aws-elastic-beanstalk)
7. [Deploying Your React Application on AWS Amplify](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#deploying-your-react-application-on-aws-amplify)

## Amazon Web Services (AWS)
[AWS](https://aws.amazon.com), a subsidiary of Amazon, is the leading provider of 
on-demand cloud computing platforms and APIs. According to 
[Canalys](https://www.canalys.com/newsroom/global-cloud-market-q4-2020), AWS owns 
around one third of the cloud computing market and has reported revenue of $12.7 billion 
for Q4 2020. Its competitors include [Microsoft Azure](https://azure.microsoft.com/en-us/free/) 
and [Google Cloud Platform](https://cloud.google.com).

## Why Deploy on AWS?
Your ability to display your capstones running on AWS servers will signal to employers your 
ability to work in a modern software development team, many of which are now responsible 
for the deployment, monitoring, and maintenance of their software stack in the cloud.

## Getting Started

### AWS Account
Working with AWS requires an AWS account. Fortunately, as of March 2021 Amazon still offers 
an extended free-tier account that gives free access for one year to all the services you 
will need to deploy your application. Even still, note that setting up an AWS account requires 
an active credit or debit card, which will be billed if your usage exceeds the free tier limit. 
These instructions will teach you how to set up daily billing alerts as an additional tool to 
prevent excessive unwanted charges. Moreover, AWS automatically tracks your service usage and will 
alert you if you have reached 85% of the usage limit for one or more AWS Free Tier-eligible services. 
Nonetheless, **prioritize security** to protect your assets.

#### Creating and Activating Your Account
1. Open the [Amazon Web Services home page](https://aws.amazon.com/).
2. Choose **Create an AWS Account**.
3. Enter your account information. [Choose a strong password](https://www.lastpass.com/password-generator), 
   document the information securely, and then choose **Continue**.
   
**You will notice that this document repeatedly mentions securing your credentials. The 
importance of this cannot be overstated. Your finances are now tied to your AWS credentials. In the 
wrong hands, an AWS account can accrue charges rapidly, even with a daily budget and AWS usage 
tracking. Consider using a password manager such as [LastPass](https://www.lastpass.com/) or 
[1Password](https://1password.com/landing/password-manager/), services whose yearly costs 
mirror the cost of one hour of the [most expensive EC2 instances](https://instances.vantage.sh/). 
Pay for basic credential management to avoid paying for someone else's unauthorized use of your 
account.**

4. Choose **Personal**.
5. Enter your personal information.
6. Enter your billing information.
7. Confirm your identity with a phone number.
8. Enter the verification code.
9. Select the **Basic Plan**.

You will receive an email to confirm that your account has been created. Once that 
happens, you can access your account with the email address and password you used to 
register. 

#### Setting Up an IAM User and Root Multi-factor Authentication
1. Open the [Amazon Web Services home page](https://aws.amazon.com/).
2. Choose **Sign In to the Console**.
3. Sign in as the **Root user**.
4. [Set up a virtual Multi-factor Authentication device for your AWS account root user.
   ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html#enable-virt-mfa-for-root)
   **Setting up multi-factor authentication is another vital step toward securing your account.**
5. Navigate to **IAM** with the search bar at the top of the screen.
6. Choose **Users** on the left navigation menu.
7. Select **Add user**.
8. Enter a **User name** and check both **Programmatic** and **AWS Management 
   Console** access.
9. Choose a **Console password** type and determine whether you would like to 
   **Require password reset** for the user at initial sign-in.
10. Choose **Next: Permissions**.
11. Choose **Create group**.
12. Enter `Admin` in the **Group name** box and select **Administrator Access** in the 
    policy name menu.
13. Choose **Create group**.
14. Choose **Next: Tags**.
15. Choose **Next: Review**.
16. If all looks as you expect, choose **Create User**.

#### Setting Up a Budget
1. Choose **My Account** from the account dropdown menu in the top right corner of the 
   screen.
2. Scroll down to **IAM User and Role Access to Billing Information** and select **edit**.
3. Select **Activate IAM Access** and **Update**.
4. Document all credentials securely: root user email address; root user password; IAM 
   username; IAM password, and account number. 
5. **Sign out** of the Root account through the account dropdown menu in the top right 
   corner of the screen. It is widely considered bad practice to perform tasks with the root 
   user. The root user should only be used to create the first IAM administrative user, to open 
   billing privileges to administrators, and to close the account. **Store the credentials securely.**
6. Open the [Amazon Web Services home page](https://aws.amazon.com/).
7. Choose **Sign In to the Console**.
8. Sign in as the **IAM user** you created above.
9. Use the search console to find **AWS Budgets**.
10. Choose **Create budget**.
11. Select **Cost budget**.
12. Name the budget. Choose **Daily** for the **Period**. Select **Recurring budget** and 
    **Fixed**. Enter $0.01 as the **Budgeted Amount**.
13. Choose **Configure thresholds**.
14. Set threshold based on **Actual cost**.
15. Set up notifications by entering an email address in the **Email recipients** box. 
16. Choose **Confirm budget**.

#### IAM Multi-factor Authentication
1. [Set up multi-factor authentication for your IAM user.](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html)
2. **Sign out** of your AWS account through the account dropdown menu in the top 
    right corner of the screen.

**Congratulations! You are now ready to deploy your application with AWS.**

## AWS Elastic Beanstalk and the AWS Elastic Beanstalk CLI

### AWS Elastic Beanstalk
AWS developed Elastic Beanstalk to facilitate the deployment of web applications with 
AWS managed services. It orchestrates the provisioning and monitoring of the 
infrastructure necessary to manage a scalable and secure web application developed 
with Java, .NET, PHP, Node.js, Python, Ruby, or Go.

### Installing the Elastic Beanstalk CLI
While it is possible to deploy an application through the AWS Elastic Beanstalk Console, you will find 
working with the CLI to be more efficient. You can find installation instructions 
[here](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install-advanced.html). Note 
that the CLI requires Python 2.7, 3.4 or later. Since Python 2 has reached its 
[end of life](https://www.python.org/doc/sunset-python-2/), you should be working in Python 3.

Review the Elastic Beanstalk Command Line reference material [here](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-cmd-commands.html).

## Deploying Your React Application with AWS Elastic Beanstalk

### AWS Elastic Beanstalk React Tutorial
Before working to deploy your application, it is worth completing a [tutorial](https://dev.to/johanrin/i-deployed-a-server-side-react-app-with-aws-elastic-beanstalk-here-s-what-i-learned-217i) to orient yourself 
to the Elastic Beanstalk tools and to make sure you have configured them correctly on your machine. It 
should take no more than an hour, which is less time than it will take you to learn the service while 
fighting to deploy your application with misconfigurations. 

### React Deployment Prerequisites
Make certain that your application works with `npm`. The Elastic Beanstalk Node.js platform uses `npm` as a package manager 
by default. If you have used `yarn` to build your application, you can remove the `node_modules` folder and `yarn.lock` file 
and then reinstall the dependencies with `npm install`. Ensure that your application runs locally after the changes. It 
is easier to debug errors on your local machine than it is to dig through AWS logs. Fix errors as necessary before trying 
to deploy your project.

### A Note About Using json-server
Many NSS frontend capstones use json-server to mock backend responses. Deploying an application to the web with such a setup
presents a **significant security risk**. In short, it allows unknown users to access and/or manipulate your data. The 
intent of these instructions for deploying your frontend application is to acclimate you to working with AWS. It is **not** 
to teach you how to deploy a frontend application with proper security protocols. **It is essential that you follow [the security 
group instructions below](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#update-your-application-load-balancers-security-group) to limit ingress to your application to your computer's IP on your home network. Failing to do so 
represents significant risk to your users and your AWS account, which - as noted above - is tied to your finances.**

### Setting Up a React Application with a JSON Server
Deploying a React application with a JSON server on Elastic Beanstalk will require you to make a few small changes to your
capstone code base. Fortunately, [Andy Collins](https://github.com/askingalot) has written a simple JSON server that you can 
incorporate your project to get it up and running in the cloud. The source code is [here](https://github.com/nss-day-cohort-29/c29-heroku-test). 
Following is an abstraction to facilitate your deployment:

#### `package.json`
1. Make sure your application includes `concurrently` and `json-server` in its `package.json` file, e.g.:

```
"dependencies": {
    "concurrently": "^5.3.0",
    "json-server": "^0.16.3",
    "react": "^16.8.3",
    "react-dom": "^16.8.3",
    "react-scripts": "^4.0.0"
  }
```

2. Alter your `scripts` block to include the following:

``` 
    "scripts": {
    "start": "npm run build && npm run json-serve",
    "json-serve": "node server.js",
    "start:dev": "concurrently \"npm run json-serve\" \"react-scripts start\"",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  } 
```

#### `server.js`
1. Create a `server.js` file in your application's main directory. Configure it as follows:

```
const path = require('path');
const dbPath = './API/db.json';
const jsonServer = require('json-server');
const server = jsonServer.create();
const router = jsonServer.router(dbPath);
const middlewares = jsonServer.defaults({ static: "./build" });
const port = process.env.PORT || 5002;

server.use(middlewares);

server.use((req, res, next) => {
    const isApiRoute = req.originalUrl.includes('/api/');
    if (isApiRoute) {
        return next();
    }
    return res.sendFile(path.join(__dirname, './build/index.html'));
});

server.use(jsonServer.rewriter({
    '/api/*': '/$1'
}));

server.use(router);

server.listen(port, () => {
    console.log(`app running on port ${port}`);
});
```

#### `API/db.json`
1. Create a folder titled `API` under your application's main directory.
2. Place your `db.json` file under that directory.

#### `/src/data.js`
Create a `data.js` file under your application's `src` directory and configure it as follows:

```
const baseUrl = process.env.NODE_ENV === 'production'
  ? '/api/'
  : "http://localhost:5002/api/";

const data = {
  getPosts: () => 
    fetch(`${baseUrl}posts`).then(resp => resp.json())
};

export default data;
```

**Congratulations! You application is now configured for development and production deployment.**

### Initializing Elastic Beanstalk
From your application's home directory, run ```eb init```. This sets default values for Elastic Beanstalk 
applications. It will ask a series of questions:

1. Select **us-east-1** as your region. The reason for this is two-fold. First, it is best practice
to deploy your application closest to its user base. Amazon's us-east-1 region is based in 
northern Virginia. The proximity argument may lead to your wanting to select us-east-2, which is 
in Ohio. Nonetheless, you should still choose us-east-1 because this is the region in which
AWS often deploys its newest services first. The benefit of having access to the newest services
far outweighs the cost of a few hundred miles.
2. Choose **[ Create new Application ]**. It will default to the the name of your application's main folder.
3. Allow the default value for `Application Name`.
4. The prompt will ask you whether you are using Node.js. Enter **Y**.
5. Choose the version of Node you used locally to develop your project.
6. Select **n** for [CodeCommit](https://aws.amazon.com/codecommit/). You do not need an AWS-managed service for 
source control.
7. Set up SSH access for your EC2 instances if you would like:
   * Select a keypair.
   * Name the keypair.
   * Enter a passphrase.
   * The public key will be saved locally to `/Users/<user>/.ssh/<keypair_name>`.
   * Enter your passphrase to upload the public key to AWS. **Store this passphrase securely**. You will need it each time 
     you use the keypair.

### Creating Your Elastic Beanstalk Application
From your application's home directory, run ```eb create```. This will generate a new Elastic Beanstalk environment and deploy your application to it.

1. Choose an environment name or allow the default value.
2. Enter a value for the prefix of your application's [DNS Canonical Name record](https://en.wikipedia.org/wiki/CNAME_record) or allow the default value.
3. Select an **application** load balancer. AWS is in the process of deprecating Classic load balancers. Network load balancers cost money, and
you do not need their low level configuration options to deploy your capstone.
5. Choose **n** for [Spot Fleet requests](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-requests.html). 
Since you are deploying a light application on the free tier, you do not need to worry about this configuration option.

From this point, the service will complete the following steps:
1. Package and deploy your application to [S3](https://aws.amazon.com/s3/).
2. Upload your [ssh keypair](https://www.ssh.com/ssh/public-key-authentication) to [EC2](https://aws.amazon.com/ec2/) if you attached one to the environment in the steps above.
3. Create a [target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html).
4. Create [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html).
5. Create an [auto-scaling](https://aws.amazon.com/autoscaling/) launch configuration and auto scaling group.
6. Create an EC2 instance.
7. Create auto scaling group policies for scaling your application's resources up and down.
8. Create two [CloudWatch](https://aws.amazon.com/cloudwatch/) alarms to support auto scaling.
9. Create an [application load balancer and load balancer listener](https://aws.amazon.com/elasticloadbalancing/).
10. Deploy your application to your EC2 instance.
11. Print the DNS CNAME of your Elastic Beanstalk environment, which will follow the pattern 
<DNS_CNAME_prefix>.us-east-1.elasticbeanstalk.com.

### Update Your Application Code 
You will need to replace all calls to `localhost:<port>` with `<elastic_beanstalk_environment_DNS_CNAME>/api`. Note the following example:

##### Original
```
return fetch(`http://localhost:5000/users?email=${email.current.value}`)
```
##### Adapted
```
return fetch(`http://eb_react_app.us-east-1.elasticbeanstalk.com/api/users?email=${email.current.value}`)
```

Deploy your changes as follows:
```
git add .
git commit -m "<your detailed commit message>"
eb deploy --staged
```

Elastic Beanstalk only updates from the `HEAD` commit of the `main` or `master` Git branch. Deploying with the `--staged` 
flag pulls all the changes you have staged for pushing to your `main` branch. This way you can confirm that your application 
works before committing the changes to the `main` branch.

### Update Your Application Load Balancer's Security Group
**IMPORTANT**: Without this step, your application's data will be exposed to the world. Take this step seriously, and 
complete it from your own computer on your home router.

1. Log in to the [AWS Management Console](https://console.aws.amazon.com/).
2. Go to the EC2 service. You can search for it in the search bar at the top or use the dropdown `Services` menu to 
locate it.
3. In the left navigation bar, select `Security Groups`.
4. Find the Security Group with the **Name** of your Elastic Beanstalk Application and the following **Description**:
```
Elastic Beanstalk created security group used when no ELB security groups are specified during ELB creation
```
5. Click on its **Security group ID**.
6. Select **Edit inbound rules**.
7. Under **Source** close the box with the entry `0.0.0.0/0`.
8. In the dropdown menu that has **Custom**, select `My IP`.
9. Select **Save rules**.
10. Your application is now restricted to your home network on the computer with which you changed the security group.
It will accept no other traffic.

**Congratulations! You have now deployed your React application with AWS Elastic Beanstalk.**

### Reviewing the Health, Status, and Configuration of Your Elastic Beanstalk Environment
To check the health of your Elastic Beanstalk environment, you can run `eb status` from the command line.

To review other facts about your Elastic Beanstalk application and environment, access the Elastic Beanstalk console via 
the **Search Bar** or **Services** menu in the AWS Console. The logs functionality is especially helpful for debugging. To 
access your application's logs, go to the Elastic Beanstalk console. Select **Environments** on the left navigation menu. 
Select your environment. In the navigation pane, choose **Request Logs**. When Elastic Beanstalk finishes processing the 
logs, choose **Download**.

## Deploying Your Django Application with AWS Elastic Beanstalk

Before trying to deploy your Django application with AWS Elastic Beanstalk, it is worth working through a 
[tutorial](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-django.html). 
It should take no more than an hour and will save you much time when working to deploy your own application.

**Before proceeding, make sure your Django application runs without a hitch on your local machine. You do not want to 
debug your application remotely.**

### Configure Your Application for Elastic Beanstalk
1. Activate your virtual environment.
2. Run `pip freeze` and save the output to a `requirements.txt` file in your project's main directory.
3. Create a directory named `.ebextensions`.
4. In the `.ebextensions` directory, create a file named `django.config` and configure it as follows:

```
option_settings:
  aws:elasticbeanstalk:container:python:
    WSGIPath: <your_application_name>.wsgi:application
```
Note: Replace `<>your_application_name>` with the name of your application.

5. Deactivate your virtual environment.
6. Review the [Django deployment checklist](https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/) and make
all necessary changes to your server code. This includes, but may not be limited to, removing the `SECRET_KEY` from your 
`settings.py` file and setting `DEBUG` to `False`. **This is a requirement to protect your deployed application. Failing
to take this step seriously jeopardizes your application and your AWS account, which is tied to your finances.**

### Initialize Elastic Beanstalk for Your Application
1. Run the `eb init` command.
2. Select **us-east-1**.
3. Select **[ Create new Application ]**.
4. Select the **default** name for the application.
5. Select **Python**.
6. Select the version of Python you used to develop your application.
7. Select **n** to the CodeCommit prompt.
8. Select **Y** to set up SSH.
9. Select an existing keypair or **[ Create a new KeyPair ]**.

### Create Your Elastic Beanstalk Django Application
1. Run `eb create`.
2. Choose the **default** environment name.
3. Choose the **default** DNS CNAME prefix.
4. Select the **application** load balancer.
5. Select **N** when prompted about Spot Fleet requests.

From this point, the service will complete the following steps:
1. Package and deploy your application to [S3](https://aws.amazon.com/s3/).
2. Upload your [ssh keypair](https://www.ssh.com/ssh/public-key-authentication) to [EC2](https://aws.amazon.com/ec2/) if you attached one to the environment in the steps above.
3. Create a [target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html).
4. Create [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html).
5. Create an [auto-scaling](https://aws.amazon.com/autoscaling/) launch configuration and auto scaling group.
6. Create an EC2 instance.
7. Create auto scaling group policies for scaling your application's resources up and down.
8. Create two [CloudWatch](https://aws.amazon.com/cloudwatch/) alarms to support auto scaling.
9. Create an [application load balancer and load balancer listener](https://aws.amazon.com/elasticloadbalancing/).
10. Deploy your application to your EC2 instance.
11. Print the DNS CNAME of your Elastic Beanstalk environment, which will follow the pattern 
<DNS_CNAME_prefix>.us-east-1.elasticbeanstalk.com.

At this point your browser will return the following error:
```
DisallowedHost at /

Invalid HTTP_HOST header: '<DNS_CNAME_prefix>.us-east-1.elasticbeanstalk.com'. You may need to add 
'<DNS_CNAME_prefix>.us-east-1.elasticbeanstalk.com' to ALLOWED_HOSTS.
```
As the error message recommends, you will need to add `'<DNS_CNAME_prefix>.us-east-1.elasticbeanstalk.com'` to `ALLOWED_HOSTS` in your `settings.py` file. 
Remember to replace `<DNS_CNAME_prefix>` with your DNS CNAME prefix.

Once you have made the appropriate change to your code, add and commit your changes to GitHub, and deploy your application again by running `eb deploy --staged`.

At this point, your application should be running in the cloud. 

### Secure Your Django Server in the Cloud

**This is a good point to restrict http traffic to your application. It is a requirement.** Follow the instructions 
[here](https://github.com/nashville-software-school/aws-deployment-instructions/blob/main/README.md#update-your-application-load-balancers-security-group) to 
limit your application's http traffic to your local machine. Failing to complete this step leaves all your data is exposed to all internet traffic, which is a 
**significant security risk** to your server in the cloud.

**Congratulations! Your Django application is now running in the cloud at the URL attached to your Elastic Beanstalk environment.**

### Serve Your Django Application's Static Files to the Cloud

Now that you have secured your server, you can attend to its visual representation. You will notice that your Admin and API pages lack the expected Django styling.
This is because your Django application has not collected its static files and your Elastic Beanstalk application has not been configured to serve them.

1. Add the following configuration to your `settings.py` file:
```
import os

PROJECT_ROOT = os.path.abspath(os.path.dirname(__name__))
STATIC_ROOT = os.path.join(PROJECT_ROOT,'static/')
STATIC_URL = '/static/'
```
Your `settings.py` file may already have `STATIC_URL = '/static/'` by default.
2. From your project's main local directory (i.e., not on the cloud server), run `python manage.py collectstatic`. Your application's static files have now been
collected under the `static` directory under your project's root directory.
3. Add the following configuration to your `django.config` file under the `.ebextensions` directory:
```
  aws:elasticbeanstalk:environment:proxy:staticfiles:
    /static: static
```
Make sure the new `aws` line matches the indentation of the `aws` line above it.
4. Add and commit all your changes to GitHub.
5. Deploy them to your Elastic Beanstalk environment with `eb deploy --staged`.
6. Merge your changes to your `main` GitHub branch.

## Deploying Your React Application on AWS Amplify

### AWS Amplify
AWS Amplify is a managed service that makes it easy to launch applications in AWS. It has 
code libraries, components, and a dedicated CLI that you can use to build a backend. The 
components include data storage, analytics, push notifications, and authentication. Amplify 
supports iOS, Android, Web and React Native mobile applications in addition to React, Ionic, 
and vue.js web applications.

### Installing and Configuring the Amplify CLI

##### Install the CLI Globally

```npm install -g @aws-amplify/cli```

Depending on your system policies, this command may require `sudo` access.

##### Configure the CLI

```amplify configure```

1. Select `us-east-1` as your region. 

2. After selecting the region, the prompt will ask you for a user name for a new IAM user. You can 
sign in to the AWS Console in order to create a new IAM user, or you can use the existing 
administrative user you created above. Either way, you will need the user's `accessKeyId` and 
`secretAccessKey` to connect the CLI. If you no longer have access to those credentials, 
you can [create new keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html).
Note that AWS tools store your credentials on your local machine under the `~/.aws` directory. 
You should be able to find them there. As always, if you create new keys, **store them securely**.

You can find a video guide for installing the CLI [here](https://www.youtube.com/watch?v=fWbM5DLh25U&t=1s).

### AWS Amplify Tutorial
Before working to deploy your application, it is worth completing a [tutorial](https://docs.amplify.aws/start/q/integration/react) to 
orient yourself to the Amplify tools and to make sure you have configured them correctly. It should take no more than an hour, which is 
less time than it will take you to learn the service while fighting to deploy your application with misconfigurations.

### Configuring Your Web Application for Deployment on Amplify

