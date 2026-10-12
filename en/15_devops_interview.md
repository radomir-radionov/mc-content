# DevOps — Interview

Answers cover every question in this set.

[← Back to hub](./15_devops.md)

### Related docs

- [DevOps](./15_devops.md) · [DevOps — Interview](./15_devops_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/).

### DevOps Fundamentals

<details>
<summary>What is DevOps and how does it enhance software delivery?</summary>

### What is DevOps and how does it enhance software delivery?

**Definition:**

DevOps is a collaborative and iterative approach that combines software development and IT operations. This synergistic model revolutionized software delivery by introducing Continuous Integration/Continuous Deployment (CI/CD) pipelines.

**How it works:**

Key Tenets of DevOps

- Automation: Streamlines repetitive tasks.
- Cross-Functional Collaboration: Fosters teamwork across traditionally siloed roles.
- Continuous Monitoring and Feedback: Ensures real-time insights, enhancing product reliability and performance.

**Pluses and minuses:**

Benefits of Using DevOps

- Accelerated Software Development: Reduced manual intervention and quick feedback cycles speed up release schedules.
- Enhanced Product Quality: The emphasis on comprehensive testing, alongside automation, minimizes human error.
- Improved Security: Continuous feedback and regular threat assessments aid in identifying and resolving security vulnerabilities early in the development process.
- Enhanced Competitiveness: Companies using DevOps can react swiftly to market changes, staying ahead of the competition.
- Cost Efficiency: Automation and streamlined processes reduce overheads, leading to cost savings.
- Resilience: The automation and monitoring components of DevOps contribute to system stability and quick recovery from faults, ensuring high availability.
- Customer Satisfaction: Rapid bug fixes and new features translate to a more satisfying user experience.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>Can you describe the key principles of DevOps?</summary>

### Can you describe the key principles of DevOps?

**Definition:**

DevOps is a set of practices designed to streamline and integrate software development (Dev) and IT operations (Ops) for faster and more reliable software delivery.

**How it works:**

Core Principles

1. Customer-Centricity: Continuous feedback and short development cycles enable rapid, customer-driven updates.
2. End-to-End Responsibility: The DevOps team is accountable for the entire software delivery process, from development to ongoing monitoring and maintenance.
3. Agility: Teams are adaptable and can readily respond to changes in requirements and the business environment.
4. Continuous Improvement: DevOps fosters a culture of ongoing learning and enhancement, seeking ongoing optimization across teams and processes.
5. Self-Service Capabilities: The team invests in enabling its members to be self-sufficient, with ready access to the tools and platforms they require.
6. Automated Quality Assurance: Automated testing ensures rapid feedback on software quality throughout the development and delivery process.
7. Security as Code: Security is integrated into all aspects of software development and delivery, using policies and code to automate security protocols.
8. Collaboration and Communication: Regular communication and active collaboration across teams, departments, and functions are key to success.

DevOps Golden Rule

The core tenet of DevOps is to break down silos present in traditional development environments. By fostering collaboration across roles, teams, and functions, every member becomes a stakeholder in the production process. This collective ownership leads to better decision-making and ultimately, higher-quality software releases.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>How do continuous integration and continuous deployment (CI/CD) relate to DevOps?</summary>

### How do continuous integration and continuous deployment (CI/CD) relate to DevOps?

**Definition:**

Continuous Integration (CI) and Continuous Deployment (CD) are foundational practices of DevOps. Their seamless orchestration ensures that development, testing, and deployment cycles are tightly integrated, rapid, and reliable.

The CI/CD pipeline employs automation tools, version control, and code repositories to build, test, and deploy software in an agile and efficient manner.

**How it works:**

Key Benefits

- Collaborative Workflow: Collaborators make small and frequent code contributions.
- Rapid Feedback: Automated testing and code quality checks provide instant feedback.
- Streamlined Deployment: Automation reduces the potential for human errors.
- Audit Trail: Version control and logging ensure traceability.
- Stakeholder Involvement: Business teams, testers, and developers have visibility and control.
- Adaptive Management: Managers can adjust priorities and requirements more flexibly.

Text Book Algorithms

- The Build Trigger: This pipeline trigger initiates a new build on code commit.
- The Unit Test Stage: This stage ensures individual components function as expected.
- The Integration Test Stage: It validates the interaction and compatibility of components.
- The Security Scanning Stage: This stage uses static analysis tools to identify vulnerabilities.
- The Staging Deployment Stage: Confirmed code changes are deployed to a staging environment.
- The Acceptance Test Stage: These tests are executed in a staging environment by the QA team to ensure the product behaves as expected.
- The UI Testing Stage: This stage validates the software user interface.
- The Release Stage: Approved code changes are deployed to the production environment.
- The Rollback Mechanism: An automated or semi-automated system is in place to revert the production environment to a prior state in case of issues after deployment.

**Examples:**

Textbook Code Example: Node.js & Jenkinsfile

Here is the Node.js code:

```javascript
// server.js
const http = require('http');
const port = 3000;
const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello, World!\n');
});
server.listen(port, () => {
    console.log(`Server running at http://localhost:${port}/`);
});
```

Below is the Jenkinsfile:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy to Staging') {
            when {
                branch 'dev'
            }
            steps {
                sh 'docker build -t myapp .'
                // The code below assumes the staging environment is already set up.
                sh 'docker run -d --name myapp-staging -p 3000:3000 myapp'
            }
        }
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                sh 'docker build -t myapp .'
                // The code below assumes the production environment is already set up.
                sh 'docker run -d --name myapp-production -p 8080:8080 myapp'
            }
        }
    }
}
```

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>What are the benefits of DevOps in software development and operations?</summary>

### What are the benefits of DevOps in software development and operations?

**Definition:**

DevOps (Development + Operations) is a collaborative approach that intertwines software development with IT operations. It handles the entire software development cycle, from code development to production deployment in a seamless, automated, and efficient manner.

**Pluses and minuses:**

Key Benefits of DevOps

- Agility: DevOps fosters rapid and iterative software development, ensuring quick adaptability to market needs and minimizing time-to-market.
- Reliability and Stability: Through practices like automated testing and consistent deployment, DevOps reduces failure rates and ensures stable, predictable releases.
- Continuous Delivery: DevOps facilitates effortless and uninterrupted software releases, enabling teams to swiftly respond to customer feedback or market changes.
- Mean Time To Recover (MTTR): By employing coding best practices and automated recovery mechanisms, DevOps significantly reduces the time taken to fix potential issues.
- Collaboration and Communication: DevOps breaks down silos, ensuring harmonious interaction and knowledge-sharing across departments.
- Security Integration: The methodology integrates security checks across the development process, identifying issues early and minimizing security risks.
- Flexibility: DevOps is ecosystem-agnostic, meaning it supports and is adaptable to various tools and technologies.
- Efficiency: Automation streamlines numerous manual tasks, saving time and resources.
- Quality Assurance: Through automated testing and continuous monitoring, DevOps maintains a high software quality standard.
- Life Cycle Management: DevOps oversees the full software life cycle, ensuring end-to-end best practices and efficiency.
- Resource Optimization: By emphasizing smaller, more frequent deployments, DevOps leads to better resource management.
- Innovation and Feedback: Real-time user feedback is efficiently incorporated, promoting constant improvement and innovation.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>Can you explain the concept of “Infrastructure as Code” (IaC)?</summary>

### Can you explain the concept of “Infrastructure as Code” (IaC)?

**Definition:**

Infrastructure as Code (IaC) is a DevOps practice that involves managing and provisioning computing infrastructure in an automated, efficient, and consistent way, using version-controlled templates.

IaC tools, such as Terraform and Ansible, define infrastructure elements, like servers and networks, in declarative or imperative styles for efficient management.

It brings several benefits:

- Risk Reduction: Automated processes diminish the potential for human error.
- Environment Consistency: Ensures that every environment, from development to production, is set up reliably and consistently.
- Agility: Rapid deployment, scaling, and changes reduce time-to-market.
- Cost Efficiencies: Optimizes resource allocation to save time and money.

IaC not only encompasses provisioning infrastructure but also includes:

- Audit trails: Tracks configuration changes.
- Compliance: Validates infrastructure against predefined security and compliance policies.
- Continuous Integration/Continuous Deployment (CI/CD): Integrates with CI/CD to automate the software release process.

**How it works:**

IaC Tools

Several tools enable IaC, each with its unique features and syntax:

- Terraform: Declares infrastructure in predefined modules and abstracts resources using providers.
- Ansible: Utilizes tasks to execute on remote hosts and is agentless, using SSH for communication.
- AWS CloudFormation: AWS-specific solution using JSON or YAML to define resources.

IaC Best Practices

- Versioning: Store code in a version control system for traceability.
- Modularity: Break the infrastructure code into manageable and reusable modules.
- Documentation: Maintain clear and up-to-date documentation.
- Testing: Implement automated testing to validate infrastructure changes.
- Change Control: Follow formal change management processes for infrastructure alterations.
- Collaboration: Implement security and access controls.

IaC Paradigms

- Imperative: Directly specifies the actions needed to achieve a particular state. Useful for quick, one-off tasks or troubleshooting.
- Declarative: Describes the desired state, allowing the IaC tool to determine the actions required to reconcile the current state with the desired state. This is the primary approach for robust, maintainable, and consistent infrastructure management.

Common IaC Tasks

Infrastructure Provisioning

- Task: Swiftly set up new infrastructure.
- Example: Deploy servers in the cloud.

Configuration Management

- Task: Efficiently configure existing systems.
- Example: Install packages and set up web servers.

Orchestration

- Task: Coordinate multiple systems.
- Example: Deploy a multi-tier application with a load balancer, web servers, and a database.

Lifecycle Management

- Task: Manage the lifecycle of infrastructure components, from creation to retirement.
- Example: Terminate obsolete resources.

IaC and Cloud Computing

- On-Premise: For traditional physical servers and datacenters, IaC tools manage these resources similarly to cloud environments.
- Public Cloud: Like AWS, Azure, or GCP, these platforms inherently support IaC. The tools provided by these platforms, like AWS CloudFormation or Azure Resource Manager, offer cloud-specific advantages.

IaC and Kubernetes

- Resource Management: Tools like kubectl and Helm enable IaC for Kubernetes by managing resources like pods, services, and deployments.
- Configuration: Kubernetes objects, defined in YAML files, help maintain configurations.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>What is meant by “Shift Left” in DevOps?</summary>

### What is meant by “Shift Left” in DevOps?

**Definition:**

Shift Left in DevOps embodies the practice of conducting various development processes and testing early in the software lifecycle. This ensures prompt issue identification and lowers remediation costs.

**How it works:**

Key Benefits

- Early Identification of Issues: Pinpointing defects at an initial stage enables swift solutions.
- Cost and Time Efficiency: Rectifying flaws early poses minimal financial and temporal burdens.
- Improved Dev-QA Synchronization: Minimizes discrepancies between development and quality assurance functions.

Techniques Aligned with Shift Left Methodology

- Test-Driven Development (TDD): Pioneered by Extreme Programming (XP), TDD dictates that a test be written prior to the code it's meant to validate.
- Continuous Integration (CI): Intermittent code integrations are executed, often multiple times each day, reducing the risk of complications during full-scale deployments.
- Automated Deployment & Testing: Key to Continuous Deployment (CD) as well, automatic processes speed up release cycles and flag any irregularities early on.

Shift Right as a Complement

Shift Right, while different in essence from its Left counterpart, doesn't necessarily contradict it. Instead, it complements it: after an application or a feature is operational, data is amassed regarding the same. This information then guides future iterations or decisions.

**Examples:**

Code Example: Shift Left with Test-Driven Development

