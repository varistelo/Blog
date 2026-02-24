---
title: Melhorando a Segurança do Buckets S3
date: 2024-08-24
draft: true
tags:
  - AWS "Bucket S3" Cloud Segurança
categories:
  - tutoriais
ShowToc: false
TocOpen: false
---

#####

Uma forma de melhor a segurança no Bucket S3 da Amazon é ativar alguns limits na política, como por exemplo:

**Statement 1 — Permissões no bucket em si**

| Ação | O que faz |
| --- | --- |
| `s3:GetBucketLocation` | Ver em qual região o bucket está |
| `s3:ListBucket` | Listar os objetos dentro do bucket |
| `s3:GetLifecycleConfiguration` | Ler regras de ciclo de vida (ex: expirar objetos após X dias) |
| `s3:PutLifecycleConfiguration` | Criar/alterar essas regras de ciclo de vida |
| `s3:GetIntelligentTieringConfiguration` | Ler configuração do Intelligent-Tiering (otimização de custo automática) |
| `s3:PutIntelligentTieringConfiguration` | Criar/alterar essa configuração |

#####
**Statement 2 — Permissões nos objetos dentro do bucket**

| Ação | O que faz |
| --- | --- |
| `s3:GetObject` | Baixar/ler arquivos |
| `s3:GetObjectAttributes` | Ler metadados dos arquivos (tamanho, checksum, etc.) |
| `s3:GetObjectTagging` | Ler tags dos arquivos |
| `s3:PutObject` | Fazer upload de arquivos |
| `s3:DeleteObject` | Deletar arquivos |

####
Política a ser aplicada no user IAM

```plain
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:GetLifecycleConfiguration",
                "s3:PutLifecycleConfiguration",
                "s3:GetIntelligentTieringConfiguration",
                "s3:PutIntelligentTieringConfiguration"
            ],
            "Resource": "arn:aws:s3:::meu-bucket-X"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetObjectAttributes",
                "s3:GetObjectTagging",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::meu-bucket-X/*"
        }
    ]
}
```

#### Bloqueio de IPs

```plain
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyAccessFromUnauthorizedIPs",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::meu-bucket-X",
                "arn:aws:s3:::meu-bucket-X/*"
            ],
            "Condition": {
                "NotIpAddress": {
                    "aws:SourceIp": [
                        "203.0.113.0/24",
                        "198.51.100.10/32"
                    ]
                },
                "Bool": {
                    "aws:ViaAWSService": "false"
                }
            }
        }
    ]
}
```

#### Resultado:

| Ação | Permitido |
| --- | --- |
| Listar objetos do bucket | ✅ (só IPs autorizados) |
| Ver localização do bucket | ✅ (só IPs autorizados) |
| Baixar arquivos | ✅ (só IPs autorizados) |
| Fazer upload de arquivos | ✅ (só IPs autorizados) |
| Deletar arquivos | ✅ (só IPs autorizados) |
| Ler metadados e tags dos arquivos | ✅ (só IPs autorizados) |
| Gerenciar ciclo de vida | ✅ (só IPs autorizados) |
| Gerenciar Intelligent-Tiering | ✅ (só IPs autorizados) |
| Acessar de IP não autorizado | ❌ |
| Deletar o bucket | ❌ |
| Criar bucket | ❌ |
| Alterar versionamento | ❌ |
| Alterar acesso público | ❌ |
| Alterar política do bucket | ❌ |
| Configurar CORS / replicação / criptografia | ❌ |
| Alterar tags dos objetos | ❌ |
| Copiar objetos entre buckets | ❌ |
| Restaurar objetos do Glacier | ❌ |
| Gerenciar ACLs dos objetos | ❌ |
| Upload multipart | ❌ |
| Acessar versões anteriores | ❌ |
| Qualquer outro serviço AWS | ❌ |
