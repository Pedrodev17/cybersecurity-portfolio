# HTB Starting Point — Appointment

| Campo | Detalhe |
|---|---|
| SO | Linux |
| Dificuldade | Muito Fácil |
| Categoria | SQL Injection — Bypass de Autenticação |
| Ferramentas | RustScan, Nmap, Navegador/Burp Suite |

## Recon

```
rustscan -a <TARGET_IP> -- -sV -sC
```

Resultado: porta 80/tcp aberta, aplicação web com formulário de login.

## Exploração

O formulário de login constrói a query SQL concatenando a entrada do usuário sem sanitização. Payload clássico de bypass no campo de usuário/senha:

```
' OR 1=1 -- -
```

A injeção força a cláusula `WHERE` a sempre avaliar como verdadeira, autenticando sem credenciais válidas e revelando a flag na página pós-login.

## Mapeamento OWASP/CWE

- **CWE-89** (SQL Injection)
- **A03:2021 – Injection** (OWASP Top 10)

## Remediação

- Usar prepared statements / queries parametrizadas em toda interação com o banco
- Validar e sanitizar entrada do usuário no backend
- Aplicar princípio de menor privilégio na conta de banco usada pela aplicação
