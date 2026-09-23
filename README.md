# Prova técnica - Devops/SRE

## 📋 Instruções Gerais para o(a) candidato(a)
* **Objetivo:** Avaliar a capacidade de análise sistêmica, entender o problema e propor soluções do(a) candidato(a).
* **Prazo de Entrega:** : 4 dias úteis
* **Formato de Entrega:** Link de um repositório público no GitHub contendo:
    1. Um arquivo `RESPOSTAS.md` com as partes conceituais e escritas.
    2. Demais arquivos versionados que foram usados nas respostas (imagens, códigos, *.yml, etc)

> Você não precisa entregar todas as questões, faça elas conforme o seu domínio técnico. Mas saiba que quanto mais completa for a entrega, maior serão as suas chances na vaga.
> Você pode utilizar exemplos de qualquer ferramenta e provedor de Cloud, mas damos mais preferência para ambientes OCI, Github, Grafana, Kubernetes e PostgreSQL

## Contexto do Desafio

Bem-vindo(a) ao processo seletivo da nossa plataforma de tecnologia. Nós desenvolvemos soluções de alta performance para a web, com foco em três pilares principais:
1. **Comunicação e Fluxos de Mensagens via WhatsApp:** Sistemas de alta concorrência e criticidade, lidando com milhares de webhooks por segundo.
2. **Agentes de Inteligência Artificial:** Arquiteturas orientadas a eventos e processamento assíncrono para IA.
3. **ERP para o Varejo:** Uma plataforma robusta que exige alta consistência de dados, segurança (LGPD/PCI) e disponibilidade contínua, especialmente em datas sazonais de pico (como Black Friday).


### Instruções para a Execução da Prova:
* Esta avaliação está dividida entre conceitos teóricos, desafios práticos e cenários situacionais do dia a dia.
* Responda cada questão demonstrando sua linha de raciocínio, arquitetura escolhida, ferramentas e boas práticas de engenharia.
* Você pode utilizar diagramas (como Mermaid, excalidraw e draw.io) para ilustrar suas arquiteturas se achar necessário.

---

## Questão 1: Infraestrutura como Código (IaC) e Segurança

**Cenário:** Você precisa provisionar um novo microsserviço que processará as requisições dos nossos Agentes de IA. Este serviço precisa de um cluster Kubernetes, um banco de dados relacional gerenciado e um bucket de storage para armazenar logs de conversas. Toda a infraestrutura deve seguir os padrões DevSecOps.

**Pergunta:**
Escreva uma estrutura ou pseudo-código em **Terraform** (ou descreva detalhadamente os blocos de recurso, variáveis e outputs) que atenda aos seguintes requisitos:
- Deverá ter separação de ambientes (staging e production).
- Nenhuma credencial sensível (senhas do banco, chaves de API da IA) **não** devem ficar expostas no código

---

## Questão 2: Pipelines CI/CD e Estratégias de Deploy 

**Cenário:** O time de Engenharia de software acabou de refatorar o componente principal de "Fluxo de Mensagens do WhatsApp". Por se tratar de um serviço crítico, o deploy não pode gerar *downtime* (zero downtime) e precisamos de um mecanismo rápido de rollback caso os novos fluxos de mensagens apresentem anomalias.

**Pergunta:**
Desenhe ou descreva um pipeline de CI/CD ideal para essa aplicação (utilizando GitHub Actions, GitLab CI ou Jenkins) contemplando as etapas (*stages*) obrigatórias de code review, qualidade (testes) e build. Não se esqueça de mencionar qual estratégia de deploy no Kubernetes você implementará e como mitigará o risco de impacto nos clientes durante a atualização da aplicação.

---

## Questão 3: Observabilidade e Troubleshooting em Tempo Real

**Cenário:** Sábado à tarde, véspera de Dia das Mães (pico de vendas no ERP de roupas e alto volume de disparos de marketing no WhatsApp). O Grafana dispara um alerta crítico no Slack: *“HTTP 5XX Error Rate > 5% no microsserviço de Webhooks do WhatsApp”*. Ao mesmo tempo, o tempo de resposta das requisições (latência) disparou e o consumo de CPU do cluster Kubernetes está em 98%.

**Pergunta:**
Descreva o seu passo a passo metodológico para investigar, isolar e mitigar este incidente em produção.
a. Quais métricas, logs ou traces você olhará primeiro no Grafana/Prometheus/Jaeger para identificar a causa raiz?
b. Caso o problema seja gargalo no banco de dados devido ao volume de requisições concorrentes das lojas do ERP, qual ação imediata você tomará para estabilizar o ambiente?
c. Como você documentará essa falha no processo pós-incidente (*Post-Mortem / Blameless Culture*)?

---

## Questão 4: Cultura DevSecOps e Governança

**Cenário:** Um relatório de segurança gerado automaticamente na nossa esteira identificou que várias imagens de container em produção contêm vulnerabilidades críticas conhecidas (CVEs). 

**Pergunta:**
Como especialista DevOps/SRE, quais ações estruturais e de cultura você implementará para corrigir esse problema?