Here is the Java code:

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println(calc.add(5, 10));  // Output: 15
    }
}
```

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>How does version control support DevOps practices?</summary>

### How does version control support DevOps practices?

**Definition:**

Version Control tools, such as Git, are pivotal in DevOps, as they enable seamless collaboration among developers, testers, and operations teams. Here is how version control underpins DevOps.

**How it works:**

Key Benefits

Version Control empowers teams with several benefits:

- Collaboration: Teams work in sync on a central code repository, avoiding duplication and out-of-sync files.
- Change Tracking: Each modification is documented, enabling thorough auditing and rollbacks to restore a stable state.
- Code Quality: Incorporation of code review workflows ensures that only approved changes are merged, improving the overall codebase.
- Task Coordination: Teams can link code changes to specific tasks or issues, providing context, and improving transparency.
- Release Management: Integration tools can align changes between different teams, automating various release tasks.

DevOps Workflows

- Continuous Integration (CI): Version control platforms trigger automated testing and build processes upon code changes, ensuring early issue identification.
- Continuous Deployment (CD): Approved changes are automatically deployed to production or staging environments in a controlled manner, reducing human error.
- Branching Strategies: Tailored strategies, such as Git-Flow or Trunk-based Development, streamline collaborative workflows.
- Automated Code Reviews: Tools like linters and static code analyzers enhance code quality, ensuring best practices adherance.
- Environment Synchronization: Teams can use infrastructure as code (IaC) to keep development and production environments in lockstep with code changes.

Common Misconceptions and Best Practices

- Deeper Version Control: Maintain track of all artifacts, scripts, and configuration files through version control, offering a more comprehensive historical reference.
- Beyond Code: Version control can also monitor changes in database schemas and other non-code artifacts, ensuring consistency across environments.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>What role does automation play in DevOps?</summary>

### What role does automation play in DevOps?

**Definition:**

Automation is the heart of DevOps, streamlining software development, testing, and deployment workflows. It ensures consistency, efficiency, and rapid iteration. Common automation tools include Jenkins, Puppet, and Ansible.

**How it works:**

Key Areas of Automation in DevOps

1. Code Building & Compilation

- Automation Goal: Quickly convert source code into deployable artifacts.
- Tools & Actions:
  - Version Control Hooks: On push, initiate build pipeline.
  - Build Tools: Maven, Ant, npm, or GNU Make.

2. Code Quality & Testing

- Automation Goal: Rapid and consistent execution of tests.
- Tools & Actions:
  - Automated Tests: Unit tests, integration tests, and regression tests.
  - Code Linters and Style Checkers: E.g., ESLint for JavaScript.
  - Test Coverage Analysis: Tools like Jacoco for Java.

3. Artifact Management

- Automation Goal: Secure, versioned, and organized storage of artifacts.
- Tools & Actions:
  - Artifact Repositories: Hosts binary artifacts. Examples are JFrog Artifactory and Sonatype Nexus.
  - Continuous Integration/Continuous Deployment (CI/CD) Pipeline: Automate the flow from build to artifact deployment.

4. Provisioning & Configuration Management

- Automation Goal: Ensure consistency in deployment environments.
- Tools & Actions:
  - Configuration Templates: Tools like Terraform and CloudFormation maintain infrastructure as code.
  - Configuration Management: Tools like Puppet and Chef ensure server configurations are consistent.

5. Container Orchestration

- Automation Goal: Simplify management of containerized applications.
- Tools & Actions:
  - Container Orchestration Tools: Kubernetes and Docker Swarm automate deployment, scaling, and management of containers.
  - Toolsets like Helm aid in packaging and deploying Kubernetes resources.

6. Continuous Integration/Continuous Deployment (CI/CD) Pipeline

- Automation Goal: Automate the entire code-to-deploy process.
- Tools & Actions:
  - CI/CD Tools: Jenkins, GitLab CI/CD, and Travis CI.
  - Deployment Strategies: Tools like Spinnaker implement automated canary releases and blue-green deployments.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>What are the common metrics used to measure DevOps success?</summary>

### What are the common metrics used to measure DevOps success?

**Definition:**

DevOps success can be measured through various quantifiable and qualitative indicators that highlight streamlined collaboration, agile development, and robust delivery pipelines.

**How it works:**

Key Metrics

- Deployment Frequency: Focus on increased frequency for quick feedback cycles and agility.
- Lead Time for Changes: Less lead time indicates faster code-to-production cycles.
- Change Failure Rate: Monitor failed changes post-deployment to gauge stability and reliability.
- Mean Time to Recovery (MTTR): A lower MTTR implies quicker incident resolution.
- Time to Restore Service (TTRS): Time taken to restore post-incident. It's especially significant for Service Level Objectives (SLOs)/SLOs.
- Rigorous Automation: Track the percentage of automation in testing, continuous integration (CI), and infrastructure as code (IaC) to optimize efficiency.
- Defect Escape Rate: This measures the effectiveness of your testing and quality processes.
- Infrastructure Scalability: As your business grows, make sure your infrastructure can keep up.
- Continuous Monitoring (or Maintenance): Regularly look at your software to make sure everything is running as it should.
- Dynamic Scaling: Like infrastructure scalability, ensure your software grows as your customer base does.
- Customer Success Metrics: Look at things like app crashes, customer inquiries, and more to see how well your application is serving your customers after deployment.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>How does DevOps differ from the Agile methodology?</summary>

### How does DevOps differ from the Agile methodology?

**Definition:**

DevOps and Agile methodologies are both designed to improve software development. However, while Agile primarily focuses on the iterative nature of software development, DevOps goes beyond the development process to ensure efficient code building, testing, and deployment.

**How it works:**

Key Areas of Focus

- Agile emphasizes consistent releases with incremental improvements and involves close-knit teams, client collaboration, and adaptability to change.
- DevOps concentrates on the automation of the build, test, and deploy processes to achieve frequent, reliable, and often automated releases. It streamlines collaboration among development, operations, and other IT teams.

Interdepartmental Collaboration

- Agile: Primarily focuses on intra-team coordination, especially between the development and product management teams.
- DevOps: Takes intra-team coordination a step further, amplifying the focus with an emphasis on the collaboration between diverse internal teams, such as development, security, and IT operations.

Automation

- Agile: While automation is beneficial, it's not a core requirement. The primary goal is to ensure short, iterative development cycles that may not necessitate manual intervention.
- DevOps: Automation is at the heart of the methodology. It's essential for continuous integration, delivery, and deployment (CI/CD) pipelines, ensuring that individual code changes are thoroughly tested and seamlessly deployed into production.

Continuity of Deployment

- Agile: Regular, small feature releases are encouraged, but the decision to deploy rests with the product owner.
- DevOps: The development and operations teams work in close tandem, facilitating multiple, automated deployments each day.

Code Quality

- Agile: Encourages clean code, but it isn't the primary driver.
- DevOps: Prioritizes robust code quality, enforcing automated standards through tools in all phases of the pipeline.

Role Integration

- Agile: Promotes the role-based segregation, such as the product owner, Scrum master, and development team.
- DevOps: Unlike Agile, which defines specific roles, DevOps encourages a more integrated approach with cross-functional teams where each member takes shared responsibility for the end-product.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

### Tools and Technologies

<details>
<summary>What are some popular tools used in the DevOps ecosystem for version control?</summary>

### What are some popular tools used in the DevOps ecosystem for version control?

**Definition:**

Git has played a pivotal role in revolutionizing version control management and is a leading choice among development teams. The real-time collaborative environment it creates and its robust history tracking are just a couple of reasons it has become the industry standard.

**How it works:**

Top Git Tools

1. GitHub: A web-based Git repository hosting service offering integrated bug tracking and wiki space. It's a prominent platform for open-source projects.
2. GitLab: Provides a variety of DevOps lifecycle tools in addition to Git repository management.
3. Bitbucket: Developed by Atlassian, it supports Mercurial and Git repositories.

Version Control Strategies

- Centralized Version Control: Employs a single repository, favorably for controlled collaboration.
- Distributed Version Control: Every user has a dedicated repository. Merging and syncing are consistent and require additional attention.

Integrated Development Environments (IDEs)

Modern IDEs often include Git integration, streamlining version control within the development workflow. Tools in this category include:

- Visual Studio: Microsoft's flagship IDE excellently couples with Git.
- IntelliJ IDEA: From JetBrains, it supports various version controls, with special plug-ins for Git.

Build and Release Management Tools

Continuous Integration/Continuous Development (CI/CD)

- Jenkins: A powerful, extensible CI server, often used with large-scale Git repositories.
- Travis CI: Popular in open-source projects, it's deeply linked with GitHub, allowing automatic builds on pushes.

Containers and Deployment

- Docker: Technically a containerization solution, but its tight integration with Git makes it an eminent choice for many deployment pipelines.
- Kubernetes: Often used in conjunction with Git for streamlined deployment or rollback of applications.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>Can you list some CI/CD tools commonly used in DevOps?</summary>

### Can you list some CI/CD tools commonly used in DevOps?

**Definition:**

Certainly! Here are the commonly used CI/CD tools classified under several categories for a logical understanding.

**How it works:**

Continuous Integration (CI) Tools

1. Visual Studio Team Services (VSTS) / Azure DevOps (ADO)
2. Jenkins
3. Bamboo
4. TeamCity
5. GitLab
6. Travis CI
7. Circle CI
8. Codeship
9. Buddy

Version Control

Several Version Control systems offer built-in or add-on CI/CD features.

- Centralized:
  - SVN: Often used with Jenkins
- Distributed:
  - Git: Provides seamless integration with CI/CD platforms.

Package Management

Many CI/CD tools integrate with package managers for ensuring that all dependencies are appropriately managed.

- Generic (JFrog Artifactory): It supports various package types like Maven, npm, NuGet, and Docker.
- Programming-Language Specific:
  - Maven: For Java projects
  - NuGet: For .NET projects
  - npm: For Node.js projects
  - PyPI: For Python projects

Static Code Analysis

These tools help to evaluate the quality of code throughout the development pipeline.

Linters

- Flake8 (for Python)
- ESLint (for JavaScript/TypeScript)

Security Scanning

- SonarQube: For comprehensive code quality checks
- Black Duck Hub: It can detect potential security issues in open-source components.

Continuous Delivery / Deployment (CD) Tools

Application Development

Docker

- Docker Hub: A Docker registry service for sharing container images.
- Docker Trusted Registry (DTR): Provides secure management of Docker images.

Kubernetes (K8S)

- Helm: A Kubernetes package manager optimizing configuration and deployment of applications running on K8S.
- Kubernetes Dashboard: Offers a web-based UI for managing resources in a K8S cluster.

Serverless

- AWS Lambda: Part of the Amazon Web Services suite for serverless computing.
- Azure Functions: Microsoft's serverless computing service.

Cloud and Infrastructure as Code (IaC)

Infrastructure as Code (IaC), alongside CI/CD, has dramatically streamlined configuration management and environment provisioning.

Amazon Web Services (AWS)

- AWS CodePipeline: Delivers changes via multiple stages.
- AWS CodeBuild: A build service, running unit tests and producing artifacts.
- AWS CodeDeploy: Automates code deployments to numerous AWS compute services.

Google Cloud Platform (GCP)

- Cloud Build: GCP's continuous integration and delivery platform.
- Deployment Manager: For the management of cloud resources via templates.

Microsoft Azure

- Azure DevOps: Formerly known as Visual Studio Team Services, it comprises numerous CI/CD tools.
- Azure Resource Manager (ARM): Manages resources within an Azure subscription.

Monitoring and Logging

- Prometheus: Popular for metrics collection.
- Grafana: Works well with Prometheus and other data sources, providing data visualization.
- ELK Stack: Stands for Elasticsearch, Logstash, and Kibana, it's used for centralized logging.
- Sentry: For real-time error tracking.

Test Automation and Orchestration

- JUnit: For unit testing in Java.
- NUnit and xUnit: For .NET, NUnit is a popular choice while xUnit is the testing framework in .NET Core.
- Robot Framework: A generic test automation framework.
- Selenium: Used for automated browser testing.
- Postman: Often used for testing APIs.

Governance, Compliance, and Collaboration

- Jira, Trello, Asana: All options for project management.
- Confluence, Microsoft Teams, Slack: Collaboration tools to aid communication among the team.
- Bitbucket, GitHub, GitLab: Not just for version control, but also for build and deployment capabilities (through features like GitHub Actions).
- AWS Config: For ensuring AWS resources are compliant with your organization's guidelines.
- Terraform Enterprise: Provides collaboration features and facilitates versioning of infrastructure code in a team setting.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>What containerization technologies are commonly used in DevOps?</summary>

### What containerization technologies are commonly used in DevOps?

**Definition:**

Containerization technologies optimize software development and deployment by packaging applications and their dependencies into self-contained units, known as containers.

Containers offer both consistency and lightweight resource isolation. Some of the most popular containerization platforms are Docker, Kubernetes, and Amazon ECS.

**How it works:**

Common Containerization Technologies

Docker

Docker facilitates the creation, distribution, and execution of containerized applications. It harnesses `Docker Engine` to manage containers, making them portable across environments.

Kubernetes

Kubernetes is a robust orchestration tool, perfect for managing clusters at scale. It streamlines container deployment, scaling, and management across diverse environments.

Amazon ECS

Amazon Elastic Container Service (ECS) is a cloud-based container orchestration service engineered to run Docker containers on AWS. It simplifies deploying, managing, and scaling containerized applications.

Rancher

Rancher offers a feature-rich management platform for Kubernetes, providing user-friendly tools for cluster setup, container orchestration, monitoring, and more.

OpenShift

OpenShift by Red Hat supplies enterprise-grade Kubernetes with additional tools for developers, such as source-to-image builds and more secure images.

Fargate

Amazon Elastic Kubernetes Service (EKS) and Amazon ECS enable Fargate, a serverless compute engine for containers. Fargate manages infrastructure, enabling you to focus solely on application design.

Nomad

Designed by HashiCorp, Nomad is a flexible orchestration platform handling container, virtual machine, and standalone application deployments.

Google Kubernetes Engine

GKE, Google's managed Kubernetes service, eliminates the burden of managing Kubernetes clusters while offering powerful Data Loss Prevention (DLP) and security features.

Azure Kubernetes Service

AKS is Microsoft's simplified Kubernetes service, integrating with Azure Active Directory for enterprise-grade authentication and Microsoft Operations Management Suite (OMS) for uniform monitoring and logging.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>Name some configuration management tools used in DevOps.</summary>

### Name some configuration management tools used in DevOps.

**Definition:**

Classic Configuration Management (CM) tools like Puppet, Chef, Ansible, SaltStack, and CFEngine are popular for managing infrastructure.

They streamline server configurations by:

- Defining configurations in human-readable code (Infrastructure as Code, IaC)
- Ensuring conformance via continuous monitoring

**How it works:**

Modern Alternatives

Tools like Terraform and AWS CloudFormation focus on orchestrating cloud resources, providing a more holistic approach to infrastructure and application management.

Hybrid Solutions

Platforms like Azure Resource Manager (ARM) and Google Cloud Deployment Manager also offer robust CM capabilities, making them relevant choices for organizations leveraging hybrid or multi-cloud setups.

Infrastructure Provisioning

Vagrant is well-suited for setting up development environments, as it leverages virtualization technologies to ensure consistent setups across diverse developer machines.

**Examples:**

Code Example: Terraform for Cloud Infrastructure

Here is the Terraform code:

```hcl
# Define variables
variable "aws_region" {
  default = "us-west-2"
}
variable "instance_type" {
  default = "t2.micro"
}

# Set up AWS provider
provider "aws" {
  region = var.aws_region
}

# Define resources
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type
}
```

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>What monitoring tools are popular in DevOps?</summary>

### What monitoring tools are popular in DevOps?

**Definition:**

Keeping in view the requirements and the limited content available, here is the shortlist:

**How it works:**

Key Monitoring Tools in DevOps

1. Nagios: Ideal for monitoring infrastructure like servers, network, and services.
   - Pros: Highly customizable, open-source, mature, and supports various plugins.
   - Cons: Can be complex to set up.
2. Zabbix: Another open-source tool known for its versatility in monitoring IT components.
   - Pros: Multi-platform support, strong auto-discovery, and a centralized web interface.
   - Cons: Initial setup complexity.
3. Icinga: A flexible, enterprise-grade monitoring tool that evolved from Nagios.
   - Pros: Offers cluster capabilities, configurations can be versioned, and features a REST API.
   - Cons: Steeper learning curve than newer tools.
4. Prometheus: A cloud-native tool designed for dynamic environments like Kubernetes.
   - Pros: Excels in containerized environments, provides powerful query language and alerting.
   - Cons: Lacks built-in long-term storage. Combines well with Grafana for visualizations.
5. Datadog: A modern SaaS monitoring solution often favored for cloud infrastructure and modern applications.
   - Pros: Easy-to-use with machine-learning powered analytics, works well for microservices, cloud and on-prem systems.
   - Cons: Cost can be a prohibiting factor for smaller teams.
6. New Relic: Offers application and infrastructure monitoring, business performance metrics, and more.
   - Pros: Great for detailed application performance monitoring, cloud-native environments, and full-stack visibility.
   - Cons: Can be expensive for comprehensive plans.
7. Stackify: Targeted for application performance management (APM) and offers tools for errors, logs, and more.
   - Pros: Designed for developers and DevOps, cost-effective for application-centric monitoring.
   - Cons: Limited capabilities for infrastructure monitoring.
8. AppDynamics: Focuses on application performance management with in-depth insights into user experience and business impact.
   - Pros: Intuitive user interface, powerful solutions for microservices and cloud applications.
   - Cons: Pricing structure can be complicated.
9. Dynatrace: Known for its AI-powered analysis and auto-discovery capabilities.
   - Pros: Minimal manual configuration, strong support for cloud and microservice architectures.
   - Cons: Higher learning curve, potential complexity in interpreting AI-driven insights.
10. Grafana: While often associated with visualization, it offers advanced monitoring and alerting features, especially when used with data sources like Prometheus.
    - Pros: Wide range of data sources, customizable dashboards, robust alerting system.
    - Cons: Not a standalone monitoring tool.

**Thoughts:**

Each tool has its unique strengths and design philosophies. Your choice will depend on your monitoring requirements, existing infrastructure, and team's expertise.

**Sources:**

- [Devinterview.io — DevOps](https://devinterview.io/questions/web-and-mobile-development/devops-interview-questions/)

</details>

<details>
<summary>Describe the role of a tool like Docker in a DevOps environment.</summary>

### Describe the role of a tool like Docker in a DevOps environment.

**Definition:**

Docker packages an application and its dependencies into an image and runs that image as a container: an isolated process with the files it needs. The same image can run on a developer machine, in CI, and in production, so “works on my machine” stops being a delivery problem.

**How it works:**

- A Dockerfile builds an image. `docker build` produces an immutable image; `docker run` starts a container from it.
- Containers share the host kernel. Each one is self-contained, isolated from other containers, and portable across machines that run Docker.
- Docker Compose defines multi-container apps in `compose.yaml` and starts them with `docker compose up`.
- Registries (Docker Hub or a private registry) are how pipelines publish the image that later stages deploy.

**When to use:**

Use Docker when the app must run the same way in dev, CI, and production, or when several services need isolated dependencies on one machine. A virtual machine is the better fit when you need a separate guest kernel.

**Examples:**

```dockerfile
# syntax=docker/dockerfile:1
FROM node:latest
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "index.js"]
```

```console
docker pull docker/welcome-to-docker
docker run --detach --name welcome --publish 8080:80 docker/welcome-to-docker
```

**Sources:**

- [Docker — What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)
- [Docker — What is an image?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)
- [Docker Compose](https://docs.docker.com/compose/)

</details>

<details>
<summary>What are orchestration tools and can you give examples?</summary>

### What are orchestration tools and can you give examples?

**Definition:**

An orchestration tool keeps a declared number of containers (or other workloads) running, places them on machines, and replaces them when they fail. Docker runs a container; an orchestrator runs the fleet.

**How it works:**

- Kubernetes is a portable platform for managing containerized workloads with declarative configuration. A Deployment manages Pods; a Service gives them a stable name and port. Changing a Deployment’s pod template starts a rolling update: new Pods come up and old Pods are terminated.
- Docker Swarm schedules services. In the replicated model the manager places a set number of tasks; `docker service scale` changes that number. A global service runs one task on every node.
- Managed control planes — Amazon EKS, and the same Kubernetes API on other clouds — remove the need to install that control plane yourself.

**When to use:**

Use Compose or a single host when you have a few containers. Use Kubernetes or Swarm when you need scheduling, restarts, rolling updates, and a stable service address across nodes.

**Examples:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello
spec:
  selector:
    app: hello
    tier: backend
  ports:
    - protocol: TCP
      port: 80
      targetPort: http
```

