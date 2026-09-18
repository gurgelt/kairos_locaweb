<div align="center">

<img width="1289" height="469" alt="image" src="https://github.com/user-attachments/assets/cecd042a-006f-4ba5-a463-1b8296386749" />


# KAIRÓS
### Plataforma de AIOps para antecipação de incidentes e risco de OLA

**Challenge Locaweb × FIAP 2026 — Sprint 4 · Solução Final (rev01)**

[![Challenge](https://img.shields.io/badge/Challenge-Locaweb%20%C3%97%20FIAP%202026-e30613?style=for-the-badge)](#)
[![Sprint](https://img.shields.io/badge/Sprint-4%20%7C%20Solu%C3%A7%C3%A3o%20Final-1f1f1f?style=for-the-badge)](#)
[![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-2ea44f?style=for-the-badge)](#)
[![Turma](https://img.shields.io/badge/Turma-2TSCPV-0d1117?style=for-the-badge)](#)

![Azure](https://img.shields.io/badge/Microsoft%20Azure-0089D6?style=flat-square&logo=microsoftazure&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

</div>

---

## 📌 Sumário

- [Sobre o projeto](#-sobre-o-projeto)
- [O time](#-o-time)
- [O desafio](#-o-desafio)
- [A persona: Ana](#-a-persona-ana)
- [Objetivos do projeto](#-objetivos-do-projeto)
- [Público-alvo × outputs](#-público-alvo--outputs)
- [Proposta da solução](#-proposta-da-solução)
- [Arquitetura em nuvem](#️-arquitetura-em-nuvem-azure)
- [Stack tecnológico](#-stack-tecnológico)
- [Projeção de custo](#-projeção-de-custo-azure)
- [Modelagem preditiva e resultados](#-modelagem-preditiva-e-resultados)
- [Demonstração — dashboards](#-demonstração--dashboards)
- [Vídeo pitch](#-vídeo-pitch)
- [Evolução das sprints](#-evolução-das-sprints-1--4)
- [Conclusão e próximos passos](#-conclusão-e-próximos-passos)
- [Links úteis](#-links-úteis)

---

## 🎯 Sobre o projeto

O **Kairós** é uma plataforma de **AIOps** que transforma o histórico de incidentes de ITSM da Locaweb em **antecipação**: prevê o volume de incidentes para **D+1 e D+7** por prioridade (P2 e P3), projeta o **risco de perda de OLA** e agrupa **causas raiz recorrentes** por meio de processamento de linguagem natural (NLP).

O projeto foi desenvolvido ao longo de 4 sprints como parte do **Challenge Locaweb × FIAP 2026**, com o objetivo de avançar para a fase **NEXT** da competição.

<div align="center">

| Métrica | Resultado |
|---|---|
| 🧠 Erro de previsão (MAE) do volume que consome OLA | **-20%** vs. baseline sazonal |
| 🛡️ Classificador de risco de violação de OLA (CatBoost) | **ROC-AUC 0,82 – 0,84** |
| 🕸️ Descrições de incidentes agrupadas por NLP | **11.372 → 20** clusters nomeáveis |
| 🎯 Captura de violações revisando 20% da fila priorizada | **63%** das violações de OLA |

</div>

---

## 👥 O time

**Grupo Kairós · Turma 2TSCPV**

<img width="1320" height="569" alt="image" src="https://github.com/user-attachments/assets/fd058f30-ba3d-4ae2-8516-42e67721f16c" />


---

## 🧩 O desafio

A **Locaweb** — mais de 25 anos de mercado, +500 mil sites hospedados e +3,4 milhões de caixas de e-mail — registra incidentes diariamente em uma plataforma de ITSM, classificados por prioridade (P1–P5), produto, categoria e equipe responsável.

### Por que isso é um problema

> **Monitoramento reativo:** a operação só age depois que o incidente estoura, colocando o SLA/OLA em risco.

### O achado que direcionou o projeto

Identificamos um **salto de 7×** no volume de incidentes em **01/09/2025**. Ao investigar, descobrimos que:
- **90,7%** desses alertas fecham como *"Sem Intervenção"* (mediana de 8 min de resolução);
- **3.766 tipos de alerta inéditos** surgiram no mesmo dia;
- Uma nova camada de monitoramento estava gerando **ruído automático que não consome OLA**.

A distinção entre a série **"Com Intervenção" (entra no KPI)** — estável, ~50 a 100 incidentes/dia — e a série **"Sem Intervenção" (ruído automático)** — que salta para 700–1.000 incidentes/dia após o pico — passou a ser o critério oficial de modelagem do Kairós: todos os modelos preveem apenas a série que de fato consome OLA.

### O que pode ser feito

A **antecipação** do volume e do risco de violação de OLA, e das **causas crônicas** que se repetem — foi esse o problema que o Kairós foi desenhado para resolver.

---

## 🙋 A persona: Ana

Para ancorar a apresentação da Solução Final em um caso de uso concreto, o pitch da Sprint 4 passou a ser narrado pela perspectiva de **Ana**, uma analista fictícia do NOC da Locaweb: hoje ela reage a incidentes depois que eles já estouraram o OLA, sem visibilidade do que vem pela frente. A jornada mostra esse "antes" reativo e o "depois" com o Kairós — dashboards preditivos indicando onde agir antes da violação acontecer. Os quatro objetivos do projeto (seção seguinte) respondem diretamente ao que **"a Ana" passa a ter em mãos** com a solução.

---

## 🎯 Objetivos do projeto

Quatro objetivos foram definidos pelo desafio — e é isso que a Ana passa a ter em mãos com o Kairós:

| # | Objetivo | Como o Kairós entrega |
|---|---|---|
| 01 | **Antecipar incidentes** | Previsão de volume para o próximo dia (D+1) e a próxima semana (D+7) |
| 02 | **Identificar tendências** | Por prioridade (P2 e P3 obrigatórias), categoria, produto e item de configuração |
| 03 | **Projetar impacto nos KPIs** | Tendência diária de volume e de perda de OLA (estouro do prazo de atendimento) |
| 04 | **Apoiar a decisão operacional** | Indica onde agir preventivamente, com recomendações práticas para a operação |

---

## 🧭 Público-alvo × Outputs

Cada camada da operação recebe a informação certa, no formato certo:

| | Equipe NOC | Engenharia / SRE | Gerência / Liderança |
|---|---|---|---|
| **Foco** | Tático · resposta rápida | Estabilidade · prevenção | Estratégico · KPIs |
| **Recebe** | Alertas proativos de curto prazo (D+0 / D+1) | Análises de clusterização a médio prazo (D+7) | Visão consolidada de risco e eficiência |
| **Formato** | Notificações no dashboard operacional | Painel analítico de padrões de falha | Dashboard executivo preditivo |
| **Uso** | Aviso de picos iminentes, causa provável e sugestão de runbook | Causas crônicas P2/P3 por produto e onde automatizar | Projeção de OLA, risco de meta e mapa de atrito por produto |

---

## 💡 Proposta da solução

Uma plataforma de AIOps que integra **ingestão de dados de ITSM**, **engenharia de features** temporais/operacionais/semânticas e **três famílias de modelos**, entregando o resultado em um dashboard de monitoramento contínuo:

| Camada | Técnica | O que faz |
|---|---|---|
| 📈 **Volumetria** | Prophet e SARIMAX | Previsão de volume de incidentes (D+1 e D+7) sobre a série que consome OLA |
| 🛡️ **Classificação** | CatBoost | Risco de violação de OLA por incidente, priorizando a fila de triagem por probabilidade |
| 🕸️ **Padrões** | NLP (TF-IDF + KMeans) | Clusterização de causas raiz, agrupando reincidências em grupos nomeáveis |
| 🧭 **Direcionamento** | Regras + scoring | Recomendação operacional dinâmica: onde agir preventivamente, integrada ao monitoramento contínuo |

---

## ☁️ Arquitetura em nuvem (Azure)

<div align="center">
<img width="1284" height="712" alt="arquitetura-azure" src="https://github.com/user-attachments/assets/4dccca0e-bc39-4069-819c-edb28a22c141" />
</div>

Arquitetura de produção seguindo o **padrão Medallion** (Bronze / Silver / Gold):

1. **Ingestão** — dados do ITSM (Excel/CSV) chegam ao Data Lake (ADLS Gen2), nas camadas Bronze + Silver.
2. **Orquestração** — o Azure Data Factory orquestra o ETL e carrega a camada Gold no Azure SQL Database.
3. **CI/CD + Treino** — GitHub Actions + Azure Container Registry publicam a imagem; o Azure Machine Learning treina os modelos.
4. **Scoring diário** — o Azure Container Instances executa o job de scoring diário e grava a previsão de volta no banco.
5. **Consumo** — o Power BI consome o dashboard; observabilidade e rede privada por padrão (VNet + Private Endpoints).

---

## 🛠 Stack tecnológico

| Camada | Tecnologia | Papel |
|---|---|---|
| Data Lake | Azure Data Lake Storage Gen2 | Bronze (bruto imutável) + Silver (enriquecido com features preditivas) |
| Orquestração | Azure Data Factory | Ingestão, Data Flow (Bronze → Silver) e carga da camada Gold |
| Banco de dados | Azure SQL Database | Camada Gold — fonte única de verdade para o Power BI e o modelo de ML |
| Machine Learning | Azure Machine Learning | Treino, versionamento e registro dos modelos (Prophet, CatBoost, KMeans) |
| CI/CD | GitHub Actions + Azure Container Registry | CI/CD da imagem Docker de scoring — nada entra em produção sem o pipeline |
| Execução | Azure Container Instances | Job batch diário de scoring: lê dados, aplica modelos e grava a previsão |
| Observabilidade | Application Insights + Azure Monitor | Logs estruturados, métricas de infra e alertas proativos |
| Segurança | Key Vault + Rede Privada | Segredos protegidos e tráfego isolado em VNet com Private Endpoints |
| Visualização | Power BI | Dashboard executivo em modo Import sobre a camada Gold — 4 páginas |

---

## 💰 Projeção de custo (Azure)

Estimativa de custo mensal para operar a arquitetura do Kairós em ambiente operacional padrão de produção:

<div align="center">
<img width="1600" height="659" alt="custo-azure" src="https://github.com/user-attachments/assets/64a932ad-c56c-4bfa-a23f-ebcf057474ea" />
</div>

| Serviço Azure | Detalhamento / Dimensionamento | Custo mensal (USD) |
|---|---|---|
| Azure Data Factory | Ingestão e pipelines diários (Medallion) | US$ 20,00 |
| Azure SQL Database | Camada Gold · vCore Propósito Geral (32 GB) | US$ 104,78 |
| Storage Accounts (ADLS Gen2) | Data Lake 100 GB Hot Tier (LRS) | US$ 3,12 |
| Azure Key Vault | Gestão de credenciais e segredos (50k ops) | US$ 0,15 |
| Azure Container Instances (ACI) | Scoring diário em batch (2 vCPU, 4 GB RAM) | US$ 1,50 |
| Azure Monitor / App Insights | Logs e observabilidade (~3 GB de ingestão) | US$ 8,50 |
| **Total mensal estimado** | **Ambiente operacional padrão de produção** | **US$ 138,05** |

> A camada Gold em Azure SQL Database concentra cerca de **76%** do custo total; o restante da arquitetura (ingestão, storage, scoring, observabilidade e segredos) soma menos de US$ 35/mês.
>
> Fonte: Calculadora de Preços da Azure.

---

## 🔬 Modelagem preditiva e resultados

| Modelo | Papel | Resultado |
|---|---|---|
| Baseline sazonal | Referência obrigatória · P2/P3 | MAE 5,2 · 11,5 |
| **Prophet** ✅ | Uma série por prioridade (D+1/D+7) | MAE 3,9 · 9,1 (**-20%**) |
| SARIMAX | Vence no volume bruto | Não escala para N séries |
| **CatBoost** ✅ | Risco de violação de OLA | **ROC-AUC 0,84** na comparação de modelos · **0,82 ± 0,01** validado em produção (50 violações no teste) |
| TF-IDF + KMeans | Clusters de causa raiz | 20 grupos (k=20) |

**Destaques de validação:**
- 🎯 Revisando apenas **20% da fila** priorizada por risco, o time captura **63% das violações de OLA** — 3,1× melhor que uma triagem aleatória.
- 🕵️ **Sinal precursor identificado:** um servidor com mais de 10 chamados P4 na semana tem **16%** de chance de gerar um incidente sério (P2/P3) nos 3 dias seguintes — quase o dobro dos ~10% de um servidor "quieto".
- ⏰ **Pior janela de risco:** sábado, das 21h às 23h — a chance de violação é **4,4×** a média da operação.
- 🔎 A flag oficial de violação de KPI não reproduz integralmente a regra documentada — **3.020 incidentes** seguem pendentes de validação junto à Locaweb.
- 🔒 **Anti-leakage:** `SEED=42`, split sempre temporal e `TimeSeriesSplit` de 5 dobras — apenas atributos conhecidos na abertura do chamado entram no modelo (duração, resolução e status ficam de fora).

---

## 📊 Demonstração — dashboards

O dashboard passou a ter **4 páginas** no Power BI, cada uma com um público e uma decisão diferentes:

### Visão Executiva

<div align="center">
<img width="1345" height="761" alt="dashboard-executiva" src="https://github.com/user-attachments/assets/1c16f4ff-05bf-4df0-ba9d-90c0d9bb3504" />
</div>

Placar do contrato (2 KPIs × 2 prioridades): **3 de 4** indicadores batidos — falta apenas a quebra de OLA em P2, por 3 quebras. Chance de fechar o ano dentro do KPI de OLA: **70% em P2, 100% em P3**. Ritmo acumulado de quebras contra o limite rateado pelo tempo mostra P2 já acima da régua (107,7% do ritmo) enquanto P3 segue confortável (74,5%).

### Visão Operacional

<div align="center">
<img width="1263" height="718" alt="dashboard-operacional" src="https://github.com/user-attachments/assets/95b51da1-96d7-41d8-94ae-2243450539b0" />
</div>

Previsão da semana: **288 chamados em D+7** (41/dia), queda de **25,6%** vs. a janela anterior. Dimensionamento sugerido para amanhã: **49 chamados → 5 analistas** (premissa de 10 chamados/analista/dia, a confirmar com a Locaweb). Concentração de risco: um único alvo (`lsin·cat31`) responde por **32 das 238 violações do ano** — 13,4% das violações vindo de apenas 0,20% do volume tratado.

### Tendência e Ação

<div align="center">
<img width="1266" height="717" alt="dashboard-tendencia-acao" src="https://github.com/user-attachments/assets/aa2ededf-4d57-438d-a892-766f5aa08d26" />
</div>

Nova página do dashboard: compara os últimos 90 dias maduros contra os 90 anteriores. **14 chamados** precisam ser revisados antes de estourar — 9 deles já furaram o OLA no teste, contra apenas 0,99% da base geral. **37% das quebras do ano** se concentram em 5 alvos. Simulações do tipo "se este alvo for resolvido" quantificam o ganho: resolver `lsin·cat77` evitaria 9 quebras e destravaria 50 p.p. de atingimento em P2. A fila do dia traz o SHAP local de cada chamado, com a ação sugerida (ex.: "realocar ou reforçar a equipe").

### Visão Analítica (Estudos)

<div align="center">
<img width="1265" height="715" alt="dashboard-analitica" src="https://github.com/user-attachments/assets/fcf1c24c-24e3-4a55-ad08-3d39a2cfac49" />
</div>

Peso de cada fator no risco de OLA via SHAP sobre o CatBoost (ROC-AUC 0,82 ± 0,01): **Equipe** é o fator nº 1 (19,2%), **Produto** é o último (6,3%). Três achados que viram ação: chamado aberto na mão viola **2,7×** mais do que o aberto por monitoramento automático; chamado reincidente viola **menos**, não mais; fins de semana violam **1,8×** mais. Pior janela: sábado 21h–23h, **4,4×** a média da operação.

> 🔗 **Link funcional do dashboard (Power BI):** [app.powerbi.com/links/qh5oOHP-vM](https://app.powerbi.com/links/qh5oOHP-vM?ctid=11dbbfe2-89b8-4549-be10-cec364e59551&pbi_source=linkShare)

---

## 🎬 Vídeo pitch

Apresentação da solução em formato *hands-on*:

📺 **[Assistir ao vídeo pitch no YouTube](https://youtu.be/v3YxQyDFvHo?si=iiiqYt5xHw7uboks)**

---

## 🗺 Evolução das sprints (1 → 4)

| Sprint | Foco | Entrega |
|---|---|---|
| **1** | Problema & contexto | Definição do desafio, benchmark e contextualização da operação |
| **2** | Arquitetura & protótipos | Proposta de solução, arquitetura, telas e tecnologias |
| **3** | Modelagem preditiva | Prophet, CatBoost e clusters; achados e engenharia de 30 features |
| **4** | Solução final | Dashboard de 4 páginas, projeção de custo, pipeline em nuvem, vídeo pitch e entrega técnica |

---

## ✅ Conclusão e próximos passos

<table>
<tr>
<th align="left">✅ Resultados obtidos</th>
<th align="left">💡 Aprendizados-chave</th>
<th align="left">🚀 Próximos passos</th>
</tr>
<tr>
<td valign="top">

- Previsão de volume D+1/D+7 com **-20%** de erro vs. baseline
- Risco de OLA priorizando a fila: **63%** das violações revisando 20%
- **11.372** descrições reduzidas a **20 clusters** operacionais nomeáveis

</td>
<td valign="top">

- Modelar a série que consome OLA (CV 0,43), não o volume bruto ruidoso
- Com poucos dados, modelos com estrutura vencem os mais flexíveis
- Honestidade metodológica: declarar limitações reforça o rigor

</td>
<td valign="top">

- Operacionalizar a solução na nuvem, atualizando os painéis em tempo real
- Integrar o modelo desenvolvido ao sistema de gestão de incidentes da Locaweb

</td>
</tr>
</table>

---

## 🔗 Links úteis

| Recurso | Link |
|---|---|
| 📊 Dashboard Power BI | [Abrir dashboard](https://app.powerbi.com/links/qh5oOHP-vM?ctid=11dbbfe2-89b8-4549-be10-cec364e59551&pbi_source=linkShare) |
| 🎬 Vídeo pitch | [Assistir no YouTube](https://youtu.be/v3YxQyDFvHo?si=iiiqYt5xHw7uboks) |

---

<div align="center">

**Grupo Kairós** · Turma 2TSCPV · Challenge Locaweb × FIAP 2026

*"Pointing the Direction"*

</div>
