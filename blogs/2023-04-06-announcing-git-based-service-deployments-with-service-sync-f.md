---
title: "Announcing Git-based service deployments with service sync for AWS Proton"
url: "https://aws.amazon.com/blogs/containers/announcing-git-based-service-deployments-with-service-sync-for-aws-proton/"
date: "Thu, 06 Apr 2023 17:58:41 +0000"
author: "Adam Keller"
feed_url: "https://aws.amazon.com/blogs/containers/tag/aws-proton/feed/"
---
<h2>Introduction</h2> 
<p>Today, <a href="https://docs.aws.amazon.com/proton/?icmpid=docs_homepage_mgmtgov">AWS Proton</a> announced service sync, a new feature that allows application developers to configure and deploy their Proton services using Git. With this feature, developers can sync their AWS Proton service with a configuration defined in a Git repository, allowing them to use Git features, like version control and pull requests, to configure, manage, review, and deploy their services. Service sync allows customers to combine AWS Proton and Git to provision standardized infrastructure through a self-service interface, keep it updated, and oversee it centrally, while continuing to use the familiar processes and mechanisms they use daily. Service sync speeds up infrastructure management and provides higher transparency and auditability over the infrastructure in use for both developers and platform teams.</p> 
<p>Git has become the indispensable tool for teams to collectively manage, version, and monitor changes in code. Its capabilities have expanded over time, transforming it into a hub for automation and orchestration, unifying version control with continuous integration and deployment processes. In this post, we’ll delve into the advantages this new feature offers developers and platform teams, explore its features, and guide you through the setup process using service sync in AWS Proton.</p> 
<h3>Developer enablement</h3> 
<p>Platform Engineering is quickly becoming the gold standard for organizations to provide a frictionless experience for developers to self-serve resources, as needed. The objective of Platform Engineering teams is to empower developers to autonomously access resources following established patterns that guarantee best practices are ingrained, while also offering visibility into their applications post deployment. This practice allows developers to focus on application development and building value for the business and avoids spending time on building out resources to support their application. With that said, there are two common patterns that platform teams use to accomplish this: building self-service internal developer platforms behind a UI (User interface) or API (Application Programming Interface), and/or GitOps. AWS Proton provides mechanisms for platform teams to centrally store templates and provides an optimal path for developers behind a rich UI and API experience. Previously, developers were required to interact with the UI or API’s to update their running service instances in AWS Proton. With today’s announcement, developers can now store their desired state of their services in a Git repository, which AWS Proton monitors and updates based on changes committed in Git.</p> 
<h2>Solution overview</h2> 
<h3>service sync manifest files</h3> 
<p>Before we demonstrate the feature, let’s first understand the required files that developers need to house in their repositories. To help understand the concepts, we’ll reference a hypothetical service template that deploys an Amazon Elastic Container Service (<a href="https://docs.aws.amazon.com/ecs/?icmpid=docs_homepage_containers">Amazon ECS</a>) Fargate service behind an Application Load Balancer.</p> 
<h3>proton-ops.yaml</h3> 
<p>The proton-ops file instructs AWS Proton about the location of the manifest file(s) for any given service instance(s) and the branch where they reside. If your team follows a trunk-based development workflow, then the instance specification (spec) files reside in the main or trunk branch. If you are using git-flow or multi-branch workflows, then each instance likely live in its own branch. This feature isn’t required on your Git workflow and enables customers to define the spec file locations and branches based on their preferences.</p> 
<p>Here’s an overview of the structure of the proton-ops file with a description of the keys and what they represent:</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">sync:
  services:
    frontend-svc:
      test:
        branch: dev
        spec: ./frontend-svc/test/frontend-spec.yaml
      staging:
        branch: main
        spec: ./frontend-svc/staging/frontend-spec.yaml
      production:
        branch: main
        spec: ./frontend-svc/production/frontend-spec.yaml
</code></pre> 
</div> 
<ul> 
 <li>sync: required top level key for AWS Proton</li> 
 <li>services: Array of services and their service instance spec file locations</li> 
 <li>frontend-svc: Represents the name of the service</li> 
 <li>service-instance-name: Name of the service instance with the branch and spec file locations</li> 
</ul> 
<h3>spec file</h3> 
<p>The spec file represents the input parameters that’re used to configure the service instance based on the developer’s needs. An example service spec could look something like this:</p> 
<div class="hide-language"> 
 <pre><code class="lang-apacheconf">proton: ServiceSpec
instances:
  - name: frontend-dev
    environment: dev-environment
    spec:
      port: 80
      desired_count: 1
      task_size: medium
      image: 'public.ecr.aws/nginx/nginx:stable'
      load_balanced: true
      load_balanced_public: true
      env_vars:
        - ENVIRONMENT=TEST
        - TESTING=True
</code></pre> 
</div> 
<p>In the above example, you can see that the developer is simply defining the inputs required for the frontend-dev instance to run in the environment called dev-environment. Depending on preference, you can stack multiple instances into a single yaml file, or have separate locations and keep the spec files scoped to individual instances. All you must do is set up the proton-ops file to match the locations of service instance spec files to branch and file location.</p> 
<p>For more information on the spec file and proton-ops file, check out the <a href="https://docs.aws.amazon.com/proton/latest/userguide/ag-service-sync-configs.html">documentation</a>.</p> 
<h2>Prerequisites</h2> 
<ul> 
 <li>A Git repository <a href="https://docs.aws.amazon.com/proton/latest/userguide/ag-repositories.html">link</a> via Codestar Connections</li> 
 <li>Have an environment to deploy the service onto</li> 
