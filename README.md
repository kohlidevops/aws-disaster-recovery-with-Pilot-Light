# aws-disaster-recovery-with-Pilot-Light

Imagine you have a complex application with multiple components, including web servers, application servers, and databases. In a pilot light scenario, you maintain a minimal version of your environment in AWS. For instance, you keep a scaled-down version of your application's infrastructure always running, with a smaller number of instances. If a disaster occurs, you can quickly scale up this pilot light environment by launching additional instances, restoring databases, and redirecting traffic.

Before start this method, Please deploy workloads using below link

**https://github.com/kohlidevops/aws-disaster-recovery/blob/main/README.md**

**https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/blob/main/README.md**

**https://github.com/kohlidevops/aws-disaster-recovery-with-region-outage/blob/main/README.md**

From now, NVirginia region is Primary and Mumbai is Secondary region.

//Delete PITR, Backup plan and DynamoDB in Mumbai region.

Back to Lambda (Mumbai region) – productDynamoDB lambda – update below code and deploy.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/bc2f70c7-a662-4d7f-a584-99e7c4ff9c32)

**To enable PITR for DynamoDB** (NVirginia – it is primary region)

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/c0597f60-06ed-4c6f-9fda-be6e2e8f46b2)

**To create Schedule Backup**

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/7d225195-eafb-4188-9734-90c99373fc7c)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/0a899e90-35c0-4b28-bdc9-bbc0c6451efa)

Then create a new plan and do resource assignment with DynamoDB.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/857a70d9-5cd0-4cdb-85e8-c92c37ac6987)

**To enable Global table in DynamoDB**

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/8a4fdd92-1d0c-46af-a64e-a2d3090f45bd)
![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/f1c3d96d-8b78-437d-8b90-fd42875db582)

Once a replica is created, then explore the items in product DynamoDB (Mumbai region). Yes, Everything replicated.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/261fa0ae-dd73-4145-aa33-ced5d7ce7362)

Then add some data in NVirginia application.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/b0e4e589-a1af-40dd-a40f-278c511d313a)

Everything replicated in Mumbai Region as quickly as possible.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/d5270da5-a5bf-4e31-b50c-3cc093d9bb8f)

If NVirginia region is completely outage, then

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/0a45005a-531d-41b4-8223-3eb37fcaf762)

Go back to Mumbai region (secondary) and start EC2 instance (which is stopped state) and Create Loadbalancer and mapped with this instance.

Secondary instance has been up and running.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/66e12d77-094b-485d-a618-5fefc85fec93)

Then navigate to Route53 and update the DNS record with Mumbai ALB DNS.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/d33d253a-ff98-45ca-8f29-82ab2b20adf6)

Once mapped, we can check with domain name. Here we go, I can able to see the domain which is mapped with secondary region instance. We promoted secondary instance as primary instance.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/assets/100069489/8c3326b0-0c2d-4b12-b7d6-89a8bddb73f3)

That's it.













