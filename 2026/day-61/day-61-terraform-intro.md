# Day 61 -- Introduction to Terraform and Your First AWS Infrastructure

## Task 1: Understand Infrastructure as Code

### What is Infrastructure as Code (IaC)? Why does it matter in DevOps?
IaC is the practice of managing infrastructure through code rather than manual configuration. It helps DevOps by enabling automated, consistent, repeatable, and version-controlled infrastructure provisioning. Terraform is a common example of an IaC tool

### What problems does IaC solve compared to manually creating resources in the AWS console?

IaC eliminates most manual infrastructure provisioning. It makes the process automated, consistent, repeatable, version-controlled, and less prone to human error.


### How is Terraform different from AWS CloudFormation, Ansible, and Pulumi?

For a multi-cloud environment, I would generally prefer Terraform because of its broad provider ecosystem and declarative approach. For an AWS-only environment, CloudFormation is also a strong option. For server configuration and application deployment, I would use Ansible.

### What does it mean that Terraform is "declarative" and "cloud-agnostic"?

Declarative means I define the desired end state and Terraform determines the required actions, while cloud-agnostic means Terraform can manage resources across different cloud providers


## Task 2: Install Terraform and Configure AWS

### Installed and configured Terraform and the AWS CLI, and verified AWS access successfully.

