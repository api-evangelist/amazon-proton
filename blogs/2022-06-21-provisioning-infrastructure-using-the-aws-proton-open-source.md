---
title: "Provisioning infrastructure using the AWS Proton open-source Backstage plugin"
url: "https://aws.amazon.com/blogs/containers/provisioning-infrastructure-using-the-aws-proton-open-source-backstage-plugin/"
date: "Tue, 21 Jun 2022 15:47:10 +0000"
author: "Niall Thomson"
feed_url: "https://aws.amazon.com/blogs/containers/tag/aws-proton/feed/"
---
<h2>Introduction</h2> 
<p>The concept of the Internal Developer Platform (IDP) is becoming increasingly popular as it’s an innovative way for organizations to boost development velocity and reduce time to market. The IDP provides a set of shared capabilities that provide a standardized way for development teams to deploy applications to production. It is common for these platforms to simplify how developers interact with underlying technology, like container orchestrators, and codify organizational standards and best practices in a manner that is easy to consume.</p> 
<p>A critical part of a successful IDP is the ability for development teams to self-serve—whether this be infrastructure provisioning, application deployments, or even kicking off new services—reducing load on platform teams who can then reinvest in platform capabilities and standards rather than repetitive processes.</p> 
<p>There are many methods by which organizations can choose to allow developers to interface with a platform—for example using a ticket system that drives automated processes or applying GitOps principles and taking advantage of its associated tooling. Some organizations take this a step further, choosing to engineer a “developer portal,” which can surface tools and platform capabilities from a unified web interface. However, these portals can be complex and expensive to develop and maintain since they become a critical piece of infrastructure themselves.</p> 
<h2>What is the Backstage project?</h2> 
<p><a href="https://backstage.io/">Backstage</a> is an open-source project that provides a framework for building developer portals, letting organizations provide development teams with features such as a software catalog, scaffolding tools for new projects, and aggregating the data they need from disparate development tools into a single pane of glass. The project is in the incubation stage in the Cloud Native Computing Foundation (CNCF), having moved out of beta with the 1.0 release in March 2022.</p> 
<p>The project lowers the barrier of entry for organizations to build sophisticated developer portals with an extensible data model and integration capabilities. However, even with Backstage providing the portal itself, platform teams must still invest significant time building out the ability to provision infrastructure and pipelines in a way that can be scaled across an organization, such as using Infrastructure-as-Code tooling like CloudFormation and Terraform.</p> 
<h2>Announcing the AWS Proton plugin for Backstage</h2> 
<p>Today, we’re excited to announce that AWS will be joining the organizations contributing to the Backstage open source community with our <a href="https://github.com/awslabs/aws-proton-plugins-for-backstage">AWS Proton plugin for Backstage</a>. AWS Proton is a managed service for platform engineers to increase the pace of innovation by defining, vending, and maintaining infrastructure templates for self-service deployments. With Proton, customers can standardize centralized templates to meet security, cost, and compliance goals. Proton helps platform engineers scale up their impact with a self-service model, resulting in higher velocity for the development and deployment process throughout an application lifecycle.</p> 
<p>This plugin will allow customers to integrate Proton’s infrastructure templating, provisioning, and lifecycle management into their Backstage portal to reduce platform engineering overhead. With Proton, platform engineers can associate their CI/CD pipeline, environment, and service templates together while only surfacing required inputs to developers. Then platform engineers can simply add the Proton plugin to their Backstage portal, allowing application developers to interact with Proton templates through the same portal they are using to monitor application metrics or search documentation.</p> 
<p><img alt="Platform and development team roles when using AWS Proton and Backstage" class="alignnone size-full wp-image-9826" height="258" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/06/15/backstage-1.png" width="831" /></p> 
<p>For example, app developers can create Backstage components that will be registered in the Backstage software catalog through the plugin, powered by AWS Proton service templates. This lets platform engineers use Proton to view deployment versions and push updates. Platform engineers no longer need to use home-grown tools to track updates across their deployed services. Proton amplifies platform engineer productivity, as they can scale to serve a large development team with templates for both environment and service infrastructure.</p> 
<h2>Integrating AWS Proton and Backstage</h2> 
<p>The plugin provides two mechanisms to integrate AWS Proton and Backstage, which can be used together or separately as necessary:</p> 
<ol> 
 <li>A <a href="https://backstage.io/docs/features/software-templates/writing-custom-actions">scaffolder custom action</a> allowing software template authors to create Proton services as part of the Backstage component creation process</li> 
 <li>An entity card that can be added to the component UI to provide an overview of the current state of the Proton service associated with a Backstage catalog component</li> 
</ol> 
<p>Let’s take a look at each of these in more detail.</p> 
<h3>Scaffolder in action</h3> 
<p>One of the great features of Backstage is “Software Templates,” the ability for platform engineers to define templates that development teams can use to bootstrap new projects. These templates will typically seed a new source code repository with a basic skeleton application, perhaps also including related artifacts like a <code>Dockerfile</code> and CI/CD pipeline. However, an application also needs infrastructure it can be deployed to—whether it’s a containerized environment like Kubernetes, Amazon Elastic Container Service, a set of virtual machines, or some other abstraction.</p> 
<p>The area of infrastructure provisioning is one that Backstage leaves to the platform teams to implement, with tools like CloudFormation and Terraform. This infrastructure management can quickly become complex, including not only the initial provisioning, but mechanisms to roll out infrastructure changes across potentially large workloads with Infrastructure-as-Code that lives in version control.</p> 
<p>The first integration method that we present with Proton plugin for Backstage combines the power of the Backstage software templates with the infrastructure template capabilities of AWS Proton. This lets platform teams define both application code templates, along with its associated Infrastructure-as-Code templates, presented together in a unified workflow for development teams to create new components and services.</p> 
<p>Backstage custom actions allow plugin creators to extend scaffolding functionality by introducing additional actions that can be leveraged by template authors. The custom action <code>aws:proton:create-service</code> provided by the Proton plugin allows templates to trigger the creation of a Proton service when a component is created by a development team.</p> 
<p>The following is a snippet of a Backstage template that leverages the action:</p> 
<div class="hide-language"> 
 <pre><code class="lang-yaml">spec:
  [...]
  steps:
    [...]
    - id: proton
      name: Create Proton Service
      action: aws:proton:create-service
      input:
        serviceName: ${{ parameters.component_id }}
        templateName: my-proton-template
        templateMajorVersion: '1'
        region: us-east-1
        repository: ${{ parameters.repoUrl | parseRepoUrl }}
        branchName: main
        repositoryConnectionArn: 'arn:aws:codestar-connections:us-west-2:1234567890:connection/4dde5c82-51d6-4ea9-918e-03aed6971ff3'
        serviceSpecPath: service_spec.yml</code></pre> 
