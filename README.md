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
Cloud Automation -> Service-level objectives -> Add new SLO
**Monitor a service-side objective:**
  * **Service-level availability:** mensura a taxa de sucessos de serviço
  * **Service-method availability:** número de solicitações de chaves bem sucedidas pelo total de serviços
  * **Service performance:** a razão entre minutos pelo total de minutos.

**Monitor a client-side objective:**  
  * User experience
  * Mobile crash-free users
  * Synthetic availability: porcentagem de sucesso sintético das execuções monitoradas.