```console
docker service create --name my_web --replicas 3 nginx
docker service scale my_web=5
```

**Sources:**

- [Kubernetes concepts](https://kubernetes.io/docs/concepts/)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [Docker Swarm services](https://docs.docker.com/engine/swarm/services/)

</details>

<details>
<summary>Are there any cloud platforms that align well with DevOps, and can you name them?</summary>

### Are there any cloud platforms that align well with DevOps, and can you name them?

**Definition:**

Amazon Web Services, Microsoft Azure, and Google Cloud all fit DevOps because each one offers APIs for compute, pipelines, and infrastructure-as-code instead of ticket-driven servers.

**How it works:**

- AWS: CloudFormation models resources in a template; Amazon EKS runs Kubernetes; Lambda runs code without you provisioning servers.
- Azure: Azure Pipelines defines build and release jobs; AKS is the managed Kubernetes service.
- Google Cloud: Cloud Build runs builds from a config file in the repo; GKE is managed Kubernetes. Terraform can provision resources on any of these clouds.

The same Terraform or Kubernetes manifests can target more than one of these clouds. The pipeline still builds, tests, and deploys; only the provider credentials change.

**When to use:**

Pick the cloud your data, identity, and existing accounts already live in. Use more than one only when a product constraint (residency, a managed service, a customer requirement) forces it.

**Sources:**

- [AWS CloudFormation overview](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
- [Azure Pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines)
- [Cloud Build overview](https://docs.cloud.google.com/build/docs/overview)

</details>

<details>
<summary>How do you manage logging in a DevOps environment?</summary>

### How do you manage logging in a DevOps environment?

**Definition:**

Logging in DevOps means applications emit structured events, a collector ships them off the machine that produced them, and a backend stores them long enough to debug a deploy or an outage. Node-local files disappear when the container or node goes away.

**How it works:**

- Containerized apps write to stdout and stderr. On Kubernetes the container runtime and kubelet capture those streams; `kubectl logs` reads the latest file on the node. `kubectl logs --previous` reads the last terminated container.
- Kubernetes does not store cluster-wide logs. A node agent, usually a DaemonSet, tails the log files and forwards them. OpenTelemetry can export logs, traces, and metrics to a backend you choose; OpenTelemetry itself is not the store.
- Keep a correlation id (trace id or request id) on each line so a failed pipeline deploy and the app logs can be searched together.

**When to use:**

Stdout plus a node agent is enough for most services. Add a sidecar only when the process writes files and cannot log to stdout. Push directly from the app when you already standardize on one telemetry SDK.

**Examples:**

```shell
kubectl logs counter
kubectl logs counter --previous
```

**Sources:**

- [Kubernetes — Logging architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
- [OpenTelemetry — What is OpenTelemetry?](https://opentelemetry.io/docs/what-is-opentelemetry/)

</details>

<details>
<summary>Explain the use of a tool like Jenkins or GitLab CI in the CI/CD pipeline.</summary>

### Explain the use of a tool like Jenkins or GitLab CI in the CI/CD pipeline.

**Definition:**

Jenkins and GitLab CI turn a commit into an ordered set of jobs: build, test, then deploy. The pipeline definition lives in the repo so the steps are reviewed like code.

**How it works:**

- Jenkins Declarative Pipeline uses a `Jenkinsfile`. `agent` allocates an executor and workspace and checks out the repo. `stages` run in order (`Build`, `Test`, `Deploy`). A failed stage stops later stages when `skipStagesAfterUnstable` is set. `post` runs cleanup regardless of the result.
- GitLab CI uses `.gitlab-ci.yml` at the repo root. Stages run in sequence; jobs in one stage run in parallel. A runner executes each job. GitLab creates a pipeline on a push, a merge request, a schedule, or a manual run.
- Either tool can build a Docker image and hand the artifact to the next stage. The tool does not replace the orchestrator; it calls it.

**When to use:**

Use GitLab CI when the source repo is already on GitLab and you want runners, merge-request pipelines, and environments in one product. Use Jenkins when you need a self-hosted automation server and plugins for tools GitLab CI does not talk to directly.

**Examples:**

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'make'
            }
        }
        stage('Test') {
            steps {
                sh 'make check'
                junit 'reports/**/*.xml'
            }
        }
        stage('Deploy') {
            steps {
                sh 'make publish'
            }
        }
    }
}
```

```yaml
stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - make build

test_job:
  stage: test
  script:
    - make test
```

**Sources:**

- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [Jenkins — Using a Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/)
- [GitLab CI/CD](https://docs.gitlab.com/ci/)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)

</details>

### Continuous Integration and Continuous Deployment (CI/CD)

<details>
<summary>Can you describe the CI/CD pipeline and its stages?</summary>

### Can you describe the CI/CD pipeline and its stages?

**Definition:**

A CI/CD pipeline is the automated path from a commit to a running release. Continuous integration builds and tests every change. Continuous delivery keeps a releasable artifact ready. Continuous deployment pushes that artifact to production when the earlier stages pass.

**How it works:**

GitLab’s model matches what Jenkins stages do:

1. **Build** — compile or `docker build`, and publish the artifact.
2. **Test** — unit and integration jobs. They start only after the build stage succeeds. Jobs in the same stage run in parallel.
3. **Deploy** — ship the artifact to an environment (`environment: production` in GitLab, or a Deploy stage in a Jenkinsfile). Later stages do not run if an earlier job fails.

A pipeline starts on a push, a merge request, a schedule, or a manual run. A runner or Jenkins agent executes the scripts.

**Examples:**

```yaml
stages:
  - build
  - test
  - deploy

compile:
  stage: build
  script:
    - make build

test1:
  stage: test
  script:
    - make test

deploy_prod:
  stage: deploy
  script:
    - make deploy
  environment: production
```

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)

</details>

<details>
<summary>How do you manage branching strategies for CI/CD?</summary>

### How do you manage branching strategies for CI/CD?

**Definition:**

A branching strategy decides which Git refs trigger a pipeline and which ref is allowed to deploy. Git makes short-lived branches cheap because a branch is a pointer and merges are three-way merges.

**How it works:**

- **Topic branches.** A short-lived branch for one change. CI runs on that branch or on the merge request. After review and a green pipeline, you merge and delete the branch.
- **Long-running branches.** A stable branch (`master` or `main`) holds released code. A `develop` or `next` branch integrates topic branches before they graduate to the stable branch.
- The pipeline config keys off the branch. GitLab can run a pipeline for a push or a merge request. Production deploy jobs are limited to the stable branch.

**When to use:**

Topic branches plus a protected stable branch fit most product teams. Long-running `develop` helps when several features must be integrated before a release. Trunk-based development (short branches, merge to main often) fits when the pipeline is fast and feature flags hide unfinished work.

**Sources:**

- [Git — Branching Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)

</details>

<details>
<summary>What is blue-green deployment?</summary>

### What is blue-green deployment?

**Definition:**

Blue-green deployment runs two production environments. One (blue) serves traffic. The other (green) receives the new release. After checks pass, the router sends users to green. The previous environment stays up for a fast switch back.

**How it works:**

1. Deploy the new version to the idle environment. It uses the same kind of artifact the live environment uses (for example the same container image with a new tag).
2. Run smoke tests against the idle environment while users still hit the live one.
3. Shift the load balancer, Service selector, or DNS target to the new environment.
4. Keep the old environment until you are sure you will not switch back.

On Kubernetes a rolling update is a different mechanism: the Deployment replaces Pods gradually inside one ReplicaSet pair. Blue-green keeps a full second copy and switches traffic once.

**When to use:**

Use blue-green when you can afford two full environments and you want an immediate switch back. Use a rolling update when you want one environment and a gradual Pod replacement.

**Sources:**

- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [AWS — Blue/green deployments on ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-type-bluegreen.html)

</details>

<details>
<summary>How does a canary release differ from blue-green deployment?</summary>

### How does a canary release differ from blue-green deployment?

**Definition:**

A canary release sends a small share of real traffic to the new version and watches errors and latency before shifting the rest. Blue-green switches the whole user base at once after offline checks on the idle environment.

**How it works:**

- Blue-green: 0% then 100%. Rollback is pointing the router back at the old environment.
- Canary: 1–5% (or a chosen subset), then more, then 100%. Rollback is setting the new version’s weight back to zero while the old version still runs.
- On Kubernetes you can run two Deployments and move a Service’s selector, or let a mesh split traffic. Istio’s data plane can route a fraction of HTTP, gRPC, WebSocket, and TCP traffic with routing rules.
- A Deployment rolling update replaces Pods by readiness, which is a limited form of gradual replacement, not a weighted user canary by itself.

**When to use:**

Choose canary when a bug would be costly and you can measure the new version on live traffic. Choose blue-green when you need a single cutover and a full idle copy to test first.

**Sources:**

- [Istio — What is Istio?](https://istio.io/latest/docs/overview/what-is-istio/)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)

</details>

<details>
<summary>What are the best practices for setting up a CI/CD pipeline?</summary>

### What are the best practices for setting up a CI/CD pipeline?

**Definition:**

A useful pipeline is fast, repeatable, and stops a bad change before it reaches users. The definition of the pipeline is versioned next to the code.

**How it works:**

- Keep the config in the repo: `Jenkinsfile` or `.gitlab-ci.yml`. Stages run build, then test, then deploy. Fail the pipeline when a test job fails so the next stage does not run.
- Build once. Later stages deploy the same artifact or image digest, not a rebuild.
- Run jobs in a stage in parallel (GitLab does this by default) so the feedback loop stays short.
- Pin the agent image (`image:` in GitLab, or a container agent in Jenkins) so the toolchain matches production.
- Protect the deploy branch, store secrets outside the file (Jenkins credentials, GitLab CI variables), and record which commit produced the deploy.
- Treat `terraform plan` or a Kubernetes diff as a review step before apply.

**Sources:**

- [Jenkins — Using a Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)

</details>

<details>
<summary>What challenges might you face in implementing CI/CD and how would you overcome them?</summary>

### What challenges might you face in implementing CI/CD and how would you overcome them?

**Definition:**

CI/CD fails when the pipeline is slow, flaky, or nobody trusts the result enough to deploy from it. The fix is a smaller, deterministic pipeline, not more manual gates.

**How it works:**

- **Slow feedback.** Split tests into a fast stage and a slower stage. Run jobs in the same stage in parallel, which GitLab already does.
- **Flaky tests.** A red job that is sometimes green trains people to rerun blindly. Quarantine the flaky test, fix the race, and keep the pipeline failing only on real regressions.
- **“Works in CI, fails in prod.”** Build a container image in CI and deploy that image. Docker’s point is that the container on the laptop and in CI carries its own dependencies.
- **Secrets in the repo.** Read them from the CI system’s secret store at runtime. Do not echo them in logs.
- **Fear of deploying.** Start with continuous delivery (artifact ready, human promotes) and move to continuous deployment once the test stage is trusted. Keep a rollback path (previous image tag, or the previous Terraform state).

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Docker — What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)

</details>

<details>
<summary>How do you ensure CI/CD pipelines are secure?</summary>

### How do you ensure CI/CD pipelines are secure?

**Definition:**

A pipeline is a privileged path to production. Securing it means the runner’s credentials, the supply chain of the build, and the checks that run before deploy are all explicit.

**How it works:**

- Limit who can change the pipeline file on the protected branch. Review `.gitlab-ci.yml` and `Jenkinsfile` like application code.
- Give the deploy job a narrow role. On AWS, Lambda and other deploy targets use an IAM role; the same idea applies to the CI role that calls `terraform apply` or `kubectl`.
- Do not store tokens in the repository. Inject them as masked CI variables or Jenkins credentials. Vault can issue short-lived credentials (dynamic secrets) so a leaked job log expires.
- Scan dependencies and the image before the deploy stage. Fail the pipeline on a policy violation.
- Pin base images by digest where you can, and record the commit SHA that produced the artifact.

**Sources:**

- [GitLab CI/CD](https://docs.gitlab.com/ci/)
- [Vault glossary — Secrets engine](https://developer.hashicorp.com/vault/docs/glossary)
- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)

</details>

<details>
<summary>Discuss the role of automated testing in CI/CD.</summary>

### Discuss the role of automated testing in CI/CD.

**Definition:**

Automated tests are the gate between a commit and a deploy. In GitLab and Jenkins the test stage runs only after build succeeds, and deploy runs only after the test jobs succeed.

**How it works:**

- Unit tests run on every branch and should finish in minutes.
- Integration tests exercise the built artifact. If the app is containerized, run them against the image you will deploy.
- A Jenkins test stage can publish JUnit reports (`junit 'reports/**/*.xml'`) so failures show on the build page.
- Tests that need a dependency you do not want to run can use a stub (service virtualization) in the test job.
- A green pipeline is evidence for the release. A skipped test stage is not a pipeline; it is a script.

**Examples:**

```groovy
stage('Test') {
    steps {
        sh 'make check'
        junit 'reports/**/*.xml'
    }
}
```

**Sources:**

- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)

</details>

<details>
<summary>How can containerization be leveraged in a CI/CD pipeline?</summary>

### How can containerization be leveraged in a CI/CD pipeline?

**Definition:**

The pipeline builds an image once, tests that image, and deploys the same image digest. The container carries the runtime and dependencies, so the agent and production do not need matching host packages.

**How it works:**

1. CI checks out the commit and runs `docker build` from the Dockerfile.
2. The test job runs the image (`docker run` or Compose) and executes the test command inside it.
3. On success, the job pushes the image to a registry with a tag tied to the commit SHA.
4. The deploy job tells Kubernetes or Swarm to run that image. A Deployment rolling update replaces Pods when the pod template changes. Swarm sets the replica count with `docker service create --replicas` or `docker service scale`.

**Examples:**

```console
docker build -t registry.example.com/app:${CI_COMMIT_SHA} .
docker push registry.example.com/app:${CI_COMMIT_SHA}
```

**Sources:**

- [Docker — What is an image?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [Docker Swarm services](https://docs.docker.com/engine/swarm/services/)

</details>

<details>
<summary>How do you roll back a deployment in a CI/CD process?</summary>

### How do you roll back a deployment in a CI/CD process?

**Definition:**

Rollback restores the previous known-good artifact and traffic route. The pipeline should make that artifact easy to name: an image digest, a Git tag, or a Terraform state that still matches the last good apply.

**How it works:**

- **Containers.** Redeploy the previous image tag. On Kubernetes, changing the Deployment template back triggers another rolling update. `kubectl rollout undo deployment/nginx-deployment` returns that Deployment to its previous revision.
- **Blue-green.** Point the load balancer or Service back at the environment that still runs the old version.
- **Canary.** Set the new version’s traffic weight to zero.
- **CloudFormation.** A failed stack update rolls back to the last working state. You can also redeploy the previous template.
- **Terraform.** Re-apply the last good configuration. `terraform apply` changes real infrastructure to match that config. Drift detection (`terraform apply -refresh-only`) updates state only; it does not revert the cloud.
- **Database.** Roll back only a changeset that has a rollback (Liquibase `rollback` on the changeset). Do not invent a reverse of a destructive data migration.

**Sources:**

- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [Kubernetes — Rolling back a Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)

</details>

### Infrastructure as Code (IaC)

<details>
<summary>What is Terraform and how is it used in IaC?</summary>

### What is Terraform and how is it used in IaC?

**Definition:**

Terraform is an infrastructure-as-code tool. You describe compute, storage, DNS, and other resources in configuration files. Terraform plans the difference and applies it so the real infrastructure matches that configuration.

**How it works:**

1. **Write** resource blocks. A resource has a type and a name, for example `aws_instance.app_server`.
2. **Initialize** (`terraform init`) so Terraform can download providers and modules.
3. **Plan** to preview creates, updates, and deletes.
4. **Apply** to perform those actions. A second apply with the same config reports “No changes. Your infrastructure matches the configuration.”
5. **State** records what Terraform created. Modules group resources so a root module can call a child module (`module.website`).

**Examples:**

```hcl
resource "aws_instance" "app_server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type

  tags = {
    Name = var.instance_name
  }
}
```

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)

</details>

<details>
<summary>Can you describe the functionality of AWS CloudFormation?</summary>

### Can you describe the functionality of AWS CloudFormation?

**Definition:**

AWS CloudFormation models and provisions AWS resources from a template. The template is JSON or YAML. A stack is the set of resources CloudFormation creates from that template.

**How it works:**

- A template has a `Resources` section (required for a useful stack) plus optional `Parameters`, `Mappings`, `Conditions`, and `Outputs`. YAML uses indentation; JSON uses braces. Both describe the same stack.
- You create a stack from the template. CloudFormation calls AWS APIs to create the resources and records them as one unit.
- To change a stack, edit the template and create a change set. The change set lists what will be added, modified, or replaced. Executing it updates only the affected resources. If the update fails, CloudFormation rolls the stack back to the last working state.
- A change set does not prove the update will succeed. It does not check account quotas or IAM permissions.

**Examples:**

```yaml
AWSTemplateFormatVersion: 2010-09-09
Resources:
  WebServer:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref LatestAmiId
      InstanceType: !Ref InstanceType
