## Which Categories Best Fit Your Submission and Why?
__Portability Enhancement__.  The majority of our changes to the NetflixOSS codebase involve tweaks to enable the projects
to run seamlessly against both AWS and Eucalyptus.  Since Eucalyptus is designed to be highly compatible with the AWS API, these changes generally involve abstraction of hardcoded AWS-isms.

## Describe your Submission

Our submission touches many NetflixOSS projects.  Different projects have different degrees of integration.  For each
NetflixOSS project, we indicate the degree of Eucalyptus integration provided by our submissions as follows:

### Supported Netflix OSS Projects

* eureka: _Full_.
* Priam: _Full_.
* servo: _Full_.
* Turbine: _Full_.

* archaius: _Partial_. No DynamoDB support in Euvalyptus at present.  Other configurations work.
* asgard: _Partial_. No RDS or SQS support in Eucalyptus at present. 
* edda: _Partial_. No ElasticCache, RDS, Route53, or SQS support in Eucalyptus at present.
* SimianArmy: _Partial_. No SimpleDB support in Eucalyptus at present.

### Not supported 

* denominator: _Not supported_. No Route53 support in Eucalyptus at present.
* ice: _Not supported_. No Programmatic Billing Access, SES, or SimpleDB in Eucalyptus at present.

### Detailed review of changes

__SimianArmy__

__asgard__

__Turbine__

__exhibitor__

String ENDPOINT_SPEC = System.getProperty("exhibitor-s3-endpoint", "https://s3$REGION$.amazonaws.com");
String fixedRegion = s3Region.equals("us-east-1") ? "" : ("-" + s3Region);
String endpoint = ENDPOINT_SPEC.replace("$REGION$", fixedRegion);
localClient.setEndpoint(endpoint);

__Priam__

Adds support for Eucalyptus endpoints:
* Adds facade for constructing AWS service clients w/ configured endpoints.
* Constructs clients using facade in AWSMembership, PriamConfiguration, and S3FileSystem.
* No support for SimpleDB currently -- SimpleDBConfigSource needs alternative credentials.

__edda__

Adds support for Eucalyptus endpoints:
* Interprets edda.region or edda.$account.region as a hostname when it is not an AWS region name.
* Uses EC2 region's DNS name existing as test for AWS region existence.
* AwsClient.setEndpoint constructs Eucalyptus URLs if not an AWS region.
* Supported: AutoScaling, CloudWatch, EC2, ELB, IAM, S3
* Unsupported: ElastiCache, RDS, Route53, SQS

__eureka__

Adds support for Eucalyptus endpoints:
* Uses EurekaClientConfig to obtain EC2 and AutoScaling endpoints.
* Introduces EucalyptusEurekaClientConfig for construcing euca URLs.
* Modifies AwsAsgUtil and EIPManager accordingly.

__servo__

Adds support for Eucalyptus endpoints:
* Add facade for constructing AWS service clients w/ configured endpoints.
* Add properties for setting AutoScaling and CloudWatch endpoints.
 * com.netflix.servo.aws.endpoint.cloudwatch
 * com.netflix.servo.aws.endpoint.autoscaling
* Constructs clients using facade in CloudWatchMetricObserver and AwsInjectableTag.

## Provide Links to Github Repos for your Submission

All of the repos corresponding to these submissions have been forked into the following organization: 

https://github.com/eucaflix/
