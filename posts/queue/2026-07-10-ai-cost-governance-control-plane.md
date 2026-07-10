<!--
tema: Governança de custo e acesso de LLM como camada de control plane (Platform Engineering 2.0)
fonte: https://platformengineering.com/features/platform-engineering-2-0-manage-ai-costs-and-risks-without-rebuilding-infrastructure/
gerado: 2026-07-10
status: draft
-->

Vi um artigo essa semana sobre um caso de custo de API de LLM saltando de 250 mil pra 400 mil dólares por dia em um único mês, dentro da mesma empresa. Ninguém decidiu isso de propósito, foi cada time plugando modelo direto no próprio serviço, sem nenhum controle central.

É basicamente o mesmo problema de shadow IT que a gente já resolveu (ou tentou resolver) com multi-account na AWS: sem guardrail central, cada squad interpreta política do jeito que dá mais liberdade pro sprint dele. "Sem PII pra modelo externo" significa uma coisa pro time de marketing e outra completamente diferente pro de finance.

O que me chamou atenção é que a solução proposta não é reconstruir a infra, é tratar o roteamento de modelo como mais uma camada de control plane, igual fizemos com rede e com secrets. Registry central, isolamento por domínio (sandbox, interno, regulado), identidade de workload em vez de API key solta em variável de ambiente. É o mesmo padrão que usamos pra separar produção de homologação com Vault e política de rede, só que agora o "recurso" é chamada de LLM.

Faz sentido tratar acesso a modelo de IA com o mesmo rigor que tratamos rede e segredo, ou isso vira mais uma camada de burocracia que ninguém segue na prática?
