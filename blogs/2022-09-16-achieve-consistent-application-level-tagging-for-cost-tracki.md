---
title: "Achieve Consistent Application-level Tagging for Cost Tracking in AWS"
url: "https://aws.amazon.com/blogs/containers/achieve-consistent-application-level-tagging-for-cost-tracking-in-aws/"
date: "Fri, 16 Sep 2022 20:57:58 +0000"
author: "Tatiana Cooke"
feed_url: "https://aws.amazon.com/blogs/containers/tag/aws-proton/feed/"
---
<h2>Introduction</h2> 
<p>As organizations transform their business or grow due to market demand, they often struggle to implement the right tools to understand their <a href="https://aws.amazon.com/">AWS</a> footprint and associated cost. A large AWS footprint may include multiple AWS accounts, different infrastructure environments, and application environments for specific projects. The complexity of this footprint grows by an order of magnitude if applications are built using a microservice architecture with individual build and deployment pipelines, and frequent release cycles.</p> 
<p>To keep pace with growth and manage large AWS footprints effectively there are approaches that can help:</p> 
<ul> 
 <li>Automation of infrastructure and resource provisioning</li> 
 <li>Standardized cost tracking</li> 
</ul> 
<p>First is <em>automation</em>, where organizations move to define their infrastructure using code (Infrastructure-as-Code [IaC]). The next level of maturity comes from defining a release lifecycle for infrastructure and applications<em>.</em> This is the process of continuous integration (CI) (build and test code continuously) and continuous delivery and/or deployment (CD) (automation) bringing agility to your product release cycle. To help speed up CI/CD adoption while maintaining control and standards, platform engineering teams can provide application teams (software developers) with a self-service mechanism to deploy standardized infrastructure.</p> 
<p>In this post, we look at how <a href="https://docs.aws.amazon.com/proton/?id=docs_gateway">AWS Proton</a> helps with standardizing and automating across an organization.</p> 
<p>Second is c<em>ost tracking</em>, where organizational level visibility and a consistent view on cost is important to understand how teams spend on AWS resources. Every resource needs to be tracked for cost, and the tracking mechanism needs to be responsive to changes in resources. This becomes a big challenge with the proliferation of microservices and different environments. An application can have several microservices with each having multiple resources (such as <a href="https://docs.aws.amazon.com/ecs/?id=docs_gateway">Amazon ECS</a> tasks, <a href="https://docs.aws.amazon.com/lambda/?icmpid=docs_homepage_serverless">AWS Lambda</a> functions, and Kubernetes deployments) and each microservice has its own release cycle (CI/CD) in different environments (development/test/stage/production).</p> 
<p>How can you effectively track costs for the modern applications with microservices?</p> 
<h3>Using tags for cost allocation</h3> 
<p><a href="https://docs.aws.amazon.com/account-billing/index.html">AWS Billing and Cost management</a> provides detailed billing information at the account level. From the billing console page ,you can get a cost breakdown at an AWS service level for the whole account. However, out of the box, this does not provide visibility at a granular microservices level. How can you get to this level of granularity?</p> 
<p>AWS provides an <a href="https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html">AWS Cost Allocation Tags</a> to achieve cost tracking. Tags are <code><strong>key:value</strong></code> pairs assigned to resources created. These tags are set by the <em>organization’s administrators</em> based on their organization’s method of classifying and tracking workloads. The following screen shot provides a way of tracking using <em>Cost Center </em>and <em>Environment</em>.</p> 
<h2>Solution overview</h2> 
<p><img alt="Customers can use tags to track the same cost center for two different workloads, one running user:Stack = Test, the other running user:Stack = Production." class="alignnone size-full wp-image-10696" height="315" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/09/16/cooke1.jpg" width="505" /></p> 
<p>Once these tags are <a href="https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html">activated</a> and applied to resources, all resources with a particular tag can be queried and costs can be consolidated to provide a granular view (such as specific cost center <code>CostCenter:78925</code> or by environment <code>userStack:Production</code>). We now have a mechanism available, but how are these tags applied and how are they used to gain visibility and get actionable insights?</p> 
<h3>Challenges with tagging and effective use of tags</h3> 
<p>Applying tags to all resource is a great solution, but what are the challenges with tagging?</p> 
<ul> 
 <li>First, every resource that has cost associated with it has to be tagged when resources are added, modified, or deleted. (Tags for individual resources can added in code using IaC).</li> 
 <li>Second, tags need to be set at a granularity so management can see environment level (such as development/test/stage/production) and application level (and at service level [microservices]) resources.</li> 
 <li>Third, granular tagging at a microservices level for containerized workloads becomes daunting as applications and microservices proliferation. Maintaining versions of microservices compounds this problem.</li> 
 <li>Finally, if each application team uses a different set of tags to classify resources for their application, then a consistent view of spend cannot be achieved at an organizational level. It’s clear that tagging needs to be managed by a central entity at the organizational level and not by individual application teams.</li> 
