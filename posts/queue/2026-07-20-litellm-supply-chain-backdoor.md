O grupo TeamPCP comprometeu a pipeline de build do Trivy, o scanner de vulnerabilidades usado em boa parte dos CI, e aproveitou o acesso para publicar duas versões maliciosas do LiteLLM, o gateway que muita gente coloca na frente de vários provedores de LLM e que é baixado cerca de 3,4 milhões de vezes por dia. As versões 1.82.7 e 1.82.8, de 24 de março, vieram com coletor de credenciais, kit de movimento lateral em Kubernetes e um backdoor persistente via systemd.

A parte mais incômoda é a persistência. Em vez de um hook de instalação, o malware larga um arquivo .pth no site-packages do Python, que roda sozinho em qualquer chamada do interpretador, sem precisar importar o litellm. Com um token de service account do Kubernetes à mão, o kit sobe um pod privilegiado em cada nó do cluster e planta o backdoor lá também.

Gateway de LLM costuma entrar em produção rápido, sob pressão de colocar IA em algo, sem o hardening que qualquer outro serviço com acesso a segredo levaria. Vale tratar esse componente como se trata o Vault: rotação de chave de API, RBAC restrito no service account, sem privilégio de cluster-admin por padrão.

Seu gateway de LLM em produção tem as mesmas políticas de rede e RBAC que o resto do cluster, ou ele meio que escapou da revisão de segurança?

Fonte: TeamPCP Backdoors LiteLLM Versions 1.82.7-1.82.8 via Trivy CI/CD Compromise - https://thehackernews.com/2026/03/teampcp-backdoors-litellm-versions.html
#AIOps #Kubernetes #SupplyChainSecurity
