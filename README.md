# Pipeline Templates

These templates will stand up all the resources necessary to hook Github repositories into a CodePipeline pipeline. The pipeline will watch for changes on environment branches (`Dev`, `Staging` or `Prod`) and pull those changes into a CodeBuild environment. CodeBuild will run unit tests, security scans and other quality gates before building a Docker image of the application in the repository and pushing the result to an ECR.

**Note**: These stacks/templates do not contain a deploy stage. (Makpar uses these stacks along with CodeDeploy to push containers to an ECS Fargate cluster; a Deploy pipeline can be set up using the ECR image outputted by this pipeline as the source stage to initiate a blue-green deployment into EKS or ECS, if desired).

## StaticStack

**Resources**
- GitHub source connection
- ECR repositories
- IAM roles for CloudWatch, CodePipeline, CodeBuild, CodeDeploy with minimal permissions
- Test report groups
- s3 bucket and cloudfront distribution for hosting documentation, test coverage, linting, etc.

## EnvStack

**Resources**
- s3 bucket for pipeline artifacts
- codebuild project
- codepipelines with source and build stages to hook into repos and push to ECR
