# NAT-Bastion Instance 생성 모듈

NAT, Bastion의 역할을 수행할 인스턴스를 생성한다.

## Inputs

| Name                | Description                                 |  Type  |   Default   | Required |
| ------------------- | ------------------------------------------- | :----: | :---------: | :------: |
| name                | 모듈에서 정의하는 모든 리소스 이름의 prefix | string |     n/a     |   yes    |
| vpc_id              | VPC ID                                      | string |     n/a     |   yes    |
| instance_type       | bastion EC2 instance type                   | string | `"t2.nano"` |    no    |
| availability_zone   | bastion EC2 instance availability zone      | string |     n/a     |   yes    |
| subnet_id           | bastion EC2 instance Subnet ID              | string |     n/a     |   yes    |
| ec2_ssh_key         | bastion이 사용할 keypair name                 | string |     n/a     |   yes    |
| ingress_cidr_blocks | bastion SSH 접속을 허용할 CIDR block 리스트      |  list  |     n/a     |   yes    |
| tags                | 모든 리소스에 추가되는 tag 맵                      |  map   |     n/a     |   yes    |

## Outputs

| Name       | Description |
| ---------- | ----------- |
| bastion_id | BASTION ID  |

## Reference

Bastion을 생성하는 terraform code는 ausbubam님의 블로그를 참고하였습니다.

[ausbubam blog](https://blog.2dal.com/2017/10/28/aws-vpc-with-terraform-modules/)
