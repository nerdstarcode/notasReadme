
# Kubernetes
Kubernetes gerencia e organiza contêineres e serviços implantados
 - pods
 - services replication controllers
 - deployments
 - statefulsets 
## Pods
 Um Pod é a menor unidade de implantação no Kubernetes. Ele representa um único processo em um cluster e encapsula uma ou mais containers. Os containers dentro de um Pod são sempre implantados juntos em um mesmo nó do cluster e compartilham recursos, como endereço IP e espaço de armazenamento.

Principais pontos a serem considerados sobre Pods:

- Os Pods são efêmeros, o que significa que podem ser criados, escalonados, reiniciados e destruídos conforme necessário.
- Os Pods são usados para agrupar containers que precisam se comunicar e compartilhar recursos.
- Um Pod é implantado em um único nó, mas pode ser movido para outros nós pelo Kubernetes de acordo com a necessidade de balanceamento de carga e recuperação de falhas.
- Cada Pod possui um endereço IP único atribuído a ele.
- O ciclo de vida de um Pod é gerenciado pelo Kubernetes. Se um Pod falhar, o Kubernetes pode reiniciá-lo automaticamente ou criar um novo Pod em seu lugar.
Referência: [Documentação oficial do Kubernetes - Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

Resumo sincero: é um recurso que encapsula os conteiners do kubernetes

### Tipos de Pods

No contexto de clusters Kubernetes, existem diferentes tipos de Pods que podem ser usados para atender a diferentes necessidades. Aqui estão alguns dos tipos comuns:

#### Pod único (Single-Container Pod):
    
É o tipo mais simples de Pod, com apenas um container em seu interior.
    
É adequado para casos de uso em que há apenas uma aplicação ou serviço sendo executado no Pod.
#### Pod com múltiplos containers (Multi-Container Pod):
    
Consiste em dois ou mais containers que compartilham o mesmo contexto e recursos em um único Pod.

Os containers dentro do Pod geralmente trabalham juntos para cumprir uma função específica, como um container de aplicação e um container sidecar para observabilidade ou armazenamento.
#### Pod de iniciador (Init Containers):
    
É um tipo especial de container que é executado antes do container principal em um Pod.

É usado para realizar tarefas de inicialização ou preparação, como inicialização de banco de dados, migração de dados ou configuração de ambiente.
#### Pod de infraestrutura (Infrastructure Pod):
São Pods usados internamente pelo Kubernetes para fornecer serviços e recursos necessários ao funcionamento do cluster.

Esses Pods geralmente não são visíveis ou gerenciados diretamente pelos usuários do Kubernetes.

Cada tipo de Pod tem suas características e casos de uso específicos. A escolha do tipo de Pod depende das necessidades e requisitos da sua aplicação ou serviço.

Referência:[ Documentação oficial do Kubernetes - Pods](https://kubernetes.io/docs/concepts/workloads/pods/)

### Diferença entre Máquinas do Cluster e Pods

Máquinas do Cluster (também chamadas de nós) e Pods são conceitos diferentes no contexto do Kubernetes.

- Máquinas do Cluster (Nodes): São os servidores físicos ou virtuais que compõem o cluster Kubernetes. Cada nó é uma máquina individual que executa o Kubernetes e hospeda os Pods. Os nós fornecem os recursos computacionais (CPU, memória, armazenamento) necessários para executar os containers.

- Pods: São unidades de implantação no Kubernetes que encapsulam um ou mais containers. Um Pod é uma abstração lógica que representa um único processo em execução no cluster. Um Pod pode ser visto como uma instância lógica de um ou mais containers, que compartilham o mesmo ambiente de execução, endereço IP e recursos.

A diferença fundamental é que os nós do cluster são as máquinas físicas ou virtuais que fornecem os recursos para executar os Pods, enquanto os Pods são as unidades de trabalho que contêm os containers em si. Cada nó pode executar vários Pods, e o Kubernetes é responsável por agendar e gerenciar a distribuição dos Pods nos nós disponíveis, levando em conta as demandas de recursos, balanceamento de carga e tolerância a falhas.

Em resumo, as máquinas do cluster (nós) fornecem a infraestrutura física ou virtual para o Kubernetes, enquanto os Pods são as unidades lógicas que encapsulam e executam os containers da aplicação.

Referência: [Documentação oficial do Kubernetes - Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/) e [Documentação oficial do Kubernetes - Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
## Cluster
Onde se encontram os Pods, dentro deles podemoster dois tipos
### Master (Control Plane)
Responsável por coordenar e gerenciar o cluster
- gerenciar o cluster
- manter e atualizar o estado desejado
- receber e executar novos comandos

Dentro do contexto do Kubernetes, a máquina Master é responsável por gerenciar o cluster e suas operações. Existem alguns componentes-chave que compõem a máquina Master e desempenham funções específicas no ecossistema do Kubernetes:

1. **API Server (Servidor API):**
   - É o componente central do Kubernetes.
   - Expõe a API do Kubernetes, que permite que os usuários e outros componentes interajam com o cluster.
   - Recebe e processa as requisições da API, autentica e autoriza as ações solicitadas.

2. **Controller Manager (Gerenciador de Controladores):**
   - É responsável por executar controladores que monitoram o estado do cluster e fazem ajustes para garantir que o estado desejado seja alcançado e mantido.
   - Exemplos de controladores incluem o controlador de replicação, que gerencia a escalabilidade dos Pods, e o controlador de endpoints, que atualiza as configurações de rede.

3. **Scheduler (Agendador):**
   - É responsável por atribuir Pods recém-criados aos nós disponíveis no cluster.
   - Leva em consideração requisitos de recursos, restrições e políticas de agendamento para tomar decisões sobre onde e como executar os Pods.

4. **etcd:**
   - É um armazenamento de dados distribuído e consistente usado pelo Kubernetes para armazenar o estado do cluster.
   - Mantém informações como configurações, metadados de objetos e estado dos Pods.
   - Garante a consistência e a alta disponibilidade dos dados.

Esses componentes trabalham em conjunto para gerenciar o cluster Kubernetes. A máquina Master é responsável por tomar decisões sobre o agendamento, controle e monitoramento dos nós e Pods no cluster.

Referências:
- [Documentação oficial do Kubernetes - Componentes do Control Plane](https://kubernetes.io/docs/concepts/overview/components/#control-plane-components)
- [Documentação oficial do Kubernetes - Componentes do Cluster](https://kubernetes.io/docs/concepts/overview/components/#master-components)

### Nodes
Responsáveis por executar os pods em cápsulas containers
- executar aplicações
Quando se trata dos nós (nodes) no Kubernetes, existem dois componentes principais que desempenham papéis importantes:

1. **kubelet:**
   - É o principal agente de execução em um nó.
   - Ele gerencia e supervisiona os containers em execução no nó, garantindo que estejam em um estado saudável.
   - O kubelet recebe instruções do controlador de replicação para iniciar, parar ou reiniciar os containers nos nós.
   - Ele também monitora os recursos do nó e relata as informações de volta para o controlador de agendamento para ajudar no processo de decisão do agendamento.

2. **kube-proxy:**
   - É um proxy de rede que opera em cada nó do cluster.
   - Ele lida com o roteamento de tráfego de rede para os serviços no cluster.
   - O kube-proxy mantém as regras de redirecionamento de tráfego com base nas configurações de serviço definidas no Kubernetes.
   - Ele permite que os serviços sejam acessíveis de outros Pods ou de fora do cluster.

Esses componentes trabalham em conjunto para garantir que os Pods sejam executados corretamente nos nós e que a rede seja roteada adequadamente para os serviços no cluster.

Além disso, cada nó também possui um container runtime (como Docker ou containerd) instalado, que é responsável por executar os containers dentro dos Pods.

Referências:
- [Documentação oficial do Kubernetes - Componentes do Cluster](https://kubernetes.io/docs/concepts/overview/components/#node-components)
- [Documentação oficial do Kubernetes - Kubelet](https://kubernetes.io/docs/concepts/overview/components/#kubelet)
- [Documentação oficial do Kubernetes - Kube-proxy](https://kubernetes.io/docs/concepts/overview/components/#kube-proxy)
## API
**API Kubernetes e kubectl**

A API do Kubernetes é uma interface de programação de aplicativos que expõe os recursos e funcionalidades do cluster Kubernetes. Ela permite que os usuários interajam com o cluster, gerenciando os objetos Kubernetes, como Pods, Serviços, Implantações, ConfigMaps e muito mais.

A API é a principal forma de comunicação com o Kubernetes e oferece uma maneira consistente e poderosa de criar, atualizar, listar e excluir recursos no cluster. Ela permite a automação de tarefas, a integração com outras ferramentas e a personalização de fluxos de trabalho.

O kubectl é a ferramenta de linha de comando oficial do Kubernetes e é usada para interagir com a API do cluster. Com o kubectl, você pode enviar comandos para o cluster para gerenciar e controlar os recursos do Kubernetes.

Algumas funcionalidades do kubectl incluem:

- Criar, atualizar e excluir objetos Kubernetes.
- Listar objetos Kubernetes e obter informações sobre os mesmos.
- Acompanhar o estado dos objetos em tempo real.
- Executar comandos dentro de um Pod em execução.
- Redimensionar e ajustar a escala dos recursos no cluster.
- Gerenciar segredos, configurações de rede, volumes e muito mais.

O kubectl também suporta recursos avançados, como execução de comandos interativos em containers, port forwarding, encaminhamento de logs e acesso a informações de diagnóstico.

A API do Kubernetes e o kubectl são essenciais para interagir e gerenciar um cluster Kubernetes de maneira eficiente e flexível.

Referências:
- [Documentação oficial do Kubernetes - kubectl Overview](https://kubernetes.io/docs/reference/kubectl/overview/)
- [Documentação oficial do Kubernetes - API Concepts](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)
- [Documentação oficial do Kubernetes - kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
