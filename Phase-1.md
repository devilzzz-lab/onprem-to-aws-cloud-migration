<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>

<h1>On-Premises to AWS Cloud Migration — WordPress Project</h1>

<p><strong>Status:</strong> <em>Phase 1 (On-Prem WordPress Setup & Validation)</em> <strong>in progress</strong>.</p>

<p>
This project demonstrates a complete enterprise-grade migration of a WordPress application from on-premises infrastructure to AWS cloud.
The migration includes containerization using <strong>Docker</strong>, orchestration with <strong>Kubernetes</strong> (on-premises only), 
CI/CD automation using <strong>Jenkins</strong>, infrastructure provisioning with <strong>Terraform</strong>, 
and comprehensive monitoring using <strong>Prometheus</strong> and <strong>Grafana</strong>.
</p>

<hr>

<h2>Project Highlights</h2>
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

<hr>

<h2>Project Architecture</h2>

<h3>On-Premises Environment</h3>
<ul>
  <li>WordPress running on Docker Compose (validation phase)</li>
  <li>Kubernetes deployment with MySQL StatefulSet and WordPress Deployment</li>
  <li>Persistent Volume Claims for database storage</li>
  <li>NodePort/Port-forward service exposure for local access</li>
  <li>Jenkins for CI/CD orchestration</li>
  <li>Prometheus + Node Exporter + kube-state-metrics for monitoring</li>
</ul>

<h3>AWS Cloud Environment (Target)</h3>
<ul>
  <li>EC2 instances for WordPress application hosting</li>
  <li>RDS MySQL for managed database service</li>
  <li>S3 for backups and static assets</li>
  <li>Auto Scaling Groups for high availability</li>
  <li>CloudWatch for logging and monitoring</li>
  <li>SNS for alerting</li>
</ul>

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
│   ├── monitoring/
│   │   └── prometheus/
│   │       ├── namespace.yaml
│   │       ├── prometheus-rbac.yaml
│   │       ├── prometheus-config.yaml
│   │       ├── prometheus-deployment.yaml
│   │       ├── prometheus-service.yaml
│   │       ├── node-exporter.yaml
│   │       └── kube-state-metrics.yaml
│   └── jenkins/
│       └── Jenkinsfile
├── terraform/
│   ├── modules/
│   └── envs/
└── docs/
</pre>

<hr>

<h2>Phase 1: On-Premises Setup & Validation</h2>

<h3>🟥 STEP 0 — Create the Project Repository</h3>

<h4>0.1: Repository Creation</h4>
<p><strong>Repository name:</strong> <code>onprem-to-aws-cloud-migration</code></p>
<p><strong>Visibility:</strong> Public</p>
<p><strong>Initialize with:</strong> README.md</p>

<h4>0.2: Clone Repository</h4>
<pre>
git clone https://github.com/devilzzz-lab/onprem-to-aws-cloud-migration.git
cd onprem-to-aws-cloud-migration
</pre>

<h4>0.3: Create Directory Structure</h4>
<pre>
mkdir -p on-prem/docker/wordpress
mkdir -p on-prem/kubernetes/wordpress
mkdir -p on-prem/monitoring/prometheus
mkdir -p on-prem/jenkins
mkdir -p terraform/modules
mkdir -p terraform/envs
mkdir -p docs
</pre>

<h4>0.4: Initial Commit</h4>
<pre>
git add .
git commit -m "Phase 1: initialize project repository and on-prem structure"
git push origin main
</pre>

<hr>

<h3>🟥 STEP 1 — Setup On-Prem Environment Validation</h3>

<h4>1.1: Verify Operating System</h4>
<pre>
uname -a
lsb_release -a || cat /etc/os-release
</pre>

<h4>1.2: Verify Docker</h4>
<pre>
docker --version
docker info
</pre>

<h4>1.3: Verify Kubernetes</h4>
<pre>
kubectl version --client
kubectl get nodes
</pre>

<p><strong>Expected:</strong> 1 node with STATUS = Ready</p>

<h4>1.4: Verify Jenkins</h4>
<pre>
jenkins --version
</pre>
<p>Or open: <code>http://localhost:8080</code></p>

