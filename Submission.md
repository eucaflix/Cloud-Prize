Edit this page to describe your Submission.

## Which Categories Best Fit Your Submission and Why?
**Portability Enhancement**.

## Describe your Submission

### Categories of "supported" projects
* Not supported:  Eucalyptus is currently missing implementations of essential AWS services to support the project.
* Transitive:  The project has no AWS dependencies, but depends upon other Netflix OSS projects/services and, so, is transitively supported by virtue of other contributions in this submission.
* Partial:  For projects which provide faceted functionality (e.g., edda's monitoring of resources) partial support means that some portion of the functionality requires services which are not supported by Eucalyptus.
* Full:  All of the functionality from a particular project works in Eucalyptus.
* Hybrid:  The project can be configured to use both AWS and Eucalyptus clouds at the same time.  This implies also that the project can be configured to only use Eucalyptus, too.

### Support Netflix OSS Projects
* archaius: No DynamoDB support, other configurations works.
* asgard: Partially supported (no RDS, SQS)
* astyanax: No AWS dependencies
* blitz4j: No AWS dependencies
* brutal: No AWS dependencies
* CassJMeter
* Cloud-Prize
* curator
* denominator: Not supported (no Route53)
* edda: Partially supported (no ElasticCache, RDS, Route53, SQS)
* eureka: Fully supported
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
* karyon: Transitive
* Lipstick: Transitive
* netflix-commons
* netflix-graph
* NfWebCrypto
* Priam: Fully supported
* pytheas
* recipes-rss: Transitive
* ribbon: Transitive
* RxJava
* servo: Fully supported
* SimianArmy: Partially supported (no SimpleDB)
* Turbine: Fully supported
* zuul: Transitive

### Detailed review

= SimianArmy =
= asgard =
= Turbine =


= exhibitor =
String ENDPOINT_SPEC = System.getProperty("exhibitor-s3-endpoint", "https://s3$REGION$.amazonaws.com");
String fixedRegion = s3Region.equals("us-east-1") ? "" : ("-" + s3Region);
String endpoint = ENDPOINT_SPEC.replace("$REGION$", fixedRegion);
localClient.setEndpoint(endpoint);

= Priam =
Add support for Eucalyptus endpoints

- Add facade for constructing AWS service clients w/ configured endpoints.
- Construct clients using facade in AWSMembership, PriamConfiguration, and
  S3FileSystem.
- No support for SimpleDB currently -- SimpleDBConfigSource needs
  alternative credentials.

= edda =
Add support for Eucalyptus endpoints

- Interpret edda.region or edda.$account.region as a hostname
  when it is not an AWS region name.
- Use EC2 region's DNS name existing as test for AWS region existence.
- AwsClient.setEndpoint construct Eucalyptus URLs if not an AWS region.
- Supported: AutoScaling, CloudWatch, EC2, ELB, IAM, S3
- Unsupported: ElastiCache, RDS, Route53, SQS

= eureka =
Add support for Eucalyptus endpoints

- Use EurekaClientConfig to obtain EC2 and AutoScaling endpoints.
- Introduce EucalyptusEurekaClientConfig for construcing euca URLs.
- Modify AwsAsgUtil and EIPManager accordingly.

= servo =
Add support for Eucalyptus endpoints

- Add facade for constructing AWS service clients w/ configured endpoints.
- Add properties for setting AutoScaling and CloudWatch endpoints.
  com.netflix.servo.aws.endpoint.cloudwatch
  com.netflix.servo.aws.endpoint.autoscaling
- Construct clients using facade in CloudWatchMetricObserver and
  AwsInjectableTag.


## Provide Links to Github Repo's for your Submission
https://github.com/eucaflix/
