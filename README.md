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

## Agile Software Methodologies
“Agile is about embracing change, treating it as an expected thing, and even a positive, as opposed to old-school, viewing change as a problem,” says Kief Morris. “If you use automation tools but still manage your infrastructure with the Iron Age approaches to change management, you’re losing the benefit. GitVops is more reliable, particularly if you use Agile engineering practices like test driven development, continuous integration and continuous delivery.”

## Everything Becomes a Software Project
Learning and understanding how to manage your virtual infrastructure as a software project is incredibly valuable. Software development has reached a level of maturity allowing its techniques to be utilized in fields such as infrastructure engineering. A GitVops workflow can be managed via Agile methodologies such as Scrum and Kanban. This allows network engineers to coordinate and manage their infrastructure with agility and achieve project velocity. Social coding techniques such as peer review increase the quality of the designs and configuration. Automated tests can be run against the GitVops configurations, finding defects and alerting engineers to bugs. As GitVops software development maturity increases, there will be fewer bugs and errors, but most importantly project velocity will increase and the network and operations environment will keep pace with the velocity of the ever-increasing software projects they are meant to support.

## Benefits of GitVops
* **Repeatability** – The very act of modeling your infrastructure in code provides repeatability. Every configuration element is captured in the code and will enforce that defined configuration each and every time it is run. IaC provides confidence that the infrastructure is configured and operating in the way it is supposed to be.
* **Automation** – The very act of abstracting out infrastructures brings us the benefits of automation.
* **Agility** – Utilizing collaborative automation techniques like configuration management provide a confidence in the various versions of the code base. This allows an engineer or administrator to roll forward or backward if a problem were encountered. Logs of who did what, when, are available and can be analyzed to determine who or what caused the problem. This minimizes the average time to fix problems (MTTR) and encourages root cause analysis.
* **Scalability** – Repeatability plus automation makes scalability much easier, especially when combined with the rapid hardware provisioning that the cloud provides.
* **Reassurance** – The fact that the architecture, design, and implementation of our infrastructure is modeled in code means that we can automatically have documentation. Any programmer can look at the source code and see at a glance how the systems work. This is a welcome change from the common scenario in which only a single sysadmin or architect holds the understanding of how the system hangs together. That is risky- this person is now able to hold the organization ransom, and should they leave or become ill, the company is endangered.
* **Disaster Recovery** – In the event of a catastrophic event that wipes out the production systems, if your entire infrastructure has been broken down into modular components and described as code, recovery is as simple as provisioning new compute power, restoring from backup, and deploying the infrastructure and application code. What may have been a business ending event in the old paradigm of custom-built, partially automated infrastructure becomes a manageable few hour outage, potentially delivering competitive value over those organizations suffering from the same external influences, but without the power and flexibility brought about by infrastructure as code.

## A Call to Arms

Now is the time to get involved and learn GitVops! Feel free to star this project, and/or schedule a meeting with Thomas via his work email - thomas.mcgonagle@suse.com

## Resources
[Original Blog Article by Brian Durden](https://ranchergovernment.com/blog/three-easy-mode-ways-of-installing-rancher-onto-harvester)

[GitVops YouTube Video Podcast](https://www.youtube.com/live/diMTPCELjJk?si=G3wsiZDTecyElw5U)

### From Brian Durden's [Original Blog on GitVops](https://ranchergovernment.com/blog/three-easy-mode-ways-of-installing-rancher-onto-harvester)
## A simple helmchart for Harvester

When we create a VM inside of Harvester's UI, we can see the resulting yaml that is generated and use that as a starting point. Here I've created a simple VM in my Lab:
![vm-instance](images/vm_instance_ui.png)

I can select `Edit YAML` and then see the innards of this VM definition. I'm going to trim some things out so it isn't so large:

```console
❯ kubectl get vm demo -o yaml
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  annotations:
    harvesterhci.io/vmRunStrategy: RerunOnFailure
    harvesterhci.io/volumeClaimTemplates: '[{"metadata":{"name":"demo-disk-0-gq2mu","annotations":{"harvesterhci.io/imageId":"default/ubuntu"}},"spec":{"accessModes":["ReadWriteMany"],"resources":{"requests":{"storage":"40Gi"}},"volumeMode":"Block","storageClassName":"longhorn-ubuntu"}}]'
<snip>
  name: demo
  namespace: default
  resourceVersion: "129514482"
  uid: afce69ef-4b06-432e-9177-dab23b7caec7
spec:
  runStrategy: RerunOnFailure
  template:
<snip>
    spec:
<snip>
      architecture: amd64
      domain:
        cpu:
          cores: 4
          sockets: 1
          threads: 1
        devices:
          disks:
          - bootOrder: 1
            disk:
              bus: virtio
            name: disk-0
          - disk:
              bus: virtio
            name: cloudinitdisk
<snip>
```

Two things to note here:
* Some of the components here like cloud-init are linked to separate resources. The CRD for the `VirtualMachine` type does allow us to define cloud-init inline, but there is a size limit. So typically, we'll dump that into a `Secret` instead. Having it in a `Secret` also allows us to hide away any actual secret data like passwords or keys that we don't want an Auditor/ViewOnly role to be able to see.
* In Harvester, an annotation is used for defining the Volume that needs to be created when this VM instance is first created. While this works well from the UI perspective, if we try to clean up this VM from the CLI, it will not delete the volume mentioned in the annotation. So what I typically do is create the `PVC` seperately

### So what can we do here?

Based upon the above, we can begin to template out not only the VM instance, but also the aforementioned `Secret` to contain the cloud-init data as well as the `PVC` that will define the volume. 

If I want this VM to become an RKE2 node, I need to install RKE2 onto it. Which means those commands need to be defined inside the cloud-init secret so that the VM will install RKE2 automatically when it first starts. To do that, my cloud-init should look something like this:
```yaml
#cloud-config
write_files:
- path: /etc/rancher/rke2/config.yaml
owner: root
content: |
    token: mysharedtoken
    tls-san:
    - 86.75.30.9 # load balancer IP
runcmd:
- - systemctl
  - enable
  - --now
  - qemu-guest-agent.service
-  mkdir -p /var/lib/rancher/rke2-artifacts && wget https://get.rke2.io -O /var/lib/rancher/install.sh && chmod +x /var/lib/rancher/install.sh
- INSTALL_RKE2_VERSION=v1.29.6+rke2r1 /var/lib/rancher/install.sh
- systemctl enable rke2-server.service
- systemctl start rke2-server.service
ssh_authorized_keys: 
- my-ssh-key
```

I need to wrap that into a `Secret`! So I do like so (and I ensure the namespace matches the VM I intend to create)
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: myvm-cloudinit
  namespace: default
stringData:
  userdata: |
    <cloud-init-here>
```

If we take this and template it out using Helm and tweak it for a control-plane config, it will look like [this file](helm/rke2/templates/rke2_cp_secret.yaml)

This same idea is extended to how we create the `PVC` and the `VM` using [this file](helm/rke2/templates/rke2_cp_vm.yaml). 

Note that these helm examples are assuming a replica count and looping on that to create multiple instances of each. See the [example values file](helm/rke2/values.yaml) for most options.
