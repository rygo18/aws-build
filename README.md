# aws-pipeline
Templates to set up AWS pipeline and basic project.

Want to be able to quickly setup a real world infrastructure with different environments, a number of code repos and a proper workflow. 

End goals;
- Dev and Production environments to host my resources and codebase, with the ability to easily add more environments
- Multiple repos that will have different types of AWS resources in them 
    e.g.
    1. `Static` -> Which will host the more static AWS resources that do not change often like Cognito, RDS, API Gateway, etc
    2. `Framework` -> Will contain parts of our application that are not static but don't change a lot such as Lamda Layers, DB build scripts, etc.
    3. `Backend` -> This will house all of our endpoints will change frequently as endpoints are built out and changed
    4. `Web` -> May end up naming this more accurately tom somehthing like `React-Web`, `Vue-Web`, etc as we may want to seemlessly evolve the frontend.
- Ability to add in more processes later on such as testing and proper code reviews/approvals
- Have a bug branch and a feature branch. Maybe the feature branch can be pushed to an additional environment such as `Beta`.

The templates in this repo are based on the ones provided in this repo [Quickstart Trek10](https://github.com/aws-quickstart/quickstart-trek10-serverless-enterprise-cicd).

Seperation of cloudformation templates;
- Enable all of the permissions that are required in all of the environment account in order for the CICD account to complete the building of the resources
- Enable all of the permissions that the CICD account (where we push our code) requires
- Create the pipeline that will manage the logic behind building and depoying the AWS resources

Ideal outcome would be that everytime I want a new environment I simply create an AWS account in my root organization, run a cloudformation template into that new environment to set the permissions and then edit the pipeline templates in the CICD environment/account.  
Adding a new repo should be should require minimal re-working of the CICD template(s), simply just adding it to and existing cloudformation template


## Getting started/instructions
- In the root AWS account add the following 3 accounts (take note of the account IDs)
    1. CICD (Where the codebase will be hosted, the pipeline will run, etc)
    2. Dev
    3. Prod
- In each of the pipeline environments i.e. Dev and Prod create a cloudformation stack using the environment.template.yaml
- In the CICD account create the following cloudformation stacks as required;
    1. once.template.yaml -> This creates the necessary permissions that the CICD environment requires
    2. repo.template.yaml -> Create this stack per repo that is required
    3. pipeline.template.yaml -> Set up the pipeline. Will need to edit this as we add/change repos and environments