# VPC 생성 모듈

## Inputs

| Name                | Description                                 |  Type  | Default | Required |
| ------------------  | ------------------------------------------- | :----: | :-----: | :------: |
| name                | 모듈에서 정의하는 모든 리소스 이름의 prefix | string |   n/a   |   yes    |
| cidr                | VPC에 할당한 CIDR block                     | string |   n/a   |   yes    |
| public_subnets      | Public Subnet IP 리스트                     |  list  |   n/a   |   yes    |
| private_subnets     | Private Subnet IP 리스트                    |  list  |   n/a   |   yes    |
| database_subnets    | Database Subnet IP 리스트                   |  list  |   n/a   |   yes    |
| availability_zones  | 사용할 availability zones 리스트              |  list  |   n/a   |   yes    |
| bastion_id          | bastion instance id                         | string |   n/a   |   yes    |
| nat_gateway_enable  | nat gateway 설정 여부, false인 경우 instance 사용 | string |   n/a   |   yes    |
| tags                | 모든 리소스에 추가되는 tag 맵               |  map   |   n/a   |   yes    |

## Outputs

| Name                      | Description                     |
| ------------------------- | ------------------------------- |
| database_subnet_group_ids | Database Subnet Group ID 리스트 |
| database_subnets_ids      | Database Subnet ID 리스트       |
| default_network_acl_id    | VPC default network ACL ID      |
| default_security_group_id | VPC default Security Group ID   |
| igw_id                    | Interget Gateway ID             |
| private_route_table_ids   | Private Route Table ID 리스트   |
| private_subnets_ids       | Private Subnet ID 리스트        |
| public_route_table_ids    | Public Route Table ID 리스트    |
| public_subnets_ids        | Public Subnet ID 리스트         |
| vpc_cidr_block            | VPC에 할당한 CIDR block         |
| vpc_id                    | VPC ID                          |

## Reference

Vpc 모듈과 Bastion을 생성하는 terraform code는 ausbubam님의 블로그를 참고하였습니다.

[ausbubam blog](https://blog.2dal.com/2017/10/28/aws-vpc-with-terraform-modules/)
