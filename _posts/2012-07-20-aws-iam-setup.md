---
layout: post
title: "AWS IAM Setup"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Yeah, it's easy to create an IAM and just run hog wild. But there's a sequence of steps that MUST be followed if you don't want the dreaded `Permission Denied` or `This access not available` or `Your kittens are being blended`.

So the idiot's way of doing it:
* Log in to AWS console
* Create IAM Group with permissions to have Usage Reports and Account Activity
* Add permissions
* Create IAM user with a password
* Assign Group to new user
* Create an AWS Account Alias from the IAM Dashboard
* Log in as this new IAM 
* Get `Permission Denied`. Woot!

And the correct way of doing it:
* Log in as admin to your AWS account
* Go to the [Personal Information](https://portal.aws.amazon.com/gp/aws/developer/account/index.html?ie=UTF8&action=edit-aws-profile) section 
    * Fill out all the 'Security Challenge Questions'
* Go to the [Manage your account](https://portal.aws.amazon.com/gp/aws/manageYourAccount) section
    * In the `IAM user access to the AWS Website` section, enable the 'Account Activity Page' and 'Usage Reports Page' checkboxes, and click 'Activate'
* Go to the [IAM Dashboard](https://console.aws.amazon.com/iam/home)
* Create a site alias from the 'AWS Account Alias' section. This is not necessary, but will make accessing the specific AWS console area much less annoying.
* Create an IAM Group with the appropriate permissions 
* Create an IAM User
    * Add a password in the 'Sign-In Credentials' section
    * Assign the previously created group(s)
* Log in as this new IAM user 
* Select the user's name from the top-right drop-down and go to Usage Reports or Account Activity
* No more `Permission Denied`

References:
* http://docs.amazonwebservices.com/IAM/latest/UserGuide/ControllingAccessWebsite.html
* http://www.nslms.com/2011/04/04/enabling-aws-console-login-for-iam-users/
