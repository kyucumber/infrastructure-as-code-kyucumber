# EKS 기반 k8s cluster 생성

EKS 기반 k8s 클러스터를 생성합니다.

해당 작업은 10분 가량 소요됩니다.

```bash
module.eks-cluster-master.aws_eks_cluster.eks_cluster: Still creating... [10m30s elapsed]
module.eks-cluster-master.aws_eks_cluster.eks_cluster: Still creating... [10m40s elapsed]
```

## Installation

IAM, EKS 클러스터 및 EKS 노드 그룹 생성을 위한 권한이 필요합니다

- `eks:*`
- `iam:*`

## EKS 사용을 위한 Vpc, Subnet 태그 정책

- VPC

```bash
"kubernetes.io/cluster/<cluster-name>" : shared
```

- Private subnet

```bash
"kubernetes.io/cluster/<cluster-name>" : shared
"kubernetes.io/role/internal-elb" : 1
```

- Public subnet

```bash
"kubernetes.io/cluster/<cluster-name>" : shared
"kubernetes.io/role/elb" : 1
```

## EKS Public, Private access 정책


- endpoint private access : `true`, endpoint public access : `true`

EKS 클러스터 api 서버에 외부 트래픽이 발생합니다.
EKS VPC 내의 Kubernetes API 요청에 대해서는 VPC 내에서 발생합니다.

- endpoint private access : `false`, endpoint public access : `true`

EKS 클러스터의 디폴트 설정, EKS 클러스터 api 서버에 외부 트래픽이 발생합니다.
EKS VPC 내의 Kubernetes API 요청도 VPC를 벗어나게 됩니다.

- endpoint private access : `true`, endpoint public access : `false`

EKS 클러스터 api 서버에 대한 모든 트래픽이 클러스터의 VPC 내에서만 발생

## Node Group 생성 실패 시

nat instance inbound에 eks용 프라이빗 서브넷의 cidr가 추가되어 있는지 여부를 확인합니다.

만약 inbound에 추가되어 있지 않다면 아래와 같은 에러가 발생합니다.

```bash
Error: error waiting for EKS Node Group (kyunam-eks-test-cluster:kyunam-eks-test-cluster-node-group) creation: NodeCreationFailure: Instances failed to join the kubernetes cluster.
```

> nat 인스턴스로 트래픽이 나가야 하는데, nat 인스턴스 인바운드에 해당 private subnet 대역이 정의되어 있지 않기 때문에 에러 발생

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
| tags                              | tags                                        | string |     n/a        |   yes    |

## kubectl 설정

` ~/.kube/config` 에 아래 내용을 추가합니다.

```bash
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: ${YOUR-CERTIFICATE-AUTHORITY-DATA}
    server: https://${YOUR-EKS-ENDPOINT}.yl4.ap-northeast-2.eks.amazonaws.com
  name: kyunam-eks-test-cluster
contexts:
- context:
    cluster: kyunam-eks-test-cluster
    user: aws
  name: aws
current-context: aws
kind: Config
preferences: {}
users:
- name: aws
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1alpha1
      args:
      - token
      - -i
      - kyunam-eks-test-cluster
      command: aws-iam-authenticator
      env: null
```

## Node group scale out
`eks.tfvars` 의 아래 내용을 수정합니다.

```bash
node_group_desired_size = 5
node_group_min_size = 5
node_group_max_size = 5
```

아래 명령을 통해 노드 그룹의 노드 수를 수정할 수 있습니다.

```bash
$ terraform apply -var-file=eks.tfvars 
```

## Reference

EKS 생성과 관련해 hashicorp의 eks-intro 문서를 참고했습니다.

[Hashicorp eks intro](https://learn.hashicorp.com/terraform/aws/eks-intro)
