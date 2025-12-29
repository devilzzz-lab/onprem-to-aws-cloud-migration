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
The migration includes containerization using <strong>Docker</strong>, orchestration with <strong>Kubernetes</strong>, 
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
</ul>

<h3>AWS Cloud Environment (Target)</h3>
<ul>
  <li>EKS (Elastic Kubernetes Service) for container orchestration</li>
  <li>RDS MySQL for managed database service</li>
  <li>EFS for persistent WordPress file storage</li>
  <li>Application Load Balancer for traffic distribution</li>
  <li>Route 53 for DNS management</li>
  <li>CloudWatch for logging and monitoring</li>
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
git clone https://github.com/&lt;your-username&gt;/onprem-to-aws-cloud-migration.git
cd onprem-to-aws-cloud-migration
</pre>

<h4>0.3: Create Directory Structure</h4>
<pre>
mkdir -p on-prem/docker/wordpress
mkdir -p on-prem/kubernetes/wordpress
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

<pre>
version: '3.8'

services:
  db:
    image: mysql:8.0
    container_name: wp-mysql
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppassword
      MYSQL_ROOT_PASSWORD: rootpassword
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wp-app
    restart: always
    ports:
      - "8081:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppassword
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  db_data:
</pre>

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

<pre>
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
  namespace: wordpress
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
</pre>

<h4>4.3: Create MySQL Deployment</h4>
<p>📄 File: <code>mysql-deployment.yaml</code></p>

<pre>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
  namespace: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        env:
        - name: MYSQL_DATABASE
          value: wordpress
        - name: MYSQL_USER
          value: wpuser
        - name: MYSQL_PASSWORD
          value: wppassword
        - name: MYSQL_ROOT_PASSWORD
          value: rootpassword
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: mysql-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-storage
        persistentVolumeClaim:
          claimName: mysql-pvc
</pre>

<h4>4.4: Create MySQL Service</h4>
<p>📄 File: <code>mysql-service.yaml</code></p>

<pre>
apiVersion: v1
kind: Service
metadata:
  name: mysql
  namespace: wordpress
spec:
  selector:
    app: mysql
  ports:
  - port: 3306
    targetPort: 3306
</pre>

<h4>4.5: Create WordPress Deployment</h4>
<p>📄 File: <code>wordpress-deployment.yaml</code></p>

<pre>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
  namespace: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wordpress
        image: wordpress:latest
        env:
        - name: WORDPRESS_DB_HOST
          value: mysql:3306
        - name: WORDPRESS_DB_USER
          value: wpuser
        - name: WORDPRESS_DB_PASSWORD
          value: wppassword
        - name: WORDPRESS_DB_NAME
          value: wordpress
        ports:
        - containerPort: 80
</pre>

<h4>4.6: Create WordPress Service</h4>
<p>📄 File: <code>wordpress-service.yaml</code></p>

<pre>
apiVersion: v1
kind: Service
metadata:
  name: wordpress
  namespace: wordpress
spec:
  type: NodePort
  selector:
    app: wordpress
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30007
</pre>

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
<p>Complete WordPress setup if needed.</p>

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

<h2>Technical Stack</h2>

<h3>On-Premises</h3>
<ul>
  <li><strong>Containerization:</strong> Docker, Docker Compose</li>
  <li><strong>Orchestration:</strong> Kubernetes (local cluster)</li>
  <li><strong>Database:</strong> MySQL 8.0</li>
  <li><strong>Application:</strong> WordPress (latest)</li>
  <li><strong>CI/CD:</strong> Jenkins</li>
  <li><strong>Version Control:</strong> Git, GitHub</li>
</ul>

<h3>AWS Cloud (Target)</h3>
<ul>
  <li><strong>Compute:</strong> EKS (Elastic Kubernetes Service)</li>
  <li><strong>Database:</strong> RDS MySQL</li>
  <li><strong>Storage:</strong> EFS (Elastic File System)</li>
  <li><strong>Load Balancing:</strong> Application Load Balancer (ALB)</li>
  <li><strong>DNS:</strong> Route 53</li>
  <li><strong>Infrastructure as Code:</strong> Terraform</li>
  <li><strong>Monitoring:</strong> Prometheus, Grafana, CloudWatch</li>
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
      <td>Deploy Prometheus</td>
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
  <li>Configure Jenkins for CI/CD automation</li>
  <li>Setup GitHub webhooks for automated builds</li>
  <li>Deploy monitoring stack (Prometheus + Grafana)</li>
  <li>Create Terraform modules for AWS infrastructure</li>
  <li>Migrate WordPress to AWS EKS</li>
  <li>Configure RDS MySQL and EFS storage</li>
  <li>Setup Application Load Balancer and Route 53</li>
  <li>Implement CloudWatch monitoring</li>
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

<hr>

<p><strong>— On-Premises to AWS Cloud Migration Project</strong></p>

</body>
</html>
