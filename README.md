# Prova técnica - Devops/SRE

## 📋 Instruções Gerais para o(a) candidato(a)
* **Objetivo:** Avaliar a capacidade de análise sistêmica, entender o problema e propor soluções do(a) candidato(a).
* **Prazo de Entrega:** : 4 dias úteis
* **Formato de Entrega:** Link de um repositório público no GitHub contendo:
    1. Um arquivo `RESPOSTAS.md` com as partes conceituais e escritas.
    2. Demais arquivos versionados que foram usados nas respostas (imagens, códigos, *.yml, etc)

> Você não precisa entregar todas as questões, faça elas conforme o seu domínio técnico. Mas saiba que quanto mais completa for a entrega, maior serão as suas chances na vaga.

## Contexto do Desafio

Bem-vindo(a) ao processo seletivo da nossa plataforma de tecnologia. Nós desenvolvemos soluções de alta performance para a web, com foco em três pilares principais:
1. **Comunicação e Fluxos de Mensagens via WhatsApp:** Sistemas de alta concorrência e criticidade, lidando com milhares de webhooks por segundo.
2. **Agentes de Inteligência Artificial:** Arquiteturas orientadas a eventos e processamento assíncrono para IA.
3. **ERP para o Varejo:** Uma plataforma robusta que exige alta consistência de dados, segurança (LGPD/PCI) e disponibilidade contínua, especialmente em datas sazonais de pico (como Black Friday).

Nossa infraestrutura opera em um modelo híbrido/multi-cloud, utilizando **AWS** (Amazon Web Services) e **OCI** (Oracle Cloud Infrastructure). 

### Instruções para a Execução da Prova:
* Esta avaliação contém **6 questões** divididas entre conceitos teóricos, desafios práticos e cenários situacionais do dia a dia.
* Responda cada questão demonstrando sua linha de raciocínio, arquitetura escolhida, ferramentas e boas práticas de engenharia.
* Você pode utilizar diagramas (como Mermaid, excalidraw e draw.io) para ilustrar suas arquiteturas se achar necessário.

---

## Questão 1: Arquitetura Multi-Cloud e Alta Disponibilidade 

**Cenário:** O nosso motor de envio e recebimento de mensagens do WhatsApp precisa operar com tolerância a falhas extremas. Se a região principal da AWS sofrer uma degradação, o fluxo de mensagens deve continuar operando pela OCI (Oracle Cloud Infrastructure) com o menor RTO (Recovery Time Objective) possível. O banco de dados do ERP de varejo também precisa ser resiliente para não interromper as vendas das lojas físicas e online.

**Pergunta:**
Desenhe uma sugestão de arquitetura multi-cloud (AWS e OCI) para garantir a alta disponibilidade desse ecossistema? Lembre de detalhar:
- Como funcionará o roteamento de tráfego global e failover (camada de DNS/Edge)?
- Como os clusters Kubernetes (EKS na AWS e OKE na OCI) se comunicarão ou sincronizarão?
- Qual estratégia você adotará para a replicação ou consistência dos dados do ERP entre as duas nuvens?

---

## Questão 2: Infraestrutura como Código (IaC) e Segurança

**Cenário:** Você precisa provisionar um novo microsserviço que processará as requisições dos nossos Agentes de IA. Este serviço precisa de um cluster Kubernetes, um banco de dados relacional gerenciado e um bucket de storage para armazenar logs de conversas. Toda a infraestrutura deve seguir os padrões DevSecOps.

**Pergunta:**
Escreva uma estrutura ou pseudo-código em **Terraform** (ou descreva detalhadamente os blocos de recurso, variáveis e outputs) que atenda aos seguintes requisitos:
1. Separação de ambientes (Workspace ou estrutura de pastas para Staging e Production).
2. Como você garantirá que credenciais sensíveis (senhas do banco, chaves de API da IA) **não** fiquem expostas no código e nem no arquivo `terraform.tfstate`?
3. Demonstre como travar a concorrência do estado (`state locking`) para evitar que dois engenheiros apliquem mudanças ao mesmo tempo.

---

## Questão 3: Pipelines CI/CD e Estratégias de Deploy 

**Cenário:** O time de Engenharia de software acabou de refatorar o componente principal de "Fluxo de Mensagens do WhatsApp". Por se tratar de um serviço crítico, o deploy não pode gerar *downtime* (zero downtime) e precisamos de um mecanismo rápido de rollback caso os novos fluxos de mensagens apresentem anomalias.

**Pergunta:**
Desenhe e descreva um pipeline de CI/CD ideal para essa aplicação (utilizando GitHub Actions, GitLab CI ou Jenkins) contemplando:
1. As etapas (*stages*) obrigatórias de validação, qualidade, segurança (DevSecOps) e build.
2. Como o artefato (imagem Docker) será versionado e armazenado de forma segura?
3. Qual estratégia de deploy no Kubernetes você implementará? Explique como mitigará o risco de impacto nos clientes durante a atualização da aplicação.

---

## Questão 4: Observabilidade e Troubleshooting em Tempo Real

**Cenário:** Sábado à tarde, véspera de Dia das Mães (pico de vendas no ERP de roupas e alto volume de disparos de marketing no WhatsApp). O Grafana dispara um alerta crítico no Slack: *“HTTP 5XX Error Rate > 5% no microsserviço de Webhooks do WhatsApp”*. Ao mesmo tempo, o tempo de resposta das requisições (latência) disparou e o consumo de CPU do cluster Kubernetes está em 98%.

**Pergunta:**
Descreva o seu passo a passo metodológico para investigar, isolar e mitigar este incidente em produção.
1. Quais métricas, logs ou traces você olhará primeiro no Grafana/Prometheus/Jaeger para identificar a causa raiz?
2. Caso o problema seja gargalo no banco de dados devido ao volume de requisições concorrentes das lojas do ERP, qual ação imediata você tomará para estabilizar o ambiente?
3. Como você documentará essa falha no processo pós-incidente (*Post-Mortem / Blameless Culture*)?

---

## Questão 5: Engenharia de Performance e Custos em Kubernetes (Prática/Teórica)

**Cenário:** O time de IA lançou novos agentes de conversação baseados em LLMs locais ou chamadas de API pesadas. Devido ao comportamento assíncrono, o consumo de recursos (CPU e Memória) flutua violentamente: de madrugada o consumo é quase zero, mas em horário comercial ocorrem picos massivos. A gerência notou um aumento de 40% na fatura da nuvem e pediu uma intervenção de SRE.

**Pergunta:**
Como você configurará o cluster Kubernetes para garantir escalabilidade elástica e, ao mesmo tempo, otimizar drasticamente os custos com instâncias cloud? 

---

## Questão 6: Cultura DevSecOps e Governança (Situacional)

**Cenário:** Um relatório de segurança gerado automaticamente na nossa esteira identificou que várias imagens de container em produção contêm vulnerabilidades críticas conhecidas (CVEs). Além disso, alguns desenvolvedores estão criando recursos diretamente via console web da nuvem (AWS/OCI) para agilizar entregas de novas funcionalidades do ERP, quebrando a rastreabilidade do código.

**Pergunta:**
Como especialista DevOps/SRE, quais ações estruturais e de cultura você implementará para corrigir esses dois problemas definitivamente, sem travar ou burocratizar o ritmo de entrega do time de desenvolvimento?
