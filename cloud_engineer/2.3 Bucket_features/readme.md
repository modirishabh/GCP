# Bucket Features in Google Cloud Storage

Google Cloud Storage Bucket Features
Google Cloud Storage offers essential features such as Bucket Locking and Object Lifecycle Management to help you control and automate the management of your data.

## Bucket Locking
Bucket Locking is a feature that allows you to set retention policies for your stored data, ensuring that it cannot be deleted until the specified retention period has passed. This is crucial for compliance and data protection strategies.

## 1. Retention Policies: 
Define how long data should be stored and prevent premature deletion of data.
## 2. Retention Periods: 
Specify the exact duration for retaining objects, ranging from one day (84,600 seconds) to 100 years (3,155,760,000 seconds).

## Object Lifecycle Management
With Object Lifecycle Management, you can automate the process of managing your objects' lifecycle based on predefined rules. These rules can:

- Set expiration times for objects to be deleted.
- Change the storage class of objects to optimize costs.
- Maintain only necessary versions of objects to prevent data bloat.

## Time to Live: Set expiration dates for objects to be automatically deleted.
## Version Management: Automatically manage versions of objects, keeping only the most recent or necessary versions.
## Storage Class Changes: Automate the transition of objects to different storage classes based on access patterns to optimize costs.

## Lifecycle Actions
## 1. Delete: 
Automatically delete objects after a specified period.
## 2. SetStorageClass: 
Transition objects to different storage classes like Standard, Nearline, Coldline, or Archive based on conditions like age, creation date, or number of newer versions.
