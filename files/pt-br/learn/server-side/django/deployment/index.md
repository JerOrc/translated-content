---
title: 'Tutorial Django Parte 11: Hospedando Django para produção'
slug: Learn/Server-side/Django/Deployment
tags:
  - Codificação de Scripts
  - Deploy do django
  - Fazendo deploy
  - Iniciante
  - django
  - servidor web
translation_of: Learn/Server-side/Django/Deployment
original_slug: Learn/Server-side/Django/Hospedagem
---
<div>{{LearnSidebar}}</div>

<div>{{PreviousMenuNext("Learn/Server-side/Django/Testing", "Learn/Server-side/Django/web_application_security", "Learn/Server-side/Django")}}</div>

<p class="summary">Agora que criou (e testou) um fantástico website de Biblioteca Local, vai querer instalá-lo em um servidor web público para que possa ser acessado pelo pessoal da biblioteca e membros através da Internet. Este artigo fornece uma visão geral de como poderá encontrar um servidor de hospedagem para instalar o seu web site, e o que precisa fazer para ter o seu site web pronto para produção.</p>

<table class="learn-box standard-table">
 <tbody>
  <tr>
   <th scope="row">Pré-requisitos:</th>
   <td>
    <p>Completar todos os tópicos do tutorial anterior, incluindo o <a href="https://developer.mozilla.org/pt-BR/docs/Learn/Server-side/Django/Testing">Tutorial Django Parte 10: Testando uma aplicação web Django.</a></p>
   </td>
  </tr>
  <tr>
   <th scope="row">Objectivo:</th>
   <td>Para saber onde e como se pode hospedar uma aplicação Django na produção.</td>
  </tr>
 </tbody>
</table>

<h2 id="Visão_geral">Visão geral</h2>

<p>Uma vez terminado o seu website (ou terminado "o suficiente" para iniciar testes públicos) vai precisar publicá-lo em um host mais público e acessível do que o seu computador de desenvolvimento pessoal.</p>

<p>Até agora tem trabalhado em um ambiente de desenvolvimento, utilizando o servidor web de desenvolvimento Django para compartilhar o seu site com o navegador/rede local, e executando o seu site com configurações de desenvolvimento (inseguras) que expõem debug e outras informações privadas. Antes de poder hospedar um website externamente, vai precisar fazer primeiro:</p>

<ul>
 <li>Faça algumas alterações nas configurações do seu projeto.</li>
 <li>Escolher um ambiente para hospedar a aplicação Django.</li>
 <li>Escolher um ambiente para hospedar qualquer arquivo estático.</li>
 <li>Configurar uma infraestrutura de nível de produção para servir seu website.</li>
</ul>

<p>Este tutorial fornece algumas orientações sobre suas opções para escolher um site de hospedagem, uma breve visão geral do que você precisa fazer para deixar seu aplicativo Django pronto para produção e um exemplo prático de como instalar o site Biblioteca Local no serviço de hospedagem em nuvem do <a href="https://www.heroku.com/">Heroku.</a></p>

<h2 id="O_que_é_um_ambiente_de_produção">O que é um ambiente de produção?</h2>

<p>O ambiente de produção é o ambiente fornecido pelo computador/servidor onde você executará seu site para consumo externo. O ambiente inclui:</p>

<ul>
 <li>Hardware de computador no qual o website é executado.</li>
 <li>Sistema operacional (por exemplo, Linux, Windows).</li>
 <li>Linguagem de programação de tempo de execução e bibliotecas de estrutura sobre as quais seu site é escrito.</li>
 <li>Servidor da Web usado para servir páginas e outros conteúdos (por exemplo, Nginx, Apache).</li>
 <li>Servidor de aplicativos que passa solicitações "dinâmicas" entre seu site Django e o servidor web.</li>
 <li>Bancos de dados dos quais seu site depende.</li>
</ul>

<div class="note">
<p><strong>Nota: </strong>Dependendo de como sua produção está configurada, você também pode ter um proxy reverso, load balancer(balanceador de carga), etc.</p>
</div>

<p>O computador/servidor pode estar localizado em suas instalações e conectado à Internet por um link rápido, mas é muito mais comum usar um computador hospedado "na nuvem". O que isso realmente significa é que seu código é executado em algum computador remoto (ou possivelmente em um computador "virtual") no(s) centro(s) de dados da empresa de hospedagem. O servidor remoto geralmente oferece algum nível garantido de recursos de computação (por exemplo, CPU, RAM, memória de armazenamento, etc.) e conectividade com a Internet por um determinado preço.</p>

<p>Esse tipo de hardware de computação/rede acessível remotamente é conhecido como <em>Infraestrutura como Serviço</em> (IaaS). Muitos fornecedores de IaaS fornecem opções para pré-instalar um sistema operacional específico, no qual você deve instalar os outros componentes de seu ambiente de produção. Outros fornecedores permitem que você selecione ambientes com mais recursos, talvez incluindo uma configuração completa de Django e servidor web.</p>

<div class="note">
<p><strong>Nota:</strong> Ambientes pré-construídos podem tornar a configuração do seu site muito fácil porque reduzem a configuração, mas as opções disponíveis podem limitar você a um servidor desconhecido (ou outros componentes) e podem ser baseadas em uma versão mais antiga do sistema operacional. Freqüentemente, é melhor instalar você mesmo os componentes, para obter os que deseja e, quando precisar atualizar partes do sistema, tenha uma ideia de por onde começar!</p>
</div>

<p>Outros provedores de hospedagem oferecem suporte a Django como parte de uma oferta de <em>Plataforma como Serviço</em> (PaaS). Nesse tipo de hospedagem, você não precisa se preocupar com a maior parte do seu ambiente de produção (servidor da web, servidor de aplicativos, balanceadores de carga), pois a plataforma host cuida disso para você (junto com a maior parte do que você precisa fazer para para dimensionar seu aplicativo). Isso torna a implantação muito fácil, porque você só precisa se concentrar em seu aplicativo da web e não em toda a outra infraestrutura de servidor.</p>

