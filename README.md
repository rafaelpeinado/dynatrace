# Dynatrace functionality overview for absolute beginners
## The Basis
### Create an account with Dynatrace
[Site Oficial Dynatrace](https://www.dynatrace.com/)
[Get started with Dynatrace](https://www.dynatrace.com/support/help/get-started)

### Deploy Dynatrace OneAgent
* Download Dynatrace OneAgent
* Download Dynatrace OneAgent for Windows


## Infrastructure Monitoring
### Host Performance data
O jeito mais fácil de pegar as informações sobre o host é clicando em:
* **Infrastructure -> Hosts** e teremos todos os hosts que foram deployados no Dynatrace.

* Podemos ver a versão do sistema operacional, a performance do sistema como memória disponível, usabilidade do CPU, Network interface (tráfego, os pacotes, as conexões), disco e network services.
* Nessa tela também é possível identificar problemas

### Host Problems monitoring
* Properties and tags: algumas informações detalhadas
* Clicando em Problems podemos verificar quais problemas de hosts estão acontencendo. No caso da aula o alerta mostra que o espaço em disco disponível está menor que 3%.
* O Problem é exibido se aconteceu no intervalo selecionado
* Dynatrace é esperto o suficiente e usa IA para entender se a causa raiz do problema é o mesmo que o anterior e agrupa tudo junto.
  * Por exemplo, o pouco espaço disponível não exibirá 60 para cada minuto em uma hora. Dynatrace vai agrupar tudo em um único alerta.
  * Problemas individuais serão logados/exibidos como problemas individuais.
* Um evento de saturação de CPU é criado quando o uso do CPU atinge um valor acima de um limite crítico. E o limite padrão definido pelo Dynatrace é de 95% em três ou cinco intervalos de um minuto, o que significa que em um período de 5 minutos, tivemos pelo menos 3 minutos em que o uso da CPU estava acima de 95%.
  * Conseguimos ver informações como **Root cause**. No caso da aula, o aplicativo CamtasiaStudio.exe está levando o uso do CPU para 99%. Ao clicar no software, temos mais detalhes das informações sobre o uso da CPU.

### Types of Dynatrace Problems monitoring
[Resource events](https://www.dynatrace.com/support/help/platform/davis-ai/basics/events/event-types/resource-events)

### Host availability
* Offline pode ter vários causas raiz, por exemplo, o servidor desligado ou problemas de conexão com o host.

Podemos ter indisponibilidade por manutenção e podemos criar um horário de manutenção em **Manage -> Settings -> Maintenance windows -> Monitoring, alerting and availability -> Add maintenance windows.**
Se não fizermos isso quando soubermos que tem manutenção no Windows, nós poderemos ter erros de apontamento dos problemas no Dynatrace.

### Host Processes monitoring
Aqui acompanhamos todos os processos que estão sendo usado no host e contribuem para as métricas de saúde.
Nós podemos ver o quanto de CPU, memória, tráfego e etc estão sendo consumidos por cada processo. Os detalhes são muito parecidos com a página incial do host como, por exemplo, Properties and tags. Além de capacidade de resposta, memória, processos em trabalho, IO, entre outros.

### Host Events
Mostra eventos que foram gerados pela inteligência artificial. Ao clicar no evento podemos ver detalhes dele e por quanto tempo o problema persistiu.

### Host Logs
É muito semelhantes às outras timelines.
Isso é um processo inteiramente automatizado. Podemos ir em **Settings -> Monitored technologies -> Log Monitoring.** Podemos cliclar em editar e verificar se o botão **Monitor Log Monitoring on every host** está ativado.
Clicando em **individual devices -> Hosts** em edit em um host específico. Vamos em **Log Monitoring -> OneAgent configuration** e decidir exatamente o que queremos monitorar.

### Quick tip on the Timeframe selector
Temos Pressets, Custom para customizar ranges e Recent.
E também podemos digitar algumas coisas, por exemplo, **last 4 months ou last 3 weeks, last 11 weeks, last 25 minutes.**

### Full Stack vs Intrastructure Only
* [Infrastructure Monitoring mode](https://www.dynatrace.com/support/help/platform-modules/infrastructure-monitoring/hosts/basic-concepts/get-started-with-infrastructure-monitoring)
* [Application and Infrastructure Monitoring (Host Units)](https://www.dynatrace.com/support/help/manage/subscriptions-and-licensing/monitoring-consumption-classic/application-and-infrastructure-monitoring)

O OneAgente é licenciado por host e nem todos os hosts têm o mesmo tamanho e o número de creditos que será consumidos dependerá se está operando em **Full Stack monitoring mode**, que é o padrão quando estamos instalamos o OneAgent ou se estamos usando a Infrastructure Monitoring Mode.
Para descobrir o tipo de monitoring mode que estamos usando e para trocar se for necessário, basta ir em S**ettings -> Monitoring -> Monitoring Overview -> Hosts -> Edit**. Precisamos primeiro verificar se o **Monitor this host** está ativado e sem seguida se está selecionado **Full-stack Monitoring**, que significa que os ambiente será monitorado em todos os aspecto. Isso inclui todos os processos, serviços e aplicações. Ao desativar o Full-stack monitoring o Dynatrace passará a monitorar apenas a infraestrutura. Ou seja, não será mais possível ver a performance das aplicações, visibilidade e dados de experiência de usuário. Como consequência consumiremos menos créditos, o que significa que o Dynatrace pode ser mais econômico.

* Infrastructure Monitoring - Compare [monitoring consumption calculation](https://www.dynatrace.com/support/help/manage/subscriptions-and-licensing) between Full-Stack and Infrastructure Monitoring modes.

* [Dynatrace pricing](https://www.dynatrace.com/pricing/): Há uma grande diferença entre os valores de Full-stack monitoring e Infrastructure monitoring.

### Frequent Issues feature
Um recurso bem importante é o de frequência de problemas. Ou seja, conseguimos ver com é o problema que acontece com muita frequência.
Quando isso acontece, não é mais criado alertas. Por exemplo, CPU saturation é uma problema frequente, então ele sinaliza que naquela semana aconteceu esse problema ocorreu diversas vezes. Ou seja, caso não estejamos mais recebendo alertas, não quer dizer que o problema foi resolvido. As vezes ele pode ter virado um **Frequent issue**. Só surgirá um alerta, caso o problema de CPU saturation fique pior do que costuma estar.
Esse recurso pode ser desligado indo em **Settings -> Anomaly detection -> Frequent issue detection**. E podemos escolher quais funcionalidades vamos desligar, sendo eles **Detect frequent issues within applications, Detect frequent issues within transactions and services, Detect frequent issues within infrastructure**.

### Renaming your host
O Dynatrace nomeia os hosts baseado no nome do DNS, exatamente como é detectado pelo OneAgent do Dynatrace.
Quando temos muitos hosts é muito importante mudar e dar nomes que expliquem o que são aqueles hosts. 
Para alterar um nome de host temos que ir até o host que deseja renomear, **clicar nos 3 pontos -> Settings -> Host name e renomear**.
Caso queira voltar para o nome padrão, basta clicar em **Reset name to detected**. **Esse é o jeito manual de alterar o nome de um host.**
Mas também podemos renomear diversos hosts automaticamente. Podemos criar uma regra para fazer isso. Basta ir em **Settings -> Monitoring -> Host naming -> Add a new rule**.
No campo Host name format eu posso colocar "Host with [ placeholder ] (no caso do curso é usado [ HostCpuCores ]) CPU Cores"

### Exclude specific Disks and Network traffic from monitoring
O Dynatrace monitora tudo por padrão.
Para exluir um disco ou um tráfego de conexão do monitoramento, basta ir para a 
* **Página inicial do host -> 3 pontos -> Settings -> Disk options -> Exclude disks -> Add item**. 
* **Página inicial do host -> 3 pontos -> Settings -> Exclude network traffic -> Exclude NIC -> Add item**. (NIC: Network Interface Card é a placa de rede)
* **Página inicial do host -> 3 pontos -> Settings -> Exclude network traffic -> Exclude IP -> Add item**. 

### Network Monitoring
**Infrastructure -> Network**


## Cloud Automation
### Cloud Automatino intro
Essa opção não está disponível no trial.
Se quisermos acessar a funcionalidade que está no Cloud Automation, precisamos entrar em contato com o time do Dynatrace e pedir pelo chat que é disponiblizado do lado do ícone de perfil.
Dynatrace pode fornecer uma demo mode para testar essas funcionalidades.

### Releases
* [Version detection strategies](https://www.dynatrace.com/support/help/platform-modules/cloud-automation/release-monitoring/version-detection-strategies)

Em Cloud Automation nós temos **Release monitoring**.
Dynatrace oferece uma solução de análise integrada de Release que ajuda você com os seguintes recursos:
* Quais são os estágios de Release das diferentes versões implantadas?
* Quais versões são implantadas em seus estágios de implantação e ambientes de produção?
* Algum bug conhecido e se eles são bloqueadores de lançamento?
* Algum risco relacionado a versões específicas?
* Como a nova versão está se comportando em comparação com as versões anteriores.

Em **Release Inventory** exibe todas as releases detectadas. Cada entrada representará um processo agrupado por Release e Build version, stage e product a que pertence.
* Em **Latest validation** os traffic light ficarão vermelho para failed, amarelo para warning e verde para passed.

Em **Release eventos** serão exibidos todos os eventos que correspondem às releases, por exemplo, reinicio de um processo ou um evento de deployment. 

**Tracked issues** irá exibir todos os problemas com as releases. Ao criar um tracked issue, temos o campo **Issue query** que é a parte mais complicada. Temos que usar placeholders específicos do sistema monitorado. Por exemplo, Jira Cloud precisa ter uma query **issueType = Bug and component in {{PRODUCT}} nad affectedVersion in {{VERSION}}**. O único sistema que não suporta os placeholders é o ServiceNow.

Ao clicar na release abrimos uma página de detalhes.

Nós podemos criar no máximo 20 issue tracking configurations.

### Service-level objectives (SLOs)
Podemos usar o Dynatrace para estabelecer estabelecer objetivos de nível de serviço. Geralmente, o time de SRE (Site-Reliability Engineering) é responsável por encontrar indicadores de nível de serviço (SLIs - Service-level indicators) para monitorar o quão confiável é o serviço.
O Dynatrace oferece mais de 2000 diferentes métricas que podem ser dedicadas.
O SLI é usado para mensurar quão sucedido foi a entrega.

Para fazer isso basta ir em **Cloud Automation -> Service-leval objectives**.

**Um exemplo, Cart loading time should be less than 100 miliiseconds.**
Com esse exemplo, podemos ver o:
* Status em porcentagem
* Error budget: é a diferença até o target definido seja considerado falha. Por exemplo, no vídeo o Target é de 96,5% e o Status é de 96%, sendo assim, o Error budget é de -0,5%

**Cart loading time should be less than 250 miliiseconds.**
* Status: 98%
* Error budget: 0,5%
* Target: 97,5%
* Warning: 98%

### Creating a new SLO
**Cloud Automation -> Service-level objectives -> Add new SLO**
**Monitor a service-side objective:**
  * **Service-level availability:** mensura a taxa de sucessos de serviço
  * **Service-method availability:** número de solicitações de chaves bem sucedidas pelo total de serviços
  * **Service performance:** a razão entre minutos pelo total de minutos.

**Monitor a client-side objective:**  
  * User experience
  * Mobile crash-free users
  * Synthetic availability: porcentagem de sucesso sintético das execuções monitoradas.


## Applications & Microservices
### Web applications - Performance Analysis
* [Apdex ratings](https://www.dynatrace.com/support/help/platform-modules/digital-experience/rum-concepts/scores-and-ratings/apdex-ratings)

Ao clicar em uma aplicação, abriremos uma página de visão serviço da aplicação: 
* temos o nome da aplicação
* Tags and Detection rules
* Framework detectada nas últimas 12 horas (por exemplo, Angular)
* Performance analysis (onde estão os número da métrica da performance da aplicação): 
  * Top browser
    * **Analyze performance**: permite analisar performance multi-dimensional
      * Podemos analisar as aplicações no browser e as ações do usuário em diversos casos.
  * **Top users type** 
  * **View Geolocation breakdown**
  * **Load actions:** é definida como um carregamento de página real no navegador. Quanto tempo para a página carregar completamente, por exemplo, imagens, HTML e etc.
  * **XHR (XMLHttpRequest) actions:** começa com o usuário clicando em um controle na página da web. Pode ser um click, double click, mouse up, kill key down.
  * **Custom actions:** por exemplo, podemos mensurar quanto tempo leva para abrir um dropdown do menu em JavaScript.
  * **Apdex rating:** O Dynatrace calculará a classificação para fornecer uma única métrica que dirá sobre o desempenho do aplicativo e os erros e impactos para os usuários.
  * **Errors:** número de erros por minuto.
    * Errors by type
  * **Resources:** exibe quais recursos essa aplicação depende
  * **Services:** lista de serviços que dão suporte a aplicação
    * **View service flow:** podemos ver a sequência de serviços que são chamados por cada serviço.
* **Availability**
* **Composite metrics across response times:** essa é uma seção que vai ajudar a descobrir de uma perspectiva de alto nível onde deveria ser o foco de performance.
  * User action duration
  * Visually complete
  * Speed index
  * DOM Interactive
  * Se clicarmos em **Show all metrics** poderemos ver:
    * Load event end
    * Load event start
    * HTML downloaded
    * Time to first byte
    * First input delay
    * Largest contentful paint
    * Cumulative layout shift
    * Server contribution
    * Network contribution
  * Compare to previous time frame: podemos, por exemplo, comparar as duas últimas horas, com as duas horas anteriores. Pode ser uma semana, entre outros.
* **Top 3 included domains**
* **Top errors**
* **Top 3 user actions**
* **Problems**

**Applications & Microservices -> Frontend**


### Web applications - User Behaviour Analysis
* [Define conversion goals](https://www.dynatrace.com/support/help/platform-modules/digital-experience/web-applications/analyze-and-use/define-conversion-goals)
Na parte de overview do menu de aplicação, temos a seção **User behavior** que permite analisar o comportamento dos usuários como sair da página, conversões, bouncers e muito mais.
Ao clicar ficará disponível:
* **Returning visitors:** será determinado por um cookie no browser do usuário final. Um ponto importante é que o cookie é válido por dois anos, se o usuário não apagar as informações de cookie. Isto é, se o usuário voltar na aplicação web depois de dois anos, ele será considerado um novo usuário. Caso contrário, ele se mantem como o mesmo usuário. O número apresentado pode ser um pouco maior do que ele realmente é.
* **Top users types:** podemos verificar os usuários verdadeiros, sintéticos ou robos.
  * usuários sintéticos faz parte do monitoramento sintético.
* **Geolocation breakdown:** mostra de onde os usuários acessam a aplicação
  * Taxa de rejeição (bounce rate)
  * Opção de usar Expand large view para mais detalhes
    * é possível ver os países que mais acessam a aplicação
* **Sessions:** podemos ver:
  * Active sessions
  * User engagement
  * Peak activity intervals
* **Entry actions:** podemos ver qual é a tendência de carregamento da primeira página durante o dia ou de ações ao longo das sessões
  * Apdex rating for entry actions
* **Other actions:** são actions que não são nem a primeira e nem a última ação do usuário durante a sessão.
* **Exit actions:** a última ação do usuário da sessão
* **Bounce rate**
  * Bounce rate analysis
  * Impact of JavaScript errors on bounce rate
* **Overall conversion:** overall conversion versus conversions goals
* **Top 5 conversion goals**
* **Top 3 bounces**
* **Top entry and exit actions**
* **Events:** erros, novas versões, deploy da aplicação, mudanças de configuração

### Web applications - Waterfall Analysis tool
Esse recurso permite que possamos analisar as ações com bastante detalhes.
Na página de visão geral da aplicação, basta procurar a seção **Top 3 user actions -> See full details -> Clica em uma action** e teremos acesso a tela com informações detalhadas sobre essa action.

* Quantos segundos até essa action estar completamente carregada
* Errors
* Apdex rating

Para usar waterfall analysis basta ir até a seção **Contributors breakdown -> Perform waterfall analysis**
O waterfall analysis é dividido em três seções:
* **Document requests:** mostra todos os conteúdos que são identificados quando a página é carregada pela ação do usuário - isso está incluso HTML e outros conteúdos que são exibidos.
* **Resources:** exibirá detalhes de tempo para todos os recursos e algumas ações de usuários - aqui podemos ver XHR requests ou fetch
Nos detalhes podemos ver o waterfall e como a ação do usuário performou em uma sequência waterfall.

### Mobile applications - User Experience metrics
**Applications & Microservices -> Frontend**
Temos seções de:
* Problems
* Top 3 actions
* AVailability
* Geographic regions
* Events

* **User Experience:** 
  * Users
  * Apdex rating
  * User actions per minute
  * Apdex ratings: terá grafícos que mostrará a média da tendência de apdex rating
  * User actions and Users
  * New users: isso contará apenas as primeiras sessões após o lançamento do aplicativo. Porém, se o mesmo usuário acessar a aplicação por outros dispositivos, será contado como novo usuário cada dispositivo.
  * App version distribution

### Mobile applications - Web Requests metrics
**Applications & Microservices -> Frontend -> Web requests**
* Web requests
* Error rate
* Top providers

### Mobile applications - Crashes and errors
**Applications & Microservices -> Frontend -> Crashes & Errors**
* Crash count
* Crash-free users
* Crash rate
* Crashes by version
* Reported errors by version

### Mobile applications - Data Privacy
**Applications & Microservices -> Frontend -> Data privacy**
Dynatrace fornece várias melhorias de privacidade que podem ser configurados em data privacy e ficar de acordo com o LGPD ou GDPR, protegendo os dados pessoais do cliente.
Se o Data privacy estiver Active, significa que o usuário por meio do opt in escolheu se vai compartilhar os dados pessoais ou não. 
Há vários níveis de privacidade de dados:
* Data collection level: mostra usuários que desligaram, estão anônimos ou permitiu pegar o comportamento do usuário
* Crash reporting
* Session replay

Para iniciar o monitoramento de um Mobile app basta:
**Applications & Microservices -> Frontend -> Mobiile apps -> Set up mobile monitoring**
Dê um nome e clique em create mobile app
Na tela de overview do app, basta ir em edit para abrir a tela de configurações:
* Data privacy -> Privacy settings -> Enable user opt-in mode
* [Configure data privacy settings for web applications](https://www.dynatrace.com/support/help/platform-modules/digital-experience/web-applications/additional-configuration/configure-real-user-monitoring-according-to-gdpr)

### Web App Settings - General settings
* [Define user action and user session properties for web applications](https://www.dynatrace.com/support/help/platform-modules/digital-experience/web-applications/additional-configuration/define-user-action-and-session-properties)
* [Dynatrace ActiveGate](https://www.dynatrace.com/support/help/setup-and-configuration/dynatrace-activegate)

* **General:**
  * Enablement and cost control
    * Pode ativar ou desativar o Real User Monitoring
    * Mudar a porcentagem da quantidade de dados trafegados (Cost and traffic control)
    * Ativar ou desativar Session Replay on crashes
  * Application name
  * Key performance metrics
  * Data privacy
  * Beacon Endpoint
    * Podemos definir onde o dado será enviado, se queremos que seja enviado para Cluster ActiveGate, Environment ActiveGate ou Instrumented web server.
* **Instrumentation wizard**
  * Podemos escolher a plataforma de monitoramento: Android, iOS, tvOS, Cordova, React Native, Flutter, Xamarin.
* **Naming rules**
  * URL cleanup rules
  * Action naming rule
  * Extraction rules
* **Request errors:** adicionar regras para excluir alguns HTTP responses que não deverão ser tratados como erros.
* **Anomaly detection**
  * **Detect unexpected low load:** compara com a semana anterior e caso esteja muito diferente, ele emite um alerta
* **Crash rate increase**
* **Symbol files:** pode fazer upload de symbol files como Android, iOS, tvOS
  * Symbol files hold a variety of data which are not actually needed when running the binaries, but which could be very useful in the debugging process. Typically, symbol files might contain: Global variables. Local variables. 
  * [Symbols and Symbol Files](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/symbols-and-symbol-files)
  * [Upload and manage symbol files for mobile applications](https://www.dynatrace.com/support/help/platform-modules/digital-experience/mobile-applications/analyze-and-use/upload-and-manage-symbol-files)
* **Session and user action properties:** Dynatrace captura várias informações sobre a performance da aplicação web e mobile, mas também é possível enriquecer as informações com metadata e transformando esse metadata para user action e user session properties.
  * É permitido criar 20 propriedadaes por aplicação.
* **Metrics:** é possível observar todas as métricas que foram criadas para o app.


### Service monitoring
Dynatrace é capaz de identificar exatamente como cada serviço contribui para a performance da aplicação. 
**Applications & Microservices -> Services**
E ao clicar em um serviço, abrirá uma página de overview.
No infográfico exibe as aplicações e se serviços que utilizam esse serviço e se o serviço chama algum outro serviço ou banco de dados.

* Application: dará detalhes sobre quais aplicações usam esse serviço
* Service: dará detalhes de quais serviços utilizam esse serviço
* Technology: informações sobre a tecnologia utilizada
* Called services: serviços que o serviço chama 
* Used database: bancos de dados que o serviço chama

* Requests: informações sobre o tempo de resposta do serviço, a porcentagem de erro e a taxa de transferência (throughput). 
  * View request
  * Ao cliclar em Analyze Backtrace, poderemos entener qual user action e outros serviços dependem desse serviço.
* Problems
* Recent hotpots detected
* Undertand dependencies
* Events

### Database monitoring
**Applications & Microservices -> Database**
* Quais statements do banco de dados foram executados com mais frequência e quais statements levou mais tempo
* Quais serviços executam esse banco de dados quando foi relatado um problema
* Dynatrace mostra se existe uma correlação entre o problema e o aumento de carga no serviço relacionado
  
No overview do database nós temos:
* Performance
* SQL Queries
  * View database statements:
    * Response time
    * Failure rate
    * Throughput
    * Database statements
    * Analyze outliers
  * Errors
* Tarefas de modificações e tarefas de transações
* Problems
* Response time failure rate
* Throughput
* Database availability
* Requests
  * Response time
  * Failure rate

### Dynatrace Resources for Queues, Distributed traces, Profiling and optimization
* [Queue concepts](https://www.dynatrace.com/support/help/platform-modules/applications-and-microservices/queues/queue-concepts)
* [Distributed traces overview](https://www.dynatrace.com/support/help/observe-and-explore/purepath-distributed-traces/distributed-traces-overview)
* [CPU profiling](https://www.dynatrace.com/support/help/platform-modules/applications-and-microservices/profiling-and-optimization/cpu-profiling)
* [Memory dump analysis](https://www.dynatrace.com/support/help/platform-modules/applications-and-microservices/profiling-and-optimization/memory-dump-analysis)


