# ProjetoDockerWP

<body>
    <h2>Projeto Final de Docker com Docker Swarm e suas Ferramentas<h2></h2>
<p>
Nosso projeto final utiliza Docker Swarm para orquestração de contêineres, garantindo escalabilidade e alta disponibilidade. Aqui estão as principais ferramentas integradas e suas funções:

Docker Swarm:

Utilizado para gerenciar e escalonar contêineres Docker em um cluster.
Permite a criação de serviços distribuídos e replicados para garantir alta disponibilidade de aplicações.

Prometheus:

Ferramenta de monitoramento e alerta que coleta métricas numéricas de sistemas e serviços.
Utilizado para registrar e visualizar métricas de desempenho, como uso de CPU, memória e tráfego de rede.

Grafana:

Plataforma de análise e monitoramento que permite criar dashboards e gráficos interativos.
Integrado com Prometheus para visualizar dados em tempo real e históricos de métricas.

Cadvisor:

Coletor de métricas para contêineres Docker, monitorando uso de recursos e desempenho.
Oferece insights detalhados sobre os contêineres em execução, facilitando a análise e otimização de recursos.

Redis:

Banco de dados em memória utilizado como cache para consultas rápidas e armazenamento temporário.
Integração com Wordpress através do plugin Redis Object Cache para melhorar o desempenho e a escalabilidade.

MySQL:

Sistema de gerenciamento de banco de dados relacional usado para armazenar dados persistentes.
Integrado com Prometheus através do mysql-exporter para monitoramento detalhado do desempenho do banco de dados.

Objetivo do Projeto:

Nosso objetivo é configurar um ambiente Docker Swarm robusto que suporte todas as partes essenciais de uma aplicação web moderna. Utilizando Prometheus, Grafana, Cadvisor, Redis e MySQL, garantimos monitoramento eficaz, escalabilidade automática e alta disponibilidade. Isso permite que nossa aplicação seja dimensionada conforme necessário e mantida com alto desempenho e eficiência operacional.</p>

