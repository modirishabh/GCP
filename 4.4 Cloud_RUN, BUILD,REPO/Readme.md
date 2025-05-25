# Cloud Run, Cloud Build, and Cloud Repositories in Google Cloud Platform (GCP)

Welcome back, Gurus. This lesson introduces the combined capabilities of Cloud Run, Cloud Build, and Cloud Repositories for deploying and managing applications in GCP.

## Introduction to Cloud Run, Cloud Build, and Cloud Repositories

In GCP, these services form a cohesive platform for developing, building, and running containerized apps without managing infrastructure.

## How Do Cloud Run, Cloud Build, and Cloud Repos Work Together?

- **Cloud Run**: Offers a fully managed serverless environment for your containers, scaling as needed and only charging for actual use.
- **Cloud Build**: Handles the build process, from code fetching to deployment, executing each step in a containerized environment.
- **Cloud Source Repositories**: Acts as a secure home for your source code, allowing Git operations and integration with other GCP services for continuous deployment.

## Deploying an Application with Cloud Run

In our demo, we successfully deployed a "Hello World" application using Cloud Run by:

1. Accessing the Cloud Shell and setting up the project configuration.
2. Creating a Cloud Run service using a pre-existing container image.
3. Deploying the container and accessing it through the provided URL.

## Key Takeaways for the GCP Cloud Associate Exam

- Cloud Run is a key serverless platform for containers, supporting languages like Java, Python, Ruby, Go, PHP, and JavaScript.
- Cloud Build facilitates the build process in a serverless manner, allowing deployment across various environments.
- Cloud Source Repositories securely manage your code and can be used to trigger builds and deployments.

Thank you for joining this session on Cloud Run, Cloud Build, and Cloud Repositories. Keep learning and practicing for the ACE exam. I'll see you in the next video!