<hr>

<h3>🟥 STEP 2 — Run WordPress using Docker</h3>

<h4>2.1: Create Docker Working Directory</h4>
<pre>
cd on-prem/docker/wordpress
</pre>

<h4>2.2: Create docker-compose.yml</h4>
<p>📄 File: <code>on-prem/docker/wordpress/docker-compose.yml</code></p>

<h4>2.3: Start WordPress using Docker</h4>
<pre>
docker compose up -d
</pre>

<h4>2.4: Verify Containers</h4>
<pre>
docker ps
</pre>

<p><strong>Expected output:</strong> Both <code>wp-app</code> and <code>wp-mysql</code> running</p>

<h4>2.5: Access WordPress</h4>
<p>Open browser: <code>http://localhost:8081</code></p>
<p>Complete the WordPress installation setup.</p>

<hr>

<h3>🟥 STEP 3 — Create Kubernetes Namespace</h3>

<h4>3.1: Create Dedicated Namespace</h4>
<pre>
kubectl create namespace wordpress
</pre>

<h4>3.2: Verify Namespace</h4>
<pre>
kubectl get namespaces
</pre>

<p><strong>Expected:</strong> <code>wordpress</code> namespace appears in list</p>

<hr>

<h3>🟥 STEP 4 — Create Kubernetes Manifests</h3>

<h4>4.1: Navigate to Kubernetes Directory</h4>
<pre>
cd on-prem/kubernetes/wordpress
</pre>

<h4>4.2: Create MySQL PVC</h4>
<p>📄 File: <code>mysql-pvc.yaml</code></p>

<h4>4.3: Create MySQL Deployment</h4>
<p>📄 File: <code>mysql-deployment.yaml</code></p>


<h4>4.4: Create MySQL Service</h4>
<p>📄 File: <code>mysql-service.yaml</code></p>


<h4>4.5: Create WordPress Deployment</h4>
<p>📄 File: <code>wordpress-deployment.yaml</code></p>

<h4>4.6: Create WordPress Service</h4>
<p>📄 File: <code>wordpress-service.yaml</code></p>

<hr>

<h3>🟥 STEP 5 — Deploy WordPress on Kubernetes</h3>

<h4>5.1: Apply All Manifests</h4>
<pre>
kubectl apply -f .
</pre>

<h4>5.2: Verify Deployment</h4>
<pre>
kubectl get pods -n wordpress
kubectl get svc -n wordpress
kubectl get pvc -n wordpress
</pre>

<p><strong>Expected output:</strong></p>
<ul>
  <li>MySQL pod: Running</li>
  <li>WordPress pod: Running</li>
  <li>PVC: Bound</li>
</ul>

<h4>5.3: Access WordPress (Port-Forward)</h4>
<pre>
kubectl port-forward svc/wordpress 8083:80 -n wordpress
</pre>

<p>Open browser: <code>http://localhost:8083</code></p>
<p>Complete WordPress setup so that in next step will show output</p>

<hr>

<h3>🟥 STEP 6 — Initialize and Verify MySQL Database</h3>

<h4>6.1: Get MySQL Pod Name</h4>
<pre>
kubectl get pods -n wordpress | grep mysql
</pre>

<h4>6.2: Exec into MySQL Pod</h4>
<pre>
kubectl exec -it &lt;mysql-pod-name&gt; -n wordpress -- mysql -u root -p
</pre>

<p><strong>Password:</strong> <code>rootpassword</code></p>

<h4>6.3: Verify Database and Tables</h4>
<pre>
SHOW DATABASES;
USE wordpress;
SHOW TABLES;
</pre>

<p><strong>Expected:</strong> WordPress tables like <code>wp_users</code>, <code>wp_posts</code>, <code>wp_options</code> should exist</p>

<h4>6.4: Exit MySQL</h4>
<pre>
exit;
</pre>

<hr>

<h3>🟥 STEP 7 — Deploy Prometheus Monitoring Stack</h3>

<h4>7.1: Navigate to Monitoring Directory</h4>
<pre>
cd on-prem/monitoring/prometheus
</pre>

