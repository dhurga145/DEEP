2. For the Web application automate the continuous integration and deployment process using Jenkins.
 Write the code in Visual Studio and push it to your GITHUB repository. 
 Modify the code and commit the updates back in GITHUB. 
 Using Jenkins configure the job as pipeline and create the stages for the Continuous Integration process. 
 Checkout GITHUB 
 Create Docker image 
 Push to Docker Hub 
 Create Cluster in Kubernetec and Deploy

c. To print rating of a movie.  




no need of maven



📁 Project Structure
web-app/
│
├── index.html
├── Dockerfile
├── deployment.yaml
└── Jenkinsfile
________________________________________
⚙️ STEP 1: Create Web Application
📄 index.html
<!DOCTYPE html>
<html>
<head>
    <title>Movie Rating</title>
</head>
<body>

<h1>Movie Rating</h1>

<p>Movie: Inception</p>
<p>Rating: ⭐⭐⭐⭐☆ (4/5)</p>

<p>Movie: Avengers</p>
<p>Rating: ⭐⭐⭐⭐⭐ (5/5)</p>

<p>Movie: Titanic</p>
<p>Rating: ⭐⭐⭐⭐☆ (4/5)</p>

</body>
</html>
________________________________________
📤 STEP 2: Push Code to GitHub
git init
git add .
git commit -m "Initial web app commit"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main

👉 Modify code (example: change rating)
git add .
git commit -m "Updated movie ratings"
git push
________________________________________
🐳 STEP 3: Create Dockerfile
📄 Dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html

EXPOSE 80
________________________________________
☸️ STEP 4: Kubernetes Deployment
📄 deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
        - name: web-app
          image: <your-dockerhub-username>/web-app:latest
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  type: NodePort
  selector:
    app: web-app
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
________________________________________
⚙️ STEP 5: Enable Kubernetes
•	Open Docker Desktop
•	Go to Settings → Kubernetes
•	Enable Kubernetes
•	Click Apply & Restart
________________________________________
🔐 STEP 6: Add Credentials in Jenkins
•	DockerHub credentials → dockerhub-creds
•	Kubernetes config → kuberconfig
________________________________________
⚙️ STEP 7: Jenkins Pipeline
📄 Jenkinsfile
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "<your-dockerhub-username>/web-app:latest"
    }

    stages {

        stage('Checkout GitHub') {
            steps {
                git branch: 'main',
                url: '<your-repo-url>'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    bat """
                    docker login -u %USER% -p %PASS%
                    docker push %DOCKER_IMAGE%
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(
                    credentialsId: 'kuberconfig',
                    variable: 'KUBECONFIG'
                )]) {
                    bat '''
                    set KUBECONFIG=%KUBECONFIG%
                    kubectl apply -f deployment.yaml
                    '''
                }
            }
        }
    }
}
________________________________________
▶️ STEP 8: Run Pipeline
•	Open Jenkins
•	Create Pipeline Job
•	Paste Jenkinsfile
•	Click Build Now
________________________________________
🔍 STEP 9: Verify Deployment
kubectl get pods
kubectl get svc
________________________________________
🌐 STEP 10: Access Application
http://localhost:30007
✅ Output: Movie ratings displayed in browser
________________________________________
📊 CI/CD Flow
🔹 Code pushed to GitHub
🔹 Jenkins checks out code
🔹 Docker image created
🔹 Image pushed to Docker Hub
🔹 Kubernetes deploys app
________________________________________


okay aaah,,,...?babiee


got veified.

ummwahhhh
