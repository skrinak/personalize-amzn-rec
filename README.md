# Amazon Personalize Workshop
## Tuesday, July 9, 2019

In this workshop we'll create a recommendation engine using the same data as our previous workshop led by Rumi Olsen. Her notebook is here: [Amazon SageMaker の Factorization Machines を使用したレコメンダ システムの構築](https://github.com/rumiio/sagemaker-rec-engine-demo). In Rumi's workshop we created a recommendation engine based on the [Amazon Customer Reviews Dataset](https://s3.amazonaws.com/amazon-reviews-pds/readme.html) from scratch using a traditional machine learning method: factorization machines. In this workshops we use the Amazon managed service Personalize to achieve the same goal. 

Personalize is a much easier approach as the service does a lot of heavy lifting for you. All the work is in the setup. To use personalizes we need to: 

* Enable Sagemaker to use personalize features 
* Coordinate usage of SageMaker, Personalize, and S3 using IAM roles
    * We'll modify the IAM role created for us in SageMaker
    * and we'll create a new role for Personalize that enables S3 access and other operations
* We'll use autoML to choose a recipe for us.
* Create a solution using the recipe from autoML
* Finally we'll create a Campaign, a live AWS endpoint, to provide real-time predictions for our recommendation engine. 

We won't be injecting live event data for ongoing updates to the recommendation engine. To explore that that please follow the workshop [personalize_temporal_holdout](https://github.com/aws-samples/amazon-personalize-samples/tree/master/personalize_temporal_holdout) which is a subset of [amazon-personalize-samples](https://github.com/aws-samples/amazon-personalize-samples)

## Graphic User Interface

In the workshop we'll switch between Jupyter notebook and the Grphical User Interface. Once your dataset is prepared you can use either method to launch your Campaign. The purpose of demonstrating both is the illustrate ease of use after the initial setup. We do not prescribe or recommend either workflow. Please choose the workflow that is best suited for your tasks. CLI and other SDK approaches are also an option though may not provide every feature as do the GUI and notebook approaches. 


## Setup
1. Create an S3 bucket
    * Ensure that the bucket is located in the Tokyo region
    * Enable public permissions (Do not do this in production. This is to ease learning in the workshop)
1. Launch a SageMaker Notebook
    * Ensure you are in the Tokyo region or the same region where you created your S3 bucket.
    * Name your notebook
    * Provide additional storage of 50G
    * Create a SageMaker role
        * Once the role is created, click on it to modify the role policies
        * Edit the AmazonSageMaker-ExecutionPolicy policy to match the following: 
        ```
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "s3:GetObject",
                        "s3:PutObject",
                        "s3:DeleteObject",
                        "s3:GetBucketPolicy",
                        "s3:PutBucketPolicy",
                        "s3:ListBucket"
                    ],
                    "Resource": [
                        "arn:aws:s3:::*"
                    ]
                },
                {
                    "Effect": "Allow",
                    "Action": [
                        "iam:CreateRole",
                        "iam:AttachRolePolicy"
                    ],
                    "Resource": "*"
                }
            ]
        }
        ```

    * Save your changes
    * Stay in IAM. Click "Attach Policies"
        * In Filter Policies seach for personalize. ```AmazonPersonalizeFullAccess``` will appear. 
        * Click the checkbox on the left.
        * Click on Attach Policy
    * Return to the SageMaker Create Notebook tab. We're done modifying the SageMaker role. We'll be creating a separate Personalize role within the notebook.
    * Create your new notebook instance
1. Launch Jupyter
1. Open the notebook: Amazon Customer Reviews.ipynb

## In the Jupypter Notebook
1. Change the S3 bucket variable from ``` bucket = "personalize-demo" ``` to the bucket name you created.
1. Execute each step with the instructot. We'll be moving between the GUI and notebook. Please don't skip ahead. If you do you're liable to miss information required to complete the workshop. 
