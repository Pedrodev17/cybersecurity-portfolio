# HTB Starting Point — Crocodile

| Campo | Detalhe |
|---|---|
| SO | Linux |
| Dificuldade | Muito Fácil |
| Categoria | FTP anônimo + reutilização de credenciais |
| Ferramentas | RustScan, Nmap, cliente FTP, navegador |

## Recon

```
rustscan -a <TARGET_IP> -- -sV -sC
```

Resultado: portas 21/tcp (FTP) e 80/tcp (HTTP) abertas.

## Exploração

Login FTP anônimo permitido:

```
ftp <TARGET_IP>
# usuário: anonymous / senha: em branco
```

Dentro do FTP, um arquivo de texto contém credenciais em texto claro. Essas credenciais são reutilizadas no painel de administração web exposto na porta 80, concedendo acesso e revelando a flag.

## Mapeamento OWASP/CWE

- **CWE-287** (Improper Authentication) no FTP anônimo
- **CWE-256** (Plaintext Storage of Password) no arquivo de credenciais
- **A07:2021 – Identification and Authentication Failures** (OWASP Top 10)

## Remediação

- Desabilitar login anônimo no servidor FTP
- Nunca armazenar credenciais em texto claro em arquivos acessíveis
- Impor políticas de não reutilização de senha entre serviços
