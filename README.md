# 🛡️ Administrar Proteção de Dados no Azure

## 📌 Centro de Backup (Azure Backup Center)
O Azure Backup Center oferece:

- **Painel centralizado**: Um local único para gerenciar todos os backups em ambientes grandes e distribuídos no Azure.

- **Gerenciamento centrado na fonte de dados**: A organização é feita com base no que está sendo protegido, como VMs, arquivos, bancos de dados, etc.

- **Integrações nativas**: Conectividade com outros serviços do Azure, como o Azure Monitor, para gerenciamento em escala e automação.

---

## 🔁 Serviços de Recuperação (Recovery Services)

### ✅ Cargas de trabalho com suporte

**Workloads do Azure**:
- Máquinas Virtuais (VMs)
- SQL Server em VMs
- Discos gerenciados
- Outros recursos compatíveis

**Workloads locais (on-premises)**:
- Servidores físicos
- VMs Hyper-V
- VMs VMware
- Arquivos e pastas em sistemas Windows

---

## 💾 Configurar Backup para Máquinas Locais (Windows)

### Etapas para backup local com o **Agente de Serviços de Recuperação (MARS Agent)**:

1. **Criar um Cofre dos Serviços de Recuperação (Recovery Services Vault)**
   - No portal do Azure, criar um Recovery Services Vault.
   - Escolher a região e o grupo de recursos adequados.

2. **Baixar o agente e o arquivo de credencial**
   - O agente MARS é responsável por executar o backup no servidor local.
   - O arquivo de credencial conecta o agente ao cofre.

3. **Instalar e registrar o agente**
   - Instalar o agente MARS no servidor ou máquina virtual Windows.
   - Durante a instalação, utilizar o arquivo de credencial para conectar ao Azure.

4. **Configurar o backup**
   - Selecionar arquivos e pastas a serem protegidos.
   - Definir a política de backup (horário de execução, retenção, etc.).

---

## 🔧 Características do Agente de Serviços de Recuperação (MARS)

✅ Permite backup e recuperação de arquivos e pastas no sistema operacional Windows (físico ou virtual).

✅ Funciona em máquinas virtuais locais ou no Azure.

❌ Não requer servidor de backup separado.

❌ Não realiza backup de aplicações, apenas:
- Arquivos
- Pastas
- Volumes (nível de volume)

❌ Não é compatível com sistemas Linux.

---

## 📸 Instantâneos (Snapshots)

Os instantâneos gerenciados são uma forma rápida e simples de fazer backup dos discos de máquinas virtuais que utilizam discos gerenciados no Azure.

- Capturam o estado do disco em um determinado momento.
- Permitem recuperação rápida.
- Normalmente usados para backups rápidos e pontuais.

Prática comum:
- Backup noturno com **Azure Backup** para retenção de longo prazo.
- Snapshots durante o dia para captura rápida do estado atual dos recursos.

---

## ☁️ Azure Backup

O Azure Backup oferece proteção mais completa e gerenciada para:
- Máquinas virtuais
- Arquivos
- Bancos de dados
- Outras cargas de trabalho

Características:
- Suporta backups consistentes com aplicativos.
- Compatível com VMs Windows e Linux.
- Utiliza VSS no Windows para garantir consistência de dados em uso.
- Ideal para políticas de retenção, agendamento e recuperação granular.

---

## 🌍 Azure Site Recovery (ASR)

O Azure Site Recovery é focado em **recuperação de desastres (Disaster Recovery)**.

- Protege VMs por meio de replicação para outra região ou localidade.
- Em caso de falha grave (ex: indisponibilidade total de uma região), permite **failover** para a região de recuperação.
- Diferente do Backup, o ASR mantém as VMs continuamente replicadas para permitir continuidade quase imediata dos serviços.

---

## 🧪 Exemplo prático

- À noite, o backup automático do Azure é executado e protege os recursos com a retenção definida.
- Durante o dia, são criados instantâneos manuais dos discos para captura rápida do estado atual.
- Útil antes de mudanças ou atualizações críticas, quando é necessário um ponto de recuperação rápido.

---

## 1️⃣ Instantâneos (Snapshots) de VMs