<p>Alguns desenvolvedores escolherão a maior flexibilidade fornecida por IaaS em vez de PaaS, enquanto outros apreciarão a sobrecarga de manutenção reduzida e escalonamento mais fácil de PaaS. Quando você está começando, configurar seu site em um sistema PaaS é muito mais fácil e é isso que faremos neste tutorial.</p>

<div class="note">
<p><strong>Dica:</strong> Se você escolher um provedor de hospedagem compatível com Python / Django, ele deve fornecer instruções sobre como configurar um site Django usando diferentes configurações de servidor da web, servidor de aplicativos, proxy reverso, etc (isso não será relevante se você escolher um PaaS ) Por exemplo, existem muitos guias passo a passo para várias configurações nos <a href="https://www.digitalocean.com/community/tutorials?q=django">documentos da comunidade Digital Ocean Django.</a></p>
</div>

<h2 id="Escolhendo_um_provedor_de_hospedagem">Escolhendo um provedor de hospedagem</h2>

<p>Existem mais de 100 provedores de hospedagem que são conhecidos por oferecer suporte ativo ou funcionar bem com o Django (você pode encontrar uma lista bastante exaustiva em <a href="https://djangofriendly.com/index.html">Djangofriendly hosts</a>). Esses fornecedores oferecem diferentes tipos de ambientes (IaaS, PaaS) e diferentes níveis de recursos de computação e rede a preços diferentes.</p>

<p>Algumas coisas a serem consideradas ao escolher um host:</p>

<ul>
 <li>O quão ocupado seu site provavelmente estará e o custo dos dados e recursos de computação necessários para atender a essa demanda.</li>
 <li>Nível de suporte para escalonamento horizontal (adicionando mais máquinas) e verticalmente (atualizando para máquinas mais potentes) e os custos de fazê-lo.</li>
 <li>Onde o fornecedor possui centros de dados e, portanto, onde o acesso provavelmente será mais rápido.</li>
 <li>O histórico de tempo de atividade e desempenho do tempo de inatividade do host.</li>
 <li>Ferramentas fornecidas para gerenciar o site - são fáceis de usar e seguras (por exemplo, SFTP x FTP).</li>
 <li>Estruturas integradas para monitorar seu servidor.</li>
 <li>Limitações conhecidas. Alguns hosts bloqueiam deliberadamente certos serviços (por exemplo, e-mail). Outros oferecem apenas um certo número de horas de "tempo de vida" em algumas faixas de preço ou oferecem apenas uma pequena quantidade de armazenamento.</li>
 <li>Benefícios adicionais. Alguns provedores oferecem nomes de domínio gratuitos e suporte para certificados SSL que, de outra forma, você teria que pagar.</li>
 <li>Se o nível "gratuito" com o qual você está contando expira com o tempo e se o custo de migrar para um nível mais caro significa que você teria ficado melhor usando algum outro serviço em primeiro lugar!</li>
</ul>

<p>A boa notícia quando você está começando é que existem alguns sites que fornecem ambientes de computação de "avaliação", "desenvolvedor" ou "amador" de graça. Esses são sempre ambientes com recursos limitados / restritos e você precisa estar ciente de que eles podem expirar após um período introdutório. No entanto, eles são ótimos para testar sites de baixo tráfego em um ambiente real e podem fornecer uma migração fácil para pagar por mais recursos quando seu site ficar mais ocupado. As escolhas populares nesta categoria incluem <a href="https://www.heroku.com/">Heroku</a>, <a href="https://www.pythonanywhere.com/">Python Anywhere</a>, <a href="https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html">Amazon Web Services</a>, <a href="https://azure.microsoft.com/en-us/pricing/details/app-service/windows/">Microsoft Azure</a>, etc.</p>

<p>Muitos provedores também têm uma camada "básica" que fornece níveis mais úteis de capacidade de computação e menos limitações. <a href="https://www.digitalocean.com/">Digital Ocean</a> e <a href="https://www.pythonanywhere.com/">Python Anywhere</a> são exemplos de provedores de hospedagem populares que oferecem uma camada de computação básica relativamente barata (na faixa de US$5 a US$10 por mês).</p>

<div class="note">
<p><strong>Nota: </strong>Lembre-se de que o preço não é o único critério de seleção. Se o seu site for bem-sucedido, pode ser que a escalabilidade seja a consideração mais importante.</p>
</div>

<h2 id="Preparando_seu_site_para_publicação">Preparando seu site para publicação</h2>

<p>O <a href="/en-US/docs/Learn/Server-side/Django/skeleton_website">esqueleto do site do Django</a> criado usando as ferramentas django-admin e manage.py é configurado para tornar o desenvolvimento mais fácil. Muitas das configurações do projeto Django (especificadas em settings.py) devem ser diferentes para produção, por motivos de segurança ou desempenho.</p>

<div class="note">
<p><strong>Dica:</strong> É comum ter um arquivo <strong>settings.py</strong> separado para produção e importar configurações confidenciais de um arquivo separado ou de uma variável de ambiente. Este arquivo deve ser protegido, mesmo se o resto do código-fonte estiver disponível em um repositório público.</p>
</div>

<p>As configurações críticas que você deve verificar são:</p>

<ul>
 <li><code>DEBUG</code>. Isso deve ser definido como <code>False</code> em produção (<code>DEBUG = False</code>). Isso impede que o rastreamento de depuração sensível/confidencial e as informações variáveis ​​sejam exibidas.</li>
 <li><code>SECRET_KEY</code>. Este é um grande valor aleatório usado para proteção contra CSRF etc. É importante que a chave usada na produção não esteja no controle de origem ou acessível fora do servidor de produção. Os documentos do Django sugerem que isso pode ser melhor carregado de uma variável de ambiente ou lido de um arquivo somente servidor.
  <pre class="brush: python notranslate"># Read SECRET_KEY from an environment variable
import os
SECRET_KEY = os.environ['SECRET_KEY']

# OR

# Read secret key from a file
with open('/etc/secret_key.txt') as f:
    SECRET_KEY = f.read().strip()</pre>
 </li>
</ul>