<h4>7.2: Create Namespace for Monitoring</h4>
<p>📄 File: <code>namespace.yaml</code></p>

<pre>
apiVersion: v1
kind: Namespace
metadata:
  name: monitoring-wp
</pre>

<p><strong>Apply:</strong></p>
<pre>
kubectl apply -f namespace.yaml
</pre>

<h4>7.3: Create Prometheus RBAC</h4>
<p>📄 File: <code>prometheus-rbac.yaml</code></p>

<h4>7.4: Create Prometheus ConfigMap</h4>
<p>📄 File: <code>prometheus-config.yaml</code></p>


<h4>7.6: Create Prometheus Service</h4>
<p>📄 File: <code>prometheus-service.yaml</code></p>


<h4>7.7: Deploy Node Exporter (DaemonSet)</h4>
<p>📄 File: <code>node-exporter.yaml</code></p>


<h4>7.8: Deploy kube-state-metrics</h4>
<p>📄 File: <code>kube-state-metrics.yaml</code></p>


<h4>7.9: Apply All Monitoring Manifests</h4>
<pre>
kubectl apply -f . -n monitoring-wp
</pre>

<h4>7.10: Verify Prometheus Stack</h4>
<pre>
kubectl get pods -n monitoring-wp
kubectl get svc -n monitoring-wp
</pre>

<p><strong>Expected output:</strong></p>
<ul>
  <li>prometheus pod: Running</li>
  <li>node-exporter pods: Running (DaemonSet)</li>
  <li>kube-state-metrics pod: Running</li>
</ul>

<h4>7.11: Access Prometheus UI</h4>
<pre>
kubectl port-forward svc/prometheus 9090:9090 -n monitoring-wp
</pre>

<p>Open browser: <code>http://localhost:9090</code></p>

<h4>7.12: Verify Metrics Collection</h4>
<p>In Prometheus UI, navigate to <strong>Graph</strong> tab and test these queries:</p>

<pre>
up
container_cpu_usage_seconds_total
kube_pod_info
</pre>

<p><strong>Expected:</strong> All queries should return data ✅</p>

<hr>

<h2>Technical Stack</h2>

<h3>On-Premises</h3>
<ul>
  <li><strong>Containerization:</strong> Docker, Docker Compose</li>
  <li><strong>Orchestration:</strong> Kubernetes (local cluster - on-prem only)</li>
  <li><strong>Database:</strong> MySQL 8.0</li>
  <li><strong>Application:</strong> WordPress (latest)</li>
  <li><strong>CI/CD:</strong> Jenkins</li>
  <li><strong>Monitoring:</strong> Prometheus, Node Exporter, kube-state-metrics</li>
  <li><strong>Version Control:</strong> Git, GitHub</li>
</ul>

<h3>AWS Cloud (Target)</h3>
<ul>
  <li><strong>Compute:</strong> EC2 instances</li>
  <li><strong>Database:</strong> RDS MySQL</li>
  <li><strong>Storage:</strong> S3 (backups and artifacts)</li>
  <li><strong>Infrastructure as Code:</strong> Terraform</li>
  <li><strong>Monitoring:</strong> CloudWatch (AWS)</li>
  <li><strong>Auto Scaling:</strong> Auto Scaling Groups</li>
  <li><strong>Alerts:</strong> SNS</li>
</ul>

<hr>

<h2>Phase 1 Completion Checklist</h2>