```bash
ubuntu@ip-172-31-16-197:~$ wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
--2026-10-04 16:36:33--  https://apt.releases.hashicorp.com/gpg
Resolving apt.releases.hashicorp.com (apt.releases.hashicorp.com)... 3.163.24.60, 3.163.24.7, 3.163.24.95, ...
Connecting to apt.releases.hashicorp.com (apt.releases.hashicorp.com)|3.163.24.60|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1725 (1.7K) [application/x-www-form-urlencoded]
Saving to: ‘STDOUT’

-                                       100%[============================================================================>]   1.68K  --.-KB/s    in 0s

2026-10-04 16:36:33 (140 MB/s) - written to stdout [1725/1725]

deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com resolute main
Hit:1 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute InRelease
Hit:2 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute-updates InRelease
Hit:3 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute-backports InRelease
Get:4 https://apt.releases.hashicorp.com resolute InRelease [12.9 kB]
Hit:5 http://security.ubuntu.com/ubuntu resolute-security InRelease
Get:6 https://apt.releases.hashicorp.com resolute/main amd64 Packages [324 kB]
Fetched 337 kB in 0s (910 kB/s)
186 packages can be upgraded. Run 'apt list --upgradable' to see them.
Installing:
  terraform

Summary:
  Upgrading: 0, Installing: 1, Removing: 0, Not Upgrading: 186
  Download size: 35.9 MB
  Space needed: 121 MB / 11.9 GB available

Get:1 https://apt.releases.hashicorp.com resolute/main amd64 terraform amd64 1.16.5-1 [35.9 MB]
Fetched 35.9 MB in 0s (122 MB/s)
Selecting previously unselected package terraform.
(Reading database ... 85303 files and directories currently installed.)
Preparing to unpack .../terraform_1.16.5-1_amd64.deb ...
Unpacking terraform (1.16.5-1) ...
Setting up terraform (1.16.5-1) ...
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
ubuntu@ip-172-31-16-197:~$ terraform -version
Terraform v1.16.5
on linux_amd64
ubuntu@ip-172-31-16-197:~$ aws configure
Command 'aws' not found, but can be installed with:
sudo snap install aws-cli  # version 1.45.46, or
sudo apt  install awscli   # version 2.31.35-1
See 'snap info aws-cli' for additional versions.
ubuntu@ip-172-31-16-197:~$
ubuntu@ip-172-31-16-197:~$ sudo apt  install awscli
Installing:
  awscli

Installing dependencies:
  docutils-common  libimagequant0  liblcms2-2      libpaper2     libwebp7        python3-colorama  python3-prompt-toolkit    python3-wcwidth
  libdeflate0      libjbig0        liblerc4        libraqm0      libwebpdemux2   python3-docutils  python3-roman-numerals
  libgraphite2-3   libjpeg-turbo8  libopenjp2-7    libsharpyuv0  libwebpmux3     python3-olefile   python3-ruamel.yaml
  libharfbuzz0b    libjpeg8        libpaper-utils  libtiff6      python3-awscrt  python3-pil       python3-ruamel.yaml.clib

Suggested packages:
  liblcms2-utils  fonts-linuxlibertine   texlive-lang-french  texlive-latex-recommended
  docutils-doc    | ttf-linux-libertine  texlive-latex-base   python-pil-doc

Summary:
  Upgrading: 0, Installing: 30, Removing: 0, Not Upgrading: 186
  Download size: 16.0 MB
  Space needed: 148 MB / 11.8 GB available

Continue? [Y/n] y
Get:1 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/universe amd64v3 python3-awscrt amd64 0.28.4+dfsg-1 [954 kB]
Get:2 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/universe amd64v3 python3-colorama all 0.4.6-4build1 [32.2 kB]
Get:3 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 docutils-common all 0.22.4+dfsg-1 [130 kB]
Get:4 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 python3-roman-numerals all 4.1.0-1 [8660 B]
Get:5 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 python3-docutils all 0.22.4+dfsg-1 [439 kB]
Get:6 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 python3-wcwidth all 0.2.14+dfsg1-1build1 [26.5 kB]
Get:7 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/universe amd64v3 python3-prompt-toolkit all 3.0.52-2 [258 kB]
Get:8 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/universe amd64v3 python3-ruamel.yaml.clib amd64 0.2.15+ds-1build1 [140 kB]
Get:9 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/universe amd64v3 python3-ruamel.yaml all 0.18.10+ds-1build1 [127 kB]
Get:10 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/universe amd64v3 awscli all 2.31.35-1 [11.1 MB]
Get:11 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libdeflate0 amd64 1.23-2ubuntu1 [48.9 kB]
Get:12 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute-updates/main amd64v3 libgraphite2-3 amd64 1.3.14-11ubuntu1.1 [72.5 kB]
Get:13 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libharfbuzz0b amd64 12.3.2-2 [525 kB]
Get:14 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libimagequant0 amd64 4.4.1-1 [264 kB]
Get:15 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libjpeg-turbo8 amd64 2.1.5-4ubuntu4 [162 kB]
Get:16 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libjpeg8 amd64 8c-2ubuntu12 [2154 B]
Get:17 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute-updates/main amd64v3 liblcms2-2 amd64 2.17-1ubuntu0.2 [172 kB]
Get:18 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 liblerc4 amd64 4.0.0+ds-5ubuntu2 [212 kB]
Get:19 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libpaper2 amd64 2.2.5-0.3maysync1 [17.3 kB]
Get:20 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libpaper-utils amd64 2.2.5-0.3maysync1 [15.6 kB]
Get:21 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libraqm0 amd64 0.10.4-1 [15.6 kB]
Get:22 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libsharpyuv0 amd64 1.5.0-0.1build1 [17.6 kB]
Get:23 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libjbig0 amd64 2.1-6.1ubuntu3 [30.4 kB]
Get:24 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libwebp7 amd64 1.5.0-0.1build1 [268 kB]
Get:25 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute-updates/main amd64v3 libtiff6 amd64 4.7.0-3ubuntu5 [209 kB]
Get:26 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libwebpdemux2 amd64 1.5.0-0.1build1 [12.9 kB]
Get:27 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 libwebpmux3 amd64 1.5.0-0.1build1 [26.4 kB]
Get:28 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute/main amd64v3 python3-olefile all 0.47-1build1 [37.2 kB]
Get:29 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute-updates/main amd64v3 libopenjp2-7 amd64 2.5.4-1ubuntu0.1 [190 kB]
Get:30 http://us-west-2.ec2.archive.ubuntu.com/ubuntu resolute-updates/main amd64v3 python3-pil amd64 12.1.1-2ubuntu1.3 [489 kB]
Fetched 16.0 MB in 0s (35.9 MB/s)
Preconfiguring packages ...
Selecting previously unselected package python3-awscrt.
(Reading database ... 85306 files and directories currently installed.)
Preparing to unpack .../00-python3-awscrt_0.28.4+dfsg-1_amd64v3.deb ...
Unpacking python3-awscrt (0.28.4+dfsg-1) ...
Selecting previously unselected package python3-colorama.
Preparing to unpack .../01-python3-colorama_0.4.6-4build1_all.deb ...
Unpacking python3-colorama (0.4.6-4build1) ...
Selecting previously unselected package docutils-common.
Preparing to unpack .../02-docutils-common_0.22.4+dfsg-1_all.deb ...
Unpacking docutils-common (0.22.4+dfsg-1) ...
Selecting previously unselected package python3-roman-numerals.
Preparing to unpack .../03-python3-roman-numerals_4.1.0-1_all.deb ...
Unpacking python3-roman-numerals (4.1.0-1) ...
Selecting previously unselected package python3-docutils.
Preparing to unpack .../04-python3-docutils_0.22.4+dfsg-1_all.deb ...
Unpacking python3-docutils (0.22.4+dfsg-1) ...
Selecting previously unselected package python3-wcwidth.
Preparing to unpack .../05-python3-wcwidth_0.2.14+dfsg1-1build1_all.deb ...
Unpacking python3-wcwidth (0.2.14+dfsg1-1build1) ...
Selecting previously unselected package python3-prompt-toolkit.
Preparing to unpack .../06-python3-prompt-toolkit_3.0.52-2_all.deb ...
Unpacking python3-prompt-toolkit (3.0.52-2) ...
Selecting previously unselected package python3-ruamel.yaml.clib.
Preparing to unpack .../07-python3-ruamel.yaml.clib_0.2.15+ds-1build1_amd64v3.deb ...
Unpacking python3-ruamel.yaml.clib (0.2.15+ds-1build1) ...
Selecting previously unselected package python3-ruamel.yaml.
Preparing to unpack .../08-python3-ruamel.yaml_0.18.10+ds-1build1_all.deb ...
Unpacking python3-ruamel.yaml (0.18.10+ds-1build1) ...
Selecting previously unselected package awscli.
Preparing to unpack .../09-awscli_2.31.35-1_all.deb ...
Unpacking awscli (2.31.35-1) ...
Selecting previously unselected package libdeflate0:amd64.
Preparing to unpack .../10-libdeflate0_1.23-2ubuntu1_amd64v3.deb ...
Unpacking libdeflate0:amd64 (1.23-2ubuntu1) ...
Selecting previously unselected package libgraphite2-3:amd64.
Preparing to unpack .../11-libgraphite2-3_1.3.14-11ubuntu1.1_amd64v3.deb ...
Unpacking libgraphite2-3:amd64 (1.3.14-11ubuntu1.1) ...
Selecting previously unselected package libharfbuzz0b:amd64.
Preparing to unpack .../12-libharfbuzz0b_12.3.2-2_amd64v3.deb ...
Unpacking libharfbuzz0b:amd64 (12.3.2-2) ...
Selecting previously unselected package libimagequant0:amd64.
Preparing to unpack .../13-libimagequant0_4.4.1-1_amd64v3.deb ...
Unpacking libimagequant0:amd64 (4.4.1-1) ...
Selecting previously unselected package libjpeg-turbo8:amd64.
Preparing to unpack .../14-libjpeg-turbo8_2.1.5-4ubuntu4_amd64v3.deb ...
Unpacking libjpeg-turbo8:amd64 (2.1.5-4ubuntu4) ...
Selecting previously unselected package libjpeg8:amd64.
Preparing to unpack .../15-libjpeg8_8c-2ubuntu12_amd64v3.deb ...
Unpacking libjpeg8:amd64 (8c-2ubuntu12) ...
Selecting previously unselected package liblcms2-2:amd64.
Preparing to unpack .../16-liblcms2-2_2.17-1ubuntu0.2_amd64v3.deb ...
Unpacking liblcms2-2:amd64 (2.17-1ubuntu0.2) ...
Selecting previously unselected package liblerc4:amd64.
Preparing to unpack .../17-liblerc4_4.0.0+ds-5ubuntu2_amd64v3.deb ...
Unpacking liblerc4:amd64 (4.0.0+ds-5ubuntu2) ...
Selecting previously unselected package libpaper2:amd64.
Preparing to unpack .../18-libpaper2_2.2.5-0.3maysync1_amd64v3.deb ...
Unpacking libpaper2:amd64 (2.2.5-0.3maysync1) ...
Selecting previously unselected package libpaper-utils.
Preparing to unpack .../19-libpaper-utils_2.2.5-0.3maysync1_amd64v3.deb ...
Unpacking libpaper-utils (2.2.5-0.3maysync1) ...
Selecting previously unselected package libraqm0:amd64.
Preparing to unpack .../20-libraqm0_0.10.4-1_amd64v3.deb ...
Unpacking libraqm0:amd64 (0.10.4-1) ...
Selecting previously unselected package libsharpyuv0:amd64.
Preparing to unpack .../21-libsharpyuv0_1.5.0-0.1build1_amd64v3.deb ...
Unpacking libsharpyuv0:amd64 (1.5.0-0.1build1) ...
Selecting previously unselected package libjbig0:amd64.
Preparing to unpack .../22-libjbig0_2.1-6.1ubuntu3_amd64v3.deb ...
Unpacking libjbig0:amd64 (2.1-6.1ubuntu3) ...
Selecting previously unselected package libwebp7:amd64.
Preparing to unpack .../23-libwebp7_1.5.0-0.1build1_amd64v3.deb ...
Unpacking libwebp7:amd64 (1.5.0-0.1build1) ...
Selecting previously unselected package libtiff6:amd64.
Preparing to unpack .../24-libtiff6_4.7.0-3ubuntu5_amd64v3.deb ...
Unpacking libtiff6:amd64 (4.7.0-3ubuntu5) ...
Selecting previously unselected package libwebpdemux2:amd64.
Preparing to unpack .../25-libwebpdemux2_1.5.0-0.1build1_amd64v3.deb ...
Unpacking libwebpdemux2:amd64 (1.5.0-0.1build1) ...
Selecting previously unselected package libwebpmux3:amd64.
Preparing to unpack .../26-libwebpmux3_1.5.0-0.1build1_amd64v3.deb ...
Unpacking libwebpmux3:amd64 (1.5.0-0.1build1) ...
Selecting previously unselected package python3-olefile.
Preparing to unpack .../27-python3-olefile_0.47-1build1_all.deb ...
Unpacking python3-olefile (0.47-1build1) ...
Selecting previously unselected package libopenjp2-7:amd64.
Preparing to unpack .../28-libopenjp2-7_2.5.4-1ubuntu0.1_amd64v3.deb ...
Unpacking libopenjp2-7:amd64 (2.5.4-1ubuntu0.1) ...
Selecting previously unselected package python3-pil:amd64.
Preparing to unpack .../29-python3-pil_12.1.1-2ubuntu1.3_amd64v3.deb ...
Unpacking python3-pil:amd64 (12.1.1-2ubuntu1.3) ...
Setting up python3-awscrt (0.28.4+dfsg-1) ...
Setting up libgraphite2-3:amd64 (1.3.14-11ubuntu1.1) ...
Setting up liblcms2-2:amd64 (2.17-1ubuntu0.2) ...
Setting up libsharpyuv0:amd64 (1.5.0-0.1build1) ...
Setting up liblerc4:amd64 (4.0.0+ds-5ubuntu2) ...
Setting up python3-colorama (0.4.6-4build1) ...
Setting up python3-olefile (0.47-1build1) ...
Setting up python3-ruamel.yaml.clib (0.2.15+ds-1build1) ...
Setting up libdeflate0:amd64 (1.23-2ubuntu1) ...
Setting up libjbig0:amd64 (2.1-6.1ubuntu3) ...
Setting up python3-wcwidth (0.2.14+dfsg1-1build1) ...
Setting up libimagequant0:amd64 (4.4.1-1) ...
Setting up libjpeg-turbo8:amd64 (2.1.5-4ubuntu4) ...
Setting up libwebp7:amd64 (1.5.0-0.1build1) ...
Setting up python3-ruamel.yaml (0.18.10+ds-1build1) ...
Setting up docutils-common (0.22.4+dfsg-1) ...
Setting up python3-roman-numerals (4.1.0-1) ...
Setting up libopenjp2-7:amd64 (2.5.4-1ubuntu0.1) ...
Setting up libharfbuzz0b:amd64 (12.3.2-2) ...
Setting up libpaper2:amd64 (2.2.5-0.3maysync1) ...
Setting up libwebpmux3:amd64 (1.5.0-0.1build1) ...
Setting up libjpeg8:amd64 (8c-2ubuntu12) ...
Setting up python3-prompt-toolkit (3.0.52-2) ...
Setting up libwebpdemux2:amd64 (1.5.0-0.1build1) ...
Setting up libpaper-utils (2.2.5-0.3maysync1) ...
Setting up libraqm0:amd64 (0.10.4-1) ...
Setting up libtiff6:amd64 (4.7.0-3ubuntu5) ...
Setting up python3-pil:amd64 (12.1.1-2ubuntu1.3) ...
Processing triggers for man-db (2.13.1-1build1) ...
Processing triggers for sgml-base (1.31+nmu1build1) ...
Setting up python3-docutils (0.22.4+dfsg-1) ...
Processing triggers for libc-bin (2.43-2ubuntu2) ...
Setting up awscli (2.31.35-1) ...
/usr/lib/python3/dist-packages/awscli/customizations/s3/s3handler.py:595: SyntaxWarning: 'return' in a 'finally' block
  return True
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
ubuntu@ip-172-31-16-197:~$ aws configure
AWS Access Key ID [None]: AKIA3Y7**************************
AWS Secret Access Key [None]: SRbwNB7c2Q**************************
Default region name [None]:
Default output format [None]:
ubuntu@ip-172-31-16-197:~$ aws sts get-caller-identity
{
    "UserId": "8095545****",
    "Account": "8095545*******",
    "Arn": "arn:aws:iam::80955********:root"
}
ubuntu@ip-172-31-16-197:~$

```

