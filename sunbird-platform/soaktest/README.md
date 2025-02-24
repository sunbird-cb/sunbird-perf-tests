**Note:**

To run these soaktest APIs jmx scripts following jars needs to be placed into JMeter’s lib/ext directory. Download the JMeter Plugins Manager JAR files from the given link and then put it into JMeter's lib/ext directory.

1. jmeter-plugins-cmn-jmeter-0.6.jar 
  - Download from https://repo1.maven.org/maven2/kg/apc/jmeter-plugins-cmn-jmeter/0.6/
2. jmeter-plugins-casutg-2.9.jar
  - Download from https://repo1.maven.org/maven2/kg/apc/jmeter-plugins-casutg/2.9/

----------
1. ## Sokatest with HPA Enabled - 25K TPS ##

- **AKS Cluster Autoscaler configuration:** Min no of nodes - 6 and Max no of nodes - 40 
- **HPA threshold: 70% CPU**
- **Max transactions per second: 25138**
- **Max no of AKS node used: 27**

**Service Pods configuration & Usage:**
| Service Name | CPU Limit(Core)| Memory Limit | Min Pods | Max Pods | Pod Usage |CPU Utilization(Max) %|
|--------------|----------|--------------|----------|----------|----------|----------|
|Analytics     |  0.8     |    2.5G      |   2      |     6     |3|94|
|API Manager   |    2     |   2.5G        |   2      |     30    |14|90|
|AdminUtils    |    1     |     2G        |   1      |     6     |4|73|
|Content       |   1      |   3.5G        |   1     |     12     |1| 15|
|Knowledgemw   |   1      |   1.5G        |   2      |     10     |4|99|
|Learner       |   1      |     3G        |    2    |     40     | 40|100|
|LMS           |   1      |     3G        |    2    |     30     |17| 100|
|Player        |    1     |     1G        |    1     |    16     |2|9|
|Nginx-private-ingress    |    0.2 |   0.3G  |   2   |     6     |2|4|
|Search        |   1      |   3G          |    1     |    10     |3|99|
|Telemetry     |   0.8    |   1G          |     2    |    40     |40|100|


**Infra Configuration & Usage:**
| Service Name | Configuration| CPU Usage (Max) | Load AVG(Max) |Memory Usage(Max) |
|--------------|----------|--------------|----------|----------|
|AKS Node      | 8 Core, 16GB |    80.72%  |     37.97     |6.500 GB|
|Cassandra    | 5 Node (16 Core , 64GB) |   75.23%   |  46.67   |13.505 GB|
|ES-LMS       | 3 nodes (16core , 64GB) |   35.59%   |  11.43   |28.5080 GB|
|COMP-LMS     | 3 nodes (16core, 32 GB) |   30.86%   |  7.11    |19.6606 GB|
|Kafka        | 3 nodes (4core , 16GB)  |   58.19%   |  5.17    |9.645 GB|
|Redis - LP   | 1 node (2core, 8GB)     |    2.64%   |  0.23    |2.3 GB|
|Redis -DP    | 1 node (32core, 128GB)  |    0.78%   |    0.3   |46.2 GB|
|KeyCloak     | 4 nodes (4core, 16GB)   |   98.49%   |  11.24   | 7.520 GB |

**JMX Details**
- cluster1.jmx (1 JMeter Master, 4 JMeter Slaves)
  - ```sendTelemetry```
  - ```readContent```
  - ```searchContent```
  - ```readForm```
  - ```getCourseHierarchy```
  - ```dialAssemble``` 
- learner.jmx (1 JMeter Master, 4 JMeter Slaves)
  - ```getUserProfileV3```
  - ```getUserProfileV2``` 
  - ```getUserProfile```
  - ```searchManagedUser```
  - ```userFeed```
  - ```readUserConsent```
  - ```updateUserConsent```
  - ```updateUser```
  - ```searchUser```
- lms.jmx (1 JMeter Master, 4 JMeter Slaves)
  - ```readContentState```
  - ```updateContentState```
  - ```istCourseEnrollments```
  - ```getBatch```
  - ```searchCourseBatches```
- ananlytics.jmx (1 JMeter Master, 2 JMeter Slaves)
  - ```deviceRegister```
  - ```deviceProfile```
  - ```registerMobileDevicev2```
- login.jmx (1 JMeter Master)
  - ```login scenario```
--------
2. ## Sokatest with HPA Enabled - 5K TPS ##

