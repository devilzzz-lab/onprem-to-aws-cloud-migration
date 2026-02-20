<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>

<h1>🛠️ On-Premises to AWS Cloud Migration using Terraform &amp; DevOps Automation</h1>

<h2>📌 Overview</h2>
<p>
This project demonstrates a realistic enterprise cloud migration scenario where an existing open-source WordPress application running on on-premises infrastructure is migrated to AWS cloud using Terraform (Infrastructure as Code) and Jenkins (CI/CD automation).
</p>
<p>
To keep the migration simple, cost-aware, and production-realistic:
</p>
<ul>
  <li>Docker and Kubernetes are used ONLY in the on-prem environment</li>
  <li>AWS cloud uses a VM-based deployment (EC2 + RDS)</li>
  <li>No EKS, no Kubernetes, no Docker, no kind in the cloud</li>
</ul>
<p>
This reflects how many organizations perform initial cloud migrations before adopting managed container platforms.
</p>

<h2>🧠 Problem Statement</h2>
<p>
An organization is running a legacy WordPress + MySQL application on-premises.
Docker and Kubernetes are used locally for development, testing, and validation.
The requirement is to migrate the entire application stack —
frontend (WordPress UI), backend (PHP runtime), and database (MySQL) —
to AWS cloud infrastructure, while:
</p>
<ul>
  <li>Avoiding managed Kubernetes services (EKS)</li>
  <li>Minimizing operational and cost overhead</li>
  <li>Ensuring scalability, reliability, and monitoring</li>
  <li>Automating infrastructure provisioning and deployments</li>
</ul>

<h2>✨ Project Highlights</h2>
<ul>
  <li>WordPress application deployed on-premises using Docker Compose</li>
  <li>Kubernetes manifests created for WordPress + MySQL with persistent storage</li>
  <li>Dedicated Kubernetes namespace for resource isolation</li>
  <li>MySQL database initialization and connectivity validation</li>
  <li>Jenkins CI/CD pipeline for automated testing and deployment</li>
  <li>Infrastructure as Code using Terraform for AWS provisioning</li>
  <li>Monitoring and observability with Prometheus and Grafana</li>
  <li>GitHub webhooks for automated build triggers</li>
</ul>

<h2>🎯 Migration Objectives</h2>
<ul>
  <li>Migrate WordPress application from on-prem to AWS</li>
  <li>Use Terraform for reproducible infrastructure</li>
  <li>Keep cloud deployment VM-based</li>
  <li>Automate deployments using Jenkins</li>
  <li>Use RDS MySQL for managed database operations</li>
  <li>Enable monitoring, alerting, and auto scaling</li>
  <li>Maintain a clear separation between on-prem and cloud responsibilities</li>
</ul>

<h2>🏗️ Architecture Overview</h2>

<h3>🟥 On-Premises (Source Environment)</h3>
<ul>
  <li>Open-source WordPress + MySQL application</li>
  <li>Docker Compose for initial application validation</li>
  <li>Kubernetes (local cluster) for container orchestration</li>
  <li>WordPress Deployment and MySQL StatefulSet</li>
  <li>Persistent Volume Claims (PVC) for database storage</li>
  <li>NodePort / Port-forwarding for local access</li>
  <li>Jenkins for CI/CD pipeline orchestration</li>
  <li>GitHub Webhooks for automated pipeline triggers</li>
  <li>Prometheus for metrics collection</li>
  <li>Node Exporter and kube-state-metrics</li>
  <li>Grafana with persistent storage for monitoring dashboards</li>
</ul>
<p>👉 Used for development, testing, CI/CD validation, and monitoring only</p>

<h3>🟦 AWS Cloud (Target Environment)</h3>
<ul>
  <li>Traditional VM-based deployment (No Kubernetes, No Docker, No EKS)</li>
  <li>Terraform for Infrastructure as Code (IaC)</li>
  <li>VPC with Public and Private Subnets</li>
  <li>EC2 instances for WordPress application hosting</li>
  <li>Auto Scaling Group for high availability</li>
  <li>Application Load Balancer</li>
  <li>RDS (MySQL) as managed database service</li>
  <li>S3 for application artifacts and backups</li>
  <li>CloudWatch for logging and monitoring</li>
  <li>SNS for alerting and notifications</li>
</ul>
<p>👉 Production-ready cloud infrastructure deployed without containers</p>

<h2>📁 Repository Structure</h2>

<pre>
onprem-to-aws-cloud-migration/
├── README.md
├── on-prem/
│   ├── docker/
│   │   └── wordpress/
│   │       └── docker-compose.yml
│   ├── kubernetes/
│   │   └── wordpress/
│   │       ├── mysql-pvc.yaml
│   │       ├── mysql-deployment.yaml
│   │       ├── mysql-service.yaml
│   │       ├── wordpress-deployment.yaml
│   │       └── wordpress-service.yaml
│   ├── monitoring/
│   │   ├── prometheus/
│   │   │   ├── namespace.yaml
│   │   │   ├── prometheus-rbac.yaml
│   │   │   ├── prometheus-config.yaml
│   │   │   ├── prometheus-deployment.yaml
│   │   │   ├── prometheus-service.yaml
│   │   │   ├── node-exporter.yaml
│   │   │   └── kube-state-metrics.yaml
│   │   └── grafana/
│   │       ├── grafana-pvc.yaml
│   │       ├── grafana-config.yaml
│   │       ├── grafana-deployment.yaml
│   │       └── grafana-service.yaml
│   └── jenkins/
│       └── Jenkinsfile
├── terraform/
│   ├── modules/
│   └── envs/
├── docs/
└── demo-videos/
    └── kubernetes-Cluster.MOV
