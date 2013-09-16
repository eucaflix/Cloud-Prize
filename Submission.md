## Which Categories Best Fit Your Submission and Why?

__*Portability Enhancement*__.  The majority of our changes to the NetflixOSS codebase involve tweaks to enable the projects
to run seamlessly against both AWS and Eucalyptus.  Since Eucalyptus is designed to be highly compatible with the AWS API, 
these changes generally involve abstraction of hardcoded AWS-isms.  

## Describe your Submission

Eucalyptus is an open source software project that allows users to build private clouds that are compatible with the
Amazon Web Services API.

Because Eucalyptus faithfully implements the AWS APIs upon which NetflixOSS depends, it should be noted that most 
NetflixOSS projects can be expected to run on Eucalyptus clouds _with no changes at all_.

There are, however, a handful NetflixOSS projects that must be modified in order to work with the latest version of
Eucalyptus (3.3.1 as of submission time).  Our submission focuses on providing portability for these NetflixOSS projects, 
as noted below.

### Netflix OSS Projects now supported under Eucalyptus with these patches

* eureka: _Full support_.
* Priam: _Full support_.
* servo: _Full support_.
* Turbine: _Full support_.
* archaius: _Partial support_. No DynamoDB support in Eucalyptus at present.  Other configurations work.
* asgard: _Partial support_. No RDS or SQS support in Eucalyptus at present. 
* edda: _Partial support_. No ElasticCache, RDS, Route53, or SQS support in Eucalyptus at present.
* SimianArmy: _Partial support_. No SimpleDB support in Eucalyptus at present.

### Not supported at all under Eucalyptus

* denominator: _Not supported_. No Route53 support in Eucalyptus at present.
* ice: _Not supported_. No Programmatic Billing Access, SES, or SimpleDB in Eucalyptus at present.

### Detailed review of changes

__eureka__

Adds support for Eucalyptus endpoints:
* Uses EurekaClientConfig to obtain EC2 and AutoScaling endpoints.
* Introduces EucalyptusEurekaClientConfig for construcing euca URLs.
* Modifies AwsAsgUtil and EIPManager accordingly.

__Priam__

Adds support for Eucalyptus endpoints:
* Adds facade for constructing AWS service clients w/ configured endpoints.
* Constructs clients using facade in AWSMembership, PriamConfiguration, and S3FileSystem.
* No support for SimpleDB currently -- SimpleDBConfigSource needs alternative credentials.

__servo__

Adds support for Eucalyptus endpoints:
* Add facade for constructing AWS service clients w/ configured endpoints.
* Add properties for setting AutoScaling and CloudWatch endpoints.
 * com.netflix.servo.aws.endpoint.cloudwatch
 * com.netflix.servo.aws.endpoint.autoscaling
* Constructs clients using facade in CloudWatchMetricObserver and AwsInjectableTag.

__Turbine__

Adds support for Eucalyptus endpoints:
* Allows for turbine.region to be a Eucalyptus host address or name.
* Tests for region-ness using DNS and checking for existence of ec2.${region}.amazonaws.com
* Configures client endpoints appropriately in AwsUtil.java

__asgard__

FIXME

__edda__

Adds support for Eucalyptus endpoints:
* Interprets edda.region or edda.$account.region as a hostname when it is not an AWS region name.
* Uses EC2 region's DNS name existing as test for AWS region existence.
* AwsClient.setEndpoint constructs Eucalyptus URLs if not an AWS region.
* Supported: AutoScaling, CloudWatch, EC2, ELB, IAM, S3
* Unsupported: ElastiCache, RDS, Route53, SQS

__SimianArmy__

Adds configuration options for Eucalyptus client properties. Example client.properties:
* simianarmy.client.aws.accountKey = 1CSDM1FT4G8ZKDZ1AZR2
* simianarmy.client.aws.secretKey = XXXXXXXXXXXX
* simianarmy.client.aws.region = us-east-1
* simianarmy.client.context.class=com.netflix.simianarmy.client.eucalyptus.EucalyptusChaosMonkeyContext
* simianarmy.client.eucalyptus.hostName = 10.111.1.119
* simianarmy.client.eucalyptus.accountKey = YRLWV7NSQK2FSXKCOTKET
* simianarmy.client.eucalyptus.secretKey = XXXXXXXXXXXX
* simianarmy.client.eucalyptus.region = us-east-1

## Provide Links to Github Repos for your Submission

All of the repos corresponding to these submissions have been forked into the following organization: 

https://github.com/eucaflix/
