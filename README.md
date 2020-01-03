## Infrastructure as code

Public cloud resource 기반 infrastructure를 코드로 관리하는 저장소입니다.

## Create VPC

[VPC](/terraform/aws/resources/vpc)

Edit vpc.tfvars
```bash
$ vim /terraform/aws/resources/vpc/vpc.tfvars
```

```bash
$ cd /terraform/aws/resources/vpc
$ terraform apply -var-file=vpc.tfvars
```

## Create EKS Cluster

[EKS Cluster](/terraform/aws/resources/eks)

Edit eks.tfvars
```bash
$ vim /terraform/aws/resources/eks/eks.tfvars
```

```bash
$ cd /terraform/aws/resources/eks
$ terraform apply -var-file=eks.tfvars
```

## Reference

Vpc 모듈과 Bastion을 생성하는 terraform code는 ausbubam님의 블로그를 참고하였습니다.

[ausbubam blog](https://blog.2dal.com/2017/10/28/aws-vpc-with-terraform-modules/)

Eks 생성과 관련된 부분은 hashicorp eks-intro 문서를 참고했습니다.

[Hashicorp eks intro](https://learn.hashicorp.com/terraform/aws/eks-intro)
