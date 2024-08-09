**Apache Airflow on AWS**

**Objetivo**

Este documento visa apresentar duas arquiteturas distintas para o projeto de Data Reliability Engineer, focando na construção e correção do Apache Airflow provisionado na AWS. A arquitetura foi definida com base nas melhores práticas e ferramentas já utilizadas pelo time da Pismo, conforme discutido em entrevista, com o objetivo de garantir uma infraestrutura que seja:

**Econômica (FinOps)**

**Escalável (Auto Scale)**

**Altamente Disponível**

**Baixa Manutenção (Serverless)**

**Próativa (Observabilidade)**


**Arquitetura**

**Arquitetura 1: Amazon Managed Workflows for Apache Airflow (MWAA)**

<img width="1198" alt="mwaa-architecture" src="https://github.com/user-attachments/assets/1bd537f7-dd4b-4cff-9250-cfe98eb7a91e">

**Arquitetura 2: Apache Airflow em Amazon Elastic Kubernetes Service (EKS)**

![airflowOnEks](https://github.com/user-attachments/assets/edfe29a7-36b8-45c1-80f1-a7a64408eed3)

**Comparação e Seleção da Arquitetura**

Para selecionar a arquitetura mais adequada, considerou-se a necessidade de garantir um produto escalável, seguro e com custo de infraestrutura saudável. Dependendo do cenário, outros fatores podem ser determinantes, como facilidade de manutenção, integrações e conhecimento especializado em ferramentas específicas, como Kubernetes.

Com base nas análises acima, optou-se pela arquitetura que oferece maior flexibilidade, custos controlados e administração total sobre a infraestrutura. Assim, a implantação do Apache Airflow em Kubernetes foi escolhida.

**Comparação de Custos**

Amazon MWAA: $624.17/mês
Amazon EKS: $189.14/mês

**Facilidade de Uso e Esforço Operacional**

O MWAA, por ser um serviço gerenciado, é mais fácil de gerenciar, com toda a infraestrutura administrada pela AWS, incluindo atualizações e manutenção. Oferece integrações nativas com ferramentas como CloudWatch, ECR e SQS, e facilita a integração com o GitHub.

Por outro lado, no EKS, apesar de ser um serviço gerenciado, é necessário configurar manualmente as ferramentas mencionadas, como o CloudWatch, além de outras configurações.

**Flexibilidade e Controle**

Em serviços gerenciados como o MWAA, a flexibilidade e o controle sobre as configurações de infraestrutura são limitados, ficando sob responsabilidade da AWS. No entanto, a configuração do Apache Airflow no EKS garante mais personalizações específicas, permitindo adequar a aplicação conforme as necessidades do projeto. A escolha entre as duas opções depende do que o projeto espera e do nível de conhecimento da equipe nas ferramentas.

**Segurança**

A segurança é um dos pré-requisitos mais importantes em ambos os cenários. O Amazon MWAA oferece integração direta e nativa com os serviços AWS, enquanto o EKS pode exigir configuração adicional e atenção especial com policies, roles e rotas.

**Observabilidade**

Com o AWS CloudWatch, armazenamos e geramos logs para cada componente, permitindo obter métricas, logs e traces seguindo os princípios dos "Golden Signals". Essas informações são usadas para criar painéis na ferramenta open-source Grafana. Além disso, é possível enviar notificações e alarmes através do AWS SNS.

![monitoring](https://github.com/user-attachments/assets/917b4f52-85f2-4aea-a592-95d68764e4fd)


**Alertas**

Para garantir uma monitoria proativa, rastreando problemas antes mesmo de acontecerem, utilizaremos o SNS da AWS, que permite enviar notificações via email, SMS, Teams, e push notifications. Também há integração com o Alert Manager, utilizando o SNS para análises mais direcionadas ao Kubernetes.

**Componentes**

Amazon CloudWatch: Serviço de monitoramento e observabilidade da AWS. Coleta métricas e logs dos serviços AWS e envia para o Grafana.

Amazon EventBridge: Serviço de roteamento de eventos. Recebe eventos de várias fontes (como falhas, alertas, logs) e os direciona para o destino apropriado, como um tópico SNS, Lambda, ou outro serviço de notificação ou automação.

Grafana: Plataforma de monitoramento e visualização. Conecta-se ao Prometheus e ao Amazon CloudWatch para exibir métricas, logs e outras informações de monitoramento em painéis visuais.

Prometheus: Sistema de monitoramento e alerta de código aberto. Coleta métricas de aplicações executando no Amazon EKS e as envia para o Grafana para visualização.

**Funcionamento Geral**

Esta arquitetura fornece um fluxo completo de monitoramento para uma aplicação baseada em Kubernetes no EKS.
Utilizando Prometheus e Grafana, obtém-se uma visualização das métricas de aplicação e infraestrutura.
Amazon CloudWatch complementa isso com logs detalhados e eventos que são roteados por EventBridge para ações subsequentes.
Com isso, a infraestrutura é capaz de monitorar, analisar e responder a eventos automaticamente, garantindo alta disponibilidade e desempenho.