</ul> 
<p>The reasons listed above clearly indicates applying tags consistently and maintaining them in a large and rapidly growing organization in not easy. For effective cost tracking, there needs to be an automated way to apply tags to resources uniformly at a granular level (in other words, AWS account level, environments created, and at application or microservice level). How can this be achieved?</p> 
<h2>Walkthrough</h2> 
<h3>AWS Proton</h3> 
<p><a href="https://aws.amazon.com/proton/">AWS Proton</a> is a managed container and serverless application delivery service that helps platform teams to automate creation of consistent infrastructure and application templates that can be consumed by application team. AWS Proton automatically tags the resources that are created in <a href="https://docs.aws.amazon.com/cloudformation/?id=docs_gateway">AWS CloudFormation</a>, and supports consistent Terraform tagging. As a result, AWS Proton is a central location from where consistent resource definition and tracking can be achieved.</p> 
<p>AWS Proton achieves this through the use of Environment templates and Service templates:</p> 
<ul> 
 <li><strong>Environment Templates</strong>: Create common infrastructure resources (such as <a href="https://docs.aws.amazon.com/vpc/?id=docs_gateway">Amazon VPC</a>, Amazon ECS, and <a href="https://docs.aws.amazon.com/parallelcluster/?id=docs_gateway">AWS ParallelCluster</a>) to provide an easy way to version control and replicate infrastructure.</li> 
 <li><strong>Service Templates</strong>: Create application-specific infrastructure resources (such as <a href="https://docs.aws.amazon.com/ecs/?icmpid=docs_homepage_serverless">AWS Fargate</a> and AWS Lambda) that provide self-service mechanism for application developers to develop and test their applications.</li> 
</ul> 
<p>The above templates are version controlled by AWS Proton, which make it easy to managed and maintain changes to infrastructure and applications in a dynamic organization from a central location.</p> 
<p>AWS Proton supports <a href="https://aws.amazon.com/cloudformation/">AWS CloudFormation</a> and <a href="https://www.terraform.io/language">HashiCorp Configuration Language <em>(Terraform)</em></a>. There are differences in how templates created with above IaC languages are managed. AWS-managed provisioning (CloudFormation) provides an integrated experience and while self-managed provisioning (HCL using Terraform) uses a pull request to a source code repository.</p> 
<p>See <a href="https://docs.aws.amazon.com/proton/latest/userguide/ag-works-prov-methods.html">Provisioning Methods</a> documentation for how AWS-managed and Self-managed provisioning works (read blog <a href="https://aws.amazon.com/blogs/containers/aws-proton-self-managed-provisioning/">AWS Proton Self-Managed Provisioning</a> for more details).</p> 
<p>How does this help with all the challenges associated with consistent tagging?</p> 
<h3>AWS Proton automatic tagging</h3> 
<p>The key mechanism that AWS Proton uses is template definition for infrastructure and Applications. When templates are created, tags can be easily injected. Since AWS Proton serves as a central location for creating infrastructure for both platform and application teams, the tagging becomes consistent automatically. This provides a consistent way of visualizing the resources based on cost tags at the Infrastructure environment and at each service.</p> 
<p>Tags at infrastructure level (such as VPC and ECS Cluster).</p> 
<p><img alt="In the AWS Console, you can see the different tags associated with a VPC, including tags specific to Proton that include proton:template, proton:environment, and proton:account." class="alignnone size-full wp-image-10697" height="780" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/09/16/cooke2.jpg" width="1429" /></p> 
<p><img alt="In the AWS Console, you can see the different tags associated with an ECS cluster, including proton:template, proton:environment, and proton:account." class="alignnone size-full wp-image-10698" height="947" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/09/16/cooke4.jpg" width="1406" /></p> 
<p>Tags at application and service level (such as frontend service).</p> 
<p><img alt="In the Proton console, the frontend ECS cluster service has the tags proton:service, proton:service-instance, proton:template, proton:environment, and proton:account." class="alignnone size-full wp-image-10699" height="609" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/09/16/cooke5.jpg" width="1438" /></p> 
<blockquote>
 <p><strong>Note</strong>: For AWS-Managed Provisioning (CloudFormation templates) Tags are added to automatically by AWS Proton. For Self-managed Provisioning (Terraform Templates), tagging is accomplished using <code><strong>default_tags</strong></code> feature of Terraform (read about default tags <a href="https://www.hashicorp.com/blog/default-tags-in-the-terraform-aws-provider">here</a>). Your AWS Proton template needs to include a Terraform variable <code><strong>proton_tags</strong></code> need to be defined in your AWS Proton template and included as default_tags in your AWS provider.</p>