- **AKS Cluster Autoscaler configuration:** Min no of nodes - 6 and Max no of nodes - 40 
- **HPA with 70% CPU Usage**
- **TPS: 4986 with 14 AKS Nodes**	

**Service Pods configuration & Usage:**
| Service Name | CPU Limit(Core)| Memory Limit | Min Pods | Max Pods | Pod Usage |CPU Utilization(Max) %|
|--------------|----------|--------------|----------|----------|----------|----------|
|Analytics     |  0.5    |      1G     |     1    |     6    |3|78|
|API Manager   |    2    |     1G    |    1   |    30    |3|77|
|AdminUtils    |    0.5    |    1G        |    1    |   1     |6|100|
|Content       |   0.3      |    3G       |   1     |    12      |1| 48|
|Knowledgemw   |    .5    |     500M      |    1    |    10      |4|100|
|Learner       | 0.5      |   3G        |  1     |    40    | 24|100|
|LMS           | 0.5      |    3G         | 1       |  30      |11|100|
|Player        |   0.3     |   600M          |   1    |  16    |3|76|
|Nginx-private-ingress    | 0.1    |  100M   |   1  |   6   |1|1|
|Search        |    0.3    |    3G         |   1     |   10    |5|100|
|Telemetry     |    0.2  |     200M       |   1     |    40    |11|99|


**Infra Configuration & Usage:**
| Service Name | Configuration| CPU Usage (Max) | Load AVG(Max) |Memory Usage(Max) |
|--------------|----------|--------------|----------|----------|
|AKS Node      | 8 Core, 16GB |  50.93%  |   22.52       |	8.34 GB|
|Cassandra    | 5 Node (16 Core , 64GB) |  38.92%    | 9.36    |12.564 GB|
|ES-LMS       | 3 nodes (16core , 64GB) |  15.13%    |    4.66   |28.5413 GB|
|COMP-LMS     | 3 nodes (16core, 32 GB) |   15.13%  |   6.06  |19.6504 GB|
|Kafka        | 3 nodes (4core , 16GB)  | 16.08%     |  1.57    |	6.796 GB|
|Redis - LP   | 1 node (2core, 8GB)     |  2.29%    |  0.32  |2.2 GB|
|Redis -DP    | 1 node (32core, 128GB)  | 10.85%     |  2.24   |	71.4 GB|
|KeyCloak     | 4 nodes (4core, 16GB)   | 99.92%   |  29.41  | 4.343 GB |

**JMX Details**
- cluster1.jmx (1 JMeter Master, 4 JMeter Slaves)
  - ```sendTelemetry```
  - ```readContent```
  - ```searchContent```
  - ```readForm```
  - ```getCourseHierarchy```
  - ```dialAssemble``` 
- learner.jmx (1 JMeter Master, 4 JMeter Slaves)
  - ```getUserProfileV3```
  - ```getUserProfileV2``` 
  - ```getUserProfile```
  - ```searchManagedUser```
  - ```userFeed```
  - ```readUserConsent```
  - ```updateUserConsent```
  - ```updateUser```
  - ```searchUser```
- lms.jmx (1 JMeter Master, 4 JMeter Slaves)
  - ```readContentState```
  - ```updateContentState```
  - ```istCourseEnrollments```
  - ```getBatch```
  - ```searchCourseBatches```
- ananlytics.jmx (1 JMeter Master, 2 JMeter Slaves)
  - ```deviceRegister```
  - ```deviceProfile```
  - ```registerMobileDevicev2```
- login.jmx (1 JMeter Master)
  - ```login scenario```


-------
**Cluster wise script details (Old scripts):**

**sokatest-autoscale-cluster1.jmx:**
- telemetry 
- readContent 
- readForm
- readFramework


**sokatest-autoscale-cluster2.jmx:**
- Content V1 Search
- OrgSearch
- ContentHierarchy
- TenantInfo 

**sokatest-autoscale-cluster3.jmx:**
- Get User Profile V3
- Get User Profile V2
- Get User Profile V1
- Search Managed User 
- User Feed Read

**lms-apis.jmx:**
- readContentState
- updateContentState
- listCourseEnrollments
- getBatch 
- searchCourseBatches 

**analytics.jmx:**
- Device Register
- Device Profile
- Register Mobile Devicev2

3. ## Sokatest with HPA Enabled - Testrun with DNS Name 
TestResult: [Test run result with DNS Name](https://github.com/juthipaul/sunbird-perf-tests/blob/master/sunbird-platform/soaktest/soaktest-with-dnsname.md).