## Task 3: Your First Terraform Config -- Create an S3 Bucket



```bash
ubuntu@ip-172-31-16-197:~$ ls
terraform-basics
ubuntu@ip-172-31-16-197:~$ cd terraform-basics/
ubuntu@ip-172-31-16-197:~/terraform-basics$ ls
ubuntu@ip-172-31-16-197:~/terraform-basics$ vim provider.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$ mv provider.tf versions.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$ ls
versions.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$ vim provider.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform init
Initializing the backend...

Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 6.0"...
- Installing hashicorp/aws v6.67.0...
- Installed hashicorp/aws v6.67.0 (signed by HashiCorp)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
ubuntu@ip-172-31-16-197:~/terraform-basics$ ls
provider.tf  versions.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$ ls -la
total 24
drwxrwxr-x 3 ubuntu ubuntu 4096 Oct  4 17:01 .
drwxr-x--- 7 ubuntu ubuntu 4096 Oct  4 17:00 ..
drwxr-xr-x 3 ubuntu ubuntu 4096 Oct  4 17:01 .terraform
-rw-r--r-- 1 ubuntu ubuntu 1481 Oct  4 17:01 .terraform.lock.hcl
-rw-rw-r-- 1 ubuntu ubuntu   42 Oct  4 17:00 provider.tf
-rw-rw-r-- 1 ubuntu ubuntu  116 Oct  4 16:57 versions.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$ ls
provider.tf  versions.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$ vim main.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform init
Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Using previously-installed hashicorp/aws v6.67.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.my_bucket will be created
  + resource "aws_s3_bucket" "my_bucket" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "my-terraform-s3-bucket-123456789"
      + bucket_domain_name          = (known after apply)
      + bucket_namespace            = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_region               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = "us-west-2"
      + request_payer               = (known after apply)
      + tags_all                    = (known after apply)
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)

      + grant (known after apply)

      + lifecycle_rule (known after apply)

      + logging (known after apply)

      + object_lock_configuration (known after apply)

      + replication_configuration (known after apply)

      + server_side_encryption_configuration (known after apply)

      + versioning (known after apply)

      + website (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.my_bucket will be created
  + resource "aws_s3_bucket" "my_bucket" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "my-terraform-s3-bucket-123456789"
      + bucket_domain_name          = (known after apply)
      + bucket_namespace            = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_region               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = "us-west-2"
      + request_payer               = (known after apply)
      + tags_all                    = (known after apply)
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)

      + grant (known after apply)

      + lifecycle_rule (known after apply)

      + logging (known after apply)

      + object_lock_configuration (known after apply)

      + replication_configuration (known after apply)

      + server_side_encryption_configuration (known after apply)

      + versioning (known after apply)

      + website (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_s3_bucket.my_bucket: Creating...
╷
│ Error: creating S3 Bucket (my-terraform-s3-bucket-123456789): operation error S3: CreateBucket, https response error StatusCode: 409, RequestID: 6DTJGTJYQEMBWYFJ, HostID: Ys+K5X2n+xSVT45l5Im88dyskxKM8bQremOA/+2saMu4NVFtFGf2wAKYWsqtYzRpgZcG6lJLplk=, BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.
│
│   with aws_s3_bucket.my_bucket,
│   on main.tf line 1, in resource "aws_s3_bucket" "my_bucket":
│    1: resource "aws_s3_bucket" "my_bucket" {
│
╵
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$ l
main.tf  provider.tf  terraform.tfstate  versions.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$ vim main.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.my_bucket will be created
  + resource "aws_s3_bucket" "my_bucket" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "abusufiyan-terraform-s3-2026-847291"
      + bucket_domain_name          = (known after apply)
      + bucket_namespace            = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_region               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = "us-west-2"
      + request_payer               = (known after apply)
      + tags_all                    = (known after apply)
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)

      + grant (known after apply)

      + lifecycle_rule (known after apply)

      + logging (known after apply)

      + object_lock_configuration (known after apply)

      + replication_configuration (known after apply)

      + server_side_encryption_configuration (known after apply)

      + versioning (known after apply)

      + website (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.my_bucket will be created
  + resource "aws_s3_bucket" "my_bucket" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "abusufiyan-terraform-s3-2026-847291"
      + bucket_domain_name          = (known after apply)
      + bucket_namespace            = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_region               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = "us-west-2"
      + request_payer               = (known after apply)
      + tags_all                    = (known after apply)
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)

      + grant (known after apply)

      + lifecycle_rule (known after apply)

      + logging (known after apply)

      + object_lock_configuration (known after apply)

      + replication_configuration (known after apply)

      + server_side_encryption_configuration (known after apply)

      + versioning (known after apply)

      + website (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_s3_bucket.my_bucket: Creating...
aws_s3_bucket.my_bucket: Creation complete after 0s [id=abusufiyan-terraform-s3-2026-847291]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
ubuntu@ip-172-31-16-197:~/terraform-basics$

```