```

**Sources:**

- [CloudFormation template format](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)

</details>

<details>
<summary>Explain the differences between declarative and imperative approaches in IaC.</summary>

### Explain the differences between declarative and imperative approaches in IaC.

**Definition:**

Declarative IaC states the desired resources. The tool computes the actions. Imperative IaC states the actions (create this, then configure that) in order.

**How it works:**

- **Declarative.** Terraform configuration and a CloudFormation template list resources and properties. `terraform plan` / a CloudFormation change set shows the diff against current infrastructure. Re-applying the same file is a no-op when nothing drifted.
- **Imperative.** A shell script or a procedural tool runs commands: `aws ec2 run-instances`, then a second command to tag the instance. Running it twice can create a second instance unless the script checks first.
- Kubernetes manifests are declarative too: you `kubectl apply` the desired Pod or Deployment, and the control plane converges.

**When to use:**

Use declarative templates for anything you must recreate or review (networks, clusters, databases). Use a short imperative script for a one-off migration the declarative model does not express, and then fold the result back into the template.

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [CloudFormation template format](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html)
- [Kubernetes concepts](https://kubernetes.io/docs/concepts/)

</details>

<details>
<summary>What are the benefits of using IaC in a cloud environment?</summary>

### What are the benefits of using IaC in a cloud environment?

**Definition:**

Infrastructure as code makes cloud resources reviewable, repeatable, and disposable. The template or Terraform config is the source of what should exist in the account.

**How it works:**

- **Review.** A pull request shows the `terraform plan` or CloudFormation change set before anything is created.
- **Repeat.** The same file can build a test stack and a production stack with different parameters (`InstanceType`, `var.instance_type`).
- **Recover.** If a stack update fails, CloudFormation returns to the last working state. Terraform can re-apply the last good configuration.
- **Drift is visible.** `terraform apply` compares real infrastructure to the configuration. Changes made in the console show up as a diff instead of silent snowflake servers.
- **Cleanup.** Destroying a stack or running destroy removes the set of resources the tool tracks, which is how you avoid leftover test accounts.

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)

</details>

<details>
<summary>What is a “stack” in the context of IaC, as seen in tools like Terraform?</summary>

### What is a “stack” in the context of IaC, as seen in tools like Terraform?

**Definition:**

A stack is one deployed copy of a template: the resources, parameters, and outputs that belong together and are updated as a unit. CloudFormation uses the word “stack” for that unit. Terraform’s closest equivalents are a root module plus its state, often called a workspace or a state file rather than a stack.

**How it works:**

- In CloudFormation you submit a template and get a stack. Updates go through a change set on that stack. Rollback restores that stack, not a neighboring one.
- In Terraform, one configuration directory (root module) and one state file track one set of resources. Child modules (`module.website.aws_s3_bucket.this`) are pieces of that set, not separate stacks, unless you split state on purpose.
- Teams often run one state per environment (dev, prod) so a change to dev cannot plan against production resources.

**When to use:**

Split stacks or state files on lifecycle and blast radius: network in one, the application in another. Keep resources that must be created and destroyed together in the same stack.

**Sources:**

- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)

</details>

<details>
<summary>How do you handle configuration drift?</summary>

### How do you handle configuration drift?

**Definition:**

Configuration drift is a real resource that no longer matches the code that is supposed to describe it. Someone changed a security group in the console, or a process edited a tag, and the template was not updated.

**How it works:**

- Terraform compares infrastructure to configuration on plan and apply. The drift tutorial shows `terraform apply -refresh-only`: Terraform reports objects changed outside Terraform and can write those changes into state without modifying the cloud. That records drift; it does not revert it.
- To remove drift, put the intended value back in the configuration and `terraform apply`, or change the configuration to accept the console edit and apply so state and code agree.
- CloudFormation change sets show the difference between the submitted template and the stack. Detect drift on the stack, then update the template and execute a change set if you want the template to win.
- Prevent drift by restricting console write access and deploying only from the pipeline.

**Sources:**

- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)

</details>

<details>
<summary>Describe idempotence in the context of IaC and its importance.</summary>

### Describe idempotence in the context of IaC and its importance.

**Definition:**

An idempotent operation leaves the system in the desired state whether you run it once or again. A second Terraform apply of an unchanged configuration reports no changes and does not create duplicate resources.

**How it works:**

- Terraform stores identities in state. The next apply updates or leaves those resources; it does not call “create” blindly.
- CloudFormation updates a stack in place. Re-submitting the same template does not create a second copy of the stack’s resources.
- Kubernetes `kubectl apply` converges a Deployment to the manifest. Applying the same manifest again does not add a second object with the same name.
- A shell script that always calls `run-instances` is not idempotent. That is why declarative tools matter for pipelines that retry.

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [CloudFormation template format](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html)

</details>

<details>
<summary>What are some security best practices for IaC?</summary>

### What are some security best practices for IaC?

**Definition:**

IaC security is about who can apply, what the template is allowed to create, and which secrets never land in the file or the state that you commit.

**How it works:**

- Review plans and change sets in pull requests. A change set can show that a resource will be replaced; read that before you execute it.
- Scope the CI role. CloudFormation documents that a stack operation still needs IAM permission on each resource; a change set does not grant those permissions.
- Do not commit cloud credentials, private keys, or database passwords. Pass parameters at apply time, or read them from Vault. Vault’s KV engine returns stored secrets; dynamic engines can mint short-lived credentials.
- Keep state access private. Terraform state can contain attribute values from the cloud.
- Scan templates in CI before apply. Fail the pipeline on open security groups or public buckets your policy forbids.

**Sources:**

- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [Vault glossary — Secrets engine](https://developer.hashicorp.com/vault/docs/glossary)
- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)

</details>

<details>
<summary>How do you test infrastructure code?</summary>

### How do you test infrastructure code?

**Definition:**

Testing IaC means checking the template before it changes a shared account, then applying it in an isolated environment and asserting the result.

**How it works:**

- **Static checks.** `terraform validate` and `terraform plan` must succeed. For CloudFormation, lint the template and read the change set. A change set previews adds, changes, and replacements; it does not prove quotas or IAM will allow the update.
- **Ephemeral apply.** The pipeline applies the same module to a test account or a short-lived stack, runs a probe (health check, `terraform plan` shows no diff), then destroys the stack.
- **Policy.** Reject plans that open `0.0.0.0/0` on admin ports or that replace a database, using whatever policy tool you already run in CI.
- **Idempotence.** Apply twice. The second apply should report no changes.

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)

</details>

<details>
<summary>What is the significance of using modules in IaC tools?</summary>

### What is the significance of using modules in IaC tools?

**Definition:**

A module is a reusable group of resources with inputs and outputs. Callers pass variables; they do not copy the resource blocks.

**How it works:**

- In Terraform, a root module can call a child module. The docs show `module.website` creating an S3 bucket with the provider configured on the root. You version the module and point each environment at a ref.
- One change in the module (encryption on the bucket, a required tag) reaches every caller on the next apply, after they bump the version.
- CloudFormation offers nested stacks and modules for the same reason: one network or one service pattern, many stacks.
- Modules do not remove state. Each root module still has its own state so dev and prod stay isolated.

**When to use:**

Extract a module when a second team needs the same pattern. Keep a one-off resource in the root module until a second caller exists.

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [CloudFormation template format](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html)

</details>

### DevOps Testing

<details>
<summary>What is test automation and how does it fit into DevOps?</summary>

### What is test automation and how does it fit into DevOps?

**Definition:**

Test automation is executable checks that run without a person clicking through the product. In DevOps they sit in the pipeline so a commit is not deployed until those checks pass.

**How it works:**

- The test stage runs after build and before deploy. In GitLab, jobs in the test stage start when the build stage succeeds, and deploy waits for them.
- Fast tests (unit) run on every push. Slower tests (API, browser) run on the merge to the stable branch or on a schedule if they cannot finish in the inner loop.
- A Jenkins test stage can archive JUnit XML so the failure is visible on the build.
- Automation does not replace exploring the product. It removes the repeated checks that would otherwise block a release.

**Examples:**

```groovy
stage('Test') {
    steps {
        sh 'make check'
        junit 'reports/**/*.xml'
    }
}
```

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)

</details>

<details>
<summary>Can you name some methods or practices for ensuring code quality?</summary>

### Can you name some methods or practices for ensuring code quality?

**Definition:**

Code quality in a DevOps pipeline is a set of checks that run on the change before it is merged: review, automated tests, and a build that uses the same artifact you will ship.

**How it works:**

- **Review.** Topic branches and merge requests keep one change visible. Git’s topic-branch workflow exists so review sees only that topic.
- **Tests in CI.** The pipeline fails the merge when the test stage fails.
- **One artifact.** Build a container image once and test that image, so quality is measured on the bits you deploy.
- **Small batches.** Short-lived branches merge often, so a defect is tied to a small diff.
- **Production signals.** Prometheus scrapes numeric time series (latency, errors). A dashboard or alert tells you whether the release stayed healthy after the pipeline went green.

**Sources:**

- [Git — Branching Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)

</details>

<details>
<summary>How does DevOps influence Quality Assurance (QA) practices?</summary>

### How does DevOps influence Quality Assurance (QA) practices?

**Definition:**

DevOps moves QA from a phase after “code complete” into the same pipeline and the same feedback loop as build and deploy. Testers design checks that a runner can execute on every change.

**How it works:**

- QA owns the test stage: which jobs block deploy, which are informational, and which data the job needs.
- Defects are found on the topic branch, when the change is still small, instead of in a release-candidate week.
- Production is part of QA. Logs (`kubectl logs`, a cluster logging backend) and metrics (Prometheus time series) show whether a release behaved. A blameless review after an incident feeds new tests.
- Exploratory testing still happens. It targets the risk the automated stage does not cover, and it does not gate every commit by hand.

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)
- [Kubernetes — Logging architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

</details>

<details>
<summary>Explain the concept of “test-driven development (TDD)” within DevOps.</summary>

### Explain the concept of “test-driven development (TDD)” within DevOps.

**Definition:**

Test-driven development writes a failing automated test for a behavior, then writes the code that makes that test pass, then cleans up the design. In DevOps those tests live in the CI test stage, so the same check guards every later commit.

**How it works:**

1. Add a test that describes the behavior and fails because the behavior is absent.
2. Change the application until the test passes locally.
3. Push the branch. The pipeline’s test stage runs that test with the rest of the suite. Deploy does not run if it fails.
4. Refactor while the stage stays green.

TDD is a way of writing the tests the pipeline already requires. It is not a separate environment.

**When to use:**

Use TDD for logic with clear inputs and outputs (pricing, parsers, API handlers). For a spike or a UI experiment, you may still write the test after you know the behavior, then lock it into the same test stage.

**Sources:**

- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)

</details>

<details>
<summary>What are service virtualization and its relevance to DevOps?</summary>

### What are service virtualization and its relevance to DevOps?

**Definition:**

Service virtualization replaces a dependency the pipeline cannot call — a third-party API, a mainframe, a partner sandbox — with a test double that speaks the same protocol and returns scripted responses.

**How it works:**

- The CI job starts the double, points the app under test at it, and runs the test stage. The job does not need the real partner endpoint to be up.
- The double returns success, timeout, and error bodies you define, so the pipeline can assert failure paths.
- Contract tests check that the double still matches the real service’s request and response shape. When the contract drifts, update the double or the test fails.
- This is for tests. Production still calls the real service. Do not ship the virtualized URL in the deploy stage.

**When to use:**

Use it when the real dependency is slow, rate-limited, paid, or shared across teams. Call the real dependency in a later, less frequent stage when you own credentials and the provider allows it.

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [WireMock — Stubbing](https://wiremock.org/docs/stubbing/)

</details>

### DevOps Monitoring and Logging

<details>
<summary>What is Application Performance Monitoring (APM)?</summary>

### What is Application Performance Monitoring (APM)?

**Definition:**

Application performance monitoring is telemetry about a running service: how long requests take, how often they fail, and which downstream call was slow. You use it to see the effect of a deploy, not only whether the process is up.

**How it works:**

- Prometheus scrapes numeric time series over HTTP (a pull model). A series is a metric name plus labels, stored with the timestamp of the sample. PromQL queries those series. Prometheus is built to stay up during an outage; it is a poor fit when you need a complete record of every request for billing.
- OpenTelemetry generates and exports traces, metrics, and logs. It is not a backend. A trace shows one request across services; metrics are the aggregates Prometheus-style systems store.
- Grafana (or another API consumer) graphs the series Prometheus stored. An alertmanager handles alerts the Prometheus server fires.

**When to use:**

Start with RED-style metrics on the HTTP service (rate, errors, duration) via Prometheus. Add traces with OpenTelemetry when a single metric cannot show which hop is slow.

**Sources:**

- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)
- [OpenTelemetry — What is OpenTelemetry?](https://opentelemetry.io/docs/what-is-opentelemetry/)
- [Grafana — Create a dashboard](https://grafana.com/docs/grafana/latest/visualizations/dashboards/)

</details>

<details>
<summary>How do you ensure effective log management in a DevOps setup?</summary>

### How do you ensure effective log management in a DevOps setup?

**Definition:**

Effective log management means every service writes events you can search after the container is gone, with enough context to tie a line to a request and a deploy.

**How it works:**

- Write to stdout and stderr. On Kubernetes the kubelet exposes those logs with `kubectl logs`. The node rotates files (`containerLogMaxSize` defaults to 10 Mi, `containerLogMaxFiles` defaults to 5). `kubectl logs` returns only the latest file.
- Ship logs off the node. Kubernetes does not store cluster-level logs. A DaemonSet agent, or an OpenTelemetry Collector, forwards them to a backend. The Collector can receive logs over OTLP and export them.
- Put stable fields on each event: service name, environment, trace or request id, and the image digest or commit. Search those fields during an incident.
- Set a retention period. Node disk is not retention.

**Examples:**

```shell
kubectl logs counter
kubectl logs counter -c count
```

**Sources:**

- [Kubernetes — Logging architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
- [OpenTelemetry — What is OpenTelemetry?](https://opentelemetry.io/docs/what-is-opentelemetry/)

</details>

<details>
<summary>What is the significance of time-series databases in monitoring?</summary>

### What is the significance of time-series databases in monitoring?

**Definition:**

A time-series store records a number with the time it was observed. Monitoring queries that history: error rate now versus the hour before the deploy. Prometheus is that kind of store.

**How it works:**

- Prometheus identifies a series by a metric name and labels, for example `http_requests_total{job="apiserver", handler="/api/comments"}`. A range selector such as `[5m]` reads samples over a window.
- The server scrapes targets on an interval (`scrape_interval`). It also writes an `up` series for each scrape so you can alert when a target cannot be scraped.
- It evaluates alerting rules on an `evaluation_interval` and sends firing alerts to Alertmanager.
- Prometheus does not try to be durable distributed storage on its own. Each server stores samples locally and keeps working if the network is impaired. Long-term retention, if you need it, is a separate system you remote-write to.

**Examples:**

```promql
http_requests_total{job="apiserver", handler="/api/comments"}
```

**Sources:**

- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)
- [Prometheus — Query examples](https://prometheus.io/docs/prometheus/latest/querying/examples/)
- [Prometheus — Jobs and instances](https://prometheus.io/docs/concepts/jobs_instances/)

</details>

<details>
<summary>How would you set up an alerting system for your applications?</summary>

### How would you set up an alerting system for your applications?

**Definition:**

An alert fires when a PromQL expression stays true for a defined time, then Alertmanager groups and routes it to a receiver. The page should be actionable: a person or a runbook can do something about it.

**How it works:**

1. Instrument the app or exporter so Prometheus can scrape it. The `up` metric is 0 when a scrape fails.
2. Point Prometheus at a rules file and at Alertmanager (`alerting.alertmanagers` in `prometheus.yml`).
3. Write a rule with `expr`, `for`, and annotations. `for: 5m` avoids paging on a single missed scrape.
4. In Alertmanager, route by a label such as `severity` and group related alerts so one outage is one notification.

**Examples:**

```yaml
groups:
  - name: example
    rules:
      - alert: InstanceDown
        expr: up == 0
        for: 5m
        labels:
          severity: page
        annotations:
          summary: "Instance {{ $labels.instance }} down"