- Criam uma cópia pontual do disco da VM.
- Usados como parte do processo de backup para recuperação rápida.
- Retenção de restauração instantânea: **1 a 5 dias**.

**Benefício**:
- Reduzem o tempo de recuperação, pois não é necessário aguardar a transferência completa dos dados para o Recovery Services Vault.

---

## 2️⃣ Recovery Services Vault (Cofre de Serviços de Recuperação)

- Repositório centralizado para armazenar backups e pontos de recuperação.
- Pode proteger múltiplas cargas de trabalho no Azure e locais:
  - Máquinas Virtuais Azure e locais (Hyper-V, VMware)
  - Arquivos e pastas
  - SQL Server, SharePoint, Exchange
  - Estado do sistema e Bare-Metal Recovery

Um único cofre pode proteger vários servidores e diferentes cargas de trabalho.

---

## 3️⃣ Processo para Backup de VMs

1. **Criar um Recovery Services Vault**
   - Deve estar na mesma região dos recursos a serem protegidos.
   - Escolher a estratégia de replicação:
     - LRS (Locally Redundant Storage)
     - GRS (Geo-Redundant Storage)
     - ZRS (Zone-Redundant Storage)

2. **Definir o backup pelo portal**
   - Configurar a política de backup.
   - Definir a criação de instantâneos (pontos de recuperação) em intervalos definidos.

3. **Armazenamento dos backups**
   - Os pontos de recuperação são armazenados no Recovery Services Vault.

4. **Agente de Backup na VM**
   - O agente/extensão de backup do Azure deve estar instalado e em execução para garantir backups consistentes.

---

## 4️⃣ Restaurar Máquinas Virtuais

- A restauração dispara um **job** monitorado pelo Azure Backup.
- O status, logs, notificações e progresso da operação podem ser acompanhados pelo portal do Azure.

---

## 5️⃣ Servidor de Backup Azure (DPM / MABS)

- Suporta backups com reconhecimento de aplicativos (SQL Server, Exchange, etc.).
- Permite backups granulares de:
  - Arquivos
  - Pastas
  - Volumes
  - Estado da máquina

Cada computador executa:
- Agente de proteção do DPM ou MABS.
- Agente MARS no servidor DPM/MABS.

Oferece maior flexibilidade para:
- Agendamento
- Gerenciamento centralizado
- Proteção de múltiplas máquinas por meio de grupos de proteção

<img width="595" height="268" alt="image" src="https://github.com/user-attachments/assets/37ac75ff-5ea2-4d7c-8053-b0883f0d8bdd" />


---

## 6️⃣ Gerenciar Exclusões Temporárias (Soft Delete)
<img width="607" height="308" alt="image" src="https://github.com/user-attachments/assets/39c19d46-1016-4ef2-b7f7-ee43a56d3a76" />

- Disponível para:
  - Recovery Services Vault
  - Contas de armazenamento
  - Compartilhamentos de arquivos

Características:
- Permite recuperar dados excluídos temporariamente.
- Os dados excluídos permanecem retidos por **14 dias**.
- Funcionalidade habilitada automaticamente em todos os cofres.

---

## 7️⃣ Implementar Azure Site Recovery (ASR)

- Replicação contínua de VMs para fins de recuperação de desastres.

### Suporte do ASR:
- VMs do Azure entre regiões do Azure.
- VMs locais (VMware, Hyper-V).
- Servidores físicos (Windows e Linux).
- VMs do Azure Stack para Azure.
- Instâncias Windows da AWS para Azure.

Permite:
- Failover para um site secundário em caso de falha da região primária.
- Planejamento de continuidade de negócios.

## 🔄 O que é Failover?

- Failover é o processo de mudar automaticamente ou manualmente a operação de um sistema do ambiente principal (primário) para um ambiente de backup (secundário) quando ocorre uma falha.
Em termos simples: deu problema no principal → o sistema “vira” para o secundário.
---

## 📚 Referências

- https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016
- https://learn.microsoft.com/azure/backup/backup-azure-vms-introduction
- https://learn.microsoft.com/azure/virtual-machines/windows/snapshot-copy-managed-disk
