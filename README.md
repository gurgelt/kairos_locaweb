<div align="center">

<img width="1289" height="469" alt="image" src="https://github.com/user-attachments/assets/cecd042a-006f-4ba5-a463-1b8296386749" />


# KAIRÓS
### Plataforma de AIOps para antecipação de incidentes e risco de OLA

**Challenge Locaweb × FIAP 2026 — Sprint 4 · Solução Final**

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
- [Objetivos do projeto](#-objetivos-do-projeto)
- [Público-alvo × outputs](#-público-alvo--outputs)
- [Proposta da solução](#-proposta-da-solução)
- [Arquitetura em nuvem](#️-arquitetura-em-nuvem-azure)
- [Stack tecnológico](#-stack-tecnológico)
- [Modelagem preditiva e resultados](#-modelagem-preditiva-e-resultados)
- [Demonstração — dashboards](#-demonstração--dashboards)
- [Vídeo pitch](#-vídeo-pitch)
- [Evolução das sprints](#-evolução-das-sprints-1--4)
- [Conclusão e próximos passos](#-conclusão-e-próximos-passos)
- [Estrutura do repositório](#-estrutura-do-repositório)
- [Links úteis](#-links-úteis)

---

## 🎯 Sobre o projeto

O **Kairós** é uma plataforma de **AIOps** que transforma o histórico de incidentes de ITSM da Locaweb em **antecipação**: prevê o volume de incidentes para **D+1 e D+7** por prioridade (P2 e P3), projeta o **risco de perda de OLA** e agrupa **causas raiz recorrentes** por meio de processamento de linguagem natural (NLP).

O projeto foi desenvolvido ao longo de 4 sprints como parte do **Challenge Locaweb × FIAP 2026**, com o objetivo de avançar para a fase **NEXT** da competição.

<div align="center">

| Métrica | Resultado |
|---|---|
| 🧠 Erro de previsão (MAE) do volume que consome OLA | **-20%** vs. baseline sazonal |
| 🛡️ Classificador de risco de violação de OLA (CatBoost) | **ROC-AUC 0,85** |
| 🕸️ Descrições de incidentes agrupadas por NLP | **11.372 → 20** clusters nomeáveis |
| 🎯 Captura de violações revisando 20% da fila priorizada | **63%** das violações de OLA |

</div>

---

## 👥 O time

**Grupo Kairós · Turma 2TSCPV**

| Integrante | RM |
|---|---|
| Arthur Medeiros | RM561916 |
| Mike Rubim | RM561888 |
| Paulo Gurgel | RM564418 |
| Pedro Pereira | RM561520 |
| Vinícius Rocha | RM554974 |

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

### O que pode ser feito

A **antecipação** do volume e do risco de violação de OLA, e das **causas crônicas** que se repetem — foi esse o problema que o Kairós foi desenhado para resolver.

---

## 🎯 Objetivos do projeto

Quatro objetivos foram definidos pelo desafio — o Kairós responde a cada um deles:

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
<img width="953" height="490" alt="image" src="https://github.com/user-attachments/assets/86be54cb-548b-409b-82f5-8dfa9666aaf5" />

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

## 🔬 Modelagem preditiva e resultados

| Modelo | Papel | Resultado |
|---|---|---|
| Baseline sazonal | Referência obrigatória · P2/P3 | MAE 5,2 · 11,5 |
| **Prophet** ✅ | Uma série por prioridade (D+1/D+7) | MAE 3,9 · 9,1 (**-20%**) |
| SARIMAX | Vence no volume bruto | Não escala para N séries |
| **CatBoost** ✅ | Risco de violação de OLA | **ROC-AUC 0,84–0,85** |
| TF-IDF + KMeans | Clusters de causa raiz | 20 grupos (k=20) |

**Destaques de validação:**
- 🎯 Revisando apenas **20% da fila** priorizada por risco, o time captura **63% das violações de OLA** — 3,1× melhor que uma triagem aleatória.
- 🔎 A flag oficial de violação de KPI não reproduz integralmente a regra documentada — **3.020 incidentes** seguem pendentes de validação junto à Locaweb.
- 🔒 **Anti-leakage:** `SEED=42`, split sempre temporal e `TimeSeriesSplit` de 5 dobras — features de calendário nunca vazam o futuro.

---

## 📊 Demonstração — dashboards

### Visão Executiva

<div align="center">
<img width="1514" height="846" alt="image" src="https://github.com/user-attachments/assets/425c0638-e88c-4d85-8a4a-435935a95631" />


</div>

Chance de bater o KPI (P2/P3), orçamento de quebras de OLA consumido no ano, volume real vs. previsto (D+1/D+7) e o alvo de maior concentração de risco.

### Visão Operacional

<div align="center">
<img width="1433" height="807" alt="image" src="https://github.com/user-attachments/assets/1cd49d07-586f-4c69-9450-6ccb545fa042" />

</div>

Previsão D+1/D+7, dimensionamento sugerido de analistas e a fila de atuação priorizada por score de ação.

### Estudos e Insights

<div align="center">
<img width="1425" height="799" alt="image" src="https://github.com/user-attachments/assets/b07bc157-f34a-41bf-b5ab-6ed428cfcbda" />

</div>

Peso de cada fator no risco de OLA (SHAP sobre o CatBoost), taxa de violação por equipe e concentração de violações por dia da semana e faixa horária.

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
| **4** | Solução final | Dashboard operacional, pipeline em nuvem, vídeo pitch e entrega técnica |

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

- Consolidar a explicabilidade por SHAP (por incidente e combinação crítica)
- Validar com a Locaweb a regra de violação de KPI (3.020 casos)
- Pipeline recorrente em nuvem alimentando o Power BI em produção

</td>
</tr>
</table>

---

## 🔗 Links úteis

| Recurso | Link |
|---|---|
| 📊 Dashboard Power BI | [Abrir dashboard](https://app.powerbi.com/links/qh5oOHP-vM?ctid=11dbbfe2-89b8-4549-be10-cec364e59551&pbi_source=linkShare) |
| 🎬 Vídeo pitch | [Assistir no YouTube](https://youtu.be/v3YxQyDFvHo?si=iiiqYt5xHw7uboks) |
| 📑 Deck da Sprint 4 | [`docs/Kairos_Sprint4_Solucao_Final.pptx`](docs/Kairos_Sprint4_Solucao_Final.pptx) |

---

<div align="center">

**Grupo Kairós** · Turma 2TSCPV · Challenge Locaweb × FIAP 2026

*"Pointing the Direction"*

</div>
