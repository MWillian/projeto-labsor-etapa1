# Projeto de LSOR — Turma bsi-26-2 (2026.2)
## Etapa 1 — Rede Virtualizada com 8 VMs e Gateway NAT (Grupo 2)
## **Professor:** Alaelson Jatoba

Este repositório contém a documentação técnica, topologia, especificações de rede e roteiro de configuração para a **Etapa 1** da disciplina de **Laboratório de Sistemas Operacionais e Redes (LSOR)** do Instituto Federal de Alagoas (IFAL - Campus Maceió).

---

### 1. Identificação do Grupo e Componentes

* **Turma:** `bsi-26-2` (2026.2)
* **Grupo:** **Grupo 2**
* **Domínio Oficial (FQDN):** `grupo2.bsi-26-2.maceio.lab`
* **Sub-rede LAN do Grupo:** `192.168.26.32/28` (Máscara: `255.255.255.240`, Broadcast: `192.168.26.47`)
* **Integrantes do Grupo:**
  * `matheus.nascimento`
  * `jose.duda`
  * `daniel.costa`
  * `mateus.santos`
  * `administrador` (Conta de manutenção/instalação)

---

### 2. Visão Geral da Topologia e Parâmetros de Rede

O ambiente é composto por **9 Máquinas Virtuais** executando Ubuntu Server (1 Gateway NAT/Firewall + 8 VMs Clientes) distribuídas fisicamente entre os 4 computadores do laboratório e interconectadas em modo Placa em Ponte (Bridge Adapter).

#### Tabela Completa de Endereçamento IP e FQDN (Grupo 2)

| Host (VM) | PC Físico | Interface | Modo VirtualBox | Endereço IP / Máscara | Gateway Padrão | FQDN Oficial | Apelido (`/etc/hosts`) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`g2-pc1-gw`** | PC 1 | `enp0s3` (WAN)<br>`enp0s8` (LAN) | Bridge Adapter<br>Bridge Adapter | `172.20.23.60/22`<br>`192.168.26.33/28` | `172.20.20.1`<br>— | `g2-pc1-gw.grupo2.bsi-26-2.maceio.lab` | `g2-pc1-gw` |
| **`g2-pc1-vm1`** | PC 1 | `enp0s3` (LAN) | Bridge Adapter | `192.168.26.34/28` | `192.168.26.33` | `g2-pc1-vm1.grupo2.bsi-26-2.maceio.lab` | `g2-pc1-vm1` |
| **`g2-pc1-vm2`** | PC 1 | `enp0s3` (LAN) | Bridge Adapter | `192.168.26.35/28` | `192.168.26.33` | `g2-pc1-vm2.grupo2.bsi-26-2.maceio.lab` | `g2-pc1-vm2` |
| **`g2-pc2-vm1`** | PC 2 | `enp0s3` (LAN) | Bridge Adapter | `192.168.26.36/28` | `192.168.26.33` | `g2-pc2-vm1.grupo2.bsi-26-2.maceio.lab` | `g2-pc2-vm1` |
| **`g2-pc2-vm2`** | PC 2 | `enp0s3` (LAN) | Bridge Adapter | `192.168.26.37/28` | `192.168.26.33` | `g2-pc2-vm2.grupo2.bsi-26-2.maceio.lab` | `g2-pc2-vm2` |
| **`g2-pc3-vm1`** | PC 3 | `enp0s3` (LAN) | Bridge Adapter | `192.168.26.38/28` | `192.168.26.33` | `g2-pc3-vm1.grupo2.bsi-26-2.maceio.lab` | `g2-pc3-vm1` |
| **`g2-pc3-vm2`** | PC 3 | `enp0s3` (LAN) | Bridge Adapter | `192.168.26.39/28` | `192.168.26.33` | `g2-pc3-vm2.grupo2.bsi-26-2.maceio.lab` | `g2-pc3-vm2` |
| **`g2-pc4-vm1`** | PC 4 | `enp0s3` (LAN) | Bridge Adapter | `192.168.26.40/28` | `192.168.26.33` | `g2-pc4-vm1.grupo2.bsi-26-2.maceio.lab` | `g2-pc4-vm1` |
| **`g2-pc4-vm2`** | PC 4 | `enp0s3` (LAN) | Bridge Adapter | `192.168.26.41/28` | `192.168.26.33` | `g2-pc4-vm2.grupo2.bsi-26-2.maceio.lab` | `g2-pc4-vm2` |

---

### 3. Especificações Técnicas de Hardware Virtual

* **Sistema Operacional:** Ubuntu Server LTS (Modo CLI / Sem interface gráfica)
* **Memória RAM por VM:** 2048 MB
* **Processadores por VM:** 1 vCPU
* **Disco Rígido:** 10 GB (Alocação Dinâmica / LVM)

---

### 4. Resolução de Nomes Local (`/etc/hosts`)

O arquivo `/etc/hosts` foi preenchido de forma idêntica em **todas as 9 VMs** para permitir a comunicação via FQDN e apelido curto:

```text
127.0.0.1       localhost
127.0.1.1       <FQDN_DA_VM_ATUAL>

# Mapeamento do Grupo 2 - bsi-26-2
192.168.26.33   g2-pc1-gw.grupo2.bsi-26-2.maceio.lab     g2-pc1-gw
192.168.26.34   g2-pc1-vm1.grupo2.bsi-26-2.maceio.lab    g2-pc1-vm1
192.168.26.35   g2-pc1-vm2.grupo2.bsi-26-2.maceio.lab    g2-pc1-vm2
192.168.26.36   g2-pc2-vm1.grupo2.bsi-26-2.maceio.lab    g2-pc2-vm1
192.168.26.37   g2-pc2-vm2.grupo2.bsi-26-2.maceio.lab    g2-pc2-vm2
192.168.26.38   g2-pc3-vm1.grupo2.bsi-26-2.maceio.lab    g2-pc3-vm1
192.168.26.39   g2-pc3-vm2.grupo2.bsi-26-2.maceio.lab    g2-pc3-vm2
192.168.26.40   g2-pc4-vm1.grupo2.bsi-26-2.maceio.lab    g2-pc4-vm1
192.168.26.41   g2-pc4-vm2.grupo2.bsi-26-2.maceio.lab    g2-pc4-vm2
```