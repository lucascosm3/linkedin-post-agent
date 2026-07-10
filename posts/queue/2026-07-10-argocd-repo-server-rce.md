A Synacktiv publicou, no dia 1 de julho, os detalhes de uma RCE não autenticada no repo-server do Argo CD, o componente que lê os repositórios Git e monta os manifests via Kustomize e Helm. A empresa reportou o problema aos mantenedores em janeiro de 2025. Passados dezoito meses, ainda não existe patch nem CVE registrado.

O vetor abusa da flag --helm-command do Kustomize, que define qual binário ele chama para renderizar os charts. Quem alcança o gRPC interno do repo-server troca esse binário por um comando qualquer e sai com o cluster inteiro.

É o tipo de coisa que passa despercebida em muitos clusters: o repo-server e o Redis do Argo CD costumam ficar expostos dentro do cluster sem nenhuma NetworkPolicy, porque o chart Helm vem com essas políticas desligadas por padrão. Aplicar as policies que o próprio projeto já disponibiliza já fecha boa parte dessa superfície.

Se você roda Argo CD em produção, não dá pra confiar só no RBAC do cluster. Vale isolar repo-server e Redis com NetworkPolicy hoje mesmo, antes que saia um exploit público.

Você já checou se o repo-server do seu Argo CD está isolado por NetworkPolicy?

Fonte: Unpatched Argo CD Repo-Server Flaw Could Let Attackers Take Over Kubernetes Clusters - https://thehackernews.com/2026/07/unpatched-argo-cd-repo-server-flaw.html

#ArgoCD #GitOps #Kubernetes
