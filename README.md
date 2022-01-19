# Pipeline Templates

These templates will stand up all the resources necessary to hook Github repositories into a CodePipeline pipeline. The pipeline will watch for changes on environment branches (`Dev`, `Staging` or `Prod`) and pull those changes into a CodeBuild environment. CodeBuild will run unit tests, security scans and other quality gates before building a Docker image of the application in the repository and pushing the result to an ECR.

**Note**: These stacks/templates do not contain a deploy stage. (Makpar uses these stacks along with CodeDeploy to push containers to an ECS Fargate cluster; a Deploy pipeline can be set up using the ECR image outputted by this pipeline as the source stage to initiate a blue-green deployment into EKS or ECS, if desired).

## Setup

Copy the sample environment file into a new file and configure the environment variables for your specific scenario. See comments in sample file for more information on the purpose of each variable,

```shell
cp .sample.env .env
```

After that, you can stand up the stacks using the scripts in the */scripts/* directory. First, the **StaticStack** should be stood up,

```shell
./scripts/static-stack 
```

Once the static resources are provisioned, you can deploy the **EnvStack** by specifying the environment into which you are deploying,

```shell
./scripts/env-stack --environment <Dev | Staging | Prod>
```
## Resources
### StaticStack

- GitHub source connection
- ECR repositories
- IAM roles for CloudWatch, CodePipeline, CodeBuild, CodeDeploy with minimal permissions
- Test report groups
- s3 bucket and cloudfront distribution for hosting documentation, test coverage, linting, etc.

### EnvStack
- s3 bucket for pipeline artifacts
- codebuild project
- codepipelines with source and build stages to hook into repos and push to ECR
