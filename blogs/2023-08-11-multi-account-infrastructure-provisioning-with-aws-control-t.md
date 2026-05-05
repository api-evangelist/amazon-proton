---
title: "Multi-account infrastructure provisioning with AWS Control Tower and AWS Proton"
url: "https://aws.amazon.com/blogs/containers/multi-account-infrastructure-provisioning-with-aws-control-tower-and-aws-proton/"
date: "Fri, 11 Aug 2023 17:46:57 +0000"
author: "Pratip Bagchi"
feed_url: "https://aws.amazon.com/blogs/containers/tag/aws-proton/feed/"
---
<h2>Introduction</h2> 
<p>The majority of the enterprise customers tend to establish centralize control and well-architected organization-wide policies when it comes to distribution of cloud resources in multiple teams. These teams are primarily divided into three categories: IT operations, Enterprise Security, and Application (App)-development. While delivery of business value from application standpoint falls under the purview of the App-development teams, the IT operations teams’ control the cloud resource provisioning and security teams ensures the delivery and coordination between these teams happens at scale.</p> 
<p>Within AWS, <a href="https://aws.amazon.com/controltower/">AWS Control Tower</a> offers the easiest way to set up and govern a secure, multi-account environment. It establishes a landing zone based on best-practices blueprints, and it enables governance using guardrails you can choose from a pre-packaged list. The landing zone is a well-architected, multi-account baseline that follows AWS best practices. First, it’s good to know that AWS Control Tower shares a lot of terminology with the AWS Organizations service, including the terms organization and organizational unit (OU). While Organization refers to an entity that you create to consolidate your AWS accounts for administration as a single unit, Organization unit acts as a container for Accounts within a root and provides necessary hierarchy.</p> 
<p>Often there is a need for securing infrastructure in a consistent and compliant fashion, which requires a decoupling of the infrastructure management from the business application delivery that results in undifferentiated heavy lifting by teams.</p> 
<p>The pattern discussed in this post addresses this challenge by using a combination of <a href="https://aws.amazon.com/controltower/">AWS Control Tower</a> to set up a well-architected, multi-account environment and <a href="https://aws.amazon.com/proton/">AWS Proton</a> to simplify multi-account continuous integration and continuous delivery (CI/CD) Deployments for application deployment and management.</p> 
<h2>Solution overview</h2> 
<h3>Multi-account deployment using AWS Proton</h3> 
<p>AWS Proton service is a two-pronged automation framework. As a platform team administrator, you create <em>environment infrastructure as code template</em> that defines shared infrastructure used by multiple applications and <em>service templates</em> that define deployment tooling for serverless and/or container-based applications. As an application developer, AWS Proton enables you to select desired service from the available <em>service templates </em>to automate your application deployments.</p> 
<p>For platform teams to improve visibility and efficiency at scale, AWS Proton offers a capability called <em>Environment Account Connections</em>. Environment account connections help platform teams to establish secure bi-directional connections between a single management account and multiple development team accounts, also referred to as environment accounts and is described in the following figure.</p> 
<p><img alt="This picture describes a typical Multi-account deployment architecture using AWS Proton, where AWS Proton's management account been used to vend infrastructure in multiple AWS accounts used in this blog." class="aligncenter size-full wp-image-13703" height="477" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/08/02/Multi-account-deployment.png" width="879" /></p> 
<h2>Walkthrough</h2> 
<p>The solution described in this post assumes that you are using AWS Control Tower to create OUs and accounts. To know more on how to create an OU in the AWS Control Tower from <a href="https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/learn-whats-new.html">AWS Management Console</a>, refer to the AWS Documentation <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org_create.html">here</a>.</p> 
<h2>Prerequisites</h2> 
<p>We used an <a href="https://aws.amazon.com/cloud9/">AWS Cloud9</a> instance to run this tutorial and if you want to create a Cloud9 instance in your account, refer to the AWS Documentation <a href="https://docs.aws.amazon.com/cloud9/latest/user-guide/create-environment.html">here</a>. If you are not using <a href="https://aws.amazon.com/cloud9/">AWS Cloud9</a> you need to install the latest version of the <a href="https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html">AWS CLI</a>.</p> 
<h3>Step 1: Create service control policy for OUs</h3> 
<p>For this solution, we created two OUs to demonstrate a typical customer use case. The first one is the Management OU for the AWS Proton Management Account, where platform teams can maintain the AWS Proton environment templates and service templates. The second one is the Development OU, which is used to create the AWS Proton Environment Account, where developer teams host their business applications.</p> 
<p>Service control policies (SCPs) are a type of organization policy that you can use to manage permissions in your OUs. Here, we have created a simple SCP <em>AWS-Proton-Blocker</em> and associated that with our Development OU. This SCP prevents any developer environment account from using AWS Proton to provision any resources while ensuring platform teams have full control on Infrastructure provisioning.</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Deny",
      "Action": [
        "proton:*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
</code></pre> 
</div> 
<p>Note: This policy can be scoped i.e., by providing fine-grained access control relevant for a particular team as per the needs of your organization.</p> 
<p>To learn more about how to create SCP, refer to AWS documentation <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_create.html#create-an-scp">here</a><a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_create.html#create-an-scp">.</a></p> 
<p>To learn more about how to attach SCP to OU, refer to AWS documentation <a href="https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_attach.html%20">here</a>.</p> 
<h3>Step 2: Testing the SCP</h3> 
<ol> 
 <li>Log in to the AWS Account created under Development OU from the login link provided to you by AWS at the time of creating AWS Account. You’ll find log in details in the welcome email. Alternatively, you can also find the account login URL on the <a href="https://aws.amazon.com/servicecatalog/">AWS Service Catalog</a> dashboard under <strong>Provisioned Product</strong>.</li> 
 <li>Search for <strong>AWS Proton</strong> from the <strong>AWS Management Console</strong>.</li> 
 <li>Create an Environment template in AWS Proton as mentioned <a href="https://docs.aws.amazon.com/proton/latest/userguide/ag-getting-started-console.html#ag-getting-started-step2">here</a>.</li> 
 <li>The Environment template creation fails due to the explicit deny at the OU level with the following error.</li> 
</ol> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">User: arn:aws:sts:[aws-account-number]:assumed-role/AWSReservedSSO_AWSAdministratorAccess_12345678abcdef/developeruser@amazon.com is not authorized to perform proton:GetTemplateSyncConfig on resource arn:aws:proton:us-east-1:[aws-account-number]:environment-template/example-environment with an explicit deny in a service control policy </code></pre> 
</div> 
<p>By using the SCP at the OU level, we ensure that the Platform Engineering team can enforce the organizational guardrails between the service teams and departments within an organization.</p> 
<h3>Step 3: Create environment template in the AWS Proton management account</h3> 
<p>To run AWS Command Line Interface (AWS CLI) commands in the provisioned AWS accounts, we created <a href="https://aws.amazon.com/cloud9/">AWS Cloud9</a> workspaces and attached an AWS Identity and Access Management (<a href="https://docs.aws.amazon.com/iam/?icmpid=docs_homepage_security">AWS IAM</a>) role with AdministratorAccess policy to the AWS Cloud9 instances in accounts created under Management and Development OUs. Please follow these <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role">instructions</a> to attach the AWS IAM role to an Amazon Elastic Compute Cloud (<a href="https://docs.aws.amazon.com/ec2/?icmpid=docs_homepage_compute">Amazon EC2</a>) instances.</p> 
<p>For this post, we forked <a href="https://github.com/aws-samples/aws-proton-workshop-code">this</a> GitHub repository and created a <a href="https://docs.aws.amazon.com/codestar/?icmpid=docs_homepage_devtools">AWS CodeStar</a> connection. Please find the steps to <em>Set up an AWS CodeStar connection</em> in the <a href="https://docs.aws.amazon.com/proton/latest/userguide/setting-up-for-service.html">AWS Documentation</a>. AWS Proton uses a source connection to trigger template updates or new application deploys whenever a new change is introduced. For the scope of this post, the connection is managed through the AWS CodeStar connections using GitHub as a provider. To read more on <em>Service Sync</em> configuration, please refer to this <a href="https://docs.aws.amazon.com/proton/latest/userguide/create-service-sync.html">documentation</a>.</p> 
<ol> 
 <li>Create a new environment template (multi-svc-env) based on the templates located under multi-svc-env/v1 and published it to a major version. This structure holds the <a href="https://docs.aws.amazon.com/cloudformation/?icmpid=docs_homepage_mgmtgov">AWS CloudFormation</a> template that we’ll use to provision the environment’s infrastructure.</li> 
</ol> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">aws proton create-environment-template \
--region ${AWS_REGION} \
--name "multi-svc-env" \
--display-name "Multi Service Environment" \
--description "Environment with VPC and public subnets"
</code></pre> 
</div> 
<p>Next, create a template sync configuration to register new environment template versions automatically.</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">REPOSITORY_ARN=$(aws proton list-repositories | \
jq -r '.repositories[] | select( .name | endswith("aws-proton-workshop-code")) | .arn');
REPOSITORY_NAME=$(echo $REPOSITORY_ARN | cut -d':' -f7);
REPOSITORY_PROVIDER=$(echo $REPOSITORY_ARN | cut -d':' -f6 | tr a-z A-Z);
aws proton create-template-sync-config \
--region ${AWS_REGION} \
--repository-name $REPOSITORY_NAME \
--repository-provider ${REPOSITORY_PROVIDER#"REPOSITORY/"} \
--branch main \
--subdirectory "aws-managed/multi-svc-env" \
--template-name "multi-svc-env" \
--template-type "ENVIRONMENT"
</code></pre> 
</div> 
<p>To publish it, we need to run the following command:</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">aws proton update-environment-template-version \
--region ${AWS_REGION} \
--template-name "multi-svc-env" \
--major-version "1" \
--minor-version "0" \
--status "PUBLISHED"
</code></pre> 
</div> 
<h3>Step 4: Create environment account connection from management account in AWS Proton</h3> 
<p>AWS Proton alleviates complicated cross-account policies by using a secure environment account connection feature. With environment account connections, platform engineers can give AWS Proton permissions to provision infrastructure in other accounts. To create and provision an environment from AWS Proton management account, log into the AWS Account created by AWS Control Tower under the Management OU. You’ll find log in details in the welcome email accordingly.</p> 
<ol> 
 <li>Create a scoped AWS IAM role in the AWS Proton management account</li> 
</ol> 
<pre><code class="lang-bash">wget -O ~/environment/proton-account-connection-roles.yaml \
"https://static.us-east-1.prod.workshops.aws/public/84bb5775-ab15-4f18-8daf-2f4674e84233/static/cf-templates/proton-account-connection-roles.yaml"

cd ~/environment

aws cloudformation deploy \
--template-file proton-account-connection-roles.yaml \
--stack-name AWSProtonWorkshop-AccountConnectionRoles \
--parameter-overrides "EnvironmentAccountId=${SECONDARY_ENV_ACCOUNT_ID}" \
--capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM"&lt;/code&gt;&lt;/pre&gt;&lt;/div&gt;</code></pre> 
<ol start="2"> 
 <li>Now log in to the AWS Account and navigate to the AWS <a href="https://us-west-2.console.aws.amazon.com/proton/home?region=us-west-2#/">Proton’s Console </a>created under the Development OU, or copy and paste the output of AWSProtonWorkshop-AccountConnectionRoles stack in your browser to login to the Development AWS Account. Choose <strong>Environment Account Connections</strong> from the navigation pane. Then select <strong>Request to Connect</strong> in <strong>Sent requests to connect to a management account</strong>. (For the Management account ID use the Proton Management Account).</li> 
</ol> 
<p><img alt="" class="aligncenter size-full wp-image-13704" height="312" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/08/02/Environment-Account.png" width="879" /></p> 
<ol start="3"> 
 <li>Switch back or log in again to the management account and you should have a new environment account connection request now in the AWS Proton Console. Go ahead and accept it!</li> 
</ol> 
<p><img alt="" class="aligncenter size-full wp-image-13705" height="139" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/08/02/Connection-requests.png" width="879" /></p> 
<h3>Step 5: Create environment in Developer Team account</h3> 
<p>After defining the environment template, service template in the AWS Proton Management account and establishing the <strong>Environment Account Connections</strong> to the development account, we’re ready to provision the environment in the development account using AWS Proton.</p> 
<p>First, we define the specification, which is a YAML-formatted string that provides inputs defined in the environment template bundle schema file. A sample can be found <a href="https://github.com/aws-samples/aws-proton-workshop-code/blob/main/aws-managed/multi-svc-env/v1/schema/schema.yaml">here</a>.</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">SPEC=$(cat &lt;&lt;-EOF
proton: EnvironmentSpec
spec:
  vpc_cidr: 172.16.0.0/16
  dns_hostname: [name-of-your-app].dev.local
EOF
);
</code></pre> 
</div> 
<p style="text-align: center;"><em>[</em>[name-of-your-app] <em>is a&nbsp; place-holder, replace that with the name of your app]</em></p> 
<p>The create-environment command creates the environment with the infrastructure needed for the App-Development teams to deploy micro-services. The template used below, deploys an Amazon Virtual Private Cloud (VPC) to secure networking boundary, an Amazon ECS cluster with optional configuration inputs like Amazon EC2 capacity, Container Insights, and Amazon ECS Executive logging for the application deployment.</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">MAJOR_VERSION=$(aws proton list-environment-template-versions \
--template-name "multi-svc-env" --region=${AWS_REGION} | \
  jq -r ' .templateVersions[] | select( .status=="PUBLISHED") | .majorVersion' | tail -1 \
); \
ACCOUNT_CONNECTION_ID=$(aws proton list-environment-account-connections \
--requested-by "MANAGEMENT_ACCOUNT" --statuses "CONNECTED" --region=${AWS_REGION} | \
  jq -r ' .environmentAccountConnections[] | select( .environmentName | startswith("multi-svc")) | .id' \
); \
aws proton create-environment \
  --region ${AWS_REGION} \
  --name "multi-svc-${AWS_ACCOUNT_ID}" \
  --template-name "multi-svc-env" \
  --template-major-version "$MAJOR_VERSION" \
  --environment-account-connection-id "$ACCOUNT_CONNECTION_ID" \
  --spec "$SPEC"
</code></pre> 
</div> 
<p>In the Developer account (i.e., environment account), you can view and access the provisioned infrastructure resources. You can now use this environment to deploy application as AWS Proton Service. Please review the <a href="https://docs.aws.amazon.com/proton/latest/userguide/ag-create-svc.html">documentation</a> to learn more about the service deployment in an AWS Proton environment.</p> 
<p><img alt="" class="aligncenter size-full wp-image-13706" height="166" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/08/02/Environments.png" width="879" /></p> 
<h2>Cleaning up</h2> 
<p>You continue to incur cost until deleting the infrastructure that you created for this post. Use the below instructions to delete AWS resources in the blog.</p> 
<p>Delete the environment:</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">aws proton delete-environment --name multi-svc-${AWS_ACCOUNT_ID}
# Wait for above to complete before proceeding next steps. It takes ~3 minutes.
</code></pre> 
</div> 
<p>Delete the environment template:</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">aws proton delete-environment-template --name multi-svc-env
</code></pre> 
</div> 
<p>Delete the template sync config:</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">aws proton delete-template-sync-config --template-name multi-svc-env --template-type ENVIRONMENT</code></pre> 
</div> 
<p>Once you have deleted your AWS Proton resources, please follow AWS Documentation <a href="https://docs.aws.amazon.com/controltower/latest/userguide/delete-account.html">here</a> to delete your AWS Control Tower resources.</p> 
<h2>Conclusion</h2> 
<p>In this post, we showed you how customers improved the security posture of multi-account infrastructure deployments by using AWS Control Tower to provision and organize AWS accounts that adhere to best-practices. We used AWS Proton to amplify platform engineering impacts and improve developer productivity. This established workflow allowed Platform teams to have a unified view of all the AWS Proton managed resources from management account using the <a href="https://aws.amazon.com/blogs/containers/introducing-the-aws-proton-dashboard/">AWS Proton dashboard</a>.</p> 
<p>To get started with AWS Proton, head over to our sample <a href="https://github.com/aws-containers/proton-codebuild-provisioning-examples">repository</a> for examples and our <a href="https://docs.aws.amazon.com/proton/latest/userguide/Welcome.html">documentation</a> for a deeper dive into its full functionality.</p>
