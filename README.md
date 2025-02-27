---
title: GitVops
author: Thomas A. McGonagle
email: thomas.mcgonagle@suse.com
---

## GitVops
GitVops is an operational framework that takes DevOps best practices used for application development, such as:

* version control
* collaboration
* compliance
* CI/CD
* continuous monitoring

and applies them to virtualization management.

## The True Power of Kubernetes
The true power of Kubernetes extends far beyond simple container orchestration, residing in its robust API and the ability to define Custom Resource Definitions (CRDs). The API provides a declarative, programmatic interface to every aspect of the cluster, enabling automation and integration with other systems. 1  CRDs, in turn, allow users to extend Kubernetes with their own custom objects and controllers, effectively turning the platform into a programmable control plane for any application or infrastructure component.  This flexibility empowers developers and operators to model complex, domain-specific workflows such as virtual machines with SUSE Virtualization and manage them with the same consistent, declarative approach used for core Kubernetes resources, unlocking unparalleled extensibility and adaptability.

## Collaborative Automation is Key
Collaborative automation is simply teams of engineers and administrators working together, towards a common goal, utilizing automation tooling to make their jobs easier, more efficient, and stress free. A secondary DevOps practice, configuration management, is important to GitVops, and has undergone a fundamental shift over the last few years as social coding via sites like github.com have become increasingly popular. Virtualization teams are able to share their software modeled VMs with other team members, solicit feedback, find bugs, and squash those bugs all because their VMs are modeled in code.

“Agile is about embracing change, treating it as an expected thing, and even a positive, as opposed to old-school, viewing change as a problem,” says Kief Morris. “If you use automation tools but still manage your infrastructure with the Iron Age approaches to change management, you’re losing the benefit. GitVops is more reliable, particularly if you use Agile engineering practices like test driven development, continuous integration and continuous delivery.”

Learning and understanding how to manage your virtual infrastructure as a software project is incredibly valuable. Software development has reached a level of maturity allowing its techniques to be utilized in fields such as infrastructure engineering. A GitVops workflow can be managed via Agile methodologies such as Scrum and Kanban. This allows network engineers to coordinate and manage their infrastructure with agility and achieve project velocity. Social coding techniques such as peer review increase the quality of the designs and configuration. Automated tests can be run against the GitVops configurations, finding defects and alerting engineers to bugs. As GitVops software development maturity increases, there will be fewer bugs and errors, but most importantly project velocity will increase and the network and operations environment will keep pace with the velocity of the ever-increasing software projects they are meant to support.

## Benefits of GitVops
* **Repeatability** – The very act of modeling your infrastructure in code provides repeatability. Every configuration element is captured in the code and will enforce that defined configuration each and every time it is run. IaC provides confidence that the infrastructure is configured and operating in the way it is supposed to be.
* **Automation** – The very act of abstracting out infrastructures brings us the benefits of automation.
* **Agility** – Utilizing collaborative automation techniques like configuration management provide a confidence in the various versions of the code base. This allows an engineer or administrator to roll forward or backward if a problem were encountered. Logs of who did what, when, are available and can be analyzed to determine who or what caused the problem. This minimizes the average time to fix problems (MTTR) and encourages root cause analysis.
* **Scalability** – Repeatability plus automation makes scalability much easier, especially when combined with the rapid hardware provisioning that the cloud provides.
* **Reassurance** – The fact that the architecture, design, and implementation of our infrastructure is modeled in code means that we can automatically have documentation. Any programmer can look at the source code and see at a glance how the systems work. This is a welcome change from the common scenario in which only a single sysadmin or architect holds the understanding of how the system hangs together. That is risky- this person is now able to hold the organization ransom, and should they leave or become ill, the company is endangered.
* **Disaster Recovery** – In the event of a catastrophic event that wipes out the production systems, if your entire infrastructure has been broken down into modular components and described as code, recovery is as simple as provisioning new compute power, restoring from backup, and deploying the infrastructure and application code. What may have been a business ending event in the old paradigm of custom-built, partially automated infrastructure becomes a manageable few hour outage, potentially delivering competitive value over those organizations suffering from the same external influences, but without the power and flexibility brought about by infrastructure as code.

## A Call to Arms

Now is the time to get involved and learn GitVops! Feel free to star this project, and/or schedule a meeting with Thomas via his calendly - thomas.mcgonagle@suse.com

## Resources
[Original Blog Article by Brian Durden](https://ranchergovernment.com/blog/three-easy-mode-ways-of-installing-rancher-onto-harvester)

[GitVops YouTube Video Podcast](https://www.youtube.com/live/diMTPCELjJk?si=G3wsiZDTecyElw5U)