<img width="1365" height="767" alt="image" src="https://github.com/user-attachments/assets/cb5c4c43-b166-4b57-98a6-615e0a84359c" />

<img width="1365" height="726" alt="image" src="https://github.com/user-attachments/assets/481304f7-c69e-4ef9-a915-1d2c438d813e" />

### Document: What did terraform init download? What does the .terraform/ directory contain?
terraform init initializes the Terraform working directory. It downloads the required provider plugins and initializes the configured backend. The .terraform/ directory contains the downloaded provider plugins and other files required by Terraform to manage the working directory.

## Task 4: Add an EC2 Instance

### Created an aws_instance resource using the appropriate AMI for the region, configured the instance type as t3.micro, and added the tag Name = "TerraWeek-Day1"

```bash
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform init
Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Using previously-installed hashicorp/aws v6.67.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform plan
data.aws_ami.ubuntu: Reading...
aws_s3_bucket.app_bucket: Refreshing state... [id=abusufiyan-terraform-s3-2026-847291]
data.aws_ami.ubuntu: Read complete after 1s [id=ami-07a134137a631b892]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.app_server will be created
  + resource "aws_instance" "app_server" {
      + ami                                  = "ami-07a134137a631b892"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + enable_primary_ipv6                  = (known after apply)
      + force_destroy                        = false
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t3.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_group_id                   = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + region                               = "us-west-2"
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags                                 = {
          + "Name" = "learn-terraform"
        }
      + tags_all                             = {
          + "Name" = "learn-terraform"
        }
      + tenancy                              = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)

      + capacity_reservation_specification (known after apply)

      + cpu_options (known after apply)

      + ebs_block_device (known after apply)

      + enclave_options (known after apply)

      + ephemeral_block_device (known after apply)

      + instance_market_options (known after apply)

      + maintenance_options (known after apply)

      + metadata_options (known after apply)

      + network_interface (known after apply)

      + primary_network_interface (known after apply)

      + private_dns_name_options (known after apply)

      + root_block_device (known after apply)

      + secondary_network_interface (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform apply
data.aws_ami.ubuntu: Reading...
aws_s3_bucket.app_bucket: Refreshing state... [id=abusufiyan-terraform-s3-2026-847291]
data.aws_ami.ubuntu: Read complete after 0s [id=ami-07a134137a631b892]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.app_server will be created
  + resource "aws_instance" "app_server" {
      + ami                                  = "ami-07a134137a631b892"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + disable_api_stop                     = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + enable_primary_ipv6                  = (known after apply)
      + force_destroy                        = false
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t3.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_group_id                   = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + region                               = "us-west-2"
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = (known after apply)
      + tags                                 = {
          + "Name" = "learn-terraform"
        }
      + tags_all                             = {
          + "Name" = "learn-terraform"
        }
      + tenancy                              = (known after apply)
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = (known after apply)

      + capacity_reservation_specification (known after apply)

      + cpu_options (known after apply)

      + ebs_block_device (known after apply)

      + enclave_options (known after apply)

      + ephemeral_block_device (known after apply)

      + instance_market_options (known after apply)

      + maintenance_options (known after apply)

      + metadata_options (known after apply)

      + network_interface (known after apply)

      + primary_network_interface (known after apply)

      + private_dns_name_options (known after apply)

      + root_block_device (known after apply)

      + secondary_network_interface (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.app_server: Creating...
aws_instance.app_server: Still creating... [00m10s elapsed]
aws_instance.app_server: Creation complete after 13s [id=i-05da863d79293ca4b]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$ batcat main.tf
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: main.tf
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │
   2   │ data "aws_ami" "ubuntu" {
   3   │   most_recent = true
   4   │
   5   │   filter {
   6   │     name   = "name"
   7   │     values = ["ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-*"]
   8   │   }
   9   │
  10   │   owners = ["099720109477"] # Canonical
  11   │ }
  12   │
  13   │ resource "aws_instance" "app_server" {
  14   │   ami           = data.aws_ami.ubuntu.id
  15   │   instance_type = "t3.micro"
  16   │
  17   │   tags = {
  18   │     Name = "learn-terraform"
  19   │   }
  20   │ }
  21   │
  22   │ resource "aws_s3_bucket" "app_bucket" {
  23   │   bucket = "abusufiyan-terraform-s3-2026-847291"
  24   │
  25   │   tags = {
  26   │     Name        = "app-bucket"
  27   │     Environment = "dev"
  28   │   }
  29   │ }
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-16-197:~/terraform-basics$


```
<img width="1360" height="764" alt="image" src="https://github.com/user-attachments/assets/cf692aa3-56a4-4fad-9896-092a9a5146e0" />

