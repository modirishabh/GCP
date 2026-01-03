# Security in Google Cloud
Understanding Google Cloud Security
Google Cloud's security is built on two main pillars: trusted cloud infrastructure and encryption at rest.

Trusted Cloud Infrastructure:

Google Cloud's infrastructure is designed to be secure by default, implementing security through various layers, including operational and device security.
Internet communications are encrypted in transit to ensure data is secure as it travels over the internet.
Identity security involves authenticating users and services, as well as protecting sensitive data with security keys.
Storage security means that any data uploaded to Google Cloud is automatically encrypted at rest, preventing unauthorized access.
Service deployment involves cryptographic methods for authentication and authorization to secure communication between services.
The hardware infrastructure, especially for on-premises services, is controlled, secured, and hardened by Google.


Encryption at Rest:

Google Cloud encrypts each chunk of uploaded data with a unique key using the 256-bit advanced encryption standard (AES), turning it into ciphertext.
This encryption ensures that even if data were to be accessed by unauthorized parties, it would be unusable without the corresponding decryption key.
Encrypted data chunks are distributed across Google's storage infrastructure for enhanced security.
Welcome back, Gurus. In this session, we delve into the intricate layers of security within Google Cloud. Our focus is on trust, security, and understanding the six layers of trusted cloud infrastructure, alongside the workings of encryption at rest.

## Trust and Security in Google Cloud

With any public cloud provider, the fundamental question is whether we can trust the platform with our sensitive data. Google Cloud addresses this with a dual focus on:

- **Trusted Cloud Infrastructure**: It employs a layered approach to security, encompassing operational and device security. The infrastructure is designed with best security practices to protect both internal and external end-user infrastructure.

- **Encryption at Rest**: Data uploaded to Google Cloud is not only stored securely but also encrypted automatically, ensuring protection against unauthorized access.

## Six Layers of Security

1. **Internet Security**: Communication over the internet is encrypted in transit.
2. **Identity Security**: User identities and services are authenticated, and sensitive data is secured using keys.
3. **Storage Security**: Uploaded data is encrypted at rest within Google Cloud.
4. **Service Deployment**: Services are deployed using cryptographic methods for secure inner-service communication.
5. **Hardware Infrastructure**: Google's on-premises service hardware is controlled, secured, and hardened.

## Encryption at Rest

When you upload data to Google Cloud, it is encrypted in chunks, each with its own unique key, using the 256-bit AES. This approach ensures that your data remains protected and inaccessible to unauthorized entities.

## Key Takeaways

- Google Cloud's security is founded on trusted infrastructure and encryption at rest.
- There are six distinct layers of security built into the trusted cloud infrastructure.
- Encryption at rest is pivotal in protecting your stored data.



Thank you for exploring this brief overview of Google Cloud's security features. As we continue to learn more about Google Cloud and prepare for the Associate Cloud Engineer exam, stay tuned for the next video. See you then, Gurus!