</blockquote> 
<h3>AWS Cost Explorer tag based visualization</h3> 
<p>As mentioned earlier <a href="https://aws.amazon.com/aws-cost-management/aws-cost-explorer/">AWS Cost Explorer</a> (and other third-party tools) can be used to visualize costs at a granular level (e.g., Environments and Services) using Cost Tags automatically injected by AWS Proton. The following diagram shows how this can be done from AWS Cost Explorer.</p> 
<ol> 
 <li>Select <strong>Tag </strong>in the <strong>Filter</strong> section on the right hand side</li> 
 <li>Select <code><strong>proton:service</strong></code> tag</li> 
 <li>Select the service name you wish to view (such as <code><strong>arn:aws:proton:us-west-2:271661399851:service/frontend</strong></code>).</li> 
</ol> 
<p><img alt="Now in Cost Explorer, you can filter based on proton-specific tags like proton:service." class="alignnone size-full wp-image-10700" height="413" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/09/16/cooke6.jpg" width="500" /></p> 
<p><img alt="Now in Cost Explorer, you can select a tag for a specific service like test-frontend." class="alignnone size-full wp-image-10701" height="417" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/09/16/cooke7.jpg" width="497" /></p> 
<p><img alt="By selecting frontent tags, you can filter to only see associated costs in Cost Explorer." class="alignnone size-full wp-image-10702" height="415" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/09/16/cooke8.jpg" width="499" /></p> 
<p>This filter applies to all application resources consumed by service frontend<strong>. </strong>The following report shows how a sample output from AWS Cost Explorer.</p> 
<p><img alt="Cost Explorer will then visualize only the costs associated with the service tag you use to filter. Getting started with AWS Proton" class="alignnone size-full wp-image-10703" height="584" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/09/16/cooke9.jpg" width="1389" /></p> 
<p>The post on <a href="https://aws.amazon.com/blogs/containers/introducing-new-templates-to-the-aws-proton-template-library/">AWS Proton Template Libraries </a>provides samples templates for both AWS CloudFormation and Terraform. Also included are sample applications corresponding to the AWS Proton Environment templates. This provides an easy way to get started with AWS Proton. You can also learn more about <a href="https://aws.amazon.com/blogs/containers/using-aws-proton-as-a-provisioning-mechanism-for-amazon-eks-clusters/">AWS Proton as a provisioning mechanism for Amazon EKS clusters</a> and <a href="https://github.com/aws-samples/aws-proton-cloudformation-sample-templates/tree/main/lambda-multi-svc">AWS Proton Sample Multi Service deployment</a>.</p> 
<h3>Additional considerations for Terraform: self-managed provisioning</h3> 
<p>As mentioned earlier AWS Proton Tagging of resources for Terraform based Templates is accomplished using <code><strong>default_tags</strong></code> feature of Terraform (read about <a href="https://www.hashicorp.com/blog/default-tags-in-the-terraform-aws-provider">Default Tags in the Terraform AWS Provider</a>). AWS Proton uses a Terraform variable <strong><code>proton_tags</code></strong>. The variable needs to be defined in your AWS Proton template and included as default_tags in your AWS provider. You can learn more <a href="https://docs.aws.amazon.com/proton/latest/adminguide/resources.html">here about example tag propagation</a>. The following example shows how you can achieve automatic tagging with Terraform.</p> 
<p><strong>Terrafrom AWS provider (with default_tags)</strong></p> 
<div class="hide-language"> 
 <pre><code class="lang-git"># Configure the AWS Provider
provider "aws" {
  region = var.aws_region
  default_tags {
    tags = var.proton_tags # All tags in proton_tags will be included as default tags
  }
}</code></pre> 
</div> 
<p>AWS Proton renders the HashiCorp Configuration Language (HCL) files by including the necessary tags in the <code>proton_tags</code> variable and AWS provider is updated to include the tags in all resources created by default.</p> 
<div class="hide-language"> 
 <pre><code class="lang-git">{
  "proton_tags" : {
    "proton:account" : "1234567890",
    "proton:template" : "arn:aws:proton:us-east-1:1234567890:environment-template/ecs-ec2-env",
    "proton:environment" : "arn:aws:proton:us-east-1:1234567890:environment/dev"
  }
}
</code></pre> 
</div> 
<p>Another consideration is to automation Terraform provisioning using GitHub actions. The AWS Samples GitHub repository provides a <a href="https://github.com/aws-samples/aws-proton-terraform-github-actions-sample">GitHub actions sample repo</a> that can be used to automate Terraform infrastructure provisioning process when AWS Proton submits a pull request.</p> 
<h2>Prerequisites</h2> 
<p>To get started, make sure you have an AWS account, at least one IaC file in CloudFormation or Terraform that you can use to define a service, and get started with AWS Proton.</p> 
<h2>Cleaning up</h2> 
<p>After trying this tagging process, remember to delete example resources to avoid future costs.</p> 
<h2>Conclusion</h2> 
<p>In this post, we showed you how to effectively track and manage resources and track costs in an organization that develops applications based on modern application development methodologies using AWS Proton. Using AWS Proton’s automatic tagging feature, an organization can apply consistent infrastructure and application-level tags and visualize cost usage by environment application (per microservice and serverless application) and get actionable insights using tools such as AWS Cost Explorer.</p>