<table border="1" cellpadding="8" cellspacing="0">
  <thead>
    <tr>
      <th>Task</th>
      <th>Status</th>
      <th>Verification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Create project repository</td>
      <td>✅</td>
      <td>GitHub repo created</td>
    </tr>
    <tr>
      <td>Setup directory structure</td>
      <td>✅</td>
      <td>tree -L 3</td>
    </tr>
    <tr>
      <td>Verify Docker installation</td>
      <td>✅</td>
      <td>docker --version</td>
    </tr>
    <tr>
      <td>Verify Kubernetes cluster</td>
      <td>✅</td>
      <td>kubectl get nodes</td>
    </tr>
    <tr>
      <td>Run WordPress using Docker</td>
      <td>✅</td>
      <td>http://localhost:8081</td>
    </tr>
    <tr>
      <td>Create Kubernetes namespace</td>
      <td>✅</td>
      <td>kubectl get ns</td>
    </tr>
    <tr>
      <td>Create Kubernetes manifests</td>
      <td>✅</td>
      <td>kubectl apply -f .</td>
    </tr>
    <tr>
      <td>Deploy WordPress on Kubernetes</td>
      <td>✅</td>
      <td>kubectl get pods -n wordpress</td>
    </tr>
    <tr>
      <td>Expose WordPress service</td>
      <td>✅</td>
      <td>kubectl port-forward svc/wordpress 8083:80 -n wordpress</td>
    </tr>
    <tr>
      <td>Initialize MySQL database</td>
      <td>✅</td>
      <td>kubectl exec + SHOW TABLES</td>
    </tr>
    <tr>
      <td>Deploy Prometheus</td>
      <td>✅</td>
      <td>kubectl get pods -n monitoring-wp</td>
    </tr>
    <tr>
      <td>Deploy Node Exporter</td>
      <td>✅</td>
      <td>kubectl get ds -n monitoring-wp</td>
    </tr>
    <tr>
      <td>Deploy kube-state-metrics</td>
      <td>✅</td>
      <td>Prometheus UI metrics visible</td>
    </tr>
    <tr>
      <td>Verify Prometheus metrics</td>
      <td>✅</td>
      <td>http://localhost:9090 + queries</td>
    </tr>
    <tr>
      <td>Configure Jenkins on-prem</td>
      <td>⏳</td>
      <td>Pending</td>
    </tr>
    <tr>
      <td>Setup GitHub webhooks</td>
      <td>⏳</td>
      <td>Pending</td>
    </tr>
    <tr>
      <td>Create CI job</td>
      <td>⏳</td>
      <td>Pending</td>
    </tr>
    <tr>
      <td>Deploy Grafana</td>
      <td>⏳</td>
      <td>Pending</td>
    </tr>
  </tbody>
</table>

<hr>

<h2>Next Steps</h2>

<ul>
  <li>Deploy Grafana and create WordPress monitoring dashboards</li>
  <li>Configure Jenkins for CI/CD automation</li>
  <li>Setup GitHub webhooks for automated builds</li>
  <li>Create Terraform modules for AWS infrastructure</li>
  <li>Migrate WordPress to AWS EC2</li>
  <li>Configure RDS MySQL database</li>
  <li>Configure Auto Scaling Groups for high availability</li>
  <li>Implement CloudWatch monitoring and SNS alerts</li>
</ul>

<hr>

<h2>Interview Talking Points</h2>

<p><strong>Why separate namespace?</strong></p>
<p>"We deployed WordPress into a dedicated Kubernetes namespace instead of default to maintain isolation and follow production best practices."</p>

<p><strong>Why Docker first, then Kubernetes?</strong></p>
<p>"In real DevOps workflows, we validate applications standalone using Docker before introducing orchestration complexity with Kubernetes."</p>

<p><strong>Database initialization approach?</strong></p>
<p>"WordPress automatically initializes the database schema during first startup. My responsibility was to ensure connectivity, persistence, and credentials were configured correctly."</p>

<p><strong>Port conflict handling?</strong></p>
<p>"The default port was already occupied by Jenkins, so we adjusted the application port to avoid conflicts. This is common practice in shared on-prem environments."</p>

<p><strong>Why Kubernetes only on-premises?</strong></p>
<p>"We used Kubernetes on-premises for development and validation. For AWS migration, we opted for a VM-based deployment using EC2 and RDS to minimize complexity and cost while maintaining enterprise-grade reliability."</p>

<p><strong>Prometheus metrics troubleshooting?</strong></p>
<p>"Initially Prometheus showed no metrics because exporters and RBAC were missing. I resolved this by deploying node-exporter, kube-state-metrics, and proper RBAC, enabling full Kubernetes observability."</p>

<hr>

<p><strong>— On-Premises to AWS Cloud Migration Project</strong></p>

</body>
</html>
