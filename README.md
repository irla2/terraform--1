# terraform-regestry

Terraform AWS
=============================
This module is basically combination of Terraform open source and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of maintaining the whole infrastructure code yourself.
=============================

PRE-REQUISITES:-
===============
```hcl
This module has a few dependencies:
--> Terraform 0.12
--> vgit.techvedika.com/Techvedika/it-devops-aws/tree/mlopsproject-demo
```
MODULES:-
--------
```hcl
1).Terraform module to create an EC2 resource on AWS with instance-profile.
2).Terraform module to create an s3 resource on AWS with s3-life cycle management, Bucketpolicy and Bucket versioning 
3).Terraform module to create an IAM resource on AWS with IAM-USER add IAM-POLICY(this policy only have s3[putobject,getobject,deleteobject and listofbuckets ]) and IAM-ROLe to add policy.
4). Terraform module to create an EBS[Elastic Block Store] attached to aws EC2 instance.
5). spot-instances

|-- ebs
|   |-- main.tf
|   |-- output.tf
|   `-- var.tf
|-- ec2
|   |-- main.tf
|   |-- mykey.pem
|   |-- output.tf
|   `-- var.tf
|-- iam-role
|   |-- main.tf
|   |-- output.tf
|   `-- var.tf
|-- s3
|   |-- main.tf
|   |-- output.tf
|   |-- templates
|   |   `-- bucket-policy.json.tpl
|   `-- var.tf
`-- spot-instances
    |-- main.tf
    |-- output.tf
    `-- var.tf
```
INPUTS:-
-------
```hcl
module "role" {
  source = "./modules/iam-role"
  instance-profile = [""]
  user-name = [""]
  role-name = [""]
  policy-name = [""]
  services = [""]
  policy-attachment = [""]  
}

module "test" {
 source = "./modules/ec2"
 subnet_id = [""]
 azs = [""]
 ami = [""]
 sg = [""]
 key-name = ""
 instance-type = ""
 root-volume-size = ""
 volume-type = ""
 user-data = "./user-data.sh"
 file = "./ramu.sh"
 file1 = "./mount.sh"
 file2 = "./user.sh"
 volume-termination-role = ""   ##[1 --> volume is not terminate] [0 --> volime is terminate]
 tagname = [""]
 instances-termination-role = "" ##[1 --> instance is not terminate] [0 -->instance terminate]
 role-name = "${module.role.instance-profile}"
}

module "ebs" {
 source = "./modules/ebs"
 azs = [""]
 volume-termination-role = ""    ##[1 --> volume is not terminate] [0 --> volime is terminate]
 volume-type = ""
 instances-id = "${module.test.instances-id}"
 extra-volume-size = ""
 volumename = [""]
}

module "s3module" {
  source = "./modules/s3"
  bucket-name = [""]
  aws-region = ""
  acl = ""
  environment = ""
  project-name = ""
}
```
OUTPUTS:-
----------
```hcl

output "instances-ips" {
  value = "${module.test.*.ip-address}"
}
output "instances-ids" {
  value = "${module.test.*.instances-id}"
}

output "attached-volumes" {
  value = "${module.ebs.*.extra-volume-id}"
}
output "iam-role" {
  value = "${module.role.*.iam-role}"
}
output "instance-profile" {
  value = "${module.role.*.instance-profile}"
}
output "bucket-ids" {
  value = "${module.s3module.s3-bucket-name}"
}

```