<img width="1365" height="721" alt="image" src="https://github.com/user-attachments/assets/5fd844fb-59cb-4be8-bf0c-f65835365baa" />

### Document: How does Terraform know the S3 bucket already exists and only the EC2 instance needs to be created?
Terraform knows the S3 bucket already exists by checking its state file (terraform.tfstate). The state file records the resources that Terraform has previously created and their current attributes. During terraform plan, Terraform compares the desired configuration with the state file and the actual infrastructure, so it identifies that the S3 bucket already exists and only creates the EC2 instance if it is missing.

## Task 5: Understand the State File

### Terraform tracks everything it creates and manages in the Terraform state file (`terraform.tfstate`). Open `terraform.tfstate` in an editor to inspect its JSON structure, then run `terraform show` to view the current state in a human-readable format, `terraform state list` to list all resources managed by Terraform, `terraform state show aws_s3_bucket.<name>` to view detailed information about a specific S3 bucket, and `terraform state show aws_instance.<name>` to view detailed information about a specific EC2 instance.

```bash

ubuntu@ip-172-31-16-197:~$ ls
terraform-basics
ubuntu@ip-172-31-16-197:~$ cd terraform-basics/
ubuntu@ip-172-31-16-197:~/terraform-basics$ ls
main.tf  provider.tf  terraform.tfstate  terraform.tfstate.1791138189.backup  terraform.tfstate.backup  versions.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$ batcat terraform.tfstate

ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform show
# data.aws_ami.ubuntu:
data "aws_ami" "ubuntu" {
    architecture          = "x86_64"
    arn                   = "arn:aws:ec2:us-west-2::image/ami-07a134137a631b892"
    block_device_mappings = [
        {
            device_name  = "/dev/sda1"
            ebs          = {
                "delete_on_termination"      = "true"
                "encrypted"                  = "false"
                "iops"                       = "0"
                "snapshot_id"                = "snap-056b092df5c9eceb2"
                "throughput"                 = "0"
                "volume_initialization_rate" = "0"
                "volume_size"                = "8"
                "volume_type"                = "gp3"
            }
            no_device    = null
            virtual_name = null
        },
        {
            device_name  = "/dev/sdb"
            ebs          = {}
            no_device    = null
            virtual_name = "ephemeral0"
        },
        {
            device_name  = "/dev/sdc"
            ebs          = {}
            no_device    = null
            virtual_name = "ephemeral1"
        },
    ]
    boot_mode             = "uefi-preferred"
    creation_date         = "2026-09-23T11:56:59.000Z"
    deprecation_time      = "2028-09-23T11:56:59.000Z"
    description           = "Canonical, Ubuntu, 24.04, amd64 noble image"
    ena_support           = true
    hypervisor            = "xen"
    id                    = "ami-07a134137a631b892"
    image_id              = "ami-07a134137a631b892"
    image_location        = "amazon/ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-20260923"
    image_owner_alias     = "amazon"
    image_type            = "machine"
    imds_support          = "v2.0"
    include_deprecated    = false
    kernel_id             = null
    last_launched_time    = null
    most_recent           = true
    name                  = "ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-20260923"
    owner_id              = "099720109477"
    owners                = [
        "099720109477",
    ]
    platform              = null
    platform_details      = "Linux/UNIX"
    product_codes         = []
    public                = true
    ramdisk_id            = null
    region                = "us-west-2"
    root_device_name      = "/dev/sda1"
    root_device_type      = "ebs"
    root_snapshot_id      = "snap-056b092df5c9eceb2"
    sriov_net_support     = "simple"
    state                 = "available"
    state_reason          = {
        "code"    = "UNSET"
        "message" = "UNSET"
    }
    tags                  = {}
    tpm_support           = null
    usage_operation       = "RunInstances"
    virtualization_type   = "hvm"

    filter {
        name   = "name"
        values = [
            "ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-*",
        ]
    }
}

# aws_instance.app_server:
resource "aws_instance" "app_server" {
    ami                                  = "ami-07a134137a631b892"
    arn                                  = "arn:aws:ec2:us-west-2:809554586098:instance/i-05da863d79293ca4b"
    associate_public_ip_address          = true
    availability_zone                    = "us-west-2a"
    disable_api_stop                     = false
    disable_api_termination              = false
    ebs_optimized                        = false
    force_destroy                        = false
    get_password_data                    = false
    hibernation                          = false
    host_id                              = null
    iam_instance_profile                 = null
    id                                   = "i-05da863d79293ca4b"
    instance_initiated_shutdown_behavior = "stop"
    instance_lifecycle                   = null
    instance_state                       = "running"
    instance_type                        = "t3.micro"
    ipv6_address_count                   = 0
    ipv6_addresses                       = []
    key_name                             = null
    monitoring                           = false
    outpost_arn                          = null
    password_data                        = null
    placement_group                      = null
    placement_group_id                   = null
    placement_partition_number           = 0
    primary_network_interface_id         = "eni-037cc18dfb0dd9aad"
    private_dns                          = "ip-172-31-32-148.us-west-2.compute.internal"
    private_ip                           = "172.31.32.148"
    public_dns                           = "ec2-18-236-62-210.us-west-2.compute.amazonaws.com"
    public_ip                            = "18.236.62.210"
    region                               = "us-west-2"
    secondary_private_ips                = []
    security_groups                      = [
        "default",
    ]
    source_dest_check                    = true
    spot_instance_request_id             = null
    subnet_id                            = "subnet-0c6cb96e293188e1c"
    tags                                 = {
        "Name" = "learn-terraform"
    }
    tags_all                             = {
        "Name" = "learn-terraform"
    }
    tenancy                              = "default"
    user_data_replace_on_change          = false
    vpc_security_group_ids               = [
        "sg-06fd87dd70209e09b",
    ]

    capacity_reservation_specification {
        capacity_reservation_preference = "open"
    }

    cpu_options {
        amd_sev_snp           = null
        core_count            = 1
        nested_virtualization = null
        threads_per_core      = 2
    }

    credit_specification {
        cpu_credits = "unlimited"
    }

    enclave_options {
        enabled = false
    }

    maintenance_options {
        auto_recovery = "default"
    }

    metadata_options {
        http_endpoint               = "enabled"
        http_protocol_ipv6          = "disabled"
        http_put_response_hop_limit = 2
        http_tokens                 = "required"
        instance_metadata_tags      = "disabled"
    }

    primary_network_interface {
        delete_on_termination = true
        network_interface_id  = "eni-037cc18dfb0dd9aad"
    }

    private_dns_name_options {
        enable_resource_name_dns_a_record    = false
        enable_resource_name_dns_aaaa_record = false
        hostname_type                        = "ip-name"
    }

    root_block_device {
        delete_on_termination = true
        device_name           = "/dev/sda1"
        encrypted             = false
        iops                  = 3000
        kms_key_id            = null
        tags                  = {}
        tags_all              = {}
        throughput            = 125
        volume_id             = "vol-045aa3fe50c96b990"
        volume_size           = 8
        volume_type           = "gp3"
    }
}

# aws_s3_bucket.app_bucket:
resource "aws_s3_bucket" "app_bucket" {
    acceleration_status         = null
    arn                         = "arn:aws:s3:::abusufiyan-terraform-s3-2026-847291"
    bucket                      = "abusufiyan-terraform-s3-2026-847291"
    bucket_domain_name          = "abusufiyan-terraform-s3-2026-847291.s3.amazonaws.com"
    bucket_namespace            = "global"
    bucket_prefix               = null
    bucket_region               = "us-west-2"
    bucket_regional_domain_name = "abusufiyan-terraform-s3-2026-847291.s3.us-west-2.amazonaws.com"
    force_destroy               = false
    hosted_zone_id              = "Z3BJ6K6RIION7M"
    id                          = "abusufiyan-terraform-s3-2026-847291"
    object_lock_enabled         = false
    policy                      = null
    region                      = "us-west-2"
    request_payer               = "BucketOwner"
    tags                        = {
        "Environment" = "dev"
        "Name"        = "app-bucket"
    }
    tags_all                    = {
        "Environment" = "dev"
        "Name"        = "app-bucket"
    }

    grant {
        id          = "aae4030bb7ea6bf493177276aaa88c6d4066b2f34b8c3099e11861ef738b88a2"
        permissions = [
            "FULL_CONTROL",
        ]
        type        = "CanonicalUser"
        uri         = null
    }

    server_side_encryption_configuration {
        rule {
            bucket_key_enabled = false

            apply_server_side_encryption_by_default {
                kms_master_key_id = null
                sse_algorithm     = "AES256"
            }
        }
    }

    versioning {
        enabled    = false
        mfa_delete = false
    }
}
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform state list
data.aws_ami.ubuntu
aws_instance.app_server
aws_s3_bucket.app_bucket
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform state show aws_s3_bucket.app_bucket
# aws_s3_bucket.app_bucket:
resource "aws_s3_bucket" "app_bucket" {
    acceleration_status         = null
    arn                         = "arn:aws:s3:::abusufiyan-terraform-s3-2026-847291"
    bucket                      = "abusufiyan-terraform-s3-2026-847291"
    bucket_domain_name          = "abusufiyan-terraform-s3-2026-847291.s3.amazonaws.com"
    bucket_namespace            = "global"
    bucket_prefix               = null
    bucket_region               = "us-west-2"
    bucket_regional_domain_name = "abusufiyan-terraform-s3-2026-847291.s3.us-west-2.amazonaws.com"
    force_destroy               = false
    hosted_zone_id              = "Z3BJ6K6RIION7M"
    id                          = "abusufiyan-terraform-s3-2026-847291"
    object_lock_enabled         = false
    policy                      = null
    region                      = "us-west-2"
    request_payer               = "BucketOwner"
    tags                        = {
        "Environment" = "dev"
        "Name"        = "app-bucket"
    }
    tags_all                    = {
        "Environment" = "dev"
        "Name"        = "app-bucket"
    }

    grant {
        id          = "aae4030bb7ea6bf493177276aaa88c6d4066b2f34b8c3099e11861ef738b88a2"
        permissions = [
            "FULL_CONTROL",
        ]
        type        = "CanonicalUser"
        uri         = null
    }

    server_side_encryption_configuration {
        rule {
            bucket_key_enabled = false

            apply_server_side_encryption_by_default {
                kms_master_key_id = null
                sse_algorithm     = "AES256"
            }
        }
    }

    versioning {
        enabled    = false
        mfa_delete = false
    }
}
ubuntu@ip-172-31-16-197:~/terraform-basics$


```

