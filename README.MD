# Deploy e Configuração do Velero

## 1. Deploy do Velero

### 1.0 Estrutura
```
├── velero
    ├── crd
    ├── core
    ├── deploy
```
### 1.2 Deploy dos CRDs

Os Custom Resource Definitions (CRDs) são essenciais para o Velero, pois definem os recursos personalizados que o Velero utiliza para gerenciar backups, restores e outros objetos relacionados. O primeiro passo é aplicar os CRDs necessários:

- [velero.io_backuprepositories.yaml](crd/velero.io_backuprepositories.yaml)
- [velero.io_backups.yaml](crd/velero.io_backups.yaml)
- [velero.io_backupstoragelocations.yaml](crd/velero.io_backupstoragelocations.yaml)
- [velero.io_deletebackuprequests.yaml](crd/velero.io_deletebackuprequests.yaml)
- [velero.io_downloadrequests.yaml](crd/velero.io_downloadrequests.yaml)
- [velero.io_podvolumebackups.yaml](crd/velero.io_podvolumebackups.yaml)
- [velero.io_podvolumerestores.yaml](crd/velero.io_podvolumerestores.yaml)
- [velero.io_restores.yaml](crd/velero.io_restores.yaml)
- [velero.io_schedules.yaml](crd/velero.io_schedules.yaml)
- [velero.io_serverstatusrequests.yaml](crd/velero.io_serverstatusrequests.yaml)
- [velero.io_volumesnapshotlocations.yaml](crd/velero.io_volumesnapshotlocations.yaml)

### 1.3 Deploy dos Cores

Os arquivos Core são necessários para definir as permissões e os papéis que o Velero utilizará no cluster Kubernetes:

- [velero-cluster-role.yaml](core/velero-cluster-role.yaml)
- [velero-clusterrolebinding.yaml](core/velero-clusterrolebinding.yaml)
- [velero-role-binding.yaml](core/velero-role-binding.yaml)
- [velero-role.yaml](core/velero-role.yaml)
- [velero-service-account.yaml](core/velero-service-account.yaml)

### 1.4 Deploy do Velero

Os arquivos de deploy configuram a implantação do Velero no cluster Kubernetes, incluindo a localização de armazenamento de backups, configuração do ConfigMap, DaemonSet para Restic e agendamento de backups:

- [velero-bkp-storage-location.yaml](deploy/velero-bkp-storage-location.yaml)
- [velero-configmap.yaml](deploy/velero-configmap.yaml)
- [velero-deploy.yaml](deploy/velero-deploy.yaml)
- [velero-restic-daemonset.yaml](deploy/velero-restic-daemonset.yaml)
- [velero-schedule.yaml](deploy/velero-schedule.yaml)
- [velero-secret.yaml](deploy/velero-secret.yaml)

# Velero Backup Configuration

Este documento descreve a configuração do Velero para backups diários às 12:00 (meio-dia) no horário de Brasília, com retenção de 31 dias. Também inclui comandos para análise, criação e visualização de backups, logs e agendamentos.

## Estrutura dos Arquivos

### 1. `schedule.yaml`

Arquivo de configuração do agendamento de backup diário.

### 2. `README.md`

Este documento de instruções e documentação do projeto.

## Ordem de Deploy

1. **Instalar o Velero:**
   Certifique-se de que o Velero está instalado e configurado no seu cluster Kubernetes.

2. **Aplicar os CRDs:**
   Aplique os CRDs listados na seção 1.1.

3. **Aplicar os arquivos Core:**
   Aplique os arquivos Core listados na seção 1.2.

4. **Aplicar os arquivos de Deploy:**
   Aplique os arquivos de Deploy listados na seção 1.3.

5. **Aplicar o agendamento de backup:**
   Use o comando abaixo para criar um agendamento de backup diário às 12:00 BRT com retenção de 31 dias ou execute o velero-schedule.yaml.

## Comandos Úteis

### Criar o Agendamento de Backup Diário

```sh
velero schedule create daily-backup --schedule="0 15 * * *" --ttl 744h 
`--schedule="0 15 * * *": Cria backups diariamente às 15:00 UTC (12:00 BRT).
`--ttl 744h: Define o tempo de retenção para 31 dias (744 horas).

Verificar os Agendamentos de Backup Configurados
velero schedule get

Criar um Backup Imediato
velero backup create immediate-backup

Listar os Backups Existentes
velero backup get

Ver Detalhes de um Backup
velero backup describe <backup-name> --details

Visualizar os Logs de um Backup Específico
velero backup logs <backup-name>

Analisar os Logs do Velero
kubectl logs deployment/velero -n velero


## maintainer

**Matheus Silveira**

## Histórico de Versões

| Nome             | Data       | Versão | Observação                                 |
|------------------|------------|--------|--------------------------------------------|
| Matheus Silveira | 2024-06-13 | 1.0.0  | Deploy-do-velero                           |
| xxxxxxxxxxxx     | xxxx-xx-xx | x.x.x  | xxxxxxxxxxxxx-----xxxxxxxxxxxx             |
| xxxxxxxxxxxx     | xxxx-xx-xx | x.x.x  | xxxxxxxxxxxxx-----xxxxxxxxxxxx             |
| xxxxxxxxxxxx     | xxxx-xx-xx | x.x.x  | xxxxxxxxxxxxx-----xxxxxxxxxxxx             |
| xxxxxxxxxxxx     | xxxx-xx-xx | x.x.x  | xxxxxxxxxxxxx-----xxxxxxxxxxxx             |
