# EKS 기반 k8s cluster 생성

EKS 기반 k8s 클러스터를 생성합니다.

해당 작업은 10분 가량 소요됩니다.

```bash
module.eks-cluster-master.aws_eks_cluster.eks_cluster: Still creating... [10m30s elapsed]
module.eks-cluster-master.aws_eks_cluster.eks_cluster: Still creating... [10m40s elapsed]
```

## Inputs

| Name                              | Description                                 |  Type  |    Default   | Required |
| --------------------------------- | ------------------------------------------- | :----: | :---------:  | :------: |
| cluster_name                      | eks 클러스터 이름                               | string |     n/a      |   yes    |
| cluster_vpc_id                    | eks 클러스터가 생성될 vpc id                     |  list  |     n/a      |   yes    |
| cluster_enable_logging            | eks 클러스터 로깅 사용 여부                       | boolean | `"t2.nano"` |    no    |
| cluster_logging_retention_in_days | eks 클러스터 로그 retension                     | number |     n/a      |   yes    |
| allow_ingress_cidr_blocks         | 마스터 노드가 허용할 ingress cidr block           | list |     n/a        |   yes    |
| availability_zones                | 사용할 availability zones 리스트                | list  |     n/a        |   yes    |
| private_subnet_cidr_blocks        | eks 클러스터가 사용할 private subnet cidr blocks | string |     n/a       |   yes    |
| public_subnet_cidr_blocks         | eks 클러스터가 사용할 public subnet cidr blocks | string |     n/a        |   yes    |
| node_group_disk_size              | eks worker node group disk size             | number |     20         |   yes    |
| node_group_ami_type               | eks worker node group ami type              | string | AL2_x86_64_GPU |   yes    |
| nat_gateway_id                    | eks private 서브넷에서 사용하는 nat gateway id   | string |     n/a        |   yes    |
| internet_gateway_id               | eks public 서브넷에서 사용하는 ig id             | string |     n/a        |   yes    |
| ec2_ssh_key                       | eks node group이 사용할 ssh key               | string |     n/a        |   yes    |
| node_group_desired_size           | eks node group desired size                 | number |     1        |   no    |
| node_group_min_size               | eks node group min size                     | number |     1       |   no    |
| node_group_max_size               | eks node group max size                     | number |     1        |   no    |
| endpoint_private_access           | eks cluster private access                  | boolean |   true    |   no    |
| endpoint_public_access            | eks cluster public access                   | boolean |   false    |   no    |
| tags                              | tags                                       | string |     n/a        |   yes    |

## Reference

EKS 생성과 관련해서 hashicorp의 eks-intro 문서를 참고했습니다.

[Hashicorp eks intro](https://learn.hashicorp.com/terraform/aws/eks-intro)