### Answer these questions in your notes:
### What information does the state file store about each resource?
The Terraform state file stores information about each managed resource, including its resource type and name, resource ID, provider details, region, current attributes and configuration values, and other metadata Terraform needs to track and manage the resource. It allows Terraform to compare the desired configuration with the current infrastructure and determine what changes are required.

### Why should you never manually edit the state file?
You should never manually edit the Terraform state file because it is managed by Terraform, and manual changes can corrupt the state, cause inconsistencies between Terraform and the actual infrastructure, and lead to unexpected resource changes or accidental destruction. Instead, use Terraform commands such as terraform state mv, terraform state rm, or terraform import when you need to modify the state.

### Why should the state file not be committed to Git?
The Terraform state file should not be committed to Git because it may contain sensitive information such as resource details, credentials, secrets, or other infrastructure metadata. It can also cause conflicts when multiple team members modify the state simultaneously. Instead, use a secure remote backend such as AWS S3 with state locking for team collaboration.

## Task 6: Modify, Plan, and Destroy
### Change the EC2 instance tag from `"TerraWeek-Day1"` to `"TerraWeek-Modified"` in `main.tf`, run `terraform plan` and carefully review the output to understand what the `~`, `+`, and `-` symbols represent and determine whether the change is an in-place update or a destroy-and-recreate operation, apply the change, verify that the tag has been updated in the AWS Console, and finally destroy all the Terraform-managed resources.

