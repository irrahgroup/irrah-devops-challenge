# Questão 1 — Infraestrutura como Código e Segurança

## Requisitos e escolhas

O [README original](README.md) exige Kubernetes, banco relacional gerenciado,
bucket de conversas, staging/production separados e credenciais fora do código,
com DevSecOps. Interpretamos a dupla negação como proibição de expor credenciais.
O enunciado aceita estrutura/pseudocódigo Terraform ou descrição detalhada.
OCI/PostgreSQL são preferências adotadas; tenancy/região únicas, OKE Enhanced,
Vault e recursos separados são decisões nossas, não novas exigências da IRRAH.

## Implementação de referência

O [módulo compartilhado](terraform/modules/environment) atende aos roots
[staging](terraform/environments/staging) e [production](terraform/environments/production).
Configura compartment/VCN por ambiente, subnets privadas, OKE com workers
gerenciados, PostgreSQL gerenciado condicional, bucket privado e Vault/chave.
Schema/validações ficam no módulo. Capacidade, versões e backups são parametrizados.

A separação reduz blast radius, com custo maior que compartilhar cluster/banco.
Tenancy, região e administradores permanecem comuns. Não há peering, NAT ou
entrada pública de rede criada para OKE/banco. Dados sintéticos em staging são
política operacional, não garantia do código.

Workload identity está **preparada**: Enhanced e policies vinculadas a cluster,
namespace e service account permitem criar objetos e ler secrets indicados.
O [exemplo Kubernetes](terraform/kubernetes/access.yaml.example) não acrescenta
RoleBinding à aplicação. Pod consumidor, SDK compatível e integração são pendentes.

Terraform cria infraestrutura de secrets, sem abastecer/ler seu conteúdo. A fase
inicial não cria banco; após abastecer o Vault, `database_bootstrap_secret` fornece
OCID/versão para criar PostgreSQL. É referência histórica: `credentials` tem
**ForceNew** no provider selecionado. Mudá-la solicita substituição, bloqueada por
`prevent_destroy`; não é mecanismo normal de rotação. Rotação posterior é operacional.

O [bootstrap de state](terraform/bootstrap-state) tem backend OCI versionado,
com buckets privados/versionados separados para bootstrap, staging e production.
Um administrador cria/importa somente o compartment e bucket iniciais; não há
backend local silencioso. State/plans são sensíveis e não são versionados;
lockfiles são mantidos. Não foram introduzidos serviços adicionais de locking.

O módulo não administra policy na raiz da tenancy. Retenção/lifecycle das conversas
são pré-requisitos administrativos documentados; sem eles, não há expiração automática.
Bucket privado/criptografia não substituem sanitização ou controles organizacionais.

## Evidência e limites

Validação local cobre formatação, schema e quatro planos mock com assertions de
recursos/entradas. **Não houve deployment real**, plan contra OCI, teste IAM/rede,
TLS, locking remoto ou restauração. Criptografia dos serviços/volumes está
representada; TLS dos clientes requer integração. Não se declara conformidade
LGPD, SLA, RPO ou RTO.

A [documentação técnica](docs/architecture/q1-iac-security.md) contém diagrama,
rede, seed/import do state, administração SQL por pod temporário, ciclo de vida
das credenciais, trade-offs e revisões necessárias antes de produção.

## Questões pendentes

- Questão 2: pendente, não implementada.
- Questão 3: pendente, não implementada.
- Questão 4: pendente, não implementada.