<body>
    <h1> Rodando o Projeto na pratica passo a passo:</h1>
    <div>
        <h2>Passo 1: Configurando o ambiente</h2>
        <ol>
            <li><p>Tenha o docker em sua máquina.</p></li>
            <li>
                <p>
                    Verifique as portas que vão ser utilizadas no projeto estejam disponiveis.
                </p>
                <ul>
                    <li>9104 : mysql-exporter</li>
                    <li>6379 : redis</li>
                    <li>8080 : wordpress</li>
                    <li>9090 : prometheus</li>
                    <li>3000 : grafana</li>
                    <li>8081 : cadivisor</li>
                </ul>
            </li>
        </ol>
    </div>
    <hr/>
    <div>
        <h2>
            Passo 2: Começe Clonando e acessando a pasta do repositorio:
        </h2>
        <ol>
            <li>
                <p>
                    Em qualquer pasta do seu computador.
                </p>
                <p>Em seguida abra o git bash e digite o código a seguir:</p>
                <pre><code>git clone  https://github.com/gabrielscostaa/ProjetoDockerWP.git</code></pre>
            </li>
            <li>
                Com o projeto clonado em sua máquina, acesse a pasta do projeto:
                <pre><code>cd ProjetoDockerWP</code></pre>
            </li>
        </ol>
    </div>
    <hr/>
    <div>
        <h2>Passo 3: Executar o projeto ProjetoDockerWP</h2>
        <ol>
            <li>
                Agora vamos iniciar nosso Docker Swarm para podermos iniciar o projeto projeto:
                <p>vá no seu terminal e digite:</p>
                <pre><code>docker swarm init</code></pre>
            </li>
            <li>
                Em seguida Escreva o comando e o execute:
                <pre><code>docker stack deploy -c docker-compose.yml "nome_stack"</code></pre>
                com este comando vamos iniciar nossos services que estão no arquivo .yml sendo eles:
                <ul>
                    <li>mysql - database</li>
                    <li>mysql-exporter - exporta os dados do mysql para serem acessados pelo prometheus</li>
                    <li>redis - usado para armazenar em cache as querys feitas pelo wordpress</li>
                    <li>wordpress - nosso site</li>
                    <li>prometheus - ferramenta de monitoramento</li>
                    <li>grafana - transforma os dados do prometheus em dashboards para melhor visualização</li>
                    <li>cadvisor - ferramenta para monitorar o docker e seus containers</li>
                </ul>
            </li>
        </ol>
    </div>
    <div>
        <h2>Passo 4: Configurar o wordpress</h2>
        <ol>
            <li>Abra seu navegador e acesse o link: <code>http://localhost:8080</code></li>
            <p>Ou invez do 8080 utilize a porta na qual você escolheu.</p>
            <li>
                Ao acessar o wordpress, configure-o seguindo suas instruções, Obs: não se preocupe com os dados infomardos, todos são apenas para test.
            </li>
            <li>
                Finalizando o processo de configuração do worpress você será redirecionado a pagina de administrador:
            </li>
            <li>
                Nesta pagina acesse a opção de plugins, você será redirecionado para está pagina onde estão os plugins instalados:
            </li>
            <li>
                Ao lado da palavra Plugins no topo da pagina você terá a opção de Adicionar plugin, apertando nesse botão você será redirecionado para está pagina:
            </li>
            <li>
                Pesquise na opção de busca o plugin: <code>Redis Object Cache</code>
                /
                Aperte na opção instalar, aguarda instalar e, após a instalação volte para a pagina de <code>Plugins Instalados</code>
            </li>
            <li>
                Abaixo dos plugins já instalados irá aparecer o Redis Object Cache, selecione a opção de Ativar(também pode aparecer como Configurações), ao acessar essa pagina podemos ver que não podemos nos conectar com o redis:
            </li>
        </ol>
    </div>
    <div>
        <h2>Corrigir erro de conexão entre o Redis e o Docker</h2>
        <ol>
            <li>
                Para isso vamos entrar no nosso terminal e digitar o comando <code>sudo su</code> para trocarmos para o usuário root do sistema, faça login com sua senha de root, em seguida digite o comando <code>docker ps</code>:
                Listamos todos os containers em execução, nessa lista vamos procurar o container que está instanciando o wordpress e copiarmos o CONTAINER ID dele.
            </li>
            <li>
                Vamos executar o comando <code>docker exec -it "ID DO SEU CONTAINER" bash</code>, assim vamos acessar a maquiná que está executando o wordpress.
            </li>
            <li>
                Em seguida vamos executar os comandos:
                <pre><code>apt update</code></pre>
                <pre><code>apt install nano</code></pre>
                Vamos atualizar o apt e em seguida baixar o aplicativo nano para que possamos editar o arquivo wp-config.php
            </li>
            <li>
                Após a conclusão do passo anterior digite o comando:
                <pre><code>nano wp-config.php</code></pre>
            </li>
            <li>
                Ao acessar a interface do nano procure a linha que diz <code>/* That's all, stop editing! Happy publishing. */</code>
                E adicione as linhas:
                <pre><code>define('WP_CACHE', true);<br>define('WP_REDIS_HOST', 'redis');<br>define('WP_REDIS_PORT', 6379);</code></pre>
            </li>
            <li>
                Salve o arquivo com o comando: "CTRL+O";<br>
                Feche o arquivo com o comando: "CTRL+X";
            </li>
            <li>
                Volte para o painel de plugins do wordpress, atualize a pagina apertando "F5" e veja se o Redis apareça como Acessível:
                Estando assim é só apertar no botão "Ativar o cache de objeto" e o Redis está configurado.
            </li>
        </ol>
    </div>
    <div>
        <h2>Passo 5: Acessar o Prometheus</h2>
        <ol>
            <li>
                Agora, acessaremos o link: <code>http://localhost:9090</code>, seremos redirecionados a tela inicial do Prometheus também sendo chamada de Graph, nesta tela podemos executar consultas de métricas.
            </li>
            <li>
                Podemos nele verificar metricas dos containers:
                <pre>container_cpu_system_seconds_total<br>container_fs_reads_total<br>container_fs_limit_bytes</pre>
                Também podemos visualizar as metricas do mysql:
                <pre>mysql_exporter_collector_success<br>mysql_global_status_connections<br>mysql_global_status_max_used_connections</pre>
                Entre várias outras métricas.
            </li>
        </ol>
    </div>
    <div>
        <h2>Passo 6: Dashboard do Grafana</h2>
        <ol>
            <li>
                Acessando a pagina: <code>http://localhost:3000</code> você será redirecionado para a pagina do grafana.
            </li>
            <li>
                Utilize o usuario admin e senha admin para entrar.
            </li>
            <li>
                Pela dashboard vamos em Connections e iremos acessar a opção "Data sources".
            </li>
            <li>
                Nessa aba, vamos apertar o botão "Add data source" e vamos buscar pelo Prometheus.
            </li>
            <li>
                Para configurar o data source do prometheus vamos seleciona-lo na lista abrindo assim suas configurações.
                Em Connection vamos adicionar a url do servidor do prometheus sendo ela <code>http://prometheus:9090</code>.
            </li>
            <li>Após adicionar sua url vamos no final da pagina e você ira apertar no botão "Save & test" assim podemos ir para a dashboards.</li>
            <li>Na dashboards do grafana vamos apertar em "Create Dashboard" e em seguida "Add visualization".</li>
            <li>No modal que apareceu vamos selecionar o Prometheus.</li>
            <li>Na opção Query, vamos seleiconar a métrica do Prometheus e apertar em run query.</li>
            <li>
                Assim será gerado um grafico e podemos apertar na opção "Apply" no canto superior direito
            </li>
        </ol>
    </div>
    <div>
        <ol>
            <li> Caso queira olhar os graficos dos containers e do docker podemos acessar o cadvisor pela url <code>http://localhost:8081</code>.
        </ol>
    </div>
</body>
