---
title: Bottlerocket OS — Operating system meant for hosting containers
categories:
- AWS
- Kubernetes
- DevOps
# feature_image: ""
---
---

##### What is Bottlerocket OS?

  Bottlerocket is a free and open-source Linux-based operating system purpose-built by Amazon Web Services meant for hosting/running containers.

##### Why to choose Bottlerocket over other general purpose Linux distributions?
  *Higher uptime with Improved security and resource utilization:*
  Hosting/running containers doesn’t require anything special from an operating system and hosting containers is all Bottlerocket aims to do. Other general purpose Linux distributions by default comes with many of the packages, tools (such as yum, ssh, tcpdump, Netconf ), dependencies installed and which are simply not required if the intend is to only host/run containers. By excluding these extraneous pieces of tools, the operational and security overhead is much reduced. As Bottlerocket has a smaller resource footprint, it has shorter boot times, and a smaller security attack surface compared to other Linux distributions.

  *Improved security from automatic OS updates:*
  Security updates to Bottlerocket are applied in a single step and can be rolled back if necessary, resulting in lower error rates and improved uptime for container applications. On other side, general-purpose Linux distributions are typically updated package-by-package.

  *CIS hardening out of the box:*
  AWS’s Bottlerocket has been certified by the Center for Internet Security(CIS) to ship secure as hardened to CIS Bottlerocket Benchmark v1.0.0. Organizations that leverage Bottlerocket can now be assured that it will successfully run on a CIS hardened environment.

  *Premium Support:*
  The use of AWS-provided builds of Bottlerocket on Amazon EC2 is covered under the same AWS support plans that also cover AWS services such as Amazon EC2, Amazon EKS, Amazon ECR.

  *No additional cost:*
  AWS-provided builds of Bottlerocket are available at no additional cost.

##### How to switch to Bottlerocket from other general purpose Linux distributions for AWS EKS setup?
  Assumptions: You already have EKS cluster setup, Auto scaling group with launch template.
  You can follow these manual steps to switch to Bottlerocket.

  1. **Finding an AMI:** 
    Retrieve the image ID of the latest recommended Amazon EKS optimized Bottlerocket AMI using AWS CLI command by using the sub-parameter image_id Note: Replace 1.29 with a supported version and region-code with an Amazon EKS supported Region for which you want the retrieve the AMI ID
    ```
    aws ssm get-parameter --name /aws/service/bottlerocket/aws-k8s-1.29/x86_64/latest/image_id --region region-code --query "Parameter.Value" --output text
    ```
> You might get Access Denied error, if you don’t have suitable permissions. Add the following policy to your role/IAM user. Once you have the access, Re-run the above AWS CLI command.

    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeParameters"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameters",
                "ssm:GetParametersByPath"
            ],
            "Resource": "arn:aws:ssm:::parameter/*"
        }
    ]

 2. **Modify the launch template:**
    From the AWS console, update the current launch template for your EKS cluster to specify the Bottlerocket AMI to be used. One important thing is user data which contains additional arguments to kubelet service. The user data must be in TOML format and must contains settings.kubernetes.cluster-certificate, settings.kubernetes.api-server, and settings.kubernetes.cluster-name This is important for Bottlerocket to provide configuration details so that the nodes will be able to enroll into EKS cluster. We can easily get this information from the EKS console or use following eksctl command to retrieve these values. Please ensure you have eksctl, jq installed. Also make sure to replace region-code and my-cluster-name with the actual values as per your setup.
    ```
    eksctl get cluster --region region-code --name cluster-name -o json \  | jq - raw-output '.[] | "[settings.kubernetes]\napi-server = \"" + .Endpoint + "\"\ncluster-certificate =\"" + .CertificateAuthority.Data + "\"\ncluster-name = \"my-cluster-name\""' > user-data.toml
    ```
    Following is user data example.
    ```
    [settings.kubernetes]
    api-server = "https://abcdef.ghijk.us-east-1.eks.amazonaws.com"
    cluster-certificate = "KQaQ0bTVSqMSUNBEVdahSLA0tHr="
    cluster-name = "test-eks"
    [settings.host-containers.admin]
    enabled = true
    superpowered = true
    ```
    > Note: Bottlerocket has an administrative container, disabled by default, that runs outside of the orchestrator in a separate instance of containerd. This container has an SSH server that lets you log in as ec2-user using your EC2-registered SSH key. Outside of AWS, you can pass in your own SSH keys.

    To enable the container, you can change the setting in user data when starting Bottlerocket, for example EC2 instance user data:
    ```
    [settings.host-containers.admin]
    enabled = true
    ```
    If Bottlerocket is already running, you can enable the admin container from the default control container like this:
            enable-admin-container
    Or you can start an interactive session immediately like this:
            enter-admin-container
  

  3. **Set the default launch template version:**
    From the AWS console, set the default version for the launch template with the one you just modified above.

  
  4. **Update node group version:** 
    From the AWS console, select the same launch template version as above. Select the Update strategy as Rolling Update and click on Update to start refreshing the nodes with the Bottlerocket AMI.

##### Conclusion
  We have seen quick and easy steps to switch using Bottlerocket OS on AWS EKS. Bottlerocket really helps to run containers efficiently, enhance better security and ensure operational excellence.

---
##### References:
  [Bottlerocket-Github](https://github.com/bottlerocket-os/bottlerocket)  
  [Bottlerocket-AWS](https://aws.amazon.com/bottlerocket/?amazon-bottlerocket-whats-new.sort-by=item.additionalFields.postDateTime&amazon-bottlerocket-whats-new.sort-order=desc)  