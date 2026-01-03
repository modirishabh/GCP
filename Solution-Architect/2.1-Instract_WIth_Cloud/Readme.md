# Interacting with Google Cloud

## Module overview
- Learn how to interact with Google Cloud using console, command line, APIs, and mobile app. [web:5]
- Practice these methods in labs and a demo on working with projects. [web:5]

## Ways to interact with Google Cloud
- **Cloud Console (web GUI):** Manage resources in a browser at `console.cloud.google.com` (for example, view and manage VM instances). [web:5]
- **Cloud Shell & gcloud CLI:** Use a browser-based shell VM with 5 GB persistent storage and the `gcloud` command-line tool to create and manage resources. [web:1][web:5]
- **APIs & client libraries:** Use App APIs (for application services) and Admin APIs (for resource management and automation) via supported languages like Python or Node.js. [web:5]
- **Cloud Mobile App:** From Android or iOS, start/stop VMs, SSH into instances, view logs and metrics, and monitor billing and alerts. [web:5]

## Using the Cloud Console in labs
- Follow GUI lab instructions like: open the navigation menu (three horizontal lines), choose a product (for example, Compute Engine), then select the specific resource page (for example, VM instances). [web:14]
- Repeated console navigation in labs builds familiarity with products and their locations. [web:14]

## Using command line in labs
- Run provided `gcloud` commands in Cloud Shell or an SSH terminal, often by copy–paste. [web:1][web:17]
- Some commands must be edited, such as inserting a unique bucket name or project ID. [web:1]

## Projects basics
- A project is the core container that groups Google Cloud resources and links them to a billing account. [web:12]
- Resources exist inside a single project, which provides isolation, quotas, and IAM boundaries. [web:12][web:15]

## Managing projects in the Console
- Create a project by opening the project picker, clicking “New project,” and setting a project name; a unique project ID is generated automatically. [web:12]
- After creation, you can open project settings, switch to another project, or schedule a project shutdown from the console. [web:12]
- Shutting down a project stops usage and billing after a grace period, during which you can undo the deletion. [web:12]

## Switching projects with Cloud Shell
- Use `gcloud config list` to see the current active project in Cloud Shell. [web:16][web:19]
- Change the active project with `gcloud config set project PROJECT_ID`. [web:16][web:19]
- Optionally store project IDs in environment variables to switch quickly between multiple projects. [web:19]
