# HTB Starting Point — Three

| Campo | Detalhe |
|---|---|
| SO | Linux/Cloud |
| Dificuldade | Fácil |
| Categoria | Bucket S3 mal configurado (exposição pública) |
| Ferramentas | RustScan, Nmap, AWS CLI |

## Recon

```
rustscan -a <TARGET_IP> -- -sV -sC
```

Resultado: serviço web expõe referência a um bucket S3 (AWS) mal configurado.

## Exploração

Listagem do conteúdo do bucket sem autenticação:

```
aws s3 ls s3://<nome-do-bucket> --no-sign-request --endpoint-url <endpoint>
```

Download dos arquivos expostos, incluindo um que revela a flag ou credenciais adicionais para o próximo estágio.

## Mapeamento OWASP/CWE

- **CWE-284** (Improper Access Control)
- Relacionado a **A01:2021 – Broken Access Control** (OWASP Top 10) e à categoria de Cloud Security Misconfiguration

## Remediação

- Configurar política de bucket (Bucket Policy) negando acesso público por padrão
- Habilitar "Block Public Access" nas configurações da conta AWS
- Auditar periodicamente permissões de buckets com ferramentas como AWS Trusted Advisor ou S3 Inspector
