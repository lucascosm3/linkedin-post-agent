O Ingress NGINX Controller foi oficialmente aposentado pelo SIG Network e pelo Security Response Committee do Kubernetes. Sem novos releases, sem patch de segurança, mesmo com boa parte dos clusters em produção ainda rodando ele hoje.

A recomendação oficial é migrar para Gateway API, mas cada equipe está escolhendo um caminho diferente: NGINX Gateway Fabric, Traefik, ou Cilium com Gateway API embutido. Quem já passou por uma migração de Ingress pra Traefik sabe que a parte chata raramente é o controller em si. É reescrever as anotações customizadas que cada time acumulou ao longo dos anos: rate limit por rota, rewrite de path, coisas que ninguém documentou direito.

Gateway API padroniza boa parte do que antes era anotação solta, mas cada provider ainda implementa um subconjunto diferente de recursos. Migrar sem quebrar rota em produção continua sendo trabalho manual, cluster por cluster.

Quem já migrou pra Gateway API ou trocou de controller: qual foi a anotação que mais deu dor de cabeça pra portar?

Fonte: Ingress NGINX: Statement from the Kubernetes Steering and Security Response Committees - https://kubernetes.io/blog/2026/01/29/ingress-nginx-statement/

#Kubernetes #GatewayAPI #PlatformEngineering