<p>Vamos mudar o aplicativo LocalLibrary para que possamos ler nosso <code>SECRET_KEY</code> e <code>DEBUG</code> variáveis ​​de variáveis ​​de ambiente se forem definidas, mas caso contrário, use os valores padrão no arquivo de configuração.</p>

<p>Abra <strong>/locallibrary/settings.py</strong>, desative o original <code>SECRET_KEY</code>configuração e adicione as novas linhas conforme mostrado abaixo em <strong>negrito</strong>. Durante o desenvolvimento, nenhuma variável de ambiente será especificada para a chave, então o valor padrão será usado (não importa qual chave você usa aqui, ou se a chave "vaza", porque você não a usará na produção).</p>

<pre class="brush: python notranslate"># SECURITY WARNING: keep the secret key used in production secret!
# SECRET_KEY = 'cg#p$g+j9tax!#a3cup@1$8obt2_+&amp;k3q+pmu)5%asj6yjpkag'
<strong>import os</strong>
<strong>SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'cg#p$g+j9tax!#a3cup@1$8obt2_+&amp;k3q+pmu)5%asj6yjpkag')</strong>
</pre>

<p>Em seguida, comente o existente <code>DEBUG</code> configuração e adicione a nova linha mostrada abaixo.</p>

<pre class="brush: python notranslate"># SECURITY WARNING: don't run with debug turned on in production!
# DEBUG = True
<strong>DEBUG = os.environ.get('DJANGO_DEBUG', '') != 'False'</strong>
</pre>

<p>O valor do <code>DEBUG</code> será <code>True</code> por padrão, mas será apenas <code>False</code> se o valor do <code>DJANGO_DEBUG</code> variável de ambiente é definida para <code>False</code>. Observe que as variáveis ​​de ambiente são strings e não tipos Python. Portanto, precisamos comparar strings. A única maneira de definir o <code>DEBUG</code> variável para <code>False</code> é realmente configurá-lo para a string <code>False</code></p>

<p>Você pode definir a variável de ambiente como False, emitindo o seguinte comando:</p>

<pre class="brush: bash notranslate">export DJANGO_DEBUG=False</pre>

<p>Uma lista de verificação completa das configurações que você pode querer mudar é fornecida na <a href="https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/">Lista de verificação de implantação</a> (documentos do Django). Você também pode listar vários deles usando o comando de terminal abaixo:</p>

<pre class="brush: python notranslate">python3 manage.py check --deploy
</pre>

<h2 id="Exemplo_Instalando_LocalLibrary_no_Heroku">Exemplo: Instalando LocalLibrary no Heroku</h2>

<p>Esta seção fornece uma demonstração prática de como instalar a LocalLibrary na nuvem <a href="https://www.heroku.com/">Heroku PaaS.</a></p>

<h3 id="Por_que_Heroku">Por que Heroku?</h3>

<p>Heroku é um dos mais antigos e populares serviços de PaaS baseados em nuvem. Originalmente, ele suportava apenas aplicativos Ruby, mas agora pode ser usado para hospedar aplicativos de muitos ambientes de programação, incluindo Django!</p>

<p>Estamos optando por usar o Heroku por vários motivos:</p>

<ul>
 <li>O Heroku tem um <a href="https://www.heroku.com/pricing">nível gratuito</a> que é realmente gratuito (embora com algumas limitações).</li>
 <li>Como PaaS, o Heroku cuida de grande parte da infraestrutura web para nós. Isso torna muito mais fácil começar, porque você não se preocupa com servidores, balanceadores de carga, proxies reversos ou qualquer outra infraestrutura web que o Heroku fornece para nós nos bastidores.</li>
 <li>Embora tenha algumas limitações, elas não afetarão este aplicativo específico. Por exemplo:
  <ul>
   <li>O Heroku fornece apenas armazenamento de curta duração, portanto, os arquivos carregados pelo usuário não podem ser armazenados com segurança no próprio Heroku.</li>
   <li>O nível gratuito suspenderá um aplicativo da web inativo se não houver solicitações dentro de um período de meia hora. O site pode levar vários segundos para responder quando for ativado.</li>
   <li>O nível gratuito limita o tempo de execução do seu site a uma determinada quantidade de horas todos os meses (sem incluir o tempo em que o site fica "adormecido"). Isso é bom para um site de baixo uso/demonstração, mas não será adequado se 100% de tempo de atividade for necessário.</li>
   <li>Outras limitações estão listadas em <a href="https://devcenter.heroku.com/articles/limits">Limites</a> (documentos do Heroku).</li>
  </ul>
 </li>
 <li>Na maioria das vezes, ele simplesmente funciona e, se você acabar adorando, dimensionar seu aplicativo é muito fácil.</li>
</ul>

<p>Embora o Heroku seja perfeito para hospedar esta demonstração, pode não ser perfeito para o seu site real. O Heroku torna as coisas fáceis de configurar e escalar, ao custo de ser menos flexível e potencialmente muito mais caro depois que você sai do nível gratuito.</p>

<h3 id="How_does_Heroku_work">How does Heroku work?</h3>

<p>Heroku runs Django websites within one or more "<a href="https://devcenter.heroku.com/articles/dynos">Dynos</a>", which are isolated, virtualized Unix containers that provide the environment required to run an application. The dynos are completely isolated and have an <em>ephemeral</em> file system (a short-lived file system that is cleaned/emptied every time the dyno restarts). The only thing that dynos share by default are application <a href="https://devcenter.heroku.com/articles/config-vars">configuration variables</a>. Heroku internally uses a load balancer to distribute web traffic to all "web" dynos. Since nothing is shared between them, Heroku can scale an app horizontally simply by adding more dynos (though of course you may also need to scale your database to accept additional connections).</p>

<p>Because the file system is ephemeral you can't install services required by your application directly (e.g. databases, queues, caching systems, storage, email services, etc). Instead Heroku web applications use backing services provided as independent "add-ons" by Heroku or 3rd parties. Once attached to your web application, the dynos access the services using information contained in application configuration variables.</p>

<p>In order to execute your application Heroku needs to be able to set up the appropriate environment and dependencies, and also understand how it is launched. For Django apps we provide this information in a number of text files:</p>

