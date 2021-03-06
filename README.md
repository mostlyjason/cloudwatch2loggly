# cloudwatch2loggly
Sends logs from Cloudwatch to Loggly using Lamda function

## More information about AWS Lambda and Loggly
  * http://aws.amazon.com/lambda/
  * https://www.loggly.com/
  
## Get the code and prepare it for the uploading to AWS
* Clone the git repo
```bash
git clone https://github.com/psquickitjayant/cloudwatch2loggly.git
cd cloudwatch2loggly
```

##Open the cloudwatch2loggly.js and provide the following information in it

* Your AWS accout details
```javascript
//AWS keys configuration
var awsConfiguration = {
  accessKeyId : 'xxx',
  secretAccessKey : 'xxx',
  region : 'xxx'
};
```

* Your Loggly customer token
```javascript
//loggly url, token and tag configuration
var logglyConfiguration = {
  url : 'http://logs-01.loggly.com/bulk',
  customerToken : 'xxx',
  tags : 'cloudwatch2loggly'
};
```
* Install required npm packages.
```
npm install
```

* zip up your code
```bash
zip -r cloudwatch2loggly.zip cloudwatch2loggly.js node_modules
```

The resulting zip (cloudwatch2loggly.zip) is what you will upload to AWS.

## Setting up AWS
For all of the AWS setup, I used the AWS console following [this 
example](http://docs.aws.amazon.com/lambda/latest/dg/getting-started-amazons3-events.html).  Below, you will find a high-level 
description of how to do this.  I also found [this blog post](http://alestic.com/2014/11/aws-lambda-cli) on how to set things up 
using the command line tools.

### Create and upload the cloudwatch2loggly function in the AWS Console
1. Create lambda function
  1. https://console.aws.amazon.com/lambda/home
  2. Click "Create a Lambda function" button. *(Choose "Upload a .ZIP file")*
    * **Name:** *cloudwatch2loggly*
    * Upload lambda function (zip file you made above.)
    * **Handler*:** *cloudwatch2loggly.handler*
    * Set Role : *S3 Execution Role*
    * Set Timeout to 2 minutes
  3. Go to your Lamda function and select the "Event sources" tab
    * Click on **Add Event Source**
    * Event Source Type : *Scheduled Event*
    * Name : Provide any customized name. e.g. Cloudwatch2Loggly Event Source
    * Description: Invokes Lambda function in every 5 minutes
    * Schedule expression : *rate(5 minutes)*
    * Enable Event Source : *Enable Now*
 Now click on submit and wait for the events to occur in Loggly

