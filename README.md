# Jenkins_demo
DAY-2


🔹 1. Clone the Project Repository

    git clone https://github.com/AshfaqShah786/Jenkins_demo.git

    cd Jenkins_demo

🔹 2. Build and Run the Dockerized App Locally

    docker build -t my-app .

    docker run -p 5000:5000 my-app

🔹 3. Create Docker Volume for Jenkins

    docker volume create jenkins_home

🔹 4. Run Jenkins in Docker

    docker run -d -p 8080:8080 -p 50000:50000 \
    -v jenkins_home:/var/jenkins_home \
    --name jenkins jenkins/jenkins:lts

🔹 5. Access Jenkins in Browser

    http://localhost:8080

🔹 6. Unlock Jenkins (First Time Setup)

    cat /var/jenkins_home/secrets/initialAdminPassword

(Or check container logs for password if running via Docker)

🔹 7. Install Required Plugins in Jenkins

Search and install:
Docker Pipeline
Git
Credentials Binding

🔹 8. Add DockerHub Credentials in Jenkins

    Jenkins Dashboard → Manage Jenkins → Credentials → Add Credentials

🔹 9. Create a New Jenkins Pipeline Job

    Jenkins Dashboard → New Item
    Enter a name → Choose Pipeline
    Select Pipeline script from SCM
    Choose Git, add repo URL
    Set branch to main (or your default branch)
    Save the job

🔹 10. Test the Pipeline Manually

    Click "Build Now" in Jenkins to run the pipeline.

🔹 11. (Optional) Add GitHub Webhook for Auto Trigger

On GitHub:

Go to Repo → Settings → Webhooks
Add webhook:

        URL: http://<your-jenkins-ip>:8080/github-webhook/

        Content type: application/json

        Event: Push only

🔹 12. Push a Change to Trigger the Pipeline

    git add .
    git commit -m "Trigger Jenkins pipeline"
    git push origin main

