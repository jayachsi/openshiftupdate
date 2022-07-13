# Day3

Many ways applications can be deployed in OpenShift

1. Via YAML
2. Using Container Image
3. Using Dockerfile
4. Using application source

1. Using YAML file
   Assumptions:
   1. Container Image is ready
   2. Application build not required
   3. Image build not required

   OpenShift will deploy your application based on YAML declartive instructions

2. Using Container Image
   Assumptions:
    1. No application build is required
    2. No Docker Image build is required

   OpenShift will pull the Container Image and deploy the application into OpenShift

3. Using Dockerfile
   Assumption is, you application binary is already there, no application build is required

   OpenShift will follow the instructions in the Dockerfile you supplied from the GitHub/GitLab,etc
   OpenShift builds the custom image, injects your application binary into the image

   OpenShift will then deploy your application into OpenShift


4.Using application source - S2I (Source to Image)
  - GitHub/GitLab/version control

  - OpenShift will detect your programming language
  - Let's say your application is a Java application
  - OpenShift will make use of a Java Docker Image with Maven/Gradle
  - OpenShift will create a Pod (Build Machine)
  - Your application will be compiled within the Pod
  - Your application binary i.e jar/war/ear file will be created

  - Build a Custom Image for Java based application
  - Copy your application binary into the Custom Image

  - Using the custom image that has your application, it will deploy into OpenShift