```bash

ubuntu@ip-172-31-16-197:~$ ls
terraform-basics
ubuntu@ip-172-31-16-197:~$ cd terraform-basics/
ubuntu@ip-172-31-16-197:~/terraform-basics$ ls
main.tf  provider.tf  terraform.tfstate  terraform.tfstate.1791138189.backup  terraform.tfstate.backup  versions.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$ vim main.tf
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform plan
data.aws_ami.ubuntu: Reading...
aws_s3_bucket.app_bucket: Refreshing state... [id=abusufiyan-terraform-s3-2026-847291]
data.aws_ami.ubuntu: Read complete after 0s [id=ami-07a134137a631b892]
aws_instance.app_server: Refreshing state... [id=i-05da863d79293ca4b]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # aws_instance.app_server will be updated in-place
  ~ resource "aws_instance" "app_server" {
        id                                   = "i-05da863d79293ca4b"
      ~ tags                                 = {
          ~ "Name" = "learn-terraform" -> "TerraWeek-Modified"
        }
      ~ tags_all                             = {
          ~ "Name" = "learn-terraform" -> "TerraWeek-Modified"
        }
        # (39 unchanged attributes hidden)

        # (9 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform apply
data.aws_ami.ubuntu: Reading...
aws_s3_bucket.app_bucket: Refreshing state... [id=abusufiyan-terraform-s3-2026-847291]
data.aws_ami.ubuntu: Read complete after 0s [id=ami-07a134137a631b892]
aws_instance.app_server: Refreshing state... [id=i-05da863d79293ca4b]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # aws_instance.app_server will be updated in-place
  ~ resource "aws_instance" "app_server" {
        id                                   = "i-05da863d79293ca4b"
      ~ tags                                 = {
          ~ "Name" = "learn-terraform" -> "TerraWeek-Modified"
        }
      ~ tags_all                             = {
          ~ "Name" = "learn-terraform" -> "TerraWeek-Modified"
        }
        # (39 unchanged attributes hidden)

        # (9 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.app_server: Modifying... [id=i-05da863d79293ca4b]
aws_instance.app_server: Modifications complete after 1s [id=i-05da863d79293ca4b]

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
ubuntu@ip-172-31-16-197:~/terraform-basics$

```


<img width="1364" height="724" alt="image" src="https://github.com/user-attachments/assets/02d397dc-7bfa-4ef9-b417-fd6d2f5ab8d9" />