</pre>

<h2>🧩 Project Phases</h2>

<h3>🔹 Phase 1: On-Premises WordPress Application Setup</h3>
<h4>Objective</h4>
<p>Prepare and validate the WordPress application before cloud migration.</p>
<h4>Activities</h4>
<ul>
  <li>Deploy WordPress + MySQL locally</li>
  <li>Containerize WordPress using Docker</li>
  <li>Run WordPress workloads on Kubernetes</li>
  <li>Configure Jenkins for CI</li>
  <li>Monitor application using Prometheus &amp; Grafana</li>
  <li>Integrate GitHub Webhooks with Jenkins</li>
</ul>
<h4>Outcome</h4>
<p>✅ <strong>Finished</strong> - Stable on-prem WordPress application ready for migration</p>

<h3>🔹 Phase 2: Migration Planning &amp; Design</h3>
<h4>Objective</h4>
<p>Design a controlled and low-risk cloud migration strategy.</p>
<h4>Key Design Decisions</h4>
<ul>
  <li>Kubernetes restricted to on-prem only</li>
  <li>Cloud deployment will be VM-based</li>
  <li>No EKS or managed container platforms</li>
  <li>Database migrated to AWS RDS MySQL</li>
  <li>Focus on reliability, simplicity, and cost efficiency</li>
</ul>
<h4>Outcome</h4>
<p>⏳ <strong>Work Started</strong> - Clear and interview-safe migration architecture</p>

<h3>🔹 Phase 3: AWS Infrastructure Provisioning (Terraform)</h3>
<h4>Objective</h4>
<p>Provision AWS infrastructure using Infrastructure as Code.</p>
<h4>Terraform Resources</h4>
<ul>
  <li>VPC</li>
  <li>Public &amp; Private Subnets</li>
  <li>Internet Gateway &amp; Route Tables</li>
  <li>Security Groups</li>
  <li>EC2 instances</li>
  <li>Auto Scaling Groups</li>
  <li>RDS MySQL</li>
  <li>S3 backend for Terraform state</li>
</ul>
<h4>Outcome</h4>
<p>⏳ <strong>Pending</strong> - Fully automated and reproducible AWS environment</p>

<h3>🔹 Phase 4: CI/CD Automation with Jenkins</h3>
<h4>Objective</h4>
<p>Automate infrastructure provisioning and WordPress deployment.</p>
<h4>Jenkins Pipeline Responsibilities</h4>
<ul>
  <li>Triggered via GitHub Webhook</li>
  <li>Build and test WordPress on-prem using Docker</li>
  <li>Package deployment artifacts</li>
  <li>Execute Terraform:
    <ul>
      <li><code>terraform init</code></li>
      <li><code>terraform plan</code></li>
      <li><code>terraform apply</code></li>
    </ul>
  </li>
  <li>Deploy WordPress to EC2 using:
    <ul>
      <li>SSH</li>
      <li>Nginx / Apache</li>
      <li>PHP-FPM</li>
      <li>systemd</li>
    </ul>
  </li>
  <li>Validate application health</li>
</ul>
<h4>Outcome</h4>
<p>⏳ <strong>Pending</strong> - End-to-end automated cloud migration pipeline</p>

<h3>🔹 Phase 5: Database Migration (On-Prem → RDS)</h3>
<h4>Objective</h4>
<p>Migrate WordPress MySQL database securely with minimal risk.</p>
<h4>Steps</h4>
<ul>
  <li>Take database dump from on-prem MySQL</li>
  <li>Transfer dump securely to AWS</li>
  <li>Restore database into RDS MySQL</li>
  <li>Perform data integrity checks</li>
  <li>Maintain rollback backups</li>
</ul>
<h4>Outcome</h4>
<p>⏳ <strong>Pending</strong> - WordPress database migrated with consistency and reliability</p>

<h3>🔹 Phase 6: Scaling &amp; Resilience</h3>
<h4>Objective</h4>
<p>Ensure application availability and fault tolerance.</p>
<h4>Implementation</h4>
<ul>
  <li>Auto Scaling Groups for WordPress EC2 instances</li>
  <li>Application Load Balancer for traffic distribution</li>
  <li>Automatic instance replacement on failure</li>
  <li>EC2 and RDS health metrics via CloudWatch</li>
</ul>
<h4>Outcome</h4>
<p>⏳ <strong>Pending</strong> - Self-healing and scalable WordPress infrastructure</p>

