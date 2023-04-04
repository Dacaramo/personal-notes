# Amplify

## Setup a backend project

> For the following process you need to have the `amplify-cli` installed

1. As a root user in the **AWS console** create the amount of **IAM users** that are going to need the permission to modify, deploy, debug, etc. the project. Don't forget to generate **Access Keys** for every user and send them the keys, or give your **IAM users** permission to generate **Access Keys** and tell them to generate the keys by them selves. The keys are needed in **step 4**.
2. Run amplify init inside the folder that will contain the backend source code
3. Complete all the configuration process asked by the `amplify-cli` after running that command
4. When your IAM users are going to work with the project they should authenticate with the Access Keys
5. Create as many environments as you want by running `amplify env add`. It depends on your needs but a common configuration is to have 3 environments: One for development, one for staging and one for production.
6. While being in the dev environment start to add Amplify available ressources just like **Lambda Functions**, **DynamoDB tables**, **Cognito UserPools**, etc. locally. Try to create the ressources in the following order:
	- For adding an **AWS Cognito User Pool**: run `amplify add auth`
	- For adding an **AWS API Gateway instance**: run `amplify add api` (The first time you run this command the `aws-cli` forces you to create a **Lambda Function** with an specific route for it)
	- For adding new API routes mapped to an specific function: run `amplify add api` and select the option that let's you add new routes to the API that already exists)
	- For adding an **AWS Lambda Layer**[^1]: run `amplify add function` and choose the option of Lambda Layer when displayed
	- For connecting a **Lambda Function** to a **Lambda Layer**: run `amplify update function`, choose the name of the function to connect with the layer, choose the "**Lambda Layers** configuration" option and finally enable **Lambda Layers** for the function by choosing the **Lambda Layer** to connect to.
	- After adding the Lambda Layer, It is much better to install all of the modules and libraries inside the layer folder and not inside the folders of every Lambda function. Maybe for modules only used on a single Lambda Function it is good to install it on the Lambda Function folder and not inside the layer's folder.
	- For adding a **DynamoDB** table run `amplify add storage`, create all the columns that you need, your **Partition Key** and **Sort Key** and your **Global Secondary Indexes**.
7. Once all the initial ressources are setted up locally run `amplify push` in order to push all the changes to the cloud. After running this command every AWS ressource that you have specified previously is going to be created in the AWS Management Console.
8. Please check on the AWS Management Console that the ressources such as the Cognito User Pool were created as you expected with all the fileds that you wanted, etc. If not, you have to run `amplify remove <the ressource you want to remove>`, choose the name of the ressource to remove and run `amplify push`.

> Don't worry, you don't need to create all the ressources again for the other environments, when you switch locally from one environment to another the `amplify-cli` will tell AWS to create all the ressources for the environment you just have changed to. The ressources are going to be totally different from the ones created on the first environmment you were located at, so basically each environment will have it's own isolated ressources.

> Environments in Amplify works just like git branches, the changes made inside an environment only exists inside that environment until you decide to merge the changes with other environment. Keep in mind that updating the code of an environment and merging locally with other environments will not reflect anything on the cloud, just like a local repo and a remote repo. In order to bring changes from the cloud made and pushed by other developers run `amplify pull`. In order to send your changes to the cloud run `amplify push`. It is a good practice to always pull the changes of the cloud before starting to work on a new feature.

## Install modules in Lambda Layer 

Being located at the root of your project go to:
`amplify/backend/function/<your-lambda-layer-name>/lib/nodejs`

And always from there, install the modules you require by running:
`npm install <the-module's-name>`

## Commands

- Look at all of the [environment related commands](https://docs.amplify.aws/cli/teams/commands/#commands-overview)

[^1]: A layer of code that contains modules, libraries and reusable code such as helpers, middleware, etc.. You are able to share this layer with your **Lambda Functions** so there's no need to have that code repeated inside the folder of each function.