<ul>
 <li><strong>runtime.txt</strong>:<strong> </strong>the programming language and version to use.</li>
 <li><strong>requirements.txt</strong>: the Python component dependencies, including Django.</li>
 <li><strong>Procfile</strong>: A list of processes to be executed to start the web application. For Django this will usually be the Gunicorn web application server (with a <code>.wsgi</code> script).</li>
 <li><strong>wsgi.py</strong>: <a href="http://wsgi.readthedocs.io/en/latest/what.html">WSGI</a> configuration to call our Django application in the Heroku environment.</li>
</ul>

<p>Developers interact with Heroku using a special client app/terminal, which is much like a Unix Bash shell. This allows you to upload code that is stored in a git repository, inspect the running processes, see logs, set configuration variables and much more!</p>

<p>In order to get our application to work on Heroku we'll need to put our Django web application into a git repository, add the files above, integrate with a database add-on, and make changes to properly handle static files.</p>

<p>Once we've done all that we can set up a Heroku account, get the Heroku client, and use it to install our website.</p>

<div class="note">
<p><strong>Note:</strong> The instructions below reflect how to work with Heroku at time of writing. If Heroku significantly change their processes, you may wish to instead check their setup documents: <a href="https://devcenter.heroku.com/articles/getting-started-with-python#introduction">Getting Started on Heroku with Django</a>.</p>
</div>

<p>That's all the overview you need in order to get started (see <a href="https://devcenter.heroku.com/articles/how-heroku-works">How Heroku works</a> for a more comprehensive guide).</p>

<h3 id="Creating_an_application_repository_in_Github">Creating an application repository in Github</h3>

<p>Heroku is closely integrated with the <strong>git</strong> source code version control system, using it to upload/synchronise any changes you make to the live system. It does this by adding a new heroku "remote" repository named <em>heroku</em> pointing to a repository for your source on the Heroku cloud. During development you use git to store changes on your "master" repository. When you want to deploy your site, you sync your changes to the Heroku repository.</p>

<div class="note">
<p><strong>Note:</strong> If you're used to following good software development practices you are probably already using git or some other SCM system. If you already have a git repository, then you can skip this step.</p>
</div>

<p>There are a lot of ways to work with git, but one of the easiest is to first set up an account on <a href="https://github.com/">Github</a>, create the repository there, and then sync to it locally:</p>