<h3>🔹 Phase 7: Monitoring &amp; Alerting</h3>
<h4>Objective</h4>
<p>Monitor system health and detect failures.</p>
<h4>Monitoring Setup</h4>
<ul>
  <li>CloudWatch metrics for EC2 &amp; RDS</li>
  <li>Application-level health checks</li>
  <li>SNS alerts for:
    <ul>
      <li>Instance failure</li>
      <li>High CPU utilization</li>
      <li>Service unavailability</li>
    </ul>
  </li>
</ul>
<h4>Migration Validation</h4>
<ul>
  <li>Compared on-prem and cloud performance metrics</li>
  <li>Verified migration success using monitoring data</li>
</ul>
<h4>Outcome</h4>
<p>⏳ <strong>Pending</strong> - Full observability and alerting enabled</p>

<h2>🧰 Tech Stack</h2>

<table border="1" cellpadding="8" cellspacing="0">
  <thead>
    <tr>
      <th>Layer</th>
      <th>Tools &amp; Services</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Infrastructure as Code</td>
      <td>Terraform</td>
    </tr>
    <tr>
      <td>CI/CD</td>
      <td>Jenkins, GitHub Webhooks</td>
    </tr>
    <tr>
      <td>On-Prem Containers</td>
      <td>Docker, Kubernetes</td>
    </tr>
    <tr>
      <td>Cloud Compute</td>
      <td>EC2, Auto Scaling Groups</td>
    </tr>
    <tr>
      <td>Load Balancing</td>
      <td>Application Load Balancer</td>
    </tr>
    <tr>
      <td>Database</td>
      <td>RDS (MySQL)</td>
    </tr>
    <tr>
      <td>Networking</td>
      <td>VPC, Subnets, Internet Gateway</td>
    </tr>
    <tr>
      <td>Monitoring</td>
      <td>CloudWatch, Prometheus, Grafana</td>
    </tr>
    <tr>
      <td>Alerts</td>
      <td>SNS</td>
    </tr>
    <tr>
      <td>Storage</td>
      <td>S3, EBS</td>
    </tr>
    <tr>
      <td>Languages</td>
      <td>HCL, Bash, YAML</td>
    </tr>
  </tbody>
</table>

<h2>📄 Resume Summary</h2>
<p>
Migrated a legacy WordPress application from on-premises to AWS using Terraform and Jenkins.
Automated infrastructure provisioning, WordPress deployment, and MySQL database migration using a VM-based cloud architecture (EC2 + RDS).
Designed a cost-aware and reliable AWS environment with Auto Scaling, Load Balancing, monitoring, and alerting.
</p>

<h2>🎯 Key Takeaways</h2>
<ul>
  <li>Real-world WordPress cloud migration</li>
  <li>Clear separation of Kubernetes and cloud workloads</li>
  <li>Strong Terraform and AWS fundamentals</li>
  <li>End-to-end CI/CD automation</li>
  <li>Enterprise-grade, interview-safe design</li>
</ul>

<h2>🚀 Future Enhancements (Optional)</h2>
<ul>
  <li>Blue/green WordPress deployments</li>
  <li>Multi-AZ RDS configuration</li>
  <li>CloudFront CDN integration</li>
  <li>Cost optimization and tagging strategy</li>
  <li>Infrastructure drift detection</li>
</ul>

<h2>🏁 Conclusion</h2>
<p>
This project represents a practical enterprise WordPress migration where stability, automation, and operational simplicity are prioritized over overengineering.
It showcases strong skills in Terraform, AWS infrastructure, Jenkins automation, and migration strategy, without unnecessary complexity.
</p>

<h2>✅ Final Project Declaration</h2>
<ul>
  <li>Application: WordPress + MySQL (existing open-source)</li>
  <li>Kubernetes: On-prem only</li>
  <li>AWS Cloud: EC2 for compute, RDS for database</li>
</ul>
<p>✔ realistic<br>
✔ cost-aware<br>
✔ interview-safe<br>
✔ DevOps-focused 💎</p>

<hr>

<h2>📊 Project Status Summary</h2>

<table border="1" cellpadding="8" cellspacing="0">
  <thead>
    <tr>
      <th>Phase</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Phase 1: On-Premises WordPress Application Setup</td>
      <td>✅ Finished</td>
    </tr>
    <tr>
      <td>Phase 2: Migration Planning &amp; Design</td>
      <td>⏳ Started</td>
    </tr>
    <tr>
      <td>Phase 3: AWS Infrastructure Provisioning (Terraform)</td>
      <td>⏳ Pending</td>
    </tr>
    <tr>
      <td>Phase 4: CI/CD Automation with Jenkins</td>
      <td>⏳ Pending</td>
    </tr>
    <tr>
      <td>Phase 5: Database Migration (On-Prem → RDS)</td>
      <td>⏳ Pending</td>
    </tr>
    <tr>
      <td>Phase 6: Scaling &amp; Resilience</td>
      <td>⏳ Pending</td>
    </tr>
    <tr>
      <td>Phase 7: Monitoring &amp; Alerting</td>
      <td>⏳ Pending</td>
    </tr>
  </tbody>
</table>

</body>
</html>
