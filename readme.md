# AWS Textract Json Parser
[![GitHub Release](https://img.shields.io/github/release/tterb/PlayMusic.svg?style=flat)]() 
This library parses the json response from AWS Textract into a more usable format.

> Selection elements now included.
> Minimum Node version 10.1+

## Installation

> npm install aws-textract-json-parser

## Basics

Once you have passed in the response from AWS Textract into the parser, you can call 3 different functions, **getTableData**, **getFormData** and **getRawData**. You can also add an optional object specifying the minimum confidence level, default is set to 0.

```javascript
const aws = require('aws-sdk');
const awsConfig = require('./aws_config');

// require the parser package
const AWSJsonParser = require('aws-textract-json-parser');

aws.config.update({
  accessKeyId: awsConfig.awsAccesskeyID,
  secretAccessKey: awsConfig.awsSecretAccessKey,
  region: awsConfig.awsRegion
});

(async () => {
  try {
    const textract = new aws.Textract();
    const dectectText = textract.getDocumentAnalysis({
      JobId: 'JobId from startDocumentAnalysis goes here...'
    });
    const response = await dectectText.promise();

    if (response.JobStatus === 'SUCCEEDED') {
      // pass the response from AWS Textract to the parser
      const res = await AWSJsonParser(response);

      // Call one of 3 functions getTableData, getFormData, getRawData
      // Optional object to specify the confidence level, default is 0

      // Example of getting the table data.
      const tableData = res.getTableData({
        minConfidence: 99
      });
      // Example of getting the form data.
      const formData = res.getFormData();
      // Example of getting the raw(line) data.
      const rawData = res.getRawData();

      console.log(tableData, formData, rawData);
    } else {
      console.log('Job not completed yet');
    }
  } catch (err) {
    console.log(err);
  }
})();
```
