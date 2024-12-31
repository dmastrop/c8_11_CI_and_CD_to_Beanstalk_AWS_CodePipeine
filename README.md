# This README is for project 11
 
test commit to CodeCommit project11 with IAM user of project9
 test commit with pipeline
 test SNS display name
 test adding second email subscription to SNS
 test push to giltab 
 test 12/26 to CodeCommit
test3

# Introduction:

This project builds upon project9 and extends the CI portion of CodeBuild1 sonar analysis and CodeBuild2 artifact creation and deploy to s3 bucket to a CD to ElasticBeanstalk of the application.

As in project9 this is a dual source project. 
push from VSCode CodeCommit OR push from VSCode to VPS gitlab pipelline. The first option uses CodeCommit as source for CodeBuilds whereas second option uses VPS gitlab repo as source for CodeBuilds.
Maven is used to build and CodeArtifact used to fetch dependencies from maven repo. pom.xml settings.xml
The CodeBuilds are integrated into discrete CodePipelines for each option so that both types can be tested. S3 bucket stage is last stage of the CI pipeline.
A new pipeline gitlab_to_CodePipeline_project11_CD_ElasticBeanstalk will be created for the deployment of the .war artifact to ElasticBeanstalk.

NOTE: as in projec9, separate CodePiplelines must be used for the CI implementation because the CodeBuilds have different configurations due to the different source code (CodeBuild repo vs. Gitlab repo)


NOTE: as in project9, The gitlab self managed instance is on the private DevOps VPS linode ecosystem and requires an AWS Connection configuration from AWS to the gitlab docker instance.
The traefik reverse proxy on the linode VPS that hosts the gitlab self managed docker instance has to be configured to whitelist the AWS source ip address blocks for us-east-1 where my developer tools CodePipeline and its constituents are configured. Otherewise the AWS connection to gitlab VPS instance fails.


NOTE: the relevant branches are ci-aws for the CI pipeline implementation and cd-aws for the CD pipeline implementation.
NOTE: the 2 pipeliens are called gitlab_to_CodePipeline_project11_CI_analysis_artifact_S3_1 for the CI gitlab source and gitlab_to_CodePipeline_project11_CD_ElasticBeanstalk for the CD gitlab source pipeline.

The standard CodeBuild pipelines are called vproifile-CI-pipeline-project11 and vprofile-CD-pipeline-project11

NOTE: project9 IAM user is used for both project9 and 11 (added the ARN of project11 to the project9 IAM user). This simplifies the role provisioning between the two projects since they are overlapping anyways.


# CD deployment extension with new pipeline to ElasticBeanstalk. 

Use the artifact build .war file from the CI pipleine and deploy it to ElasticBeanstalk using a new CodePipeline.

First get the basic push to CodeCommit CD pipeline working again then extend to a gitlab source CD pipeline.

Note that the application.properties has to be modified in the vprofile tomcat frontend app so that it can connect to a backend RDS mysql (mariadb) server running on AWS. So the ElasticBeanstalk tomcat will host the vprofile frontennt app, the .war file has to be rebuild in a new pipeline stage that incorporates the RDS endpoint, login and password, and finally this beanstalk .war needs to be deployed to the running Beanstalk instance.  

Also note that RDS security group has to permit the inbound connections from the tomcat Beanstalk instances. There are 2 loadbalanced tomcat instances configured in the Beanstalk environment.   Note that the healthcheck has to to be changed to /login, the default redirected page of the frontend app.  Also note that cookie based stickiness needs to be configured on the tomcat instances so that the same connection flows thorugh the same tomcat EC2 instance which connects to the backend RDS db. The backend RDS connection requires that persistent connections be made for each separate browser instance login to the frontend app.


# Prerequisites
#
- JDK 1.8 or later
- Maven 3 or later
- MySQL 5.6 or later

# Technologies 
- Spring MVC
- Spring Security
- Spring Data JPA
- Maven
- JSP
- MySQL
# Database
Here,we used Mysql DB 
MSQL DB Installation Steps for Linux ubuntu 14.04:
- $ sudo apt-get update
- $ sudo apt-get install mysql-server

Then look for the file :
- /src/main/resources/accountsdb
- accountsdb.sql file is a mysql dump file.we have to import this dump to mysql db server
- > mysql -u <user_name> -p accounts < accountsdb.sql


