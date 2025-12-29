<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Phase 1 – On-Prem WordPress Setup & Validation</title>
</head>
<body>

<h1>On-Premises to AWS Cloud Migration — WordPress Project</h1>

<p><strong>Status:</strong> <em>Phase 1 (On-Prem WordPress Setup & Validation)</em> <strong>Completed</strong>.</p>

<p>
This project demonstrates a realistic enterprise cloud migration scenario where an existing
<strong>WordPress + MySQL</strong> application is first validated in an on-premises environment
using <strong>Docker</strong> and <strong>Kubernetes</strong>, before migrating to a
<strong>VM-based AWS architecture (EC2 + RDS)</strong>.
</p>

<p>
<strong>Important design decision:</strong>
Docker and Kubernetes are intentionally restricted to the on-prem environment only.
AWS cloud will use traditional virtual machines and managed database services to minimize
operational complexity and cost.
</p>

<hr>

<h2>Project Highlights</h2>
<ul>
  <li>Existing open-source WordPress application used (no app development)</li>
  <li>Docker Compose used for standalone application validation</li>
  <li>Kubernetes used on-prem for container orchestration</li>
  <li>Dedicated Kubernetes namespace for isolation</li>
  <li>MySQL database initialization and persistence validation</li>
  <li>Strong foundation for cloud migration using Terraform and Jenkins (next phases)</li>
</ul>

<hr>

<h2>Architecture Overview</h2>

<h3>🟥 On-Premises Environment (Phase 1)</h3>
<ul>
  <li>macOS local lab simulating on-prem infrastructure</li>
  <li>Docker & Docker Compose</li>
  <li>Kubernetes (local cluster)</li>
  <li>WordPress Deployment</li>
  <li>MySQL Deployment with Persistent Volume Claim</li>
  <li>Jenkins (CI/CD orchestration – upcoming)</li>
</ul>

<h3>🟦 AWS Cloud (Target – Future Phases)</h3>
<ul>
  <li>EC2 (Frontend & Backend VMs)</li>
  <li>RDS MySQL (managed database)</li>
  <li>Auto Scaling Groups</li>
  <li>CloudWatch & SNS</li>
  <li>Terraform for Infrastructure as Code</li>
</ul>

<p><strong>Note:</strong> Kubernetes, Docker, EKS, EFS, and ALB are NOT used in the AWS cloud.</p>

<hr>

<h2>Repository Structure</h2>

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
│   └── jenkins/
│       └── Jenkinsfile
├── terraform/
│   ├── modules/
│   └── envs/
└── docs/
    └── Phase-1.html
</pre>

<hr>

<h2>Phase 1 – On-Prem Setup & Validation</h2>

<h3>Step 1: Environment Validation</h3>
<pre>
uname -a
docker --version
kubectl get nodes
</pre>

<p>
On-prem environment simulated using macOS with Docker Desktop and local Kubernetes.
</p>

<hr>

<h3>Step 2: WordPress Validation using Docker</h3>

<pre>
docker compose up -d
docker ps
</pre>

<p>
WordPress validated at:
<code>http://localhost:8081</code>
</p>

<p>
Port was adjusted due to Jenkins already using port 8080.
</p>

<hr>

<h3>Step 3: Kubernetes Namespace Creation</h3>

<pre>
kubectl create namespace wordpress
kubectl get namespaces
</pre>

<hr>

<h3>Step 4: Kubernetes Deployment</h3>

<pre>
kubectl apply -f .
kubectl get pods -n wordpress
kubectl get svc -n wordpress
kubectl get pvc -n wordpress
</pre>

<hr>

<h3>Step 5: Access WordPress on Kubernetes</h3>

<pre>
kubectl port-forward svc/wordpress 8083:80 -n wordpress
</pre>

<p>
WordPress accessible at:
<code>http://localhost:8083</code>
</p>

<hr>

<h3>Step 6: MySQL Database Initialization</h3>

<pre>
kubectl exec -it &lt;mysql-pod&gt; -n wordpress -- mysql -u root -p
SHOW DATABASES;
USE wordpress;
SHOW TABLES;
</pre>

<p>
WordPress tables were automatically created, confirming successful database initialization.
</p>

<hr>

<h2>Phase 1 Completion Summary</h2>

<ul>
  <li>Docker-based WordPress validated</li>
  <li>Kubernetes manifests created and deployed</li>
  <li>Dedicated namespace used</li>
  <li>Persistent MySQL storage verified</li>
  <li>Application accessible and stable</li>
</ul>

<hr>

<h2>Interview Talking Points</h2>

<p><strong>Why Kubernetes only on-prem?</strong></p>
<p>
“Kubernetes was used on-prem for development and validation.
Cloud deployment intentionally uses VM-based architecture to reduce cost and operational overhead.”
</p>

<p><strong>Why Docker before Kubernetes?</strong></p>
<p>
“Validating applications in Docker before Kubernetes helps isolate issues and reduces orchestration complexity.”
</p>

<p><strong>Database initialization strategy?</strong></p>
<p>
“WordPress initializes the database schema automatically.
My responsibility was ensuring correct connectivity, credentials, and persistence.”
</p>

<hr>

<h2>Next Phase</h2>

<ul>
  <li>Configure Jenkins CI pipeline</li>
  <li>Integrate GitHub webhooks</li>
  <li>Prepare Terraform modules for AWS EC2 & RDS</li>
  <li>Begin on-prem → AWS migration</li>
</ul>

<hr>

<p><strong>— Phase 1 completed successfully</strong></p>

</body>
</html>
