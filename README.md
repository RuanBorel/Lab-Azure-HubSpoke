# Lab-Azure-HubSpoke
Laboratório prático de criação de uma infraestrutura de rede cloud baseada na topologia HubSpoke

# Projeto: Topologia Hub-Spoke no Azure com NVA, UDR e Private DNS

Este laboratório demonstra a construção de uma topologia **Hub-Spoke** no Microsoft Azure utilizando:

- 3 VNets (Hub, Spoke A e Spoke B)
- Peering Global e Regional
- Roteamento centralizado via **NVA**
- **UDR (User Defined Routes)** para tráfego East-West
- Private DNS para resolução entre VMs em diferentes VNets
- NSGs aplicados por subnet
- Testes de conectividade (ping por IP e hostname)

---

## 📌 Objetivo do Projeto

Criar uma arquitetura de rede semelhante ao ambiente usado em empresas reais, onde:

- Todo o tráfego entre VNets é centralizado no **Hub**
- O **NVA** faz o roteamento entre Spokes
- As VMs conseguem se comunicar usando **nomes (FQDN)** via Private DNS
- A topologia pode ser expandida no futuro com Azure Firewall, VPN Gateway ou Azure Bastion

Este projeto faz parte do meu portfólio de evolução para **Azure Administrator / Cloud Engineer** e também como preparação para a certificação **AZ-104**.

---

## 🗺 Arquitetura

<img width="647" height="440" alt="image" src="https://github.com/user-attachments/assets/725edddd-6379-45d6-8569-ae21a89e49ec" />

---

## Componentes Implementados

### 🟦 VNETs
|   VNET  |    Range    |      Descrição       |
|---------|-------------|----------------------|
|   Hub   | 10.1.0.0/16 | Central de roteamento |
| Spoke A | 10.2.0.0/16 | Serviços do grupo A |
| Spoke B | 10.3.0.0/16 | Serviços do grupo B |

---

### 🟩 Subnets
- `10.1.1.0/24` – Subnet HUB (NVA)
- `10.2.1.0/24` – Subnet Spoke A (VM1)
- `10.2.2.0/24` – Subnet Spoke A (VM2)
- `10.3.1.0/24` – Subnet Spoke B (VM1)

---

### 🛡 NSGs e ASGs
- NSG-HUB
- NSG-SPOK-A
- NSG-SPOK-B

Regras configuradas para permitir tráfego interno.

---

### 🌐 Peering
- **Hub ↔ Spoke A (Global)**
- **Hub ↔ Spoke B (Regional)**

---

## 🚦 Roteamento (UDR)
Rota Spoke A → Spoke B:
```
10.3.0.0/16 → 10.1.1.4 (NVA)
```

Rota Spoke B → Spoke A:
```
10.2.0.0/16 → 10.1.1.4 (NVA)
```

---

## 🧭 NVA (Router)
- Máquina virtual no Hub  
- IP: 10.1.1.4  
- Responsável pelo tráfego East-West  
- Testado com sucesso via ping

---

## 🔵 Private DNS
- Criado Private DNS Zone
- Registrado registros A das VMs
- Comunicação entre VMs via hostname funcionando

Exemplo:
```
ping vm-spokb-01.dominio.com
```

---

## 🧪 Testes Realizados

### ✔ Ping Spoke A → Spoke B via IP  
### ✔ Ping Spoke B → Spoke A via IP  
### ✔ Ping Spoke A → Spoke B via hostname  
### ✔ Tráfego roteado corretamente pelo NVA  
### ✔ Consulta DNS funcionando

---

## 🚀 Próximas Melhorias (Versão 2.0)

- Adicionar Azure Firewall no Hub  
- Implementar Azure Bastion  
- Adicionar Log Analytics + Diagnostics  
- Criar rota default (0.0.0.0/0) para NVA ou Firewall  
- Implementar Private DNS Resolver  

---

## 📚 Tecnologias Utilizadas

- Azure Virtual Network  
- Subnets  
- Peering  
- NVA  
- UDR  
- NSG  
- Private DNS  
- Azure Portal + Azure CLI  

---

## 🧑‍💻 Autor

**Ruan Carlos Eduardo Borel**  
Azure Administrator (em preparação – AZ-104)  
LinkedIn: www.linkedin.com/in/ruan-borel-198806185
GitHub: https://github.com/RuanBorel

---

## 📌 Licença
Projeto livre para uso educacional.
