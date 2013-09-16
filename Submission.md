## Which Categories Best Fit Your Submission and Why?
__Portability Enhancement__.  The majority of our changes to the NetflixOSS codebase involve tweaks to enable the projects
to run seamlessly against both AWS and Eucalyptus.  Since Eucalyptus is designed to be highly compatible with the AWS API, these changes generally involve abstraction of hardcoded AWS-isms.

## Describe your Submission

Our submission touches many NetflixOSS projects.  Different projects have different degrees of integration.  For each
NetflixOSS project, we indicate the degree of Eucalyptus integration provided by our submissions as follows:

### Categories of Supported Projects
* _Full_:  All of the functionality from a particular project works in Eucalyptus.
* _Hybrid_:  The project can be configured to use both AWS and Eucalyptus clouds at the same time.  This also implies that the project can be configured use only Eucalyptus.
* _Transitive_:  The project has no AWS dependencies, but depends upon other Netflix OSS projects/services and, so, is transitively supported by virtue of other contributions in this submission.
* _Partial_:  For projects which provide faceted functionality (e.g., edda's monitoring of resources) partial support means that some portion of the functionality requires services which are not supported by Eucalyptus.

### Supported Netflix OSS Projects
* archaius: _Partial_. No DynamoDB support, other configurations works.
* asgard: _Partial_. No RDS, SQS.
* edda: _Partial_. No ElasticCache, RDS, Route53, SQS.
* eureka: _Full_.
* karyon: _Transitive_.
* Lipstick: _Transitive_.
* Priam: _Full_.
* recipes-rss: _Transitive_.
* ribbon: _Transitive_.
* servo: _Full_.
* SimianArmy: _Partial_. No SimpleDB.
* Turbine: _Full_.
* zuul: _Transitive_.

### Not supported or N/A
* astyanax: No AWS dependencies
* blitz4j: No AWS dependencies
* brutal: No AWS dependencies
* CassJMeter
* Cloud-Prize
* curator
* denominator: Not supported (no Route53)
* EVCache:
* exhibitor: ????
* feign
* frigga
* gcviz
* genie
* governator
* gradle-template
* Hystrix
* ice: Not supported (no Programmatic Billing Access, SES, SimpleDB)
* netflix-commons
* netflix-graph
* NfWebCrypto
* pytheas
* RxJava

### Detailed review

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
