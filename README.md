<p align="center">
  <img src="./assets/vidamais-banner.jpeg" alt="VidaMais Telemedicina" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/AWS-re%2FStart-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white" />
  <img src="https://img.shields.io/badge/LGPD-Conforme-0052B4?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Região-sa--east--1%20(São%20Paulo)-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" />
</p>


## Sumário

- [Sumário](#sumário)
- [Sobre o projeto](#sobre-o-projeto)
- [Problema](#problema)
- [Solução](#solução)
- [Antes x Depois](#antes-x-depois)
- [Arquitetura Proposta](#arquitetura-proposta)
- [Escopo do MVP](#escopo-do-mvp)
- [Requisitos Não-Funcionais](#requisitos-não-funcionais)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Integrantes do Grupo](#integrantes-do-grupo)


## Sobre o projeto

Este projeto é o Trabalho de Conclusão de Curso (TCC) do programa **AWS re/Start**, referente ao **Case VidaMais Telemedicina**. 

A rede de clínicas VidaMais oferece consultas médicas por videochamada e mantém o prontuário eletrônico dos pacientes. O objetivo deste trabalho é propor a modernização da sua arquitetura legada, migrando-a para a nuvem da AWS. 

Com isso, pretende-se centralizar o prontuário eletrônico para todas as unidades, garantindo maior disponibilidade, resiliência, segurança e total conformidade com a Lei Geral de Proteção de Dados (**LGPD**).


## Problema

Atualmente, a estrutura da VidaMais apresenta diversas fragilidades que comprometem tanto o atendimento, quanto a segurança dos dados dos pacientes: 

- **Prontuários isolados:** Cada clínica possui seu próprio sistema de prontuário eletrônico local, sem integração entre as unidades da rede.
- **Ponto único de falha:** Apenas um servidor físico por clínica, sem redundância. Caso ele falhe, o acesso ao histórico dos pacientes daquela unidade é perdido.
- **Backup inadequado:** Os backups são realizados manualmente em fita magnética, apenas uma vez por semana, representando um alto risco de perda de dados.
- **Dificuldade em situações de urgência:** A falta de integração dificulta o acesso rápido ao histórico médico em momentos críticos.
- **Dados sensíveis em risco:** A arquitetura atual não atende aos requisitos de controle de acesso, criptografia e proteção exigidos para dados de saúde pela LGPD.


## Solução

A proposta consiste em migrar toda a estrutura para a nuvem da AWS, criando um **prontuário eletrônico centralizado, seguro e sempre disponível** para toda a rede de clínicas. 

Dessa forma, busca-se: 

- **Unificação dos dados:** Histórico médico acessível para qualquer profissional autorizado, em qualquer unidade da rede.
- **Alta disponibilidade:** Utilização de serviços gerenciados e arquitetura Multi-AZ para garantir o funcionamento, mesmo em caso de falha.
- **Segurança reforçada:** Controle de acesso rigoroso, autenticação com MFA, criptografia em repouso e em trânsito, em conformidade com a LGPD.
- **Backup automatizado:** Substituição das fitas magnéticas por backups automatizados, com cópia isolada e retenção imutável.
- **Agilidade em urgências:** Acesso imediato ao prontuário em situações críticas.
- **Experiência completa ao paciente:** Agendamento, atendimento por videochamada, emissão digital de receitas e atestados e lembretes automáticos.


## Antes x Depois

| Aspecto | **Antes (Arquitetura Legada)** | **Depois (Proposta na AWS)** |
| :------- | :---------------------------- | :-------------------------- |
| **Prontuário** | Isolado por clínica, sem integração entre unidades. | Centralizado e unificado para toda a rede. |
| **Disponibilidade** | Servidor físico único por clínica (ponto único de falha). | Arquitetura Multi-AZ com serviços gerenciados da AWS. |
| **Backup** | Manual, em fita magnética, apenas uma vez por semana. | Automatizado, com cópia isolada e retenção imutável. |
| **Acesso em urgência** | Limitado à clínica onde o paciente foi atendido. | Acesso rápido ao histórico para profissionais autorizados em qualquer unidade. |
| **Recuperação de Desastres** | Sem metas definidas. | **RPO** < 1 minuto \| **RTO** entre 30 e 60 minutos. |
| **Segurança** | Controles limitados, sem padronização. | Controle de acesso, MFA, criptografia e conformidade com a LGPD. |


## Arquitetura Proposta

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'background': '#ffffff', 'primaryColor': '#ffffff', 'primaryTextColor': '#000000', 'primaryBorderColor': '#000000', 'lineColor': '#333333', 'secondaryColor': '#f8f9fa', 'tertiaryColor': '#f8f9fa', 'clusterBkg': '#f9fbff', 'clusterBorder': '#7C8C92', 'edgeLabelBackground': '#ffffff' }}}%%

graph TD
    %% Definição de Cores por Camada
    classDef users fill:#E8F4FD,stroke:#1F78B4,stroke-width:2px,color:#000000
    classDef edge fill:#FFF3E0,stroke:#FF9900,stroke-width:2px,color:#000000
    classDef auth fill:#FDECEA,stroke:#D13212,stroke-width:2px,color:#000000
    classDef vpc fill:#F0F7FF,stroke:#0052B4,stroke-width:2px,color:#000000
    classDef az fill:#FFFFFF,stroke:#7C8C92,stroke-width:1.5px,color:#000000
    classDef compute fill:#FFF0E6,stroke:#D13212,stroke-width:2px,color:#000000
    classDef data fill:#F3F0FF,stroke:#3B48CC,stroke-width:2px,color:#000000
    classDef cache fill:#E8F6FF,stroke:#2F9CFF,stroke-width:2px,color:#000000
    classDef storage fill:#F0FFF4,stroke:#3F8624,stroke-width:2px,color:#000000
    classDef search fill:#E6FFFA,stroke:#319795,stroke-width:2px,color:#000000
    classDef video fill:#E6F7FF,stroke:#1890FF,stroke-width:2px,color:#000000
    classDef reminders fill:#F6FFED,stroke:#52C41A,stroke-width:2px,color:#000000
    classDef security fill:#FFF1F0,stroke:#CF1322,stroke-width:2px,color:#000000
    classDef gov fill:#FEF6FF,stroke:#722ED1,stroke-width:2px,color:#000000
    classDef dr fill:#F8F9FA,stroke:#595959,stroke-width:2px,color:#000000

    %% Usuários
    subgraph USR ["Usuários"]
        Users["Pacientes / Médicos<br/>(Web / App)"]
    end

    %% Nuvem AWS
    subgraph CLOUD ["Amazon Web Services"]

        %% Camada de Borda
        subgraph EDGE ["Camada de Borda"]
            CF["Amazon CloudFront"]
            WAF["AWS WAF"]
            SHIELD["AWS Shield"]
            APIGW["Amazon API Gateway"]
            ALB["Application Load Balancer"]
            COGNITO["Amazon Cognito"]
        end

        %% Região Principal
        subgraph SA_EAST ["Região Principal - sa-east-1 (São Paulo)"]

            %% VPC
            subgraph VPC ["VPC - Subnets Privadas | Multi-AZ | Auto Scaling"]
                
                %% Camada de Aplicação
                FARGATE["AWS Fargate<br/>(Serviços da Aplicação)"]

                %% Zona de Disponibilidade A
                subgraph AZ_A ["Zona de Disponibilidade A"]
                    AURORA_A[("Amazon Aurora PostgreSQL<br/>(Instância Primária)")]
                    ELASTICACHE["Amazon ElastiCache<br/>(Cache)"]
                    S3["Amazon S3<br/>(Documentos Digitais)"]
                end

                %% Zona de Disponibilidade B
                subgraph AZ_B ["Zona de Disponibilidade B"]
                    AURORA_B[("Amazon Aurora PostgreSQL<br/>(Instância Standby)")]
                    OPENSEARCH["Amazon OpenSearch Service<br/>(Busca)"]
                end
            end

            %% Serviços Complementares
            subgraph VIDEO ["Atendimento por Vídeo"]
                CHIME["Amazon Chime SDK<br/>(WebRTC - Videochamada)"]
            end

            subgraph REMINDERS ["Notificações e Lembretes"]
                EVB["Amazon EventBridge Scheduler"]
                SNS["Amazon SNS"]
                SES["Amazon SES<br/>(E-mail / SMS)"]
            end

            subgraph GOV ["Segurança, Auditoria e Governança"]
                KMS["AWS KMS<br/>(Criptografia)"]
                CT["AWS CloudTrail<br/>(Auditoria)"]
                CONFIG["AWS Config<br/>(Conformidade)"]
                GD["Amazon GuardDuty<br/>(Detecção de Ameaças)"]
                SH["AWS Security Hub<br/>(Visão Centralizada)"]
                MACIE["Amazon Macie<br/>(Proteção de Dados Sensíveis)"]
            end
        end

        %% Região Secundária - DR
        subgraph DR ["Região Secundária - Disaster Recovery"]
            AURORA_DR[("Amazon Aurora Global Database<br/>(Replicação Cross-Region)")]
            S3_DR[("Amazon S3 Cross-Region Replication")]
            BACKUP["AWS Backup + AWS Backup Vault Lock<br/>(Imutabilidade)"]
        end
    end

    %% Fluxos Principais
    Users --> CF
    CF --> WAF --> SHIELD --> APIGW --> ALB
    ALB --> COGNITO
    COGNITO --> FARGATE

    %% Conexões com Dados e Aplicação
    FARGATE --> AZ_A
    FARGATE --> AZ_B
    AURORA_A -. "Replicação Síncrona (Multi-AZ)" .- AURORA_B

    FARGATE --> CHIME
    FARGATE --> EVB
    EVB --> SNS --> SES

    %% Proteção e Governança
    FARGATE --> KMS

    %% Replicação para DR
    AZ_A -. "Replicação Cross-Region" .-> AURORA_DR
    AZ_B -. "Replicação Cross-Region" .-> AURORA_DR
    S3 -. "Replicação Cross-Region" .-> S3_DR
    AZ_A --> BACKUP
    AZ_B --> BACKUP

    %% Aplicação das Classes
    class Users users
    class CF,WAF,SHIELD,APIGW,ALB edge
    class COGNITO auth
    class VPC vpc
    class AZ_A,AZ_B az
    class FARGATE compute
    class AURORA_A,AURORA_B,AURORA_DR data
    class ELASTICACHE cache
    class S3,S3_DR storage
    class OPENSEARCH search
    class CHIME video
    class EVB,SNS,SES reminders
    class KMS,CT security
    class CONFIG,GD,SH,MACIE gov
    class DR,BACKUP dr
```

<p align="center">
  <em>Figura 1 - Arquitetura proposta para a centralização do prontuário eletrônico na AWS</em>
</p>

| Serviço AWS | Finalidade |
| :----------- | :----------- |
| **Amazon Aurora PostgreSQL (Multi-AZ)** | Banco de dados centralizado e resiliente para o prontuário eletrônico. |
| **Amazon ECS com AWS Fargate** | Execução da aplicação de telemedicina de forma escalável e gerenciada. |
| **Amazon Cognito** | Autenticação, autorização e aplicação de Autenticação Multifator (MFA). |
| **AWS KMS** | Gerenciamento de chaves para criptografia em repouso e em trânsito. |
| **Amazon S3** | Armazenamento seguro de documentos digitais (receitas, atestados e outros arquivos). |
| **Amazon Chime SDK / WebRTC** | Realização de consultas médicas por videochamada. |
| **Amazon EventBridge Scheduler** | Agendamento automatizado dos lembretes de consulta. |
| **Amazon SNS / Amazon SES** | Envio de notificações e lembretes via SMS, e-mail ou outros canais. |
| **AWS WAF** | Proteção da aplicação contra ataques na camada de borda. |

**Região utilizada:** `sa-east-1` (São Paulo)


## Escopo do MVP

O escopo da primeira versão (MVP) contempla os seguintes requisitos funcionais: 

- [x] **RF-01** - Agendar consulta e realizar atendimento por videochamada.
- [x] **RF-02** - Acessar e atualizar o prontuário eletrônico do paciente durante o atendimento.
- [x] **RF-03** - Emitir receita e atestado digitalmente ao final da consulta.
- [x] **RF-04** - Disponibilizar o histórico médico do paciente para qualquer profissional autorizado da rede.
- [x] **RF-05** - Enviar lembrete automático de consulta ao paciente antes do horário marcado.


## Requisitos Não-Funcionais

| Requisito | Descrição |
| :---------- | :----------- |
| **RNF-01 - Segurança e Conformidade** | Dados de saúde sensíveis, exigindo controle de acesso rigoroso, autenticação com MFA, criptografia e conformidade com a LGPD. |
| **RNF-02 - Disponibilidade** | O prontuário deve estar disponível a qualquer momento, inclusive em situações de urgência. |
| **RNF-03 - Desempenho** | A videochamada deve possuir boa qualidade de conexão e baixa latência. |
| **RNF-04 - Retenção** | O prontuário eletrônico deve ser mantido por muitos anos, conforme exigência legal. |
| **RNF-07 - Recuperação** | Metas estabelecidas para o projeto: **RPO < 1 minuto** e **RTO entre 30 e 60 minutos**. 


## Tecnologias Utilizadas

| Categoria | Tecnologias |
| :----------- | :----------- |
| **Nuvem** | Amazon Web Services (AWS) |
| **Banco de Dados** | Amazon Aurora PostgreSQL (Multi-AZ) |
| **Computação** | Amazon ECS com AWS Fargate |
| **Segurança e Identidade** | Amazon Cognito, AWS KMS, AWS WAF |
| **Armazenamento** | Amazon S3 |
| **Comunicação e Notificações** | Amazon Chime SDK, Amazon EventBridge Scheduler, Amazon SNS, Amazon SES |


## Integrantes do Grupo

| Nome | Função no Projeto |
| :---- | :----------------- |
| **Alexandre G.** | Cloud Engineer |
| **Aynoã N.** | Cloud Solutions Architect |
| **Ayra L.** | Cloud Business Analyst |
| **Felipe G.** | Compliance e LGPD em Nuvem |
| **Maria E.** | Cloud Product Owner |
| **Sthefany S.** | Cloud Data Engineer |
| **Wiani A.** | Cloud Security e SRE |