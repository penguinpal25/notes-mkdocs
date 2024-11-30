## Why GitOps?
Previously, managing IT infrastructure was a complex task. Most infrastructure configurations were manually managed, configured and distributed by system administrators(IAC). And, because different team members simultaneously collaborated on these infrastructures, this often resulted in multiple errors.

Since it allowed multiple teams to collaborate in the infrastructure configuration simultaneously, it required a review and approval process. It lacked tests to confirm changes from individual team members, and infrastructure updates were done manually. DevOps teams needed an automated, scalable way to document and provision application infrastructure; GitOps.

## What is GitOps?
In easy words, GitOps is a smart and intelligent way for Continuous Deployment of Cloudnative application. It is a way to do Kubernetes cluster management and application delivery.

It works by using Git as a single source of truth for declarative infrastructure and applications, together with tools ensuring the actual state of infrastructure and applications converges towards the desired state declared in Git. With Git at the center of your delivery pipelines, developers can make pull requests to accelerate and simplify application deployments and operations tasks to your infrastructure or container-orchestration system (e.g. Kubernetes).

The core idea of GitOps is having a Git repository that always contains declarative descriptions of the infrastructure currently desired in the production environment and an automated process to make the production environment match the described state in the repository. If you want to deploy a new application or update an existing one, you only need to update the repository — the automated process handles everything else. It’s like having cruise control for managing your applications in production.

## Three components of GitOps
GitOps is no single tool or product; Instead, it combines IAC and complete DevOps pipeline practices. To get started with GitOps, you should consider these components.

- Infrastrure as Code (IaC)
  
  IAC defines infrastructure using configuration files(code) rather than manually using a graphical user interface.
- Merge Requests (MRs)

  Merge requests (MRs) are the modification method used by GitOps for any updates to the infrastructure. Teams can collaborate on the infrastructure through reviews and comments before changes are officially approved in the main branch. A merge request acts as an audit log and helps keep track of the changes implemented by each team member. With an inbuilt roll-back feature, revert changes that are to the desired version.
- Continuous integration and continuous deployment (CI/CD)

  In a GitOps environment, infrastructure management is automated using a CI/CD(Continuous integration and Continuous deployment) pipeline. So, whenever a code is merged to the main branch, CI/CD acts as a loop. It initiates the change from the git repository to the environment. GitOps automation and continuous deployment eliminate human errors that arise when infrastructure configuration is done manually.

## How does GitOps work?
GitOps guarantees that a system’s cloud infrastructure is reconfigured and syncs with the state of a Git repository as soon as the merged request is approved. 

Consider it this way, assume you developed a central repository containing all of the configuration files (YAML files), documentation, and code required for Kubernetes. You then automated it such that other system administrators responsible for Kubernetes deployment can simply clone the repository, modify the code, and submit a merge request to the central Git repository.

![gitops1](https://github.com/vyshnavlal/notes/assets/65948438/1c11c495-4799-4cbb-986e-e24b0a5b0bd2)

The workflow of a typical GitOps environment looks like this:

- You make a pull request for a new feature to IAC, configuration, or application code on the Git repository.
- The code will get reviewed and the changes approved.
- After that, the code gets merged into the Git repository.
- Immediately CI build pipelines are established and triggered from pull requests. It runs a few checks, and if all of them pass, it creates a new image and sends it to the image container.
- The deployment Automator identifies changes to the image repository when the merge request is finished, grabs it from the registry, and updates the YAML configuration file.
- This declares the changes operational, and the modification is automatically distributed to the cluster.

## What is Declarative IaC v/s Imperative IaC
That is using script, Json, Yaml, terraform templates instead of running cli like kubectl, bash which is imperative.

## Benefits of GitOps
- **Allows team collaboration**

  Since it uses version control, respective members can simultaneously contribute to the configurations. Team members can create a merge request that suggests changes to the code. There would not be any cases of conflicting changes as every new configuration goes through the review process before it’s approved.
- **Version control**

  GitOps is a version control system; therefore, it has all of the capabilities of git, such as detailed audit records of all changes made. Details like the committer’s identity, the commit timestamp, and the commit ID are saved when a commit is made. As a result, you have total access to the system and can track all changes made to the infrastructure configuration.
- **Increases reliability**

  Manually configuring your application infrastructure can be unreliable, but with an automated system, you can achieve fewer errors and faster problem resolution.
- **Automation**

  Automating the entire deployment process saves time and increases productivity for a DevOps team. You can even increase the number of changes made to the configuration and spend more experimenting with new infrastructure configurations.

## Pull-based Deployments
![gitops2](https://github.com/vyshnavlal/notes/assets/65948438/b8508c0a-7c60-4488-b80f-27bcc634d7d9)

The Pull-based deployment strategy uses the same concepts as the push-based variant but differs in how the deployment pipeline works. Traditional CI/CD pipelines are triggered by an external event, for example when new code is pushed to an application repository. With the pull-based deployment approach, the operator is introduced. It takes over the role of the pipeline by continuously comparing the desired state in the environment repository with the actual state in the deployed infrastructure. Whenever differences are noticed, the operator updates the infrastructure to match the environment repository. Additionally the image registry can be monitored to find new versions of images to deploy.

Just like the push-based deployment, this variant updates the environment whenever the environment repository changes. However, with the operator, changes can also be noticed in the other direction. Whenever the deployed infrastructure changes in any way not described in the environment repository, these changes are reverted. This ensures that all changes are made traceable in the Git log, by making all direct changes to the cluster impossible.

![gitops3](https://github.com/vyshnavlal/notes/assets/65948438/d728f97f-8b80-4b5f-9563-0f1421ea7bf1)

## GitOps Ecosystem
![gitops4](https://github.com/vyshnavlal/notes/assets/65948438/49107a1d-78c2-4e20-bdec-d02a84bc6b28)

