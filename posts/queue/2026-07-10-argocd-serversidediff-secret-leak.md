<!--
tema: Vulnerabilidade crítica no ArgoCD (CVSS 9.6) - vazamento de Secret via ServerSideDiff
fonte: https://github.com/argoproj/argo-cd/security/advisories/GHSA-3v3m-wc6v-x4x3
gerado: 2026-07-10
status: draft
-->

Semana passada saiu um advisory crítico do ArgoCD (CVSS 9.6) que vale a pena todo mundo que roda GitOps em produção ler com calma. Qualquer usuário com permissão de leitura numa Application conseguia extrair Secret em texto puro, sem estar autorizado pra isso.

O problema estava no endpoint ServerSideDiff. Ele monta a resposta usando os estados PredictedLive e NormalizedLive do dry-run do Kubernetes, e esses dois não passavam pela mesma máscara de dado sensível que os outros endpoints do ArgoCD aplicam. Bastava a Application ter a annotation IncludeMutationWebhook habilitada.

Isso escancara um hábito perigoso: tratar "somente leitura" como sinônimo de "sem risco". Não é. Token de service account, certificado TLS, credencial de banco, tudo isso pode estar exposto atrás de um endpoint que ninguém revisou com a mesma atenção que revisa o endpoint principal de Secret.

Quem está em 3.2.0 até 3.3.8 precisa atualizar pra 3.3.9 ou 3.2.11 o quanto antes.

Vocês têm alguma rotina de revisão de permissão de leitura no ArgoCD, ou o RBAC fica só no "configurei uma vez e esqueci"?

Fonte: https://github.com/argoproj/argo-cd/security/advisories/GHSA-3v3m-wc6v-x4x3

#ArgoCD #GitOps #Kubernetes