<ol>
 <li>Visit <a href="https://github.com/">https://github.com/</a> and create an account.</li>
 <li>Once you are logged in, click the <strong>+</strong> link in the top toolbar and select <strong>New repository</strong>.</li>
 <li>Fill in all the fields on this form. While these are not compulsory, they are strongly recommended.
  <ul>
   <li>Enter a new repository name (e.g. <em>django_local_library</em>), and description (e.g. "Local Library website written in Django".</li>
   <li>Choose <strong>Python</strong> in the <em>Add .gitignore</em> selection list.</li>
   <li>Choose your preferred license in the <em>Add license</em> selection list.</li>
   <li>Check <strong>Initialize this repository with a README</strong>.</li>
  </ul>
 </li>
 <li>Press <strong>Create repository</strong>.</li>
 <li>Click the green "<strong>Clone or download</strong>" button on your new repo page.</li>
 <li>Copy the URL value from the text field inside the dialog box that appears (it should be something like: <strong>https://github.com/<em>&lt;your_git_user_id&gt;</em>/django_local_library.git</strong>).</li>
</ol>

<p>Now that the repository ("repo") is created we are going to want to clone it on our local computer:</p>

<ol>
 <li>Install <em>git</em> for your local computer (you can find versions for different platforms <a href="https://git-scm.com/downloads">here</a>).</li>
 <li>Open a command prompt/terminal and clone your repository using the URL you copied above:
  <pre class="brush: bash notranslate">git clone https://github.com/<strong><em>&lt;your_git_user_id&gt;</em></strong>/django_local_library.git
</pre>
  This will create the repository in a new folder in the current working directory.</li>
 <li>Navigate into the new repo.
  <pre class="brush: bash notranslate">cd django_local_library</pre>
 </li>
</ol>

<p>The final steps are to copy your application into this local project directory and then add (or "push", in git lingo) the local repository to your remote Github repository:</p>

<ol>
 <li>Copy your Django application into this folder (all the files at the same level as <strong>manage.py</strong> and below, <strong>not</strong> their containing locallibrary folder).</li>
 <li>Open the <strong>.gitignore</strong> file, copy the following lines into the bottom of it, and then save (this file is used to identify files that should not be uploaded to git by default).
  <pre class="notranslate"># Text backup files
*.bak

# Database
*.sqlite3</pre>
 </li>
 <li>Open a command prompt/terminal and use the <code>add</code> command to add all files to git. This adds the files which aren't ignored by the <strong>.gitignore</strong> file to the "staging area".
  <pre class="brush: bash notranslate">git add -A
</pre>
 </li>
 <li>Use the <code>status</code> command to check that all files you are about to <code>commit</code> are correct (you want to include source files, not binaries, temporary files etc.). It should look a bit like the listing below.
  <pre class="notranslate">&gt; git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD &lt;file&gt;..." to unstage)

        modified:   .gitignore
        new file:   catalog/__init__.py
        ...
        new file:   catalog/migrations/0001_initial.py
        ...
        new file:   templates/registration/password_reset_form.html</pre>
 </li>
 <li>When you're satisfied, <code>commit</code> the files to your local repository. This is essentially equivalent to signing off on the changes and making them an official part of the local repository.
  <pre class="brush: bash notranslate">git commit -m "First version of application moved into github"</pre>
 </li>
 <li>At this point, the remote repository has not been changed. Synchronise (<code>push</code>) your local repository to the remote Github repository using the following command:
  <pre class="notranslate">git push origin master</pre>
 </li>
</ol>

<p>When this operation completes, you should be able to go back to the page on Github where you created your repo, refresh the page, and see that your whole application has now been uploaded. You can continue to update your repository as files change using this add/commit/push cycle.</p>

<div class="note">
<p><strong>Tip:</strong> This is a good point to make a backup of your "vanilla" project — while some of the changes we're going to be making in the following sections might be useful for deployment on any platform (or development) others might not.</p>

<p>The <em>best</em> way to do this is to use <em>git</em> to manage your revisions. With <em>git</em> you can not only go back to a particular old version, but you can maintain this in a separate "branch" from your production changes and cherry-pick any changes to move between production and development branches. <a href="https://help.github.com/articles/good-resources-for-learning-git-and-github/">Learning Git</a> is well worth the effort, but is beyond the scope of this topic.</p>

<p>The <em>easiest</em> way to do this is to just copy your files into another location. Use whichever approach best matches your knowledge of git!</p>
</div>

<h3 id="Update_the_app_for_Heroku">Update the app for Heroku</h3>

<p>This section explains the changes you'll need to make to our <em>LocalLibrary</em> application to get it to work on Heroku. While Heroku's <a href="https://devcenter.heroku.com/articles/getting-started-with-python#introduction">Getting Started on Heroku with Django</a> instructions assume you will use the Heroku client to also run your local development environment, our changes are compatible with the existing Django development server and the workflows we've already learned.</p>

<h4 id="Procfile">Procfile</h4>

<p>Create the file <code>Procfile</code> (no extension) in the root of your GitHub repository to declare the application's process types and entry points. Copy the following text into it:</p>

<pre class="notranslate">web: gunicorn locallibrary.wsgi --log-file -</pre>

<p>The "<code>web:</code>" tells Heroku that this is a web dyno and can be sent HTTP traffic. The process to start in this dyno is <em>gunicorn</em>, which is a popular web application server that Heroku recommends. We start Gunicorn using the configuration information in the module <code>locallibrary.wsgi</code> (created with our application skeleton: <strong>/locallibrary/wsgi.py</strong>).</p>

<h4 id="Gunicorn">Gunicorn</h4>

<p><a href="http://gunicorn.org/">Gunicorn</a> is the recommended HTTP server for use with Django on Heroku (as referenced in the Procfile above). It is a pure-Python HTTP server for WSGI applications that can run multiple Python concurrent processes within a single dyno (see <a href="https://devcenter.heroku.com/articles/python-gunicorn">Deploying Python applications with Gunicorn</a> for more information).</p>

<p>While we won't need <em>Gunicorn</em> to serve our LocalLibrary application during development, we'll install it so that it becomes part of our <a href="#requirements">requirements</a> for Heroku to set up on the remote server.</p>

<p>Install <em>Gunicorn</em> locally on the command line using <em>pip</em> (which we installed when <a href="/en-US/docs/Learn/Server-side/Django/development_environment">setting up the development environment</a>):</p>

<div class="blockIndicator note">
<p>Note: Make sure that you're in your Python virtual environment (use the <code>workon [name-of-virtual-environment]</code> command) before you install <em>Gunicorn</em> and further modules with <em>pip</em>, or you might experience problems with importing these modules in your <strong>/locallibrary/settings.py</strong> file in the later sections. </p>
</div>

<pre class="brush: bash notranslate">pip3 install gunicorn
</pre>

<h4 id="Database_configuration">Database configuration</h4>

<p>We can't use the default SQLite database on Heroku because it is file-based, and it would be deleted from the <em>ephemeral</em> file system every time the application restarts (typically once a day, and every time the application or its configuration variables are changed).</p>

<p>The Heroku mechanism for handling this situation is to use a <a href="https://elements.heroku.com/addons#data-stores">database add-on</a> and configure the web application using information from an environment <a href="https://devcenter.heroku.com/articles/config-vars">configuration variable</a>, set by the add-on. There are quite a lot of database options, but we'll use the <a href="https://devcenter.heroku.com/articles/heroku-postgres-plans#plan-tiers">hobby tier</a> of the <em>Heroku postgres</em> database as this is free, supported by Django, and automatically added to our new Heroku apps when using the free hobby dyno plan tier.</p>

<p>The database connection information is supplied to the web dyno using a configuration variable named <code>DATABASE_URL</code>. Rather than hard-coding this information into Django, Heroku recommends that developers use the <a href="https://warehouse.python.org/project/dj-database-url/">dj-database-url</a> package to parse the <code>DATABASE_URL</code> environment variable and automatically convert it to Django’s desired configuration format. In addition to installing the <em>dj-database-url</em> package we'll also need to install <a href="http://initd.org/psycopg/">psycopg2</a>, as Django needs this to interact with Postgres databases.</p>

<h5 id="dj-database-url_Django_database_configuration_from_environment_variable">dj-database-url (Django database configuration from environment variable)</h5>

<p>Install <em>dj-database-url</em> locally so that it becomes part of our <a href="#requirements">requirements</a> for Heroku to set up on the remote server:</p>

<pre class="notranslate">$ pip3 install dj-database-url
</pre>

<h5 id="settings.py">settings.py</h5>

<p>Open <strong>/locallibrary/settings.py</strong> and copy the following configuration into the bottom of the file:</p>

<pre class="notranslate"># Heroku: Update database configuration from $DATABASE_URL.
import dj_database_url
db_from_env = dj_database_url.config(conn_max_age=500)
DATABASES['default'].update(db_from_env)</pre>

<div class="note">
<p><strong>Note:</strong></p>

<ul>
 <li>We'll still be using SQLite during development because the <code>DATABASE_URL</code> environment variable will not be set on our development computer.</li>
 <li>The value <code>conn_max_age=500</code> makes the connection persistent, which is far more efficient than recreating the connection on every request cycle. However, this is optional and can be removed if needed.</li>
</ul>
</div>

<h5 id="psycopg2_Python_Postgres_database_support">psycopg2 (Python Postgres database support)</h5>

<p>Django needs <em>psycopg2</em> to work with Postgres databases and you will need to add this to the <a href="#requirements">requirements.txt</a> for Heroku to set this up on the remote server (as discussed in the requirements section below).</p>

<p>Django will use our SQLite database locally by default, because the <code>DATABASE_URL</code> environment variable isn't set in our local environment. If you want to switch to Postgres completely and use our Heroku free tier database for both development and production then you can. For example, to install psycopg2 and its dependencies locally on a Debian-flavoured Linux system you would use the following Bash/terminal commands:</p>

<pre class="brush: bash notranslate"><code>sudo apt-get install python-pip python-dev libpq-dev postgresql postgresql-contrib</code>
pip3 install psycopg2-binary
</pre>

<p>Installation instructions for the other platforms can be found on the <a href="http://initd.org/psycopg/docs/install.html">psycopg2 website here</a>.</p>

<p>However, you don't need to do this — you don't need PostgreSQL active on the local computer, as long as you give it to Heroku as a requirement, in <code>requirements.txt</code> (see below).</p>

<h4 id="Serving_static_files_in_production">Serving static files in production</h4>

<p>During development we used Django and the Django development web server to serve our static files (CSS, JavaScript, etc.). In a production environment we instead typically serve static files from a content delivery network (CDN) or the web server.</p>

<div class="note">
<p><strong>Note:</strong> Serving static files via Django/web application is inefficient because the requests have to pass through unnecessary additional code (Django) rather than being handled directly by the web server or a completely separate CDN. While this doesn't matter for local use during development, it would have a significant performance impact if we were to use the same approach in production. </p>
</div>

<p>To make it easy to host static files separately from the Django web application, Django provides the <em>collectstatic</em> tool to collect these files for deployment (there is a settings variable that defines where the files should be collected when <em>collectstatic</em> is run). Django templates refer to the hosting location of the static files relative to a settings variable (<code>STATIC_URL</code>), so that this can be changed if the static files are moved to another host/server.</p>

<p>The relevant setting variables are:</p>

<ul>
 <li><code>STATIC_URL</code>: This is the base URL location from which static files will be served, for example on a CDN. This is used for the static template variable that is accessed in our base template (see <a href="/en-US/docs/Learn/Server-side/Django/Home_page">Django Tutorial Part 5: Creating our home page</a>).</li>
 <li><code>STATIC_ROOT</code>: This is the absolute path to a directory where Django's "collectstatic" tool will gather any static files referenced in our templates. Once collected, these can then be uploaded as a group to wherever the files are to be hosted.</li>
 <li><code>STATICFILES_DIRS</code>: This lists additional directories that Django's collectstatic tool should search for static files.</li>
</ul>

<h5 id="settings.py_2">settings.py</h5>

<p>Open <strong>/locallibrary/settings.py</strong> and copy the following configuration into the bottom of the file. The <code>BASE_DIR</code> should already have been defined in your file (the <code>STATIC_URL</code> may already have been defined within the file when it was created. While it will cause no harm, you might as well delete the duplicate previous reference).</p>

<pre class="notranslate"># Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.1/howto/static-files/

# The absolute path to the directory where collectstatic will collect static files for deployment.
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

# The URL to use when referring to static files (where they will be served from)
STATIC_URL = '/static/'
</pre>

<p>We'll actually do the file serving using a library called <a href="https://warehouse.python.org/project/whitenoise/">WhiteNoise</a>, which we install and configure in the next section.</p>

<p>For more information, see <a href="https://devcenter.heroku.com/articles/django-assets">Django and Static Assets</a> (Heroku docs).</p>

<h4 id="Whitenoise">Whitenoise</h4>

<p>There are many ways to serve static files in production (we saw the relevant Django settings in the previous sections). Heroku recommends using the <a href="https://warehouse.python.org/project/whitenoise/">WhiteNoise</a> project for serving of static assets directly from Gunicorn in production.</p>

<div class="note">
<p><strong>Note: </strong>Heroku automatically calls <em>collectstatic</em> and prepares your static files for use by WhiteNoise after it uploads your application. Check out <a href="https://warehouse.python.org/project/whitenoise/">WhiteNoise</a> documentation for an explanation of how it works and why the implementation is a relatively efficient method for serving these files.</p>
</div>

<p>The steps to set up <em>WhiteNoise</em> to use with the project are <a href="http://whitenoise.evans.io/en/stable/django.html">given here</a> (and reproduced below):</p>

<h5 id="WhiteNoise">WhiteNoise</h5>

<p>Install whitenoise locally using the following command:</p>

<pre class="notranslate">$ pip3 install whitenoise
</pre>

<h5 id="settings.py_3">settings.py</h5>

<p>To install <em>WhiteNoise</em> into your Django application, open <strong>/locallibrary/settings.py</strong>, find the <code>MIDDLEWARE</code> setting and add the <code>WhiteNoiseMiddleware</code> near the top of the list, just below the <code>SecurityMiddleware</code>:</p>

<pre class="notranslate">MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    <strong>'whitenoise.middleware.WhiteNoiseMiddleware',</strong>
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
</pre>

<p>Optionally, you can reduce the size of the static files when they are served (this is more efficient). Just add the following to the bottom of <strong>/locallibrary/settings.py</strong>:</p>

<pre class="notranslate"># Simplified static file serving.
# https://warehouse.python.org/project/whitenoise/
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
</pre>

<h4 id="Requirements">Requirements</h4>

<p>The Python requirements of your web application must be stored in a file <strong>requirements.txt</strong> in the root of your repository. Heroku will then install these automatically when it rebuilds your environment. You can create this file using <em>pip</em> on the command line (run the following in the repo root):</p>

<pre class="brush: bash notranslate">pip3 freeze &gt; requirements.txt</pre>

<p>After installing all the different dependencies above, your <strong>requirements.txt</strong> file should have <em>at least</em> these items listed (though the version numbers may be different). Please delete any other dependencies not listed below, unless you've explicitly added them for this application.</p>

<pre class="notranslate">dj-database-url==0.5.0
Django==2.1.5
gunicorn==19.9.0
<strong>psycopg2-binary==2.7.7</strong>
whitenoise==4.1.2
</pre>

<div class="note">
<p>Make sure that a <strong>psycopg2</strong> line like the one above is present! Even if you didn't install this locally then you should still add it to <strong>requirements.txt</strong>.</p>
</div>

<h4 id="Runtime">Runtime</h4>

<p>The <strong>runtime.txt</strong> file, if defined, tells Heroku which programming language to use. Create the file in the root of the repo and add the following text:</p>

<pre class="notranslate">python-3.7.0</pre>

<div class="note">
<p><strong>Note:</strong> Heroku only supports a small number of <a href="https://devcenter.heroku.com/articles/python-support#supported-python-runtimes">Python runtimes</a> (at time of writing, this includes the one above). Heroku will use a supported runtime irrespective of the value specified in this file.</p>
</div>

<h4 id="Re-test_and_save_changes_to_Github">Re-test and save changes to Github</h4>

<p>Before we proceed, lets test the site again locally and make sure it wasn't broken by any of our changes above. Run the development web server as usual and then check the site still works as you expect on your browser.</p>

<pre class="brush: bash notranslate">python3 manage.py runserver</pre>

<p>Next, lets <code>push</code> our changes to Github. In the terminal (after having navigated to our local repository), enter the following commands:</p>

<pre class="brush: python notranslate">git add -A
git commit -m "Added files and changes required for deployment to heroku"
git push origin master
</pre>

<p>We should now be ready to start deploying LocalLibrary on Heroku.</p>

<h3 id="Get_a_Heroku_account">Get a Heroku account</h3>

<p>To start using Heroku you will first need to create an account:</p>

<ul>
 <li>Go to <a href="https://www.heroku.com/">www.heroku.com</a> and click the <strong>SIGN UP FOR FREE</strong> button.</li>
 <li>Enter your details and then press <strong>CREATE FREE ACCOUNT</strong>. You'll be asked to check your account for a sign-up email.</li>
 <li>Click the account activation link in the signup email. You'll be taken back to your account on the web browser.</li>
 <li>Enter your password and click <strong>SET PASSWORD AND LOGIN</strong>.</li>
 <li>You'll then be logged in and taken to the Heroku dashboard: <a href="https://dashboard.heroku.com/apps">https://dashboard.heroku.com/apps</a>.</li>
</ul>

<h3 id="Install_the_client">Install the client</h3>

<p>Download and install the Heroku client by following the <a href="https://devcenter.heroku.com/articles/getting-started-with-python#set-up">instructions on Heroku here</a>.</p>

<p>After the client is installed you will be able run commands. For example to get help on the client:</p>

<pre class="brush: bash notranslate">heroku help
</pre>

<h3 id="Create_and_upload_the_website">Create and upload the website</h3>

<p>To create the app we run the "create" command in the root directory of our repository. This creates a git remote ("pointer to a remote repository") named <em>heroku</em> in our local git environment.</p>

<pre class="brush: bash notranslate">heroku create</pre>

<div class="note">
<p><strong>Note:</strong> You can name the remote if you like by specifying a value after "create". If you don't then you'll get a random name. The name is used in the default URL.</p>
</div>

<p>We can then push our app to the Heroku repository as shown below. This will upload the app, package it in a dyno, run collectstatic, and start the site.</p>

<pre class="brush: bash notranslate">git push heroku master</pre>

<p>If we're lucky, the app is now "running" on the site, but it won't be working properly because we haven't set up the database tables for use by our application. To do this we need to use the <code>heroku run</code> command and start a "<a href="https://devcenter.heroku.com/articles/deploying-python#one-off-dynos">one off dyno</a>" to perform a migrate operation. Enter the following command in your terminal:</p>

<pre class="brush: bash notranslate">heroku run python manage.py migrate</pre>

<p>We're also going to need to be able to add books and authors, so lets also create our administration superuser, again using a one-off dyno:</p>

<pre class="brush: bash notranslate">heroku run python manage.py createsuperuser</pre>

<p>Once this is complete, we can look at the site. It should work, although it won't have any books in it yet. To open your browser to the new website, use the command:</p>

<pre class="brush: bash notranslate">heroku open</pre>

<p>Create some books in the admin site, and check out whether the site is behaving as you expect.</p>

<h3 id="Managing_addons">Managing addons</h3>

<p>You can check out the add-ons to your app using the <code>heroku addons</code> command. This will list all addons, and their price tier and state.</p>

<pre class="brush: bash notranslate">&gt; heroku addons

Add-on                                     Plan       Price  State
─────────────────────────────────────────  ─────────  ─────  ───────
heroku-postgresql (postgresql-flat-26536)  hobby-dev  free   created
 └─ as DATABASE</pre>

<p>Here we see that we have just one add-on, the postgres SQL database. This is free, and was created automatically when we created the app. You can open a web page to examine the database add-on (or any other add-on) in more detail using the following command:</p>

<pre class="brush: bash notranslate">heroku addons:open heroku-postgresql
</pre>

<p>Other commands allow you to create, destroy, upgrade and downgrade addons (using a similar syntax to opening). For more information see <a href="https://devcenter.heroku.com/articles/managing-add-ons">Managing Add-ons</a> (Heroku docs).</p>

<h3 id="Setting_configuration_variables">Setting configuration variables</h3>

<p>You can check out the configuration variables for the site using the <code>heroku config</code> command. Below you can see that we have just one variable, the <code>DATABASE_URL</code> used to configure our database.</p>

<pre class="brush: bash notranslate">&gt; heroku config

=== locallibrary Config Vars
DATABASE_URL: postgres://uzfnbcyxidzgrl:j2jkUFDF6OGGqxkgg7Hk3ilbZI@ec2-54-243-201-144.compute-1.amazonaws.com:5432/dbftm4qgh3kda3</pre>

<p>If you recall from the section on <a href="#Getting_your_website_ready_to_publish">getting the website ready to publish</a>, we have to set environment variables for <code>DJANGO_SECRET_KEY</code> and <code>DJANGO_DEBUG</code>. Let's do this now.</p>

<div class="note">
<p><strong>Note:</strong> The secret key needs to be really secret! One way to generate a new key is to use the <a href="https://www.miniwebtool.com/django-secret-key-generator/">Django Secret Key Generator</a>.</p>
</div>

<p>We set <code>DJANGO_SECRET_KEY</code> using the <code>config:set</code> command (as shown below). Remember to use your own secret key!</p>

<pre class="brush: bash notranslate">&gt; heroku config:set DJANGO_SECRET_KEY='eu09(ilk6@4sfdofb=b_2ht@vad*$ehh9-)3u_83+y%(+phh&amp;='

Setting DJANGO_SECRET_KEY and restarting locallibrary... done, v7
DJANGO_SECRET_KEY: eu09(ilk6@4sfdofb=b_2ht@vad*$ehh9-)3u_83+y%(+phh
</pre>

<p>We similarly set <code>DJANGO_DEBUG</code>:</p>

<pre class="brush: bash notranslate">&gt; heroku config:set <code>DJANGO_DEBUG=

Setting DJANGO_DEBUG and restarting locallibrary... done, v8</code></pre>

<p>If you visit the site now you'll get a "Bad request" error, because the <a href="https://docs.djangoproject.com/en/2.1/ref/settings/#allowed-hosts">ALLOWED_HOSTS</a> setting is <em>required</em> if you have <code>DEBUG=False</code> (as a security measure). Open <strong>/locallibrary/settings.py</strong> and change the <code>ALLOWED_HOSTS</code> setting to include your base app url (e.g. 'locallibrary1234.herokuapp.com') and the URL you normally use on your local development server.</p>

<pre class="brush: python notranslate">ALLOWED_HOSTS = ['&lt;your app URL without the https:// prefix&gt;.herokuapp.com','127.0.0.1']
# For example:
# ALLOWED_HOSTS = ['fathomless-scrubland-30645.herokuapp.com', '127.0.0.1']
</pre>

<p>Then save your settings and commit them to your Github repo and to Heroku:</p>

<pre class="brush: bash notranslate">git add -A
git commit -m 'Update ALLOWED_HOSTS with site and development server URL'
git push origin master
git push heroku master</pre>

<div class="note">
<p>After the site update to Heroku completes, enter a URL that does not exist (e.g. <strong>/catalog/doesnotexist/</strong>). Previously this would have displayed a detailed debug page, but now you should just see a simple "Not Found" page.</p>
</div>

<h3 id="Debugging">Debugging</h3>

<p>The Heroku client provides a few tools for debugging:</p>

<pre class="brush: bash notranslate"># Show current logs
heroku logs

# Show current logs and keep updating with any new results
heroku logs --tail

# Add additional logging for collectstatic (this tool is run automatically during a build)
heroku config:set DEBUG_COLLECTSTATIC=1

# Display dyno status
heroku ps
</pre>

<p>If you need more information than these can provide you will need to start looking into <a href="https://docs.djangoproject.com/en/2.1/topics/logging/">Django Logging</a>.</p>

<ul>
</ul>

<h2 id="Summary">Summary</h2>

<p>That's the end of this tutorial on setting up Django apps in production, and also the series of tutorials on working with Django. We hope you've found them useful. You can check out a fully worked-through version of the <a href="https://github.com/mdn/django-locallibrary-tutorial">source code on Github here</a>.<br>
 <br>
 The next step is to read our last few articles, and then complete the assessment task.</p>

<h2 id="See_also">See also</h2>

<ul>
 <li><a href="https://docs.djangoproject.com/en/2.1/howto/deployment/">Deploying Django</a> (Django docs)

  <ul>
   <li><a href="https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/">Deployment checklist</a> (Django docs)</li>
   <li><a href="https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/">Deploying static files</a> (Django docs)</li>
   <li><a href="https://docs.djangoproject.com/en/2.1/howto/deployment/wsgi/">How to deploy with WSGI</a> (Django docs)</li>
   <li><a href="https://docs.djangoproject.com/en/2.1/howto/deployment/wsgi/modwsgi/">How to use Django with Apache and mod_wsgi</a> (Django docs)</li>
   <li><a href="https://docs.djangoproject.com/en/2.1/howto/deployment/wsgi/gunicorn/">How to use Django with Gunicorn</a> (Django docs)</li>
  </ul>
 </li>
 <li>Heroku
  <ul>
   <li><a href="https://devcenter.heroku.com/articles/django-app-configuration">Configuring Django apps for Heroku</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/getting-started-with-python#introduction">Getting Started on Heroku with Django</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/django-assets">Django and Static Assets</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/python-concurrency-and-database-connections">Concurrency and Database Connections in Django</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/how-heroku-works">How Heroku works</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/dynos">Dynos and the Dyno Manager</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/config-vars">Configuration and Config Vars</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/limits">Limits</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/python-gunicorn">Deploying Python applications with Gunicorn</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/articles/deploying-python">Deploying Python and Django apps on Heroku</a> (Heroku docs)</li>
   <li><a href="https://devcenter.heroku.com/search?q=django">Other Heroku Django docs</a></li>
  </ul>
 </li>
 <li>Digital Ocean
  <ul>
   <li><a href="https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-uwsgi-and-nginx-on-ubuntu-16-04">How To Serve Django Applications with uWSGI and Nginx on Ubuntu 16.04</a></li>
   <li><a href="https://www.digitalocean.com/community/tutorials?q=django">Other Digital Ocean Django community docs</a></li>
  </ul>
 </li>
</ul>

<p>{{PreviousMenuNext("Learn/Server-side/Django/Testing", "Learn/Server-side/Django/web_application_security", "Learn/Server-side/Django")}}</p>

<h2 id="In_this_module">In this module</h2>

<ul>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Introduction">Django introduction</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/development_environment">Setting up a Django development environment</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Tutorial_local_library_website">Django Tutorial: The Local Library website</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/skeleton_website">Django Tutorial Part 2: Creating a skeleton website</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Models">Django Tutorial Part 3: Using models</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Admin_site">Django Tutorial Part 4: Django admin site</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Home_page">Django Tutorial Part 5: Creating our home page</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Generic_views">Django Tutorial Part 6: Generic list and detail views</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Sessions">Django Tutorial Part 7: Sessions framework</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Authentication">Django Tutorial Part 8: User authentication and permissions</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Forms">Django Tutorial Part 9: Working with forms</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Testing">Django Tutorial Part 10: Testing a Django web application</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/Deployment">Django Tutorial Part 11: Deploying Django to production</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/web_application_security">Django web application security</a></li>
 <li><a href="/en-US/docs/Learn/Server-side/Django/django_assessment_blog">DIY Django mini blog</a></li>
</ul>