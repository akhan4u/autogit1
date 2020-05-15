### Create Infra Application Pipeline

This Cloudformation stack will help you Deploy Application to ECS with Codepipeline.

### Pre-Requisites:

* Console or Programmatic Access to AWS Account with permissions to create a CloudFormation Stack.
* Name of S3 bucket containing the uploaded scripts and zip file. ( **THE SOURCE BUCKET MUST EXIST IN SAME AWS REGION!!!** ) 
* Email ID of the user who wish to recieve the pipeline notifications.
* Name of the ECS Cluster and Service for the Environment, where the application service has to be deployed.
* Name of the ECR Repository.
* Image Defination file which contains the details of the Service containers name, image and tag.


### Steps:

1. Create a CloudFormation Stack with the file `vm-managed-appdeploy-codepipeline.yaml`.
2. Create a zip file named `vm-managed-ecs.zip` with `imagedefinition.json` file.
3. Upload the file `vm-managed-ecs.zip` on the S3 bucket. 
4. Provide a name to your Stack and provide details of the following:
    - `S3Bucket` : The name of the S3 bucket that contains your uploaded zip file. (_`vm-managed-ecs.zip`_)
    - `S3ObjectKey` : Name of the S3 Object uploaded in this case, `vm-managed-ecs.zip`.
    - `ImageDefFileName` : Name of Image defination file, defaults to `imagedefinition.json`.
    - `SNSTopicName` : Name of the SNS Topic to get email notifications, defaults to `vm-managed-dev-appdeploy-notification`.
    - `Email`: Email ID of user who wish to recieve the Codepipeline notifications, defaults to `martin_bradley@persistent.com`.
    - `ECRRepositoryName` : Name of the AWS ECR Repostiory polled for deploying images, defaults to `vm-managed-dev-repo`.
    - `ECSClusterName` : Name of the ECS Cluster, defaults to `vm-managed-dev-ecs-cluster`.
    - `ECSServiceName` : Name of the ECS Cluster Service Name where you want to deploy your container, defaults to `vm-managed-dev-bffqa-service`.

5. Follow the next steps to create the stack and provision your stack.

6. Post creation of stack following activities will be performed:
	- Pipeline will be created in codepipeline with following as source: (**SourceStage**)
        - S3 as Source and `vm-managed-ecs.zip` as the object
        - ECR as Source to watch `vm-managed-dev-repo` for  container images with tag latest
	- In the next stage of codepipeline, the Application container image with get deployed to the ECS Cluster `vm-managed-dev-ecs-cluster` and to the service `vm-managed-dev-bffqa-service` (**AppDeployStage**)

#### Resources Created: 

- Cloudformation Stack to Create Application Deployment Pipeline in CodePipeline
- IAM Service Role with IAM Managed Policy for Codepipeline Service
- IAM Execution Role with IAM Managed Policy for Codepipeline Service
