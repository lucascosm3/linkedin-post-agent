A HashiCorp lançou em beta público o tfpolicy, framework de policy-as-code nativo do Terraform, direto no HCP Terraform. A proposta resolve uma dor conhecida de quem já tentou manter Sentinel ou OPA funcionando ao lado do Terraform: duas linguagens, dois ciclos de vida, e a política sempre um passo atrás do que foi provisionado de fato.

A regra em tfpolicy é escrita em HCL e roda dentro do próprio fluxo do Terraform. Dá pra avaliar relação entre recursos (exigir que toda role IAM tenha ao menos uma policy anexada, por exemplo), consultar data source pra cruzar com inventário aprovado, bloquear download de provider ou module antes de baixar, e validar depois do apply, inclusive valores que só existem depois do provisionamento.

Isso ataca dois problemas que costumam ficar sem dono: política que não enxerga o grafo de recursos e política que não checa o que só nasce depois do deploy. É comum ver time acumulando um Sentinel policy set gigante que ninguém mais entende, ou pulando validação pós-apply porque a ferramenta nunca alcança esse momento do ciclo.

Ainda é beta, só no HCP Terraform, e falta ver como isso se comporta com módulo de terceiro e monorepo grande de infra. Mesmo assim, escrever a política na mesma linguagem do recurso já reduz bastante o atrito de manter isso vivo.

Sua política de Sentinel ou OPA ainda está sincronizada com a infra real, ou já virou aquele repo que ninguém mexe mais?

Fonte: Introducing tfpolicy: A declarative policy workflow built for Terraform - https://www.hashicorp.com/en/blog/introducing-tfpolicy-a-declarative-policy-workflow-built-for-terraform
#Terraform #PlatformEngineering #PolicyAsCode
