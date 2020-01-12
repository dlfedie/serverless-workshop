# serverless-workshop
Starter repo for the serverless workshop I'm running.

This roughly two-hour workshop is a step-by-step tour of the basics of serverless compute platforms using AWS Lambda.

By the end of the workshop, you should have a understanding of:
1. the fundamentals of serverless architecture
2. the reasons for its meteoric rise in popularity
3. the tradeoffs involved relative to a traditional server-based backend
4. the differences between serverless architecture and microservices architecture 
5. how incredibly easy it is to get up and running from literally zero

Plus you'll have a fully armed and operational API when you leave -- available over HTTPS, secured by modern authentication, and ready to demo for all the recruiters who will no doubt shortly be banging down your door!

## Prep Beforehand (~15-30 minutes)
Please come prepared to the workshop by doing the following before attending:

Hopefully it's a safe assumption that if you're coming to this workshop, you have an IDE installed. If you don't, please do so before coming.

Install NodeJS / npm if you haven't.

0. (Windows only) Set up a Bash-like environment on your machine. I recommend installing Git for Windows and using Git Bash.
1. Fork this repo to your personal. OPTIONAL: set up GitHub SSH, see below
2. `$ npm install`
3. Create an AWS free-tier account if you don't have one. https://aws.amazon.com/ (Yes, you need to put in a credit card. No, this shouldn't cost you anything.)
4. Install the AWS CLI: https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html
5. Install a REST client. I recommend ARC: https://install.advancedrestclient.com/install

### Setting up SSH for GitHub
To avoid having to type your password in all the time, add an SSH key to your GitHub account and use that to authenticate instead.

1. `$ ssh-keygen` (just press Enter through all the prompts)
2. `$ cat ~/id_rsa.pub`
3. Copy that whole output to a new SSH key on your GitHub account at Settings | SSH and GPG keys | New SSH key 
4. Add that SSH key to your local keyagent: `$ eval $(ssh-agent -s)`, then `$ ssh-add ~/.ssh/id_rsa`
5. Verify: `$ ssh -T git@github.com` (press enter through prompts/warnings)
5. Add those two lines to your `.bash_profile` so they load every time you open bash. (`/USER_HOME/.bash_profile`, wherever your USER_HOME is)
6. Change your git remote to use SSH: `$ git remote set-url origin ssh://git@github.com/YOUR_USERNAME_HERE/serverless-workshop.git`
7. Verify it works by making some trivial update to the README or somewhere and doing a `git push`

## Workshop Topics

### What are serverless platforms?

### Why choose serverless?

### Serverless limitations

## Workshop Agenda

#### Create IAM user
* Create user
* Download the csv!
* Create group `serverless-admin` & attach policies: AWSLambdaFullAccess, AmazonAPIGatewayAdministrator
* Create group `iam-admin` & attach policies: IAMFullAccess
* Add user to groups
* Create a password for the user
* * Note the web console sign-in link
* Sign out, then sign in as that user

#### Configure AWS CLI
`$ aws2 configure`

* Credentials from CSV
* Region: us-west-2
* Output: yaml

verify: `$ aws2 sts get-caller-identity`


#### Set up IAM Role
`$ aws2 iam create-role --generate-cli-skeleton yaml-input > cli/iam-create-role.yaml`

^ edit those values

`$ aws2 iam create-role --cli-input-yaml file://cli/iam-create-role.yaml` <- file path is relative to working directory

note: `iam-trust-relationship.json` says "this role can be assumed by Lambdas"

Attach policies (generate skeleton): `$ aws2 iam attach-role-policy --generate-cli-skeleton yaml-input > cli/iam-attach-role-policy.yaml`

^ edit those values

Attach policies(run command): `$ aws2 iam attach-role-policy --cli-input-yaml file://cli/iam-attach-role-policy.yaml`

#### Package & bundle our code
`$ npm install`

`$ npm run-script bundle`

NOTE: Further explanation of this step is available on demand. 

#### Set up S3
Create bucket (generate skeleton): `$ aws2 s3api create-bucket --generate-cli-skeleton yaml-input > cli/s3api-create-bucket.yaml`

^ edit those values -- note s3 bucket name uniqueness

Create bucket (run command): `$ aws2 s3api create-bucket --cli-input-yaml file://cli/s3api-create-bucket.yaml`

Upload code (run command): `$ aws2 s3 cp dist/bundle.zip s3://serverless-workshop-prime/bundle.zip` <-- change s3 bucket name

#### Create our Lambda!
https://docs.aws.amazon.com/cli/latest/reference/lambda/index.html

Create Lambda (generate skeleton): `$ aws2 lambda create-function --generate-cli-skeleton yaml-input > cli/lmabda-create-function.yaml`

Create Lambda (run command) `$ aws2 lambda create-function --cli-input-yaml file://cli/lambda-create-function.yaml`

note: differences in web docs vs "live docs" in the CLI!

#### Create API Gateway resource

