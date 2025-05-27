
Jenkins CI/CD Pipeline with Docker:

Objective:

Set up an automated Jenkins pipeline that:

- Pulls code from a GitHub repository
- Builds a Docker image of the application
- (Optionally) Runs tests
- Deploys the application in a Docker container

Tools Required:

- [Jenkins](https://www.jenkins.io/) (local or cloud instance such as AWS EC2)
- [Docker](https://www.docker.com/)
- [GitHub](https://github.com/) (or any Git-based code hosting platform)


Step 1: Install Jenkins

You can install Jenkins either **locally** or on a **cloud VM (recommended)**.
Option A: Local Installation

Follow instructions on the official Jenkins site:  
👉 [https://www.jenkins.io/doc/book/installing/](https://www.jenkins.io/doc/book/installing/)
Option B: Ubuntu Cloud Server (e.g., AWS EC2)

Run the following commands:
bash:
sudo apt update
sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins

Step 2: Install Required Jenkins Plugins

Once Jenkins is running:

1. Go to: **Manage Jenkins > Manage Plugins**
2. Install the following plugins:

   * **Git plugin**
   * **Docker plugin**
   * **Pipeline**
   * **Blue Ocean** (optional, for a better UI)
   * **GitHub Integration Plugin**

Step 3: Configure Docker for Jenkins

Ensure Docker is installed and Jenkins has permission to use it:
bash:
sudo apt install docker.io -y
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins

Step 4: Create a Sample Application (Optional)

You can use your own app or create a simple Node.js app.
Project Structure

my-app/
├── app.js
├── Dockerfile
├── package.json
└── Jenkinsfile

Sample `Dockerfile`

dockerfile:
FROM node:14
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["node", "app.js"]

Step 5: Create the Jenkinsfile

Place the following `Jenkinsfile` in the root of your project repository:

pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t my-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add test commands here if necessary
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 3000:3000 my-app'
            }
        }
    }
}
Step 6: Set Up Jenkins Job:

1. Open the Jenkins dashboard
2. Click **New Item**
3. Enter a name (e.g., `My-CICD-Pipeline`)
4. Select **Pipeline** and click OK
5. In the configuration screen:

   * Under **Pipeline > Definition**, choose **Pipeline script from SCM**
   * Select **Git** as SCM and provide your repository URL
   * Set the script path to `Jenkinsfile` (or specify the path if located in a subdirectory)
6. Click **Save**

Step 7: Set Up GitHub Webhook (For Auto Trigger)
In GitHub Repository:

1. Go to **Settings > Webhooks**
2. Click **Add Webhook**
3. Set the following:

   * **Payload URL**: `http://<your-jenkins-server>/github-webhook/`
   * **Content type**: `application/json`
   * **Events**: Just the push event
4. Click **Add Webhook**
In Jenkins:

* Go to your job > **Configure**
* Under **Build Triggers**, check **GitHub hook trigger for GITScm polling**
* Save the changes

Step 8: Test the Pipeline

1. Push a change to your GitHub repository
2. Jenkins should automatically trigger the pipeline
3. Monitor the pipeline progress in the Jenkins UI
4. Verify the running container with: docker ps

Expected Output:
* Jenkins clones your repo on every push
* Builds the Docker image from your app
* Runs any defined tests
* Deploys the app as a Docker container accessible on port `3000`


```

---

Let me know if you'd like help creating the sample repo with this structure and code, or need a downloadable `.zip` version to test locally.
```