</div> 
<p>You can see a full example of a software template that leverages this action <a href="https://github.com/awslabs/aws-proton-plugins-for-backstage/blob/main/docs/tutorial-assets/fargate-nginx-template/template.yaml">here</a>. A template using this action will provide feedback in the <em>Task Activity</em> screen when a developer creates a component:</p> 
<p><img alt="Proton scaffolder action in the Backstage UI" class="alignnone size-full wp-image-9827" height="421" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/06/15/backstage-2.png" width="1125" /></p> 
<p>The following diagram illustrates how the various components within the scaffolding system work together with this action:</p> 
<p><img alt="Architecture of the Proton scaffolder action in Backstage" class="alignnone size-full wp-image-9828" height="431" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/06/15/backstage-3.png" width="811" /></p> 
<p>The main flow can be summarized as:</p> 
<ol> 
 <li>The Backstage scaffolder will template the initial source code for the application and push it to a new GitHub repository</li> 
 <li>The action invokes the Proton CreateService API to create the corresponding service using a Proton service template that has already been created by the platform team</li> 
 <li>Proton will create the service and service instances associated with the specification it was provided in the template</li> 
 <li>The CodePipeline pipeline created by the Proton service will access the code in the GitHub repository to build and deploy the service to one or more service instances</li> 
</ol> 
<h3>Entity Card</h3> 
<p>Composability is a central principle to Backstage. It drives how Backstage aggregates information from disparate information sources for developers to view in a single location. Plugin authors can contribute user interface elements that can used by platform teams to construct views that are most relevant to development teams.</p> 
<p>One of the most common types of interface element that a plugin might contribute is commonly referred to as an “Entity Card,” which is generally a small UI widget that can be included on the overview page of something like a component entity. The Proton plugin for Backstage provides one of these interface elements, a condensed view of a Proton service associated with the entity being displayed including its version information, deployment status, and service instances.</p> 
<p><img alt="Screenshot of Proton entity card in the Backstage UI" class="alignnone size-full wp-image-9829" height="454" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/06/15/backstage-4.png" width="889" /></p> 
<p>After installing the plugin, this view can be activated for any entity by annotating the corresponding YAML like so:</p> 
<div class="hide-language"> 
 <pre><code class="lang-yaml">apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name:my-service
  annotations:
    aws.amazon.com/aws-proton-service: arn:aws:proton:us-west-2:1234567890:service/my-proton-service
spec:
  [...]
</code></pre> 
</div> 
<p>This functionality works by calling the AWS Proton API to retrieve data about the Proton service, and renders it with the user interface primitives provided by Backstage.</p> 
<p><img alt="Architecture of the Proton entity card in Backstage " class="alignnone size-full wp-image-9830" height="191" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/06/15/backstage-5.png" width="652" /></p> 
<h2>Getting Started</h2> 
<p>The <a href="https://github.com/awslabs/aws-proton-plugins-for-backstage/blob/main/docs/tutorial.md">Using the AWS Proton plugins for Backstage tutorial</a>, included in the GitHub repository for plugins, provides instructions on how to start using the plugin with your AWS account. This will illustrate how Backstage and Proton can be integrated to bootstrap a new application that deploys a static Nginx workload to ECS Fargate across multiple environments, including a CI/CD pipeline provided by AWS CodePipeline.</p> 
<p>It will lead you through:</p> 
<ul> 
 <li>Creating the pre-requisite AWS Proton resources</li> 
 <li>Bootstrapping your own Backstage application</li> 
 <li>Installing the Proton plugin for Backstage</li> 
 <li>Authoring a software template that integrates the plugin as part of the scaffolding process</li> 
 <li>Creating a component that uses this software template</li> 
</ul> 
<h2>What’s Next</h2> 
<p>We’re looking forward to seeing how customers can combine Backstage and AWS Proton to power their IDP initiatives, and we’re excited to evolve this integration as the Backstage project moves forward. To request features or improvements to the Proton plugin, you can raise issues in the project <a href="https://github.com/awslabs/aws-proton-plugins-for-backstage/issues">GitHub repository</a> or take a look as the <a href="https://github.com/aws/aws-proton-public-roadmap">AWS Proton service public roadmap</a>.</p> 
<p>We also know that the community is already looking for other ways to use Backstage with AWS services, and we’re keeping an eye on the requests in the Backstage repository for integration with services such as <a href="https://github.com/backstage/backstage/issues/2256">AWS CodePipeline</a> and <a href="https://github.com/backstage/backstage/issues/5355">Amazon Elastic Container Service</a>. Please provide feedback on those issues and create new requests to give us a better idea of what you’d like to see in Backstage.</p>
