# HTB Starting Point — Dancing

| Campo | Detalhe |
|---|---|
| SO | Windows |
| Dificuldade | Muito Fácil |
| Categoria | Enumeração SMB, Acesso Anônimo |
| Ferramentas | RustScan, Nmap, smbclient |

## Recon

```
rustscan -a <TARGET_IP> -- -sV -sC
```

Resultado: porta 445/tcp aberta (SMB), identificando o serviço Microsoft-DS.

## Exploração

Verificação de shares disponíveis via sessão anônima (null session):

```
smbclient -L //<TARGET_IP>/ -N
```

A listagem revela um share chamado **WorkShares**, acessível sem autenticação. Conexão direta:

```
smbclient //<TARGET_IP>/WorkShares -N
```

Dentro do share, navegação até encontrar o arquivo com a flag (`flag.txt`), capturada via `get`.

## Mapeamento OWASP/CWE

- **CWE-287** (Improper Authentication) — serviço SMB permite sessão nula (null session) sem exigir credenciais
- Relacionado a **A07:2021 – Identification and Authentication Failures** (OWASP Top 10)

## Remediação

- Desabilitar sessões anônimas/nulas no SMB (`RestrictAnonymous` via GPO)
- Restringir ACLs de compartilhamento para exigir autenticação
- Auditar shares SMB expostos regularmente com ferramentas como `enum4linux`/`smbmap` do lado defensivo