```

**Sources:**

- [Prometheus — Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)
- [Prometheus — Alerting tutorial](https://prometheus.io/docs/tutorials/alerting_based_on_metrics/)

</details>

<details>
<summary>Discuss the role of dashboards in DevOps monitoring.</summary>

### Discuss the role of dashboards in DevOps monitoring.

**Definition:**

A dashboard is a set of panels that query a data source and show related series in one place. During a deploy or an incident it answers “did the golden signals move?” faster than ad-hoc queries.

**How it works:**

- Grafana panels run queries against a data source. Prometheus is a typical source for metrics. A time-series panel plots a PromQL result; a stat panel shows the latest reduced value.
- Build one dashboard per service: request rate, error ratio, latency, and saturation (CPU or the `up` metric). Tie the time range to the deploy.
- Dashboards do not replace alerts. Alerts tell you to look; the dashboard shows which label (instance, handler, version) moved.
- Keep the dashboard in version control when you can, so a panel change is reviewed like the service.

**Sources:**

- [Grafana — Dashboards](https://grafana.com/docs/grafana/latest/visualizations/dashboards/)
- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)

</details>

### DevOps Security (DevSecOps)

<details>
<summary>What is DevSecOps and how does it integrate with DevOps?</summary>

### What is DevSecOps and how does it integrate with DevOps?

**Definition:**

DevSecOps is DevOps with security checks on the same pipeline as build and test, instead of a review that happens only before a quarterly release. The security work is automated where a rule can be automated, and reviewed where it cannot.

**How it works:**

- The pipeline already has stages. A security job sits in the test stage: dependency review, template policy, and a check that the image you will deploy has no forbidden findings.
- Infrastructure changes go through `terraform plan` or a CloudFormation change set so a security group or IAM change is visible in the pull request.
- Secrets are not committed. The deploy job reads them from CI variables or from Vault.
- Runtime policy continues after deploy. On Kubernetes, Pod Security Standards can warn or enforce a profile such as `baseline` on a namespace.
- A failed security job blocks deploy the same way a failed test does.

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Kubernetes — Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)

</details>

<details>
<summary>How can you implement security scanning within CI/CD pipelines?</summary>

### How can you implement security scanning within CI/CD pipelines?

**Definition:**

Security scanning in CI is a job that inspects the commit, the dependencies, or the built image and fails the pipeline when a policy matches. It runs before the deploy stage.

**How it works:**

- Add a job in the test stage of `.gitlab-ci.yml` or a Jenkins stage after `docker build` and before deploy. The job scans the artifact you will ship, not a different rebuild.
- Fail on a defined severity. A report that nobody gates on does not change what reaches production.
- Scan the IaC diff in the same pipeline: a plan or change set that opens a wide CIDR or a public bucket fails the job.
- Keep the scanner’s database update in the job so new advisories apply to the next commit. Pin the scanner version so the job is reproducible.
- Do not print secrets while the scanner runs. Mask CI variables.

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)

</details>

<details>
<summary>What are security considerations when working with Docker and Kubernetes?</summary>

### What are security considerations when working with Docker and Kubernetes?

**Definition:**

A container is an isolated process that shares the host kernel. It is not a virtual machine with its own kernel. Kubernetes schedules those containers as Pods. Security is the image you run, what the Pod is allowed to do on the node, and how services authenticate to each other.

**How it works:**

- Build from a known base image and deploy by digest. The pipeline should ship the image it scanned.
- Pod Security Standards define profiles. The `baseline` profile warns on privileged containers, host namespaces, and hostPath volumes. Label the namespace (`pod-security.kubernetes.io/enforce=baseline`) when you want the API to reject Pods that violate it.
- Give each workload a ServiceAccount with the permissions it needs. Mount credentials as a Secret volume or file; do not bake them into the image.
- For service-to-service traffic, a mesh such as Istio can require mutual TLS (`ISTIO_MUTUAL` on a DestinationRule) so workloads present a workload identity.

**Examples:**

```shell
kubectl label --overwrite ns example \
  pod-security.kubernetes.io/warn=baseline \
  pod-security.kubernetes.io/warn-version=latest
```

**Sources:**

- [Docker — What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)
- [Kubernetes — Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- [Istio — Destination Rule](https://istio.io/latest/docs/reference/config/networking/destination-rule/)

</details>

<details>
<summary>How do you manage secrets in a DevOps workflow?</summary>

### How do you manage secrets in a DevOps workflow?

**Definition:**

A secret is a credential, key, or token the running system needs and the Git repository must not contain. The workflow injects it at deploy or at runtime, and rotates it without rewriting history.

**How it works:**

- Keep the value in a secrets engine. Vault’s KV engine returns stored secrets. A dynamic engine can mint unique credentials and revoke them. Clients authenticate first (Kubernetes auth uses the Pod’s service-account token; AppRole uses a role id and secret id) and then read a path such as `kv-v2/data/creds`.
- CI reads the secret into a masked variable for the deploy job. The Jenkinsfile and `.gitlab-ci.yml` reference the variable; they do not embed the value.
- On Kubernetes, a Pod can consume a Secret. Prefer a manager that syncs or injects from Vault when you need rotation and an audit of reads.
- Terraform state and CloudFormation parameters can hold sensitive attributes. Restrict who can read state. Pass sensitive values as variables, not committed literals.

**Examples:**

```bash
curl \
  --header "X-Vault-Token: ${VAULT_TOKEN}" \
  "${VAULT_ADDR}/v1/devcreds/creds/developer"
