---
title: Configurando um Cluster Kubernetes com Terraform
date: 2025-06-15
draft: true
tags:
  - kubernetes
  - terraform
  - devops
categories:
  - tutoriais
ShowToc: true
TocOpen: true
---

## Introdução

Neste tutorial, vamos configurar um cluster Kubernetes na AWS usando Terraform.

## Pré-requisitos

- Terraform v1.5+
- AWS CLI configurado
- kubectl instalado

## Configuração do Provider

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_eks_cluster" "main" {
  name     = "meu-cluster"
  role_arn = aws_iam_role.eks.arn

  vpc_config {
    subnet_ids = var.subnet_ids
  }
}
```

## Criando o Cluster

Execute os seguintes comandos:

```bash
terraform init
terraform plan
terraform apply
```

## Verificando

```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

## Conclusão

Com poucos comandos, temos um cluster Kubernetes funcional na AWS.
