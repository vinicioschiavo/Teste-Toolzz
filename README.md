# test-toolzz
Projeto referente ao desafio para vaga de DevOps Pleno na test-toolzz.


## 1. Criar infraestrutura via terraform
![terraform](imagens/terraform.png)
Esta configuração do Terraform configura uma infraestrutura AWS com os seguintes componentes:

Visão Geral

1 - Rede:

 - Uma VPC com sub-redes públicas e privadas.

 - Gateway de Internet para sub-redes públicas.

 - Gateway NAT para sub-redes privadas.

 - Tabelas de rotas para roteamento de tráfego de internet.

2 - Recursos de Computação:

 - Instâncias EC2 públicas e privadas.

3 - EKS (Elastic Kubernetes Service):

 - Cluster EKS e Grupo de Nós com funções e políticas IAM.

4 - RDS (Relational Database Service):

 - Instância de banco de dados PostgreSQL com grupo de sub-rede associado e grupo de segurança.

5 - Armazenamento:

 - Bucket S3 para backups.

 - Repositório ECR (Elastic Container Registry) para imagens Docker.

## 2. Criação de Namespaces no kubernetes para flowise
![kubernetes](imagens/kubernetes.png)

1 - Passo a passo para a criação do Namespaces
 - Uma vez que concluida a criação do ELK via terraform é necessario a criação de um namespace para
 a aplicação poder dar continuidade no projeto.

 - "aws eks update-kubeconfig --name vi-cluster"

 - "kubectl create namespace flowise"
 
 - "kubectl config set-context arn:aws:eks:us-west-2:399679827371:cluster/vi-cluster --namespace flowise"

   ![Image](https://github.com/user-attachments/assets/a2d07889-e614-405a-a7a0-07cacb2ed677)

   ![Image](https://github.com/user-attachments/assets/c379e539-0a3e-4fa0-a9f9-6426e6b160f8)


## 3. Criar fluxo de CI/CD
![ci-cd](imagens/ci-cd.png)

Visão Geral

Este arquivo define um pipeline CI/CD no GitHub Actions para compilação, envio e implantação de uma aplicação no cluster EKS (Elastic Kubernetes Service). Ele abrange os seguintes estágios:

1 - Gatilhos: Configuração para execução em push ou pull request para o branch main.

2 - Variáveis de Ambiente: Configuração das variáveis necessárias para a execução do pipeline.

3 - Jobs:
 - Build: Compilação e envio da imagem Docker para o Amazon ECR.
 - Deploy: Implantação da aplicação no Kubernetes.

 Gatilhos
O pipeline é ativado pelos seguintes eventos:

![Image](https://github.com/user-attachments/assets/6b829786-0813-4225-b243-8294e3654978)

Isso garante que o pipeline execute quando houver atualizações no branch principal.

Variáveis de Ambiente

As variáveis configuradas para o pipeline incluem:

 - AWS_REGION: Região da AWS (us-west-2).

 - EKS_CLUSTER_NAME: Nome do cluster EKS (vi-cluster).

 - NAMESPACE: Namespace no Kubernetes (flowise).

 - IMAGE_REPO_NAME: Nome do repositório de imagens Docker (flowise).

 - IMAGE_TAG: Tag gerada dinamicamente para a imagem Docker baseada no número de execução do GitHub.

Jobs

1. Build
Este job realiza as seguintes etapas:
 - Checkout do Código:
 
![Image](https://github.com/user-attachments/assets/421a4d71-70c0-43db-be9f-88fd582bd833)

 - Configuração do Docker:

 ![Image](https://github.com/user-attachments/assets/24144b04-8ae6-4cf9-8935-ab32e29707ef)

 - Configuração do Credenciais-AWS:

![Image](https://github.com/user-attachments/assets/e58aea60-546c-4f14-847b-48d100b90c3e)
    
 - Login no Amazon ECR:
 
 ![Image](https://github.com/user-attachments/assets/ce0d75a6-43ca-4abd-b154-5f097d8d9f5d)

 - Build e Push da Imagem Docker:
 
 ![Image](https://github.com/user-attachments/assets/d5e9920e-71d8-4d4d-bf20-c9da6f576fee)

2. Deploy
Este job depende do job "build" e realiza as seguintes etapas:

 - Checkout do Código: 

 ![Image](https://github.com/user-attachments/assets/421a4d71-70c0-43db-be9f-88fd582bd833)

 - Configuração das Credenciais AWS:

![Image](https://github.com/user-attachments/assets/e58aea60-546c-4f14-847b-48d100b90c3e)

 - Instalação do Kubectl:

![Image](https://github.com/user-attachments/assets/3d5f16d5-6e37-47a7-a118-11655c22fc9d)

 - Atualização do Kubeconfig:

 ![Image](https://github.com/user-attachments/assets/6e836f19-fdbc-41be-8e26-35924dd33d1b)

 - Implantação no Kubernetes:

![Image](https://github.com/user-attachments/assets/963c3627-318f-4796-95f9-157300c6eb47)


## 4. Validação do deploy via github action.

![Image](https://github.com/user-attachments/assets/90daf18b-7bad-4683-b3d3-9947bb342fad)

## 5. Validação do Flowise rodando localmente.

![Image](https://github.com/user-attachments/assets/d9c88b6d-8c05-4456-bb1b-440e6713fefe)

## 6. Diagrama da arquitetura.

![Image](https://github.com/user-attachments/assets/be9b3fc6-dad2-452c-88ee-30c4ca57c033)







