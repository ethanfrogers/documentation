---
layout: post
title: Getting Started
order: 10
---

In this guide you will learn how to install Armory Spinnaker on AWS. You will learn how to configure Spinnaker so that you can use it to deploy your applications to any environment safely and securely.

As you go through this guide, keep in mind that there are four major milestones:

- Installing Spinnaker
- Re-deploying Spinnaker
- Deploying an Application
- Production Readiness


## Spinnaker's Workflow

Before we dive in, it is important to understand the workflow that Spinnaker prefers. This will allow us to understand what we need to install and configure and why. Lets go over it from the beginning to the end. The process starts with a code change and ends with that code running in production. The general case is:

1. A change is made to master in your code repository.
2. The build system creates an artifact.
3. The build system pushes that artifact to a central artifact repository or registry.
4. This causes a deploy pipeline to trigger within Spinnaker.
5. Spinnaker downloads the artifact from the central artifact repository or registry.
6. (Optional) Spinnaker creates an image from the artifact.
7. Spinnaker deploys the image to an environment.


For example, suppose you are using GitHub, Jenkins, Artifactory and AWS. Then a deployment workflow might look like:

1. A change is made to master in your code repository.
2. Jenkins creates a deb package from the code in the GitHub repo.
3. Jenkins archives the deb package and uploads it to Artifactory.
4. Spinnaker triggers a pipeline based on the Jenkins job completing.
5. Spinnaker downloads the artifact from Artifactory.
6. Spinnaker creates and AMI with the artifact and any of its dependencies.
7. Spinnaker deploys the AMI to an environment.



## Milestones

The following steps are broken up into milestones. However, you should follow them serially.


### Installing Armory Spinnaker
[**Install Armory Spinnaker**]({% link _install_guide/install.md %})  
The first goal is to get Spinnaker installed and running to the point where we can connect to its UI.

On install, Armory Spinnaker is configured to be in HA mode. 

#### Tips during configuration:
- Scale down Spinnaker to be just 1 instance.  
Click on `nonpolling` server group and go to `Server Group Actions` > `Resize`
![gif](https://cl.ly/003B1D0b0P0C/Screen%20Recording%202017-09-05%20at%2006.06%20PM.gif)


- Change the Auto Scaling Group Healthcheck from `ELB` to `EC2`. This will allow you to restart Armory Spinnaker without the ASG terminating the instance.  
Click on the remaining ASG and go to `Advance Settings` > `Edit Advance Settings`
![gif](https://cl.ly/2A1A1V3t3d2R/Screen%20Recording%202017-09-05%20at%2006.12%20PM.gif)


Start by SSH'ing to the single instance of Armory Spinnaker. Throughout this guide you will be making changes to the `*-local.yml` files in `/opt/spinnaker/config/`. In order for the changes to take affect you will need to restart Armory Spinnaker. To restart run the following:
```
$ sudo service armory-spinnaker stop && sleep 15 && sudo service armory-spinnaker start  # restart spinnaker
```

It will take a moment for the services to come back online. You can check its status by running:
```
$ watch curl http://localhost:5000/healthcheck
```
Then `ctrl+c` to exit watch once it is healthy.



### Re-Deploying Spinnaker

Next we will configure Spinnaker so that Spinnaker can re-deploy Spinnaker. This way we can make configuration changes and provide Spinnaker with an upgrade path.

We will do the following:

- [Tag AWS Subnets]({% link _install_guide/subnets.md %})
- [Store Spinnaker's configuration files in a repo]({% link _install_guide/config_repo.md %})
- [Configure Spinnaker to trigger from Jenkins]({% link _install_guide/jenkins.md %})
- [Create a pipeline for Spinnaker to deploy Spinnaker]({% link _install_guide/spinnaker-deploy-spinnaker.md %})



### Deploying An Application

After we have Spinnaker in a re-deployable state we can get started on deploying an application.

We will do the following:

- [Add additional cloud provider accounts (and possibly subnets)]({% link _install_guide/adding_accounts.md %})
- [Understanding Packer Scripts]({% link _install_guide/packer.md %})
- [Create a pipeline to deploy the application]({% link _install_guide/application_pipeline.md %})


### Production Ready

Before opening Spinnaker up for general availability within your organization, there are several concerns to address.

We will do the following:

- [Add a DNS name and SSL]({% link _install_guide/dns_and_ssl.md %})
- [Setup Authentication]({% link _install_guide/auth.md %})
- [Setup Authorization]({% link _install_guide/authz.md %})
- [Configure centralized logging]({% link _install_guide/logging.md %})
- [Pre-create common Packer templates]({% link _admin_guides/rosco.md %})

Finally we can undo some shortcuts when we were configuring Spinnaker. Just undo everything in the [Tips during configurations Guide](#tips-during-configuration)


### Next Steps

Optionally, you may want to:

- [Configure Notifications]({% link _admin_guides/notifications.md %})
- [Setup Audit Logging]({% link _install_guide/logging.md %})

Once you have Spinnaker production ready, it is time to onboard users. You may want to:

- Make Packer scripts to help migrate from Chef, Puppet or Salt.
- Share the [User Guides]({% link _user_guides/application-screen.md %})
- Provide example pipelines that are specific to your organization