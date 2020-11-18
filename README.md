# element-awssesemailform
Turn any static web form into a AWS SES powered email enquiry form


## Usage
* requires [element](https://github.com/Cloudouble/element) included in the page
* load with ```window.Cloudouble.Element.load()``` , and ensure that the ```Aws``` element is also loaded in the list first as it inherits from this base class
* set up a IAM role to be used for your public web site visitors, with a statement included similar to below: 
```
Statement": [
        {
            "Sid": "elementAwssesemailform",
            "Effect": "Allow",
            "Action": [
                "ses:SendEmail",
                "ses:SendTemplatedEmail"
            ],
            "Resource": [
                "arn:aws:ses:ap-southeast-2:771795544492:identity/email@you-wish-to-send-from.com"
            ]
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "ses:CreateTemplate",
                "ses:GetTemplate"
            ],
            "Resource": "*"
        }
    ]
```
* configure AWS SES with at least one verified email address ready to send from, including ensuring your account is not in sandbox mode 
* configure the attributes as detailed below to point your form to the right backends

It will look something like this in your HTML:
```
<cloudouble-element-awssesemailform 
	region="ap-southeast-2" accountid="1234567890" identitypoolid="ap-southeast-2:00000000-0000-0000-0000-000000000000"
	proxy="#footer form"
	from="My Enquiry Form<accounts@example.com>"
	to="My Enquiries<info@example.com>"
	return="bounces@example.com"
	tags="example.com,enquiry"
	></cloudouble-element-awssesemailform>
```

You can see it in action over at [cloudouble.com](https://cloudouble.com), it is the email enquiry form at the bottom of every page


## Attributes inherited from the ```aws``` element 
(see [element-aws](https://github.com/Cloudouble/element-aws) for more information)
* **region** => (required) the AWS region to use
* **accountid** => (required) the AWS accountid
* **identitypoolid** => (required) the CognitoIdentity pool to use for public authentication


## Attributes to also configure for this element
* **proxy** => a querySelector pointing to another element on the page to take the submission fields and events from, use to quickly incorporate into an 
existing template without interfering with CSS rules etc
* **from** => the email address to send your email from, needs to be verified in SES
* **to** => the email address to send your email to
* **return** => the email address to send undeliverable messages to
* **tags** => comma-separated list of tags to attach to this email message in SES
* **to** => the email address to send your email to, can be multiple if comma-separated
* **cc** => the email address to CC your email to, can be multiple if comma-separated
* **bcc** => the email address to BCC your email to, can be multiple if comma-separated
* **replyto** => the email address to use as the ReplyTo for your emails, can be multiple if comma-separated
* **messagehtml** => the message HTML to send, no templating will be done
* **messagetext** => the message TXT to send, no templating will be done
* **messagesubject** => the subject of the message to send, no templating will be done
* **message** => the message to send - both HTML and TXT, no templating will be done
* **template** => the name of a template (if already stored in SES), or the URL of a .json file to fetch and use as a template object, or a JSON-encoded template object. 
The template object is an object like ```{TemplateName, SubjectPart, HtmlPart, TextPart}```, all with string values
* **templatehtml** => the HTML to set your template to instead of specifying in ```template```
* **templatetext** => the TXT to set your template to instead of specifying in ```template```
* **templatesubject** => the subject to set your template to instead of specifying in ```template```
* **templatename** => the name to set your template to instead of specifying in ```template```


## Events Emitted
* **sendstart** => emitted when email sending has started
* **sendfinish** => emitted when email sending has completed
* **senderror** => emitted when email sending failed
* **sendcreatetemplate** => emitted when a template has been created or updated in SES
* **sendgettemplate** => emitted when a template was retrieved from SES


## Form fields For Templating
* any fields with a ```name``` attribute either within the elemnt itself (if no proxy set), or within the proxy element (if set) will be mapped as the merge fields for templating on send.
* you can also include form fields with a name formatted like ```${template}``` (including the '${' and '}' around the field name), in order to override on send any hardcoded attributes 
set on the element itself as attributes (not including inherited attributes).
* this enables setting this attributes via hidden fields, or dynamically depending on user input (like having different templates used depending on the value of a ```select``` input)
* you can learn more about how to format your template to process these merge fields at https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-personalized-email-advanced.html


## Attribute Hooks To Use in CSS styling 
* the element, and it's proxy element (if set) will have an attribute named ```sendstatus``` set to the most recent event type emitted to aid in 
CSS-based styling to show send progress to end users.


## Further Reading 

[AWS Identity and Access Management Documentation](https://docs.aws.amazon.com/iam/index.html)

[Amazon Cognito Identity Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html)

[Amazon SES](https://docs.aws.amazon.com/ses)

SES Merge Field Info: [Advanced email personalization](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-personalized-email-advanced.html)


The source code of the ```#footer``` element over at https://cloudouble.com for a complete implementation in the real world.


## License

[MIT](https://choosealicense.com/licenses/mit/)
