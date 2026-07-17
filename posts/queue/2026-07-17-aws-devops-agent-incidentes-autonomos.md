O AWS DevOps Agent saiu do preview e virou GA em março, prometendo investigar incidente do jeito que um engenheiro sênior investigaria: cruzando telemetria, deploy e código de várias ferramentas ao mesmo tempo, em vez de alguém abrindo cinco abas numa call de incidente. A AWS divulga 75% de redução de MTTR e 94% de acerto na causa raiz, número vindo de clientes que testaram no preview, então vale ler com um pé atrás até aparecer mais gente contando o resultado real em produção.

O que separa isso do AIOps de sempre não é o agente ler métrica e log e sugerir uma causa, isso qualquer ferramenta decente já faz. É a parte de rodar runbook pré-aprovado sem intervenção humana no meio do incidente, o que muda a conversa de "IA ajuda a investigar" pra "IA resolve sozinha", inclusive em produção.

Time de plataforma que já sofreu com alerta ruidoso tende a ver isso como alívio. Quem já viu automação disparar a ação errada num cluster em produção quer ver trilha de auditoria completa antes de soltar a mão do botão de pausa.

Vocês deixariam um agente rodar runbook sozinho em produção hoje, ou ainda preferem humano aprovando cada execução?

Fonte: Announcing General Availability of AWS DevOps Agent - https://aws.amazon.com/blogs/mt/announcing-general-availability-of-aws-devops-agent/
#AIOps #SRE #AWS
