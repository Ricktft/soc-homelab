# soc-homelab
# SOC Homelab – Análise de Incidente com Splunk  
**TryHackMe – Room: Splunk (SOC Level 1)**  
*Henrique David Silva | Outubro 2025*

---

## Cenário do Lab  
Simulação de ataque real com tráfego malicioso (C2, exfiltração, ransomware) no TryHackMe.

---

## As 7 Fases da Cyber Kill Chain  

| Fase | Evidência no Splunk | Query SPL |
|------|---------------------|----------|
| 1. Reconnaissance | DNS para `malicious.com` | `index=network sourcetype=dns query=*malicious*` |
| 2. Weaponization | Payload em PowerShell | `index=endpoint sourcetype=sysmon Image="*powershell.exe"` |
| 3. Delivery | Execução maliciosa | `index=endpoint EventCode=1` |
| 4. Exploitation | Injeção de processo | `parent_image="*powershell.exe"` |
| 5. Installation | Persistência via registro | `index=endpoint EventCode=13` |
| 6. Command & Control | C2 para 185.117.x.x | `index=network dest_ip=185.117.*` |
| 7. Actions on Objectives | Exfiltração de dados | `index=network bytes_out>1000000` |

---

## Dashboard Criado no Splunk  
![Dashboard](dashboard.png)  
*Print do painel com alertas em tempo real*

---

## Root Cause  
Processo `svchost.exe` injetado por `powershell.exe` → comunicação C2 com IP malicioso.

---

## Mitigação Proposta  
- Bloqueio de IP via NSG (Azure)  
- Restrição de PowerShell via AppLocker  
- Regra customizada no Wazuh HIDS

---

## Arquivos do Projeto  
- [Relatório completo (PDF)](relatorio.pdf)  
- [Queries SPL](queries.txt)  
- [Script Python de parsing](python/parse_logs.py)

---

> **Este lab foi realizado no TryHackMe como parte do caminho SOC Analyst.**
