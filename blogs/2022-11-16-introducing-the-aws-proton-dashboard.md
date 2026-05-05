---
title: "Introducing the AWS Proton dashboard"
url: "https://aws.amazon.com/blogs/containers/introducing-the-aws-proton-dashboard/"
date: "Wed, 16 Nov 2022 19:30:33 +0000"
author: "Sasi Jayalekshmi"
feed_url: "https://aws.amazon.com/blogs/containers/tag/aws-proton/feed/"
---
<h2>Introduction</h2> 
<p>Today, we are excited to announce the launch of the <a href="https://docs.aws.amazon.com/proton/latest/userguide/monitoring-dashboard.html" rel="noopener" target="_blank">AWS Proton Dashboard</a>, which is a single dashboard pane to view all the AWS Proton resources created to manage templates and deploy infrastructure. For customers who use Infrastructure as Code (IaC) to manage shared resource environments and service-specific infrastructure, the AWS Proton dashboard makes it easy to see your existing template resources and the status of your deployed infrastructure.</p> 
<p><a href="https://docs.aws.amazon.com/proton/latest/userguide/Welcome.html">AWS Proton</a> is a managed service for platform engineers to increase the pace of innovation by defining, vending, and maintaining infrastructure templates for self-service deployments. With AWS Proton, customers can standardize centralized templates to meet security, cost, and compliance goals. AWS Proton helps platform engineers scale up their impact with a self-service model, which results in higher velocity for the development and deployment processes throughout an application’s lifecycle.</p> 
<h2>Solution overview</h2> 
<p>With AWS Proton, application developers can use a self-service infrastructure to deploy new applications or update existing applications. The new dashboard gives the overall status of resources across environments in your account. The dashboard has four panes: <strong>Resources</strong>, <strong>Resource templates</strong>, <strong>Resource status summary</strong>, and <strong>Service instances</strong>.</p> 
<div class="wp-caption alignnone" id="attachment_10860" style="width: 1692px;">
 <img alt="Screenshot of AWS Proton dashboard console view" class="wp-image-10860 size-full" height="799" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/10/16/prot-1.jpg" width="1682" />
 <p class="wp-caption-text" id="caption-attachment-10860">Figure 1: AWS Proton dashboard console view</p>
</div> 
<p>Let’s look into each pane of the dashboard.</p> 
<ul> 
 <li><strong>Resources: </strong>This gives users an overall understanding of how many total AWS Proton resources have been created and provisioned, including environments, services and components.</li> 
</ul> 
<div class="wp-caption alignnone" id="attachment_10861" style="width: 905px;">
 <img alt="Screenshot of resources pane" class="size-full wp-image-10861" height="278" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/10/16/prot-2.jpg" width="895" />
 <p class="wp-caption-text" id="caption-attachment-10861">Figure 2: Resources pane of AWS Proton dashboard</p>
</div> 
<ul> 
 <li><strong>Resource templates:</strong> This view can show users how many total templates are registered with AWS Proton. This includes both draft and published templates.</li> 
</ul> 
<div class="wp-caption alignnone" id="attachment_10862" style="width: 1367px;">
 <img alt="Screenshot of resource template pane" class="size-full wp-image-10862" height="281" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/10/16/prot-3.jpg" width="1357" />
 <p class="wp-caption-text" id="caption-attachment-10862">Figure 3: Resource template pane of AWS Proton dashboard</p>
</div> 
<ul> 
 <li><strong>Resource status summary: </strong>This pane simplifies the navigation and reduces the time spent by platform teams to identify which environments and/or service instances are up to date with the latest version of the templates versus those needing updates. This includes minor and major version changes to the template. You can learn more about minor and major versions <a href="https://docs.aws.amazon.com/proton/latest/userguide/ag-template-versions.html">here</a>.</li> 
</ul> 
<div class="wp-caption alignnone" id="attachment_10863" style="width: 1362px;">
 <img alt="Screenshot of Resource status pane" class="wp-image-10863 size-full" height="276" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/10/16/prot-4.jpg" width="1352" />
 <p class="wp-caption-text" id="caption-attachment-10863">Figure 4: Resource status summary pane of AWS Proton dashboard</p>
</div> 
<ul> 
 <li><strong>Service instances: </strong>This pane provides a detailed view of each service instance with it’s name, deployment status, template version, and any deployment issues that needs to be addressed. With this view, it’s easy to sort by latest deployment date and to view trends in the types of service templates your development teams are using.</li> 
</ul> 
<div class="wp-caption alignnone" id="attachment_10864" style="width: 1368px;">
 <img alt="Screenshot of service instances pane" class="size-full wp-image-10864" height="232" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/10/16/prot-5.jpg" width="1358" />
 <p class="wp-caption-text" id="caption-attachment-10864">Figure 5: Service instances pane of AWS Proton dashboard</p>
</div> 
<p>You can search and filter service instances by date created or with the associated environment. This helps to search and filter those with deployment issues, quickly identify services that are related in the same environment and identify which infrastructure components are being updated most frequently.</p> 
<div class="wp-caption alignnone" id="attachment_10865" style="width: 1626px;">
 <img alt="Screenshot of the Service instances search and filter option" class="size-full wp-image-10865" height="788" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/10/16/prot-6.jpg" width="1616" />
 <p class="wp-caption-text" id="caption-attachment-10865">Figure 6: Service instances search and filter option</p>
</div> 
<p>Once you select the <strong>service instances gear icon</strong>, a <strong>Preference</strong> dialog box opens from which you can configure the view based on your needs.</p> 
<div class="wp-caption alignnone" id="attachment_10866" style="width: 793px;">
 <img alt="Screenshot of service instance preferences" class="wp-image-10866 size-full" height="635" src="https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2022/10/16/prot-7.jpg" width="783" />
 <p class="wp-caption-text" id="caption-attachment-10866">Figure 7: Service instances preferences</p>
</div> 
<h2>Conclusion</h2> 
<p>In this post, we outlined how the AWS Proton dashboard feature provides a unified view of all the AWS Proton managed resources. The dashboard is now available in all supported regions. Your feedback is really important to us. Please reach out to us with any feedback or feature requests via our <a href="https://github.com/aws/aws-proton-public-roadmap">public roadmap</a>.</p>