```

**Sources:**

- [Vault glossary — Secrets engine](https://developer.hashicorp.com/vault/docs/glossary)
- [Vault — Kubernetes auth](https://developer.hashicorp.com/vault/docs/auth/kubernetes)
- [Kubernetes — Distribute credentials securely](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

</details>

<details>
<summary>What is the importance of compliance and governance in DevOps?</summary>

### What is the importance of compliance and governance in DevOps?

**Definition:**

Governance is the set of rules for who may change production and how that change is recorded. Compliance is evidence that those rules were followed. DevOps does not skip them; it turns them into pipeline checks and reviewable plans.

**How it works:**

- Protected branches and reviewed pipeline files show who approved a release.
- `terraform plan` and CloudFormation change sets are the record of an infrastructure change before it runs. IAM still decides whether the apply is allowed.
- Vault audit devices and CI logs show who read a secret or who started a deploy. Keep those logs in the same retained store as application logs.
- Policy jobs fail a merge that violates a control (public bucket, privileged Pod, unscanned image) so an exception is a reviewed change, not a console click.
- Map controls to stages you already have. A control with no job and no reviewer is a document, not a control.

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [Vault glossary — Secrets engine](https://developer.hashicorp.com/vault/docs/glossary)

</details>

### Cloud Services and DevOps

<details>
<summary>How do cloud services facilitate DevOps practices?</summary>

### How do cloud services facilitate DevOps practices?

**Definition:**

Cloud APIs let a pipeline create, change, and delete environments the same way it builds code. You do not wait for a rack; you submit a template or a function package.

**How it works:**

- **Provision.** CloudFormation turns a YAML template into a stack of AWS resources. Terraform does the same across clouds: init, plan, apply.
- **Run.** Amazon EKS runs the Kubernetes control plane for you. Lambda runs a function on an event without you provisioning the server, and scales each invocation independently.
- **Ship.** The CI job uses cloud credentials (a role, not a key in Git) to push an image and update a Deployment or a function.
- **Observe.** The same account’s metrics and logs feed Prometheus-style or vendor backends so the deploy job’s commit is visible next to the graph.

**Sources:**

- [CloudFormation — Welcome](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
- [Amazon EKS — What is EKS?](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
- [AWS Lambda — Getting started](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html)

</details>

<details>
<summary>What are some of the DevOps-friendly features of AWS?</summary>

### What are some of the DevOps-friendly features of AWS?

**Definition:**

AWS services that matter for DevOps are the ones a pipeline can call: templates for resources, a managed Kubernetes control plane, and functions that run without a server to patch.

**How it works:**

- **CloudFormation** creates a stack from a template and updates it through a change set. A failed update rolls back to the last working stack state.
- **Amazon EKS** runs upstream, conformant Kubernetes and manages the control plane’s availability. You keep using the Kubernetes API (`Deployment`, `Service`, `kubectl`).
- **Lambda** runs your code on events. You supply a deployment package, a handler, and an IAM execution role. Lambda provisions capacity, scales, and patches the execution environment.
- **IAM roles for CI** let the pipeline apply a stack or update a function without long-lived keys in the repo.

**Sources:**

- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [Amazon EKS user guide — Kubernetes concepts](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-concepts.html)
- [AWS Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-functions.html)

</details>

<details>
<summary>Can you describe the use of managed Kubernetes services such as EKS, AKS, or GKE?</summary>

### Can you describe the use of managed Kubernetes services such as EKS, AKS, or GKE?

**Definition:**

EKS, AKS, and GKE run the Kubernetes control plane for you. You still deploy workloads with the Kubernetes API: Pods, Deployments, and Services. You do not install etcd and the API server yourself.

**How it works:**

- Amazon EKS manages control-plane security and availability and runs conformant Kubernetes, so existing manifests and `kubectl` keep working. You focus on applications. EKS Auto Mode can also provision and patch the data plane; without it you still manage node groups.
- AKS and GKE are the same split on Azure and Google Cloud: the provider runs the control plane; you apply manifests and run a pipeline that builds images.
- The cluster is still Kubernetes. A Deployment rolling update, a Service DNS name (`my-service.my-ns`), and Pod Security labels behave as in the Kubernetes docs.
- The CI role needs cloud IAM to change the cluster and Kubernetes RBAC to change objects. Those are two different permission systems.

**When to use:**

Use a managed service when you want Kubernetes without operating the control plane. Run upstream Kubernetes yourself only when you must control that plane’s installation.

**Sources:**

- [Amazon EKS user guide — Kubernetes concepts](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-concepts.html)
- [Azure Kubernetes Service](https://learn.microsoft.com/en-us/azure/aks/what-is-aks)
- [GKE overview](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/kubernetes-engine-overview)
- [Kubernetes — Service](https://kubernetes.io/docs/concepts/services-networking/service/)

</details>

<details>
<summary>How do serverless architectures fit into DevOps?</summary>

### How do serverless architectures fit into DevOps?

**Definition:**

Serverless here means AWS Lambda: you upload a function, and Lambda runs it in response to events without you provisioning servers. Capacity, OS patching, and scaling are the service’s job. Your job is the code, the execution role, and the pipeline that publishes the package.

**How it works:**

- A function has a name, a runtime, a handler (`file.function`), an IAM role, and a deployment package (a zip). `Publish=True` creates a version.
- An event source invokes the function with JSON. Each invocation is independent; Lambda scales by running more invocations.
- The pipeline packages the zip or image, updates the function, and runs a test invoke before the alias that users hit moves to the new version.
- Rollback is pointing the alias at the previous version. You do not SSH to a box.
- Secrets still must not live in the zip. Read them from a secrets manager at runtime. The function’s role is the only AWS permission it should have.

**When to use:**

Use Lambda for event-shaped work (a queue message, an HTTP request, a file upload) with modest runtime limits. Use containers on EKS when you need a long-running process, a custom server, or a workload that does not fit the function model.

**Sources:**

- [AWS Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-functions.html)
- [AWS Lambda — Getting started scenario](https://docs.aws.amazon.com/lambda/latest/dg/example_lambda_Scenario_GettingStartedFunctions_section.html)

</details>

<details>
<summary>Explain how you would use cloud storage services within DevOps.</summary>

### Explain how you would use cloud storage services within DevOps.

**Definition:**

Object storage holds pipeline artifacts, Terraform state, and log archives as objects in a bucket. The CI job uploads and downloads them with the cloud API. The bucket is not the Git repo and not the container image registry.

**How it works:**

- **Artifacts.** The build stage writes a package or a test report to a bucket and later stages fetch that object by key. The key should include the commit SHA.
- **Templates.** CloudFormation can read a template from an S3 URL when you create a change set. A local template is uploaded to an S3 bucket in the account automatically if you submit it from disk.
- **State.** A remote Terraform state object in a locked bucket lets the whole team plan against the same state. Restrict who can read it; state can contain attribute values.
- **Logs.** A logging backend can retain objects longer than the node disk. Kubernetes still does not store those logs for you; the agent writes them out.

**When to use:**

Use object storage for blobs and history. Use a container registry for images. Use Git for source and pipeline config.

**Sources:**

- [Amazon S3 — What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)

</details>

### Containerization and Orchestration

<details>
<summary>What is Kubernetes and what problem does it solve?</summary>

### What is Kubernetes and what problem does it solve?

**Definition:**

Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services. It solves running many containers across machines: scheduling, restarting, rolling out changes, and giving them a stable network name.

**How it works:**

- You declare the desired state. A Deployment owns Pods created from a pod template. A Service selects those Pods by labels and exposes a port.
- Changing `.spec.template` starts a rolling update: new Pods are created and old Pods are terminated.
- DNS (CoreDNS in a typical cluster) watches Services. A Pod in namespace `my-ns` can resolve `my-service`; other namespaces use `my-service.my-ns`.
- The control plane does this continuously. Amazon EKS can run that control plane for you; the API is still Kubernetes.
- Scaling is a replica count. `kubectl autoscale deployment/my-nginx --min=1 --max=3` creates a HorizontalPodAutoscaler when a metrics source exists.

**Sources:**

- [Kubernetes concepts](https://kubernetes.io/docs/concepts/)
- [Kubernetes — Service](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)

</details>

<details>
<summary>How does a container differ from a virtual machine?</summary>

### How does a container differ from a virtual machine?

**Definition:**

A virtual machine is an operating system with its own kernel. A container is an isolated process plus the files it needs, and it shares the host kernel. Docker uses containers so the same image runs on a laptop, in CI, and in the cloud without installing the app’s dependencies on the host.

**How it works:**

- A VM includes hardware drivers, programs, and applications. Starting a VM only to isolate one process carries that overhead.
- Containers are self-contained, isolated from each other, independently removable, and portable. Several containers on one machine share one kernel, so you run more processes on less infrastructure.
- Cloud VMs often host a container runtime. One VM then runs many containers instead of one application per machine.
- Kubernetes schedules containers inside Pods on those nodes. The Pod is the Kubernetes object; the container is still the process.

**When to use:**

Use containers to package an application. Use a VM when you need a separate kernel, a different operating system, or a hard isolation boundary the threat model requires. In practice the VM is the node and containers are the workloads on it.

**Sources:**

- [Docker — What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)
- [Kubernetes — Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

</details>

<details>
<summary>What is the purpose of a Kubernetes pod?</summary>

### What is the purpose of a Kubernetes pod?

**Definition:**

A Pod is the smallest deployable unit you create and manage in Kubernetes. It is one or more containers that share storage and network and are scheduled together.

**How it works:**

- The usual case is one container per Pod. Kubernetes manages Pods, not standalone containers. A Deployment creates those Pods from a template when you need replicas.
- A multi-container Pod is for tightly coupled helpers that must share the Pod’s network namespace and volumes (a log sidecar is the logging-doc example). Do not put unrelated apps in one Pod to get more copies; raise the replica count instead.
- A Pod can also run init containers at startup. Ephemeral containers can be injected to debug a running Pod.
- Every node needs a container runtime so Pods can run there.
- Pods are mortal. A Service and a controller (Deployment) give you a stable name and a replacement when a Pod dies.

**Examples:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.14.2
```

**Sources:**