</ul> 
<h2>Walkthrough</h2> 
<p>Let’s walk through deploying a service leveraging the new service sync functionality. For this example, we have an environment deployed called service-sync-env, which contains an Amazon ECS cluster and an Amazon Virtual Private Cloud (VPC). Our service template deploys a containerized application onto the environment’s Amazon ECS cluster based on a user provided Docker image with some additional configurable inputs. We’ll use the nginx image in Amazon ECR Public located <a href="https://gallery.ecr.aws/nginx/nginx">here</a>.</p> 
<p>We’re going to start by navigating to the AWS Proton console, selecting Services and then <strong>Create</strong>. From here, we’ll be presented with a catalog of service templates available for us to deploy our application code onto. In this case, we have one service template called <strong>Load Balanced ECS Fargate Service,</strong> so we’ll choose that and proceed to the next step.</p> 
<p><img alt="choose a service template" class="aligncenter wp-image-12751 size-full" height="433" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/04/04/Service-Template.png" width="879" /></p> 
<p>Next, we’ll name our service service-sync-demo and proceed to configuring our service instances and setting up service sync.</p> 
<p><img alt="configure service sync for a service" class="aligncenter wp-image-12752 size-full" height="1150" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/04/04/Configure-Service-Instances.png" width="879" /></p> 
<p>At the top of the page, we have the choice of managing our service via the console/API, or syncing our service configuration from Git, using the new service sync feature. Once we select that option, we’re prompted with additional inputs related to the configuration.</p> 
<p>The first section is where we decide to either let AWS Proton create the spec files and push them to our Git repository via a pull request, or if we already have configuration files and will commit those files on our own. We‘ll proceed with having AWS Proton create the files.</p> 
<p>The next step is to choose the repository where we want our service-related manifest files to live. This includes the proton-ops file, as well as any service instance spec files. Some approaches to manage these files in Git are:</p> 
<ul> 
 <li>Coupled with the application code: With this approach, the proton-ops file and service spec files live alongside the application code. By coupling the manifests with the application code, developers don’t have to leave the Git repository and can easily refer to these files to understand the configuration of their service instances deployed across environments.</li> 
 <li>Central configuration repository: This approach uses a single repository for the configuration files related to all services and their respective instances, providing teams with a central location for service-related spec files. It’s recommended to use this approach when teams prefer to have a single place for all configuration files that may also want to combine multiple services into a single proton-ops file.</li> 
</ul> 
<p><img alt="configure the service instance and the service sync configuration file location for that instance" class="aligncenter wp-image-12753 size-full" height="1256" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/04/04/Service-Instances.png" width="879" /></p> 
<p>We then configure our service with the desired inputs to configure based on the application’s needs. The new addition to this screen is the service instance sync configuration. At this point, we get to define the branch and spec file path that we want AWS Proton to monitor for any changes related to the service instance spec file. We’re just adding one instance for the demonstration, so we’ll select <strong>Next</strong> to proceed.</p> 
<p><img alt="review screen prior to creating the service" class="aligncenter wp-image-12754 size-full" height="1285" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/04/04/Service-Definition-Fiels.png" width="879" /></p> 
<p>On the review page, we can see a preview of the instance spec and proton-ops files. These are the files that AWS Proton submits as pull requests to the desired branches based on the configuration set earlier. Select <strong>Create</strong>.</p> 
<p>Now it’s time for AWS Proton to get to work. Behind the scenes AWS Proton connects to the Git repository, creating the manifest files and submitting the pull requests. In moments, we can see that we have two pull requests: one for the proton-ops file, and the other for the service instance spec.</p> 
<p><img alt="view of the pull requests submitted in github from proton" class="aligncenter wp-image-12755 size-full" height="375" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/04/04/Serice-Sync-Demo.png" width="879" /></p> 
<p>Here is what the files look like:</p> 
<p><img alt="view of the proton ops file" class="aligncenter wp-image-12756 size-full" height="293" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/04/04/Service-Sync.png" width="879" /></p> 
<p><img alt="the service spec for the service instance we deployed earlier" class="aligncenter wp-image-12757 size-full" height="531" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/04/04/Proton-Spec-File.png" width="879" /></p> 
<p>AWS Proton is now monitoring the main branch and the two files for any changes. As soon as these files are merged, AWS Proton sees the commit, reviews the files for any changes, and then triggers the creation or update of the service instances.</p> 
<p>Heading back to the console, we should now see the service instance named <strong>launch-demo </strong>being deployed to our <strong>service-sync-env</strong> environment!</p> 
<p><img alt="view of the service in proton with a service instance being deployed via service sync" class="aligncenter wp-image-12758 size-full" height="370" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2023/04/04/Service-Sync-Env.png" width="879" /></p> 
<p>After a couple of minutes, the deployment status should show that it was successful. Going forward, any changes made to the spec file in Git automatically triggers an update of the service instance. The last point to address with this feature is drift. When updating a service instance via the console (i.e., with service sync enabled), AWS Proton puts a hold on syncing from Git until the user removes the hold via the proton console (or API). This is to ensure that when desired state drifts from what is in code, changes don’t get reverted.</p> 
<h2>Cleaning up</h2> 
<p>To clean up the resources provisioned, simply navigate to the service in the AWS Console. From there, select “Actions” and “Delete”. This will delete the service and all running instances.</p> 
<h2>Conclusion</h2> 
<p>In this post, we showed how users can use Git as the source of deployment orchestration when deploying applications through AWS Proton. The AWS Proton team is continuously looking at how we can innovate to provide platform engineers and developers a seamless and positive experience. We look forward to hearing how customers use this feature and welcome feedback! For any questions, bugs, or feature requests, folks can reach out to us on our public <a href="https://github.com/aws/aws-proton-public-roadmap/projects/1">roadmap</a>. Additionally, we recommend that you review the documentation prior to using the feature. Happy deploying!</p>
