# Shared VPC in Google Cloud Platform (GCP)

Welcome back, Gurus. This lesson delves into Shared VPC, a powerful feature in GCP that allows for cross-project resource sharing and streamlined network management.

## Introduction to Shared VPC

Shared VPC enables projects within an organization to connect and utilize shared network resources, fostering collaboration while maintaining centralized control.

## Key Aspects of Shared VPC

- **Host and Service Projects**: Define the structure of Shared VPC, with the host project containing the VPC and the service projects utilizing its resources.
- **Subnet Sharing**: Allows specific subnets within the VPC to be shared with service projects.
- **Access Control**: Through IAM roles, users are granted permission to deploy resources within the shared subnets.

## Hands-On Demo: Using Shared VPC

The practical demonstration included:

1. Establishing a Shared VPC in a host project.
2. Sharing a dev subnet with a service project for development purposes.
3. Assigning the `compute.networkUser` role to a user, enabling them to create VM instances in the shared subnet.
4. Launching an instance in the shared subnet as a user with the appropriate permissions.

## Key Takeaways for the Exam

- Understand the architecture of Shared VPC, including host and service projects.
- Learn how to share network resources across projects securely.
- Recognize the importance of IAM roles in managing access to shared subnets.

Thank you for exploring Shared VPC with me. Be sure to review this lesson and practice in your own account to solidify your understanding of how Shared VPC works. See you in the next video!