```bash

ubuntu@ip-172-31-16-197:~/terraform-basics$ terraform destroy
data.aws_ami.ubuntu: Reading...
aws_s3_bucket.app_bucket: Refreshing state... [id=abusufiyan-terraform-s3-2026-847291]
data.aws_ami.ubuntu: Read complete after 1s [id=ami-07a134137a631b892]
aws_instance.app_server: Refreshing state... [id=i-05da863d79293ca4b]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # aws_instance.app_server will be destroyed
  - resource "aws_instance" "app_server" {
      - ami                                  = "ami-07a134137a631b892" -> null
      - arn                                  = "arn:aws:ec2:us-west-2:809554586098:instance/i-05da863d79293ca4b" -> null
      - associate_public_ip_address          = true -> null
      - availability_zone                    = "us-west-2a" -> null
      - disable_api_stop                     = false -> null
      - disable_api_termination              = false -> null
      - ebs_optimized                        = false -> null
      - force_destroy                        = false -> null
      - get_password_data                    = false -> null
      - hibernation                          = false -> null
      - id                                   = "i-05da863d79293ca4b" -> null
      - instance_initiated_shutdown_behavior = "stop" -> null
      - instance_state                       = "running" -> null
      - instance_type                        = "t3.micro" -> null
      - ipv6_address_count                   = 0 -> null
      - ipv6_addresses                       = [] -> null
      - monitoring                           = false -> null
      - placement_partition_number           = 0 -> null
      - primary_network_interface_id         = "eni-037cc18dfb0dd9aad" -> null
      - private_dns                          = "ip-172-31-32-148.us-west-2.compute.internal" -> null
      - private_ip                           = "172.31.32.148" -> null
      - public_dns                           = "ec2-18-236-62-210.us-west-2.compute.amazonaws.com" -> null
      - public_ip                            = "18.236.62.210" -> null
      - region                               = "us-west-2" -> null
      - secondary_private_ips                = [] -> null
      - security_groups                      = [
          - "default",
        ] -> null
      - source_dest_check                    = true -> null
      - subnet_id                            = "subnet-0c6cb96e293188e1c" -> null
      - tags                                 = {
          - "Name" = "TerraWeek-Modified"
        } -> null
      - tags_all                             = {
          - "Name" = "TerraWeek-Modified"
        } -> null
      - tenancy                              = "default" -> null
      - user_data_replace_on_change          = false -> null
      - vpc_security_group_ids               = [
          - "sg-06fd87dd70209e09b",
        ] -> null
        # (9 unchanged attributes hidden)

      - capacity_reservation_specification {
          - capacity_reservation_preference = "open" -> null
        }

      - cpu_options {
          - core_count            = 1 -> null
          - threads_per_core      = 2 -> null
            # (2 unchanged attributes hidden)
        }

      - credit_specification {
          - cpu_credits = "unlimited" -> null
        }

      - enclave_options {
          - enabled = false -> null
        }

      - maintenance_options {
          - auto_recovery = "default" -> null
        }

      - metadata_options {
          - http_endpoint               = "enabled" -> null
          - http_protocol_ipv6          = "disabled" -> null
          - http_put_response_hop_limit = 2 -> null
          - http_tokens                 = "required" -> null
          - instance_metadata_tags      = "disabled" -> null
        }

      - primary_network_interface {
          - delete_on_termination = true -> null
          - network_interface_id  = "eni-037cc18dfb0dd9aad" -> null
        }

      - private_dns_name_options {
          - enable_resource_name_dns_a_record    = false -> null
          - enable_resource_name_dns_aaaa_record = false -> null
          - hostname_type                        = "ip-name" -> null
        }

      - root_block_device {
          - delete_on_termination = true -> null
          - device_name           = "/dev/sda1" -> null
          - encrypted             = false -> null
          - iops                  = 3000 -> null
          - tags                  = {} -> null
          - tags_all              = {} -> null
          - throughput            = 125 -> null
          - volume_id             = "vol-045aa3fe50c96b990" -> null
          - volume_size           = 8 -> null
          - volume_type           = "gp3" -> null
            # (1 unchanged attribute hidden)
        }
    }

  # aws_s3_bucket.app_bucket will be destroyed
  - resource "aws_s3_bucket" "app_bucket" {
      - arn                         = "arn:aws:s3:::abusufiyan-terraform-s3-2026-847291" -> null
      - bucket                      = "abusufiyan-terraform-s3-2026-847291" -> null
      - bucket_domain_name          = "abusufiyan-terraform-s3-2026-847291.s3.amazonaws.com" -> null
      - bucket_namespace            = "global" -> null
      - bucket_region               = "us-west-2" -> null
      - bucket_regional_domain_name = "abusufiyan-terraform-s3-2026-847291.s3.us-west-2.amazonaws.com" -> null
      - force_destroy               = false -> null
      - hosted_zone_id              = "Z3BJ6K6RIION7M" -> null
      - id                          = "abusufiyan-terraform-s3-2026-847291" -> null
      - object_lock_enabled         = false -> null
      - region                      = "us-west-2" -> null
      - request_payer               = "BucketOwner" -> null
      - tags                        = {
          - "Environment" = "dev"
          - "Name"        = "app-bucket"
        } -> null
      - tags_all                    = {
          - "Environment" = "dev"
          - "Name"        = "app-bucket"
        } -> null
        # (3 unchanged attributes hidden)

      - grant {
          - id          = "aae4030bb7ea6bf493177276aaa88c6d4066b2f34b8c3099e11861ef738b88a2" -> null
          - permissions = [
              - "FULL_CONTROL",
            ] -> null
          - type        = "CanonicalUser" -> null
            # (1 unchanged attribute hidden)
        }

      - server_side_encryption_configuration {
          - rule {
              - bucket_key_enabled = false -> null

              - apply_server_side_encryption_by_default {
                  - sse_algorithm     = "AES256" -> null
                    # (1 unchanged attribute hidden)
                }
            }
        }

      - versioning {
          - enabled    = false -> null
          - mfa_delete = false -> null
        }
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

aws_s3_bucket.app_bucket: Destroying... [id=abusufiyan-terraform-s3-2026-847291]
aws_instance.app_server: Destroying... [id=i-05da863d79293ca4b]
aws_s3_bucket.app_bucket: Destruction complete after 1s
aws_instance.app_server: Still destroying... [id=i-05da863d79293ca4b, 00m10s elapsed]
aws_instance.app_server: Still destroying... [id=i-05da863d79293ca4b, 00m20s elapsed]
aws_instance.app_server: Destruction complete after 30s

Destroy complete! Resources: 2 destroyed.
ubuntu@ip-172-31-16-197:~/terraform-basics$
ubuntu@ip-172-31-16-197:~/terraform-basics$

```

### Verify in the AWS console -- both the S3 bucket and EC2 instance should be gone
<img width="1365" height="767" alt="image" src="https://github.com/user-attachments/assets/9605bf59-3a05-48f4-89b4-ed5f46beaafd" />