- [Kubernetes — Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

</details>

<details>
<summary>Explain the role of service discovery in container orchestration.</summary>

### Explain the role of service discovery in container orchestration.

**Definition:**

Service discovery lets a client find a workload after Pods are rescheduled and their IPs change. In Kubernetes the stable name is a Service, and DNS publishes it.

**How it works:**

- A Service selects Pods with a label selector and has a cluster IP and port. The IP stays with the Service while Pod IPs come and go.
- With cluster DNS (CoreDNS), a Service named `my-service` in namespace `my-ns` is `my-service` inside that namespace and `my-service.my-ns` from others. Those names resolve to the cluster IP.
- Named ports get SRV records. A port named `http` with TCP can be found with `_http._tcp.my-service.my-ns`.
- `ExternalName` Services are resolved only through this DNS path.
- Docker Swarm gives a service a name on the swarm’s network; tasks behind `--replicas` share that service. Clients use the service name, not a task IP.

**Sources:**

- [Kubernetes — Service](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Kubernetes — DNS for Services and Pods](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)
- [Docker Swarm services](https://docs.docker.com/engine/swarm/services/)

</details>

<details>
<summary>How can you scale containers using Docker Swarm or Kubernetes?</summary>

### How can you scale containers using Docker Swarm or Kubernetes?

**Definition:**

Scaling sets how many copies of a container should run. Swarm calls them replicas of a service. Kubernetes sets replicas on a Deployment, or a HorizontalPodAutoscaler changes that count from metrics.

**How it works:**

- **Swarm.** `docker service create --name my_web --replicas 3 nginx` asks for three tasks. The manager spreads them across nodes. `docker service scale my_web=5` changes the desired count. A task stays on the node it was assigned to. A global service runs one task on every node instead of a fixed count.
- **Kubernetes.** Set `spec.replicas` on the Deployment. `kubectl autoscale deployment/my-nginx --min=1 --max=3` creates an HPA. Autoscaling needs a metrics source; the command does not invent metrics.
- Scale stateless replicas first. A database does not become safe because you raised a replica count; it needs its own replication design.

**Examples:**

```console
docker service create --name my_web --replicas 3 nginx
docker service scale my_web=5
```

```shell
kubectl autoscale deployment/my-nginx --min=1 --max=3
```

**Sources:**

- [Docker Swarm — Scale the service](https://docs.docker.com/engine/swarm/swarm-tutorial/scale-service/)
- [Kubernetes — Horizontal Pod Autoscaling](https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/)
- [Kubernetes — Managing deployments](https://kubernetes.io/docs/concepts/workloads/management/)

</details>

### DevOps Practices and Methodologies

<details>
<summary>How does pair programming fit into DevOps culture?</summary>

### How does pair programming fit into DevOps culture?

**Definition:**

Pair programming is two people working on one change at the same time: one drives the keyboard, the other reviews the intent continuously. In DevOps it is a collaboration practice on a topic branch, not a substitute for the pipeline.

**How it works:**

- The pair shares one short-lived Git branch. The work stays one topic, which is what Git’s topic-branch workflow is for.
- They still push and let CI run the test stage. A pair does not merge because two people saw the code; the pipeline must pass.
- Pairing spreads how the deploy, the alert, and the runbook work, so operations knowledge is not locked in one person.
- Rotate driver and navigator. Keep the session on one change so the branch stays reviewable.

**When to use:**

Pair on production incidents, unfamiliar pipeline code, and security-sensitive changes. Solo work plus a pull-request review is enough for a well-understood edit.

**Sources:**

- [Git — Branching Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)

</details>

<details>
<summary>Can you describe the concept of a blameless postmortem?</summary>

### Can you describe the concept of a blameless postmortem?

**Definition:**

A blameless postmortem is a written review after an incident that asks how the system allowed the failure, not which person to punish. Google’s SRE guidance treats blameless postmortems as a way to learn from outages and to keep people willing to report them.

**How it works:**

- Record the timeline from metrics, logs, and the deploy that shipped (commit SHA, image digest). Prometheus and `kubectl logs` are inputs, not the document itself.
- State impact, detection, mitigation, and what will change: a new alert rule (`for` duration, a clearer `summary`), a test, or a rollback step.
- Assume people made reasonable decisions with the information they had. Fix the missing information (an alert, a dashboard, a runbook).
- Share the write-up. A private blame session does not update the pipeline.

**Sources:**

- [Google SRE — Postmortem culture](https://sre.google/sre-book/postmortem-culture/)
- [Prometheus — Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)

</details>

<details>
<summary>What is meant by “systems thinking” in the context of DevOps?</summary>

### What is meant by “systems thinking” in the context of DevOps?

**Definition:**

Systems thinking looks at the path from a commit to a running service, including handoffs, queues, and feedback, instead of optimizing one team’s local step. A faster build does not help if deploys wait a week for a manual window.

**How it works:**

- Follow one change across Git, the pipeline stages, the deploy, and the production signal (Prometheus series, logs). The constraint is the slowest or riskiest step on that path.
- Improve the constraint: parallel jobs in a stage, a smaller test, or a rollback that does not need a meeting.
- Watch the system under failure. Prometheus is designed so you can query it during an outage because each server stores data locally.
- Local wins that push pain downstream (skipping tests, console edits that drift from Terraform) are not improvements. Drift and a red test stage are system signals.

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)
- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)

</details>

<details>
<summary>How can DevOps practices reduce technical debt?</summary>

### How can DevOps practices reduce technical debt?

**Definition:**

Technical debt is a shortcut that makes the next change more expensive. DevOps reduces it by making the shortcut visible in version control and in the pipeline, and by paying it down in small merges instead of a rewrite project.

**How it works:**

- **Snowflakes.** Console edits drift from Terraform or CloudFormation. Detecting drift and applying the configuration again removes one-off servers.
- **Untested paths.** A missing test becomes a test-stage job. The next change cannot reintroduce the bug quietly.
- **Big batches.** Topic branches that merge often keep the diff small, so a cleanup rides with the feature.
- **Images and manifests.** Rebuilding from a Dockerfile and a Deployment beats patching a long-lived VM by hand.
- Debt that is a deliberate choice stays in the backlog with a trigger (“when we next touch billing”). The pipeline does not delete a product decision.

**Sources:**

- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)
- [Git — Branching Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows)
- [Docker — What is an image?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)

</details>

<details>
<summary>What is ChatOps and how can it support DevOps initiatives?</summary>

### What is ChatOps and how can it support DevOps initiatives?

**Definition:**

ChatOps puts operational notices and a narrow set of commands in the team chat room so deploys, alerts, and incident notes are visible to the people who need them. The pipeline and the orchestrator remain the systems that actually change production.

**How it works:**

- The CI job posts a message when a deploy stage starts and finishes: commit SHA, environment, pipeline URL. Slack incoming webhooks are one way to post that message.
- Alertmanager routes a firing alert to the same channel. The message should include the alert `summary` and a link to the dashboard or runbook.
- A chat command may start a job the CI system already allows (a manual GitLab job, a Jenkins build). The token for that command is a CI credential with a small scope, stored outside the chat log.
- Decisions and timelines written in the channel become the raw material for the postmortem. Chat is not the source of truth for manifests; Git is.

**When to use:**

Use chat for broadcast and incident coordination. Keep apply, rollback, and secret access in the pipeline and the secrets manager, where they are reviewed and audited.

**Sources:**

- [Slack — Sending messages using incoming webhooks](https://docs.slack.dev/messaging/sending-messages-using-incoming-webhooks)
- [Prometheus — Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)

</details>

### Advanced DevOps Concepts

<details>
<summary>What are microservices and how do they influence DevOps?</summary>

### What are microservices and how do they influence DevOps?

**Definition:**

Microservices split an application into separately deployed processes, each with its own pipeline and release. DevOps feels this as many pipelines, many dashboards, and a network between services instead of one function call.

**How it works:**

- Each service is a container image and, on Kubernetes, a Deployment plus a Service. Teams ship by changing that service’s image without rebuilding the others.
- Discovery is DNS on the Service name (`my-service.my-ns`), not a hard-coded Pod IP.
- The pipeline is per service: build, test, deploy that image. A change in one repo should not require a lockstep release unless the API contract changed.
- Observability has to cross services. OpenTelemetry traces and Prometheus metrics with a service label show which hop failed. Istio can collect metrics, logs, and traces for mesh traffic without changing every call site.
- More services mean more on-call surface. Start with a boundary that has its own data and its own reason to deploy.

**Sources:**

- [Kubernetes — Service](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Istio — What is Istio?](https://istio.io/latest/docs/overview/what-is-istio/)
- [OpenTelemetry — What is OpenTelemetry?](https://opentelemetry.io/docs/what-is-opentelemetry/)

</details>

<details>
<summary>Can you outline the benefits and challenges of a microservices architecture?</summary>

### Can you outline the benefits and challenges of a microservices architecture?

**Definition:**

Microservices let teams release a service on its own schedule. The cost is distributed failure, versioned APIs, and operational load that a single process did not have.

**How it works:**

Benefits that the platform actually provides:

- Independent deploy: one Deployment rolling update does not rebuild every other service.
- Fit-for-purpose scale: `kubectl autoscale` or a Swarm replica count applies to the busy service.
- Isolated dependencies: each image carries its own runtime, which is Docker’s packaging model.

Challenges you should plan for:

- A Service IP and DNS name are required because Pod IPs change.
- Partial failure: the caller needs timeouts and the mesh or the client needs a visible error rate. Istio can add retries and fault injection; those are explicit rules, not a default that hides bugs.
- Data is no longer one transaction. A schema change is a Liquibase changelog for that service’s database, coordinated with the app deploy.
- Many pipelines. GitLab or Jenkins must stay green per service or you lose the independent release.

**When to use:**

Split out a service when a team needs to deploy it on a different cadence. Keep one deployable unit while one team ships one product.

**Sources:**

- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [Istio — What is Istio?](https://istio.io/latest/docs/overview/what-is-istio/)
- [Docker — What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)

</details>

<details>
<summary>How does feature flagging work and why is it useful in DevOps?</summary>

### How does feature flagging work and why is it useful in DevOps?

**Definition:**

A feature flag is a runtime check that turns a code path on or off without a new deploy. It separates “the commit is on main” from “users see the behavior,” which is what lets a team merge small and release when ready.

**How it works:**

- The application asks a flag provider for a boolean or a variant, keyed by user, tenant, or environment. The default path stays the old behavior.
- The pipeline deploys the image with the flag off. After the Deployment is healthy, you turn the flag on for a subset (a canary of users) and watch the Prometheus series for that path.
- Turning the flag off is the rollback when the new path misbehaves and you do not want another rolling update.
- Store flag definitions outside the image. A flag left on forever is unfinished cleanup; delete the branch in code once the flag is fully on.
- OpenFeature defines a vendor-neutral API for this evaluation so the app is not hard-wired to one flag vendor.

**When to use:**

Use a flag for a behavior change you may need to hide quickly, or for a trunk commit that is not ready for every user. Do not use a flag as a substitute for a schema migration that cannot be toggled.

**Sources:**

- [OpenFeature specification](https://openfeature.dev/specification/)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)

</details>

<details>
<summary>What is a service mesh and how does it fit into a DevOps environment?</summary>

### What is a service mesh and how does it fit into a DevOps environment?

**Definition:**

A service mesh is a dedicated layer for service-to-service traffic: routing, mutual TLS, and telemetry, without rewriting each service. Istio is that layer. It sits on top of an existing distributed application.

**How it works:**

- The control plane (`istiod`) configures the data plane. In sidecar mode, an Envoy proxy next to each workload handles that workload’s traffic. In ambient mode, a node-level ztunnel proxy does it.
- Istio can require mutual TLS between services (`ISTIO_MUTUAL` on a DestinationRule), split traffic for a canary, and emit metrics, logs, and traces for mesh traffic.
- The application Deployment stays the source of the image. The mesh does not build or roll out that image; the CI pipeline and Kubernetes do. Argo CD or the pipeline syncs manifests; Istio policies are manifests too.
- Add a mesh when you already have many services and need uniform mTLS or weighted routing. It is extra proxies and config to operate.

**When to use:**

Use a mesh for cross-service security and traffic policy you do not want to code into every client. Skip it for a handful of services that call each other through ordinary Kubernetes Services.

**Sources:**

- [Istio — What is Istio?](https://istio.io/latest/docs/overview/what-is-istio/)
- [Istio — Data plane modes](https://istio.io/latest/docs/overview/dataplane-modes/)
- [Istio — Destination Rule](https://istio.io/latest/docs/reference/config/networking/destination-rule/)

</details>

<details>
<summary>Describe the concept of immutable infrastructure.</summary>

### Describe the concept of immutable infrastructure.

**Definition:**

Immutable infrastructure replaces a running instance instead of changing it in place. You ship a new image or a new template revision. You do not SSH in and patch packages on a long-lived machine.

**How it works:**

- A Docker image is built from a Dockerfile and identified by its content. The pipeline deploys a new digest. The old container is stopped; it is not upgraded with a package manager inside the running process.
- On Kubernetes, a Deployment rolling update creates new Pods from the updated template and terminates the old Pods.
- For cloud resources, you change the Terraform configuration or the CloudFormation template and apply. Replacing a resource is a planned action in the plan or change set, not a manual edit that becomes drift.
- Logs and state live outside the instance (a logging backend, a remote Terraform state object), because the instance is disposable.
- Rollback is deploying the previous digest or template, not reversing a shell history on the box.

**When to use:**

Use immutable releases for application nodes and containers. Databases and other systems of record still need careful in-place schema changes (a Liquibase changelog), because you cannot throw away the data volume.

**Sources:**

- [Docker — What is an image?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)

</details>

### Soft Skills and Culture

<details>
<summary>How would you foster a DevOps culture in a traditionally siloed organization?</summary>

### How would you foster a DevOps culture in a traditionally siloed organization?

**Definition:**

A siloed organization hands work over a wall: development finishes, operations deploys. Fostering DevOps means one team owns a service from the commit to the alert, with a pipeline both sides can change.

**How it works:**

- Start with one product path, not a company-wide rename. Put the deploy steps in a `Jenkinsfile` or `.gitlab-ci.yml` that both groups review.
- Give the people who write the service access to its logs and the Prometheus dashboard, and give the people who get paged a say in the test stage.
- Replace ticketed server builds with a Terraform plan or a CloudFormation change set so infrastructure changes are reviewed like code.
- After the first production incident, write a blameless postmortem and add the missing check to the pipeline. That is the culture: the system changes, not the scapegoat.
- Do not declare the culture done because a tool was installed. The signal is that a small change can ship without a handoff meeting.

**Sources:**

- [GitLab CI/CD](https://docs.gitlab.com/ci/)
- [Google SRE — Postmortem culture](https://sre.google/sre-book/postmortem-culture/)
- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)

</details>

<details>
<summary>What soft skills are important for a DevOps engineer?</summary>

### What soft skills are important for a DevOps engineer?

**Definition:**

The work crosses build, deploy, and incident response, so the useful soft skills are the ones that keep a change understandable to the people who will operate it.

**How it works:**

- **Write it down.** A pipeline stage, an alert annotation, and a postmortem are documents other people act on. A `summary` on a Prometheus alert is a soft skill expressed as config.
- **Review in public.** Explain a `terraform plan` or a failing test stage in the merge request so the next person can deploy without you.
- **Stay calm on a timeline.** During an outage, state what you know, what you are rolling back, and what you still need. The postmortem depends on that record.
- **Ask what the other role needs.** A developer needs a fast red build. An on-call engineer needs a rollback. Design the pipeline for both.
- **Learn the next tool from its docs,** not from a slide. Docker, Kubernetes, and Terraform change; the habit is reading the page you will cite.

**Sources:**

- [Google SRE — Postmortem culture](https://sre.google/sre-book/postmortem-culture/)
- [Prometheus — Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)

</details>

<details>
<summary>How do you communicate and collaborate effectively within a DevOps team?</summary>

### How do you communicate and collaborate effectively within a DevOps team?

**Definition:**

Effective collaboration in DevOps is a shared, reviewable record of the change: the branch, the pipeline run, and the alert. Meetings fill gaps; they do not replace that record.

**How it works:**

- Use a topic branch and a merge request so discussion attaches to the diff. Git is built for that kind of short-lived branch.
- Let the pipeline speak first. A red test stage is the status. Link the job instead of pasting a local log that nobody else can reproduce.
- During a deploy, post the commit and the environment to the team channel. During an incident, keep one timeline: what changed, what you rolled back, what the graph showed.
- Write the runbook and the alert `summary` for the person who is on call next, not for yourself.
- Agree who can apply to production (protected branch, CI role). Collaboration includes a clear permission, not everyone clicking in the console.

**Sources:**

- [Git — Branching Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Slack — Sending messages using incoming webhooks](https://docs.slack.dev/messaging/sending-messages-using-incoming-webhooks)

</details>

<details>
<summary>Can you explain the importance of empathy in a DevOps context?</summary>

### Can you explain the importance of empathy in a DevOps context?

**Definition:**

Empathy in DevOps is designing the pipeline and the pager for the person who will use them at a bad time. A deploy that only the author understands will fail when that author is offline.

**How it works:**

- Read the on-call view: the alert name, the `summary` annotation, and whether the dashboard shows the error rate. If you would not know what to do, the next person will not either.
- A blameless postmortem assumes the previous actor had incomplete information. That stance keeps the next incident report honest.
- When you add a stage, consider who waits on it. A long test stage with no parallel jobs is a tax on every developer. GitLab runs jobs in one stage in parallel so the wait stays shorter.
- Ask operations what a safe rollback looks like before you automate the deploy. Ask development which log fields they need before you drop them.

**Sources:**

- [Google SRE — Postmortem culture](https://sre.google/sre-book/postmortem-culture/)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Prometheus — Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)

</details>

<details>
<summary>How do you stay up-to-date with DevOps practices and tools?</summary>

### How do you stay up-to-date with DevOps practices and tools?

**Definition:**

Stay current by reading the release notes and concept pages of the tools you run, and by trying the change in a non-production pipeline before you adopt it.

**How it works:**

- Track the projects you depend on: Docker docs, Kubernetes release notes, Terraform CLI, GitLab CI, Jenkins Pipeline. A blog summary is a pointer; the behavior you automate comes from the doc page.
- When a Kubernetes feature is alpha (for example splitting stdout and stderr logs behind the `PodLogsQuerySplitStreams` feature gate), leave it off until the docs mark it stable.
- Upgrade one tool on a branch. Run the existing test and deploy stages. `terraform plan` shows what the new version will change before you apply it.
- Share a short note: what changed, what you tried, what you will not adopt yet.
- Depth beats a tour of every new product. Master the pipeline, the orchestrator, and the metrics you already page on.

**Sources:**

- [Kubernetes — Logging architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [Docker — What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)

</details>

### DevOps Challenges and Problem-Solving

<details>
<summary>How do you address resistance to DevOps transformation within a team or organization?</summary>

### How do you address resistance to DevOps transformation within a team or organization?

**Definition:**

Resistance is usually a specific fear: a bad deploy, a lost role, or a tool that adds work. Address the fear with a smaller change and a rollback, not with a slogan.

**How it works:**

- Ask what broke the last time a release was rushed. Put that check in the test stage so the pipeline is the protection, not the threat.
- Keep a rollback: the previous image, the previous CloudFormation stack state, or the feature flag off. People accept automation when they can undo it.
- Do not start by renaming teams. Start with one service whose owners agree. Show that a small change ships from a pipeline file in Git.
- Invite the skeptical person to own the alert or the plan review. Losing a manual gate feels better when they still approve the change set.
- If the objection is “the tool is unfamiliar,” pair on one Jenkinsfile or one Terraform module and leave the docs link in the repo.

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)

</details>

<details>
<summary>What steps would you take to resolve a conflict between development and operations teams?</summary>

### What steps would you take to resolve a conflict between development and operations teams?

**Definition:**

A development-versus-operations conflict is two goals that both matter: ship the change, and keep the service up. Resolve it by writing both goals into the pipeline and the rollback.

**How it works:**

1. State the concrete dispute (a Friday deploy, a manual SSH session, an immutable image). Leave motives out of the notes.
2. Look at the signal. The Prometheus error rate, the last pipeline, and the deploy diff show whether the risk is real.
3. Propose a mechanism both sides can see: a test stage that encodes the operational check, a canary or a blue-green cutover, and a rollback to the previous image digest or Deployment template.
4. Agree who may apply to production. A protected branch and a CI role beat a ticket queue and a shared password.
5. After the release, write what happened. If the fear was right, add the missing alert. If it was not, remove the extra gate.

**Sources:**

- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [Google SRE — Postmortem culture](https://sre.google/sre-book/postmortem-culture/)

</details>

<details>
<summary>How would you deal with a failed deployment?</summary>

### How would you deal with a failed deployment?

**Definition:**

A failed deployment is a release that did not become healthy. Stop the bleed by returning traffic to the last good artifact, then fix forward from a green pipeline.

**How it works:**

1. Confirm impact from the Service’s error rate or the `up` metric, and from `kubectl logs` on the new Pods.
2. Roll back. On Kubernetes, restore the previous pod template so the Deployment rolls again. On a blue-green cutover, point the router back at the idle environment. On Lambda, point the alias at the previous version.
3. If CloudFormation’s stack update failed, let it roll back to the last working state. Do not keep retrying a bad template.
4. Freeze the pipeline’s deploy stage until the test that would have caught this exists, or the flag is off.
5. Write the timeline while it is fresh. The next change is a test or an alert, not a longer deploy window.

**Sources:**

- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)
- [CloudFormation — Updating a stack with a change set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html)
- [Kubernetes — Logging architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

</details>

<details>
<summary>Describe how you would handle a significant system outage.</summary>

### Describe how you would handle a significant system outage.

**Definition:**

Handle an outage by restoring the service first, then explaining it. Prometheus is meant to be queryable during failure because each server stores samples locally and does not depend on remote storage.

**How it works:**

1. Say who is coordinating and open one timeline in the incident channel.
2. Check the last deploy, the firing alerts (`up == 0`, error ratio), and recent logs. Decide whether this is a bad release or a dependency.
3. Mitigate: roll back the image, shift the canary weight to zero, or fail over to the blue environment. Prefer a known action from the runbook over new debugging on the live path.
4. Tell users what is affected and what you are doing. Update when the mitigation lands.
5. After recovery, write a blameless postmortem: impact, detection gap, and the alert or test you will add.

**Sources:**

- [Prometheus overview](https://prometheus.io/docs/introduction/overview/)
- [Google SRE — Postmortem culture](https://sre.google/sre-book/postmortem-culture/)
- [Slack — Sending messages using incoming webhooks](https://docs.slack.dev/messaging/sending-messages-using-incoming-webhooks)

</details>

<details>
<summary>What is your approach to root cause analysis in a DevOps context?</summary>

### What is your approach to root cause analysis in a DevOps context?

**Definition:**

Root cause analysis finds the change in the system that allowed the incident, using the timeline of deploys, configs, and signals. It is the content of a blameless postmortem, not a search for a person at fault.

**How it works:**

- Start from the user-visible failure and walk backward: alert time, deploy time, commit SHA, `terraform apply` or stack update, and the first bad log line.
- Separate trigger and condition. The trigger may be a new image. The condition may be a missing test, a flag defaulted on, or drift a console edit introduced.
- Check whether the same apply is idempotent and whether state matches the repo. A second apply that wants to change resources is a clue.
- Write one sentence: “The service returned errors because X, and we did not catch it because Y.” Y becomes a pipeline job or an alert rule with a `for` duration.
- Stop when you have a change you can make. A deeper cause you cannot act on does not belong in the action list.

**Sources:**

- [Google SRE — Postmortem culture](https://sre.google/sre-book/postmortem-culture/)
- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)
- [Prometheus — Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)

</details>

### DevOps Case Studies and Experience

<details>
<summary>Can you share an example where DevOps helped to improve software delivery?</summary>

### Can you share an example where DevOps helped to improve software delivery?

**Definition:**

Interviewers want a before-and-after on one delivery path. Use a real change you shipped. The sample below is a pattern, not a claim about a specific employer.

**How it works:**

Sample you can retell with your own dates and service name:

- Before: a release was a checklist on a shared VM. A missed step meant Friday deploys failed in production only.
- Change: a Jenkinsfile or `.gitlab-ci.yml` with build, test, and deploy stages. The deploy stage shipped one image digest. A Deployment rolling update replaced Pods.
- After: the test stage caught the regression that used to land on users. Rollback was the previous digest, not a shell history.

State the metric you actually had (pipeline minutes, failed releases per month). Do not invent a percentage.

**Sources:**

- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)

</details>

<details>
<summary>Describe a DevOps project you worked on and the value it added.</summary>

### Describe a DevOps project you worked on and the value it added.

**Definition:**

Describe one project you owned: the tool, the user of the tool, and what became possible. The sample is a Terraform-plus-pipeline project. Swap in your stack.

**How it works:**

- Problem: test environments were clicked together in the console and drifted from production.
- Project: a Terraform root module per environment, `terraform plan` on the merge request, `terraform apply` from the protected branch only. Application deploys stayed a separate pipeline that pushed an image.
- Value: a new environment was the same module with different variables. Drift showed up as a plan instead of a surprise during an incident.
- Say what you personally wrote (the module, the Jenkins stage, the alert rule) and what you would redo.

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [Terraform — Resource drift](https://developer.hashicorp.com/terraform/tutorials/state/resource-drift)

</details>

<details>
<summary>What lessons have you learned from a DevOps implementation that didn’t go as planned?</summary>

### What lessons have you learned from a DevOps implementation that didn’t go as planned?

**Definition:**

A useful lesson names the mechanism that failed, not “we needed more culture.” Tie it to a tool behavior you can point at.

**How it works:**

Sample lesson, retell only if it matches your history:

- We automated deploy before the test stage was trusted. A green pipeline still shipped a broken migration because the database step was a manual SQL file outside Liquibase.
- Fix: the changelog is in Git, `liquibase update` runs in the pipeline, and every changeset has a rollback or is explicitly marked as not reversible.
- Second lesson: a Kubernetes alpha feature (log stream split behind a feature gate) is not a production dependency. Wait until the docs say it is stable.
- The interviewer wants what you changed afterward. End on the new stage or the deleted tool, not on the blame.

**Sources:**

- [Liquibase — formatted SQL changelog](https://docs.liquibase.com/secure/user-guide-5-1/sql-changelog-example)
- [Kubernetes — Logging architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

</details>

<details>
<summary>How have you contributed to reducing release times or improving release quality?</summary>

### How have you contributed to reducing release times or improving release quality?

**Definition:**

Release time drops when the path from commit to artifact has fewer manual steps and the test stage is parallel. Quality improves when the thing you deploy is the thing you tested.

**How it works:**

Talk about a change you made, for example:

- Split a serial test script into GitLab jobs in one `test` stage so they run in parallel. Deploy still waits for every job in that stage.
- Build the image once, tag it with the commit SHA, and deploy that tag. Stop rebuilding in the deploy job.
- Add a `for: 5m` alert on `up == 0` so a bad release pages you before users file tickets. That is quality after the deploy, which the pipeline alone does not see.

Quote your own before/after if you measured it. If you did not measure it, say what the pipeline graph showed (stage duration), not a rounded success rate.

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Docker — What is an image?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)
- [Prometheus — Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)

</details>

<details>
<summary>Share an experience where you automated a significant manual process.</summary>

### Share an experience where you automated a significant manual process.

**Definition:**

Pick a process with a checklist and a person who had to be awake. Show the file that replaced the checklist and how you knew the automation matched the old steps.

**How it works:**

Sample, grounded in the Terraform workflow:

- Manual process: an operator created an instance, a security group, and DNS from the console, then hoped the next environment matched.
- Automation: those resources became Terraform configuration. `terraform plan` showed the diff. `terraform apply` created them. A second apply reported no changes, which is how you know the run is idempotent.
- You kept a rollback: the previous configuration in Git, not a wiki page.
- Mention the edge you did not automate (a data load, a vendor console) so you do not over-claim.

**Sources:**

- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)

</details>

### DevOps Trends and Innovation

<details>
<summary>Where do you see the DevOps movement heading in the next few years?</summary>

### Where do you see the DevOps movement heading in the next few years?

**Definition:**

The direction already visible in current tools is Git as the source of the desired state, platforms that run the control plane for you, and telemetry that is not locked to one vendor.

**How it works:**

- GitOps: Argo CD treats a Git repo as the desired state of a Kubernetes application and syncs the cluster to it. CI builds the image; Git records the digest to deploy.
- Managed control planes (EKS and the same idea on other clouds) and Lambda remove undifferentiated server work. You still own the pipeline, IAM, and the incident.
- OpenTelemetry is the common way to emit traces, metrics, and logs without picking the backend first. Prometheus remains the pull-based time-series store many of those metrics land in.
- Supply-chain checks stay in the pipeline: the image you scanned is the image you deploy. That pressure increases; it does not require a new philosophy name.

**Sources:**

- [Argo CD](https://argo-cd.readthedocs.io/en/stable/)
- [OpenTelemetry — What is OpenTelemetry?](https://opentelemetry.io/docs/what-is-opentelemetry/)
- [Amazon EKS user guide — Kubernetes concepts](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-concepts.html)

</details>

<details>
<summary>How do you think AI and machine learning will impact DevOps?</summary>

### How do you think AI and machine learning will impact DevOps?

**Definition:**

AI tools can draft a pipeline file, summarize a log, or suggest a PromQL expression. They do not replace the apply, the test stage, or the rollback. Anything an assistant writes still has to match the official behavior of Terraform, Kubernetes, or GitLab CI.

**How it works:**

- Use a draft Jenkinsfile or `.gitlab-ci.yml` as a starting point, then run it on a branch. The runner is the test of the draft, not the chat transcript.
- A model can cluster log lines during an incident. You still confirm with `kubectl logs` and the Prometheus series before you roll back.
- Do not grant an assistant your CI role, cloud keys, or Vault token. A generated `terraform apply` is a plan a human reviews.
- Prometheus itself does not ship an AI control loop. Alerting is still an `expr` you wrote, a `for` duration, and Alertmanager routes.
- If a vendor adds a model on top of traces, read that product’s docs before you describe it in an interview. Do not claim a feature you have not run.

**Sources:**

- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)
- [Terraform — Apply](https://developer.hashicorp.com/terraform/tutorials/cli/apply)
- [Prometheus — Alerting rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)

</details>

<details>
<summary>What is GitOps and how might it shape future DevOps practices?</summary>

### What is GitOps and how might it shape future DevOps practices?

**Definition:**

GitOps uses a Git repository as the source of truth for the desired state of a system. Argo CD is a GitOps continuous delivery tool for Kubernetes: it compares the live cluster to the manifests in Git and syncs them.

**How it works:**

- You commit the Kubernetes manifests (or the Helm/Kustomize revision) that should be running. Argo CD is a controller that reports when live state drifts from that target and can sync automatically (`argocd app set <APPNAME> --sync-policy automated`) or on demand.
- CI still builds and tests the image. A common split is a second Git repo for manifests, which Argo CD recommends keeping apart from application source. The pipeline commits the new image digest there; Argo CD deploys it.
- Rollback is a Git revert, then a sync. The cluster is not patched by hand.
- This does not remove IAM, secrets, or database migrations. Those stay Vault and Liquibase (or equivalent), not unstructured edits on the cluster.

**Sources:**

- [Argo CD](https://argo-cd.readthedocs.io/en/stable/)
- [Argo CD — Automated sync](https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/)
- [Argo CD — CI automation](https://argo-cd.readthedocs.io/en/stable/user-guide/ci_automation/)

</details>

<details>
<summary>Can you discuss the emerging concept of NoOps?</summary>

### Can you discuss the emerging concept of NoOps?

**Definition:**

NoOps is a slogan for “the platform runs the servers.” It is not a product in Kubernetes, Terraform, or GitLab. The closest AWS feature is Lambda: you supply code, and Lambda handles capacity, OS patching, and scaling.

**How it works:**

- Lambda still needs a pipeline, an IAM execution role, a deployment package, tests, logs, and alarms. An invocation failure is your incident.
- EKS removes control-plane installation. You still write Deployments, set Pod security, and roll out images.
- What actually goes away is ticketed machine builds and SSH patching, if you stop doing those and use immutable images or functions instead.
- What does not go away: schema changes, on-call, cost, and identity. Calling that NoOps oversells the platform.

**When to use:**

Use the concrete service (Lambda, a managed control plane) in the interview answer. Use “NoOps” only to say which toil you removed and which responsibility remains.

**Sources:**

- [AWS Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-functions.html)
- [Amazon EKS user guide — Kubernetes concepts](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-concepts.html)

</details>

<details>
<summary>How could blockchain technology potentially be used in DevOps?</summary>

### How could blockchain technology potentially be used in DevOps?

**Definition:**

Docker, Kubernetes, Jenkins, GitLab CI, and Terraform do not use a blockchain to build, deploy, or record releases. Git already stores content-addressed objects: a commit points at a tree, and the history is a chain of hashes in the repository you already operate.

**How it works:**

- The audit trail interviewers sometimes attribute to a blockchain is, in these tools, Git history plus CI logs plus Terraform state or a CloudFormation stack event. Those are the records you can actually show.
- A separate distributed ledger would be another system to run, permission, and back up. None of the deploy flows in the Jenkins Pipeline, GitLab CI, Argo CD, or CloudFormation docs require one.
- If a vendor product writes an audit hash to a chain, describe that product from its own docs. Do not add it to a design that only needs `git log` and the pipeline URL.
- For supply chain, ship the image digest you built and scanned. The digest is a hash of the image, recorded in the registry and in the manifest GitOps commits.

**Sources:**

- [Git — Git Objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects)
- [Argo CD](https://argo-cd.readthedocs.io/en/stable/)
- [Docker — What is an image?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)

</details>

### DevOps for Databases

<details>
<summary>How do you apply DevOps practices to database management?</summary>

### How do you apply DevOps practices to database management?

**Definition:**

Database DevOps means schema changes are reviewed, versioned, and applied by the same pipeline as the application, instead of a one-off SQL session on production.

**How it works:**

- Store a changelog in Git. Liquibase tracks changesets and applies only those that have not run. `liquibase update --changelog-file=changelog.sql` deploys pending changesets.
- The pipeline runs that command against a test database in the test stage, then against production in the deploy stage, using credentials from Vault or CI variables.
- The application image and the changelog that it expects ship together. A Deployment rolling update does not migrate data by itself.
- People still review destructive changes. `diff` and `diff-changelog` can show drift between databases, but Liquibase’s own guidance says diff is a poor fit for large data and for test rows that must not be copied to production.

**Sources:**

- [Liquibase — SQL changelog example](https://docs.liquibase.com/secure/user-guide-5-1/sql-changelog-example)
- [Liquibase — Update command](https://docs.liquibase.com/secure/reference-guide-5-1/init-update-and-rollback-commands/update)

</details>

<details>
<summary>What are the considerations for database version control?</summary>

### What are the considerations for database version control?

**Definition:**

Version-control the change, not a dump of the whole database. Liquibase does that with a changelog of changesets. Each changeset has an author and an id and is recorded when it runs so it is not applied twice.

**How it works:**

- One changelog file (SQL, XML, YAML, or JSON) lives next to the service that owns the schema. Review it in the merge request like application code.
- Changesets are ordered. Liquibase stores which ones ran. Editing a changeset that already ran in production does not rewrite history; add a new changeset.
- Include a `--rollback` when the change can be reversed (`drop table` for a create). Data fixes and destructive drops often cannot be rolled back safely; say so in the review.
- Do not commit passwords or production data. Connection settings come from CI variables.
- `generate-changelog` can snapshot an existing database when you adopt the tool. After that, new changes are hand-written changesets, not repeated dumps. Diff output can miss differences that are still valid, such as rows inserted in a different order.

**Examples:**

```sql
--liquibase formatted sql

--changeset app:person_table
create table person (
  id int primary key,
  name varchar(50) not null
);
--rollback drop table person;
```

**Sources:**

- [Liquibase — SQL format](https://docs.liquibase.com/secure/user-guide-5-1/sql-changelog-example)

</details>

<details>
<summary>Can you discuss the tools and strategies for database CI/CD?</summary>

### Can you discuss the tools and strategies for database CI/CD?

**Definition:**

A database pipeline applies a reviewed changelog to a disposable database in CI, then applies the same changelog to production on release. Liquibase’s `update` command is the apply step.

**How it works:**

- **Pull request.** Run `liquibase update` against an empty or migrated test database so a bad changeset fails the test stage. GitLab runs that job before deploy.
- **Artifact.** The changelog ships with the commit that contains the application code expecting the new schema. Expand-contract when you cannot change both at once: add a nullable column, deploy the app, backfill, then enforce the constraint in a later changeset.
- **Production.** The deploy job uses a narrow database credential. It does not use a shared admin shell.
- **Drift.** `diff` compares two databases. Use it to find an out-of-band change, then turn that into a changeset. Do not let diff copy test data into production; Liquibase documents that data diffs are a bad fit for that.

**Examples:**

```bash
liquibase update --changelog-file=changelog.sql
```

**Sources:**

- [Liquibase — Update command](https://docs.liquibase.com/secure/reference-guide-5-1/init-update-and-rollback-commands/update)
- [GitLab CI/CD pipelines](https://docs.gitlab.com/ci/pipelines/)

</details>

<details>
<summary>How does database schema change management fit into DevOps?</summary>

### How does database schema change management fit into DevOps?

**Definition:**

Schema change management is the ordered, reviewed set of changesets that move a database from one revision to the next. It sits in the deploy stage beside the application image, because the code and the schema have to match.

**How it works:**

- A changeset is one unit: create a table, add a column, or a data fix, with an id and an author. Liquibase applies pending ones in order and records them.
- Backward-compatible changes go out first (new column, new table). The app deploy follows. A drop or a rename waits until no running version reads the old shape. That is how you avoid coupling a rolling update to a destructive migration: old Pods are still running during the rollout.
- Failed updates stop the pipeline. You do not “click around” to half-apply the file.
- Immutable application servers do not imply an immutable database. You migrate the schema; you do not replace the data volume with an empty one.

**Sources:**

- [Liquibase — SQL format](https://docs.liquibase.com/secure/user-guide-5-1/sql-changelog-example)
- [Kubernetes — Rolling update](https://kubernetes.io/docs/tasks/run-application/update-deployment-rolling/)

</details>

<details>
<summary>What is the significance of database release automation?</summary>

### What is the significance of database release automation?

**Definition:**

Database release automation applies the changelog with the application release so production is not a different set of SQL than the set CI already ran. Liquibase `update` is repeatable: changesets that already ran are skipped.

**How it works:**

- The deploy job runs `liquibase update` with the changelog from that commit, then rolls the application to the image from that commit.
- Automation removes missed manual steps and makes the release idempotent if the job is retried.
- Rollback is explicit. A changeset can carry `--rollback`. `rollback-one-changeset` reverts one changeset when you pass its author, id, and path; it is not a magic undo of every data change.
- The significance for DevOps is the same as for Terraform: the desired change is in Git, the tool converges the target, and a second run does not invent a second copy of the change.

**Examples:**

```bash
liquibase update --changelog-file=changelog.sql
```

**Sources:**

- [Liquibase — Update command](https://docs.liquibase.com/secure/reference-guide-5-1/init-update-and-rollback-commands/update)
- [Liquibase — rollback-one-changeset](https://docs.liquibase.com/secure/reference-guide-5-1/init-update-and-rollback-commands/rollback-one-changeset)

</details>
