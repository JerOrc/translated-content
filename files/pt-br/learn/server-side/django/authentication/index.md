---
title: 'Tutorial Django Parte 8: Autenticação de usuário e permissões'
slug: Learn/Server-side/Django/Authentication
translation_of: Learn/Server-side/Django/Authentication
---
<div>{{LearnSidebar}}</div>

<div>{{PreviousMenuNext("Learn/Server-side/Django/Sessions", "Learn/Server-side/Django/Forms", "Learn/Server-side/Django")}}</div>

<p class="summary">Neste tutorial, mostraremos como permitir que os usuários efetuem login no seu site com suas próprias contas e como controlar o que eles podem fazer e ver com base em se eles estão ou não conectados e em suas permissões. Como parte desta demonstração, estenderemos o <a href="https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Tutorial_local_library_website">LocalLibrary</a> website, adicionando páginas de login e logout e páginas específicas do usuário e da equipe para visualizar os livros emprestados.</p>

<table class="learn-box standard-table">
 <tbody>
  <tr>
   <th scope="row">Pré-requisitos:</th>
   <td>Conclua todos os tópicos do tutorial anterior, incluindo <a href="/en-US/docs/Learn/Server-side/Django/Sessions">Django Tutorial Part 7: Sessions framework</a>.</td>
  </tr>
  <tr>
   <th scope="row">Objetivo:</th>
   <td>Para entender como configurar e usar a autenticação e permissões de usuário.</td>
  </tr>
 </tbody>
</table>

<h2 id="Visão_global">Visão global</h2>

<p>O Django fornece um sistema de autenticação e autorização ("permissão"), construído sobre a estrutura da sessão discutida no <a href="/en-US/docs/Learn/Server-side/Django/Sessions">tutorial anterior</a>, que permite verificar as credenciais do usuário e definir quais ações cada usuário tem permissão para executar. A estrutura inclui modelos internos para <code>Users</code> e <code>Groups</code> (uma maneira genérica de aplicar permissões a mais de um usuário por vez), permissões/sinalizadores que designam se um usuário pode executar uma tarefa, formulários e exibições para efetuar logon em usuários e exibir ferramentas para restringir o conteúdo.</p>

<div class="note">
<p><strong>Nota</strong>: De acordo com o Django, o sistema de autenticação pretende ser muito genérico e, portanto, não fornece alguns recursos fornecidos em outros sistemas de autenticação na web. Soluções para alguns problemas comuns estão disponíveis como pacotes de terceiros. Por exemplo, limitação de tentativas de login e autenticação contra terceiros (por exemplo, OAuth).</p>
</div>

<p>Neste tutorial, mostraremos como habilitar a autenticação do usuário no diretório <a href="https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Tutorial_local_library_website">LocalLibrary</a> website, crie suas próprias páginas de logon e logout, adicione permissões aos seus modelos e controle o acesso às páginas. Usaremos a autenticação/permissões para exibir listas de livros que foram emprestados para usuários e bibliotecários.</p>

<p>O sistema de autenticação é muito flexível e você pode criar seus URLs, formulários, visualizações e modelos a partir do zero, se quiser, apenas chamando a API fornecida para efetuar login no usuário. No entanto, neste artigo, vamos usar as visualizações e formulários de autenticação "stock" do Django para nossas páginas de logon e logout. Ainda precisamos criar alguns modelos, mas isso é bem fácil.</p>

<p>Também mostraremos como criar permissões e verificar o status e as permissões de login nas visualizações e nos modelos.</p>

<h2 id="Ativando_a_autenticação">Ativando a autenticação</h2>

<p>A autenticação foi ativada automaticamente quando <a href="/en-US/docs/Learn/Server-side/Django/skeleton_website">criamos o esqueleto do site</a> (no tutorial 2), para que você não precise fazer mais nada neste momento.</p>

<div class="note">
<p><strong>Nota</strong>: A configuração necessária foi feita para nós quando criamos o aplicativo usando o comando <code>django-admin startproject</code>. As tabelas de banco de dados para usuários e permissões de modelo foram criadas quando chamamos pela primeira vez <code>python manage.py migrate</code>.</p>
</div>

<p>A configuração está definida nas seções <code>INSTALLED_APPS</code> e <code>MIDDLEWARE</code> no settings.py (<strong>locallibrary/locallibrary/settings.py</strong>), como mostrado abaixo:</p>

<pre class="brush: python notranslate">INSTALLED_APPS = [
    ...
<strong>    'django.contrib.auth',  </strong>#Core authentication framework and its default models.
<strong>    'django.contrib.contenttypes',  #</strong>Django content type system (allows permissions to be associated with models).
    ....

MIDDLEWARE = [
    ...
<strong>    'django.contrib.sessions.middleware.SessionMiddleware',</strong>  #Manages sessions across requests
    ...
<strong>    'django.contrib.auth.middleware.AuthenticationMiddleware',</strong>  #Associates users with requests using sessions.
    ....
</pre>

<h2 id="Criando_usuários_e_grupos">Criando usuários e grupos</h2>

<p>Você já criou seu primeiro usuário quando olhamos para o <a href="/en-US/docs/Learn/Server-side/Django/Admin_site">site Django admin</a> no tutorial 4 (este era um superusuário, criado com o comando <code>python manage.py createsuperuser)</code>. Nosso superusuário já está autenticado e tem todas as permissões, portanto, precisamos criar um usuário de teste para representar um usuário normal do site. Usaremos o site de administração para criar nossos grupos de bibliotecas de locais e logins de sites, pois é uma das maneiras mais rápidas de fazer isso.</p>

<div class="note">
<p><strong>Nota</strong>: Você também pode criar usuários programaticamente, conforme mostrado abaixo. Você precisaria fazer isso, por exemplo, se desenvolvesse uma interface para permitir que os usuários criassem seus próprios logins (você não deve conceder aos usuários acesso ao site de administração).</p>

<pre class="brush: python notranslate">from django.contrib.auth.models import User

# Create user and save to the database
user = User.objects.create_user('myusername', 'myemail@crazymail.com', 'mypassword')

# Update fields and then save again
user.first_name = 'John'
user.last_name = 'Citizen'
user.save()
</pre>
</div>

<p>Abaixo, primeiro criaremos um grupo e depois um usuário. Embora ainda não tenhamos permissões para adicionar aos membros da nossa biblioteca, se precisarmos mais tarde, será muito mais fácil adicioná-los uma vez ao grupo do que individualmente a cada membro.</p>

<p>Inicie o servidor de desenvolvimento e navegue até o site de administração em seu navegador da web local (<a href="http://127.0.0.1:8000/admin/">http://127.0.0.1:8000/admin/</a>). Entre no site usando as credenciais da sua conta de superusuário. O nível superior do site Admin exibe todos os seus modelos, classificados por "aplicativo Django". Na seção <strong>Autenticação e Autorização</strong>, você pode clicar nos links <strong>Usuários </strong>ou <strong>Grupos </strong>para ver seus registros existentes.</p>

<p><img alt="Admin site - add groups or users" src="https://mdn.mozillademos.org/files/14091/admin_authentication_add.png" style="border-style: solid; border-width: 1px; display: block; height: 364px; margin: 0px auto; width: 661px;"></p>

<p>Primeiro vamos criar um novo grupo para os membros da nossa biblioteca.</p>

<ol>
 <li>Clique no botão <strong>Adicionar</strong> (ao lado de Grupo) para criar um novo grupo; digite o <strong>Nome</strong> "Library Members" para o grupo.<img alt="Admin site - add group" src="https://mdn.mozillademos.org/files/14093/admin_authentication_add_group.png" style="border-style: solid; border-width: 1px; display: block; height: 561px; margin: 0px auto; width: 800px;"></li>
 <li>Não precisamos de permissões para o grupo, então pressione <strong>SALVAR </strong>(você será direcionado para uma lista de grupos).</li>
</ol>

<p>Agora vamos criar um usuário:</p>

<ol>
 <li>Volte para a página inicial do site de administração</li>
 <li>Clique no botão <strong>Adicionar </strong>ao lado de <em>Usuários </em>para abrir a caixa de diálogo <em>Adicionar usuário</em>.<img alt="Admin site - add user pt1" src="https://mdn.mozillademos.org/files/14095/admin_authentication_add_user_prt1.png" style="border-style: solid; border-width: 1px; display: block; height: 409px; margin: 0px auto; width: 800px;"></li>
 <li>Digite um nome de <strong>usuário </strong>e uma <strong>senha/confirmação de senha</strong> adequados para o usuário de teste</li>
 <li>Pressione <strong>SALVAR </strong>para criar o usuário.<br>
  <br>
  O site de administração criará o novo usuário e levará você imediatamente para uma tela Alterar usuário, na qual é possível alterar seu <strong>nome de usuário</strong> e adicionar informações aos campos opcionais do modelo de usuário. Esses campos incluem o nome, o sobrenome, o endereço de email e o status e as permissões do usuário (somente o sinalizador <strong>Ativo </strong>deve ser definido). Mais abaixo, você pode especificar os grupos e permissões do usuário e ver datas importantes relacionadas ao usuário (por exemplo, a data de ingresso e a última data de login).<img alt="Admin site - add user pt2" src="https://mdn.mozillademos.org/files/14097/admin_authentication_add_user_prt2.png" style="border-style: solid; border-width: 1px; display: block; height: 635px; margin: 0px auto; width: 800px;"></li>
 <li>Na seção <em>Grupos</em>, selecione grupo de <strong>Library Members</strong> na lista de <em>Grupos disponíveis</em> e pressione a <strong>seta para a direita</strong> entre as caixas para movê-lo para a caixa <em>Grupos escolhidos</em>.<img alt="Admin site - add user to group" src="https://mdn.mozillademos.org/files/14099/admin_authentication_user_add_group.png" style="border-style: solid; border-width: 1px; display: block; height: 414px; margin: 0px auto; width: 933px;"></li>
 <li>Não precisamos fazer mais nada aqui; basta selecionar <strong>SALVAR </strong>novamente, para ir para a lista de usuários.</li>
</ol>

<p>É isso aí! Agora você tem uma conta de "membro normal da biblioteca" que poderá usar nos testes (depois de implementarmos as páginas para permitir o login).</p>

<div class="note">
<p><strong>Nota</strong>: Você deve tentar criar outro usuário membro da biblioteca. Além disso, crie um grupo para bibliotecários e adicione um usuário a ele também!</p>
</div>

<h2 id="Configurando_suas_views_de_autenticação">Configurando suas views de autenticação</h2>

<p>O Django fornece quase tudo que você precisa para criar páginas de autenticação para lidar com o login, logout e gerenciamento de senhas "out of the box". Isso inclui um mapeador de URL, visualizações e formulários, mas não inclui os modelos - precisamos criar os nossos!</p>

<p>Nesta seção, mostramos como integrar o sistema padrão no site <em>LocalLibrary </em>e criar os modelos. Vamos colocá-los nos principais URLs do projeto.</p>

<div class="note">
<p><strong>Nota</strong>: Você não precisa usar nenhum desses códigos, mas é provável que queira, porque isso facilita muito as coisas. Você quase certamente precisará alterar o código de manipulação de formulários se alterar seu modelo de usuário (um tópico avançado!), Mas, mesmo assim, ainda poderá usar as funções padrão das views.</p>
</div>

<div class="note">
<p><strong>Nota: </strong>Nesse caso, poderíamos colocar razoavelmente as páginas de autenticação, incluindo os URLs e modelos, dentro do nosso aplicativo de catálogo. No entanto, se tivéssemos vários aplicativos, seria melhor separar esse comportamento de login compartilhado e disponibilizá-lo em todo o site, e é isso que mostramos aqui!</p>
</div>

<h3 id="URLs_do_Projeto">URLs do Projeto</h3>

<p>Adicione o seguinte à parte inferior do arquivo urls.py do projeto (<strong>locallibrary/locallibrary/urls.py</strong>):</p>

<pre class="brush: python notranslate">#Add Django site authentication urls (for login, logout, password management)
urlpatterns += [
    path('accounts/', include('django.contrib.auth.urls')),
]
</pre>

<p>Navegue até URL <a href="http://127.0.0.1:8000/accounts/">http://127.0.0.1:8000/accounts/</a> (observe a barra à direita!) e o Django mostrará um erro que não foi possível encontrar esse URL e listará todos os URLs que ele tentou. A partir disso, você pode ver os URLs que funcionarão, por exemplo:</p>

<div class="note">
<p><strong>Nota: </strong>O uso do método acima adiciona os seguintes URLs com nomes entre colchetes, que podem ser usados para reverter os mapeamentos de URL. Você não precisa implementar mais nada - o mapeamento de URL acima mapeia automaticamente os URLs mencionados abaixo.</p>

<pre class="brush: python notranslate">accounts/ login/ [name='login']
accounts/ logout/ [name='logout']
accounts/ password_change/ [name='password_change']
accounts/ password_change/done/ [name='password_change_done']
accounts/ password_reset/ [name='password_reset']
accounts/ password_reset/done/ [name='password_reset_done']
accounts/ reset/&lt;uidb64&gt;/&lt;token&gt;/ [name='password_reset_confirm']
accounts/ reset/done/ [name='password_reset_complete']</pre>
</div>

<p>Agora tente navegar para o URL de login (<a href="http://127.0.0.1:8000/accounts/login/">http://127.0.0.1:8000/accounts/login/</a>). Isso falhará novamente, mas com um erro informando que estamos perdendo o modelo necessário (<strong>registration/login.html</strong>) no caminho de pesquisa do modelo. Você verá as seguintes linhas listadas na seção amarela na parte superior:</p>

<pre class="brush: python notranslate">Exception Type:    TemplateDoesNotExist
Exception Value:    <strong>registration/login.html</strong></pre>

<p>A próxima etapa é criar um diretório de registro no caminho de pesquisa e adicionar o arquivo <strong>login.html</strong>.</p>

<h3 id="Diretório_de_Templates">Diretório de Templates</h3>

<p>Os URLs (e implicitamente, visualizações) que acabamos de adicionar esperam encontrar seus modelos associados em um diretório <strong>/registration/</strong> em algum lugar no caminho de pesquisa de modelos.</p>

<p>Neste site, colocaremos nossas páginas HTML no diretório <strong>templates/registration/</strong>. Esse diretório deve estar no diretório raiz do projeto, ou seja, o mesmo diretório que a pasta <strong>catalog</strong> e <strong>locallibrary</strong>. Por favor, crie essas pastas agora.</p>

<div class="note">
<p><strong>Nota:</strong> Sua estrutura de pastas agora deve se parecer como abaixo:<br>
 locallibrary (Django project folder)<br>
    |_catalog<br>
    |_locallibrary<br>
    |_templates <strong>(new)</strong><br>
                 |_registration</p>
</div>

<p>Para tornar esses diretórios visíveis para o carregador de modelos (ou seja, para colocar esse diretório no caminho de pesquisa de modelos), abra as configurações do projeto (<strong>/locallibrary/locallibrary/settings.py</strong>) e atualize o seção <code>TEMPLATES</code> linha <code>'DIRS'</code> como mostrado abaixo.</p>

<pre class="brush: python notranslate">TEMPLATES = [
    {
        ...
<strong>        'DIRS': [os.path.join(BASE_DIR, 'templates')],</strong>
        'APP_DIRS': True,
        ...
</pre>

<h3 id="Template_de_login">Template de login</h3>

<div class="warning">
<p><strong>Importante</strong>: Os modelos de autenticação fornecidos neste artigo são uma versão muito básica/ligeiramente modificada dos modelos de login de demonstração do Django. Pode ser necessário personalizá-los para seu próprio uso!</p>
</div>

<p>Crie um novo arquivo HTML chamado <strong>/locallibrary/templates/registration/login.html</strong> e forneça o seguinte conteúdo:</p>

<pre class="brush: html notranslate">{% extends "base_generic.html" %}

{% block content %}

  {% if form.errors %}
    &lt;p&gt;Your username and password didn't match. Please try again.&lt;/p&gt;
  {% endif %}

  {% if next %}
    {% if user.is_authenticated %}
      &lt;p&gt;Your account doesn't have access to this page. To proceed,
      please login with an account that has access.&lt;/p&gt;
    {% else %}
      &lt;p&gt;Please login to see this page.&lt;/p&gt;
    {% endif %}
  {% endif %}

  &lt;form method="post" action="{% url 'login' %}"&gt;
    {% csrf_token %}
    &lt;table&gt;
      &lt;tr&gt;
        &lt;td&gt;\{{ form.username.label_tag }}&lt;/td&gt;
        &lt;td&gt;\{{ form.username }}&lt;/td&gt;
      &lt;/tr&gt;
      &lt;tr&gt;
        &lt;td&gt;\{{ form.password.label_tag }}&lt;/td&gt;
        &lt;td&gt;\{{ form.password }}&lt;/td&gt;
      &lt;/tr&gt;
    &lt;/table&gt;
    &lt;input type="submit" value="login" /&gt;
    &lt;input type="hidden" name="next" value="\{{ next }}" /&gt;
  &lt;/form&gt;

  {# Assumes you setup the password_reset view in your URLconf #}
  &lt;p&gt;&lt;a href="{% url 'password_reset' %}"&gt;Lost password?&lt;/a&gt;&lt;/p&gt;

{% endblock %}</pre>

<p id="sect1">Este modelo compartilha algumas semelhanças com as que já vimos antes - estende nosso modelo base e substitui o bloco <code>content</code>. O restante do código é um código de manipulação de formulário bastante padrão, que discutiremos em um tutorial posterior. Por enquanto, tudo o que você precisa saber é que isso exibirá um formulário no qual é possível inserir seu nome de usuário e senha e que, se você inserir valores inválidos, será solicitado que você digite os valores corretos quando a página for atualizada.</p>

<p>Navegue de volta para a página de login (<a href="http://127.0.0.1:8000/accounts/login/">http://127.0.0.1:8000/accounts/login/</a>). Depois de salvar seu modelo, você verá algo assim:</p>

<p><img alt="Library login page v1" src="https://mdn.mozillademos.org/files/14101/library_login.png" style="border-style: solid; border-width: 1px; display: block; height: 173px; margin: 0px auto; width: 441px;"></p>

<p>Se você fizer login usando credenciais válidas, será redirecionado para outra página (por padrão, isso será <a href="http://127.0.0.1:8000/accounts/profile/">http://127.0.0.1:8000/accounts/profile/</a>). O problema é que, por padrão, o Django espera que, ao fazer o login, você deseje ser levado para uma página de perfil, o que pode ou não ser o caso. Como você ainda não definiu esta página, receberá outro erro!</p>

<p>Abra as configurações do projeto (<strong>/locallibrary/locallibrary/settings.py</strong>) e adicione o texto abaixo na parte inferior. Agora, quando você faz login, deve ser redirecionado para a página inicial do site por padrão.</p>

<pre class="brush: python notranslate"># Redirect to home URL after login (Default redirects to /accounts/profile/)
LOGIN_REDIRECT_URL = '/'
</pre>

<h3 id="Template_de_logout">Template de logout</h3>

<p>Se você navegar para o URL de logout (<a href="http://127.0.0.1:8000/accounts/logout/">http://127.0.0.1:8000/accounts/logout/</a>) você verá um comportamento estranho - seu usuário será desconectado com certeza, mas será direcionado para a pagina de logout do <strong>Admin</strong>. Não é isso que você deseja, apenas porque o link de login nessa página o leva para a tela de login do administrador (e está disponível apenas para usuários que têm a permissão <code>is_staff</code>).</p>

<p>Crie e abra /<strong>locallibrary/templates/registration/logged_out.html</strong>. Copie o texto abaixo:</p>

<pre class="brush: html notranslate">{% extends "base_generic.html" %}

{% block content %}
  &lt;p&gt;Logged out!&lt;/p&gt;
  &lt;a href="{% url 'login'%}"&gt;Click here to login again.&lt;/a&gt;
{% endblock %}</pre>

<p>Este modelo é muito simples. Ele apenas exibe uma mensagem informando que você foi desconectado e fornece um link que você pode pressionar para voltar à tela de login. Se você acessar o URL de logoff novamente, deverá ver esta página:</p>

<p><img alt="Library logout page v1" src="https://mdn.mozillademos.org/files/14103/library_logout.png" style="border-style: solid; border-width: 1px; display: block; height: 169px; margin: 0px auto; width: 385px;"></p>

<h3 id="Templates_para_reset_de_password">Templates para reset de password</h3>

<p>O sistema de redefinição de senha padrão usa o email para enviar ao usuário um link de redefinição. Você precisa criar formulários para obter o endereço de email do usuário, enviar o email, permitir que ele insira uma nova senha e anotar quando todo o processo está completo.</p>

<p>Os seguintes modelos podem ser usados como ponto de partida.</p>

<h4 id="Formulário_para_reset_de_password">Formulário para reset de password</h4>

<p>Este é o formulário usado para obter o endereço de email do usuário (para enviar o email de redefinição de senha). Crie <strong>/locallibrary/templates/registration/password_reset_form.html</strong> e forneça o seguinte conteúdo:</p>

<pre class="brush: html notranslate">{% extends "base_generic.html" %}

{% block content %}
  &lt;form action="" method="post"&gt;
  {% csrf_token %}
  {% if form.email.errors %}
    \{{ form.email.errors }}
  {% endif %}
      &lt;p&gt;\{{ form.email }}&lt;/p&gt;
    &lt;input type="submit" class="btn btn-default btn-lg" value="Reset password"&gt;
  &lt;/form&gt;
{% endblock %}
</pre>

<h4 id="Password_reset_done">Password reset done</h4>

<p>Este formulário é exibido após a coleta do seu endereço de email. Crie <strong>/locallibrary/templates/registration/password_reset_done.html</strong>, e forneça o seguinte conteúdo:</p>

<pre class="brush: html notranslate">{% extends "base_generic.html" %}

{% block content %}
  &lt;p&gt;We've emailed you instructions for setting your password. If they haven't arrived in a few minutes, check your spam folder.&lt;/p&gt;
{% endblock %}
</pre>

<h4 id="Password_reset_email">Password reset email</h4>

<p>Este modelo fornece o texto do email em HTML que contém o link de redefinição que enviaremos aos usuários. Crie<strong> /locallibrary/templates/registration/password_reset_email.html</strong> e forneça o seguinte conteúdo:</p>

<pre class="brush: html notranslate">Someone asked for password reset for email \{{ email }}. Follow the link below:
\{{ protocol}}://\{{ domain }}{% url 'password_reset_confirm' uidb64=uid token=token %}
</pre>

<h4 id="Password_reset_confirm">Password reset confirm</h4>

<p>É nesta página que você digita sua nova senha depois de clicar no link no e-mail de redefinição de senha. Crie <strong>/locallibrary/templates/registration/password_reset_confirm.html</strong> e forneça o seguinte conteúdo:</p>

<pre class="brush: html notranslate">{% extends "base_generic.html" %}

{% block content %}
    {% if validlink %}
        &lt;p&gt;Please enter (and confirm) your new password.&lt;/p&gt;
        &lt;form action="" method="post"&gt;
        {% csrf_token %}
            &lt;table&gt;
                &lt;tr&gt;
                    &lt;td&gt;\{{ form.new_password1.errors }}
                        &lt;label for="id_new_password1"&gt;New password:&lt;/label&gt;&lt;/td&gt;
                    &lt;td&gt;\{{ form.new_password1 }}&lt;/td&gt;
                &lt;/tr&gt;
                &lt;tr&gt;
                    &lt;td&gt;\{{ form.new_password2.errors }}
                        &lt;label for="id_new_password2"&gt;Confirm password:&lt;/label&gt;&lt;/td&gt;
                    &lt;td&gt;\{{ form.new_password2 }}&lt;/td&gt;
                &lt;/tr&gt;
                &lt;tr&gt;
                    &lt;td&gt;&lt;/td&gt;
                    &lt;td&gt;&lt;input type="submit" value="Change my password" /&gt;&lt;/td&gt;
                &lt;/tr&gt;
            &lt;/table&gt;
        &lt;/form&gt;
    {% else %}
        &lt;h1&gt;Password reset failed&lt;/h1&gt;
        &lt;p&gt;The password reset link was invalid, possibly because it has already been used. Please request a new password reset.&lt;/p&gt;
    {% endif %}
{% endblock %}
</pre>

<h4 id="Password_reset_complete">Password reset complete</h4>

<p>Este é o último modelo de redefinição de senha, exibido para notificá-lo quando a redefinição de senha for bem-sucedida. Crie <strong>/locallibrary/templates/registration/password_reset_complete.html</strong> e forneça o seguinte conteúdo:</p>

<pre class="brush: html notranslate">{% extends "base_generic.html" %}

{% block content %}
  &lt;h1&gt;The password has been changed!&lt;/h1&gt;
  &lt;p&gt;&lt;a href="{% url 'login' %}"&gt;log in again?&lt;/a&gt;&lt;/p&gt;
{% endblock %}</pre>

<h3 id="Testando_as_novas_páginas_de_autenticação">Testando as novas páginas de autenticação</h3>

<p>Agora que você adicionou a configuração da URL e criou todos esses modelos, as páginas de autenticação agora devem funcionar!</p>

<p>Você pode testar as novas páginas de autenticação tentando fazer login e sair da sua conta de superusuário usando estes URLs:</p>

<ul>
 <li><a href="http://127.0.0.1:8000/accounts/login/">http://127.0.0.1:8000/accounts/login/</a></li>
 <li><a href="http://127.0.0.1:8000/accounts/logout/">http://127.0.0.1:8000/accounts/logout/</a></li>
</ul>

<p>Você poderá testar a funcionalidade de redefinição de senha no link na página de login. <strong>Esteja ciente de que o Django enviará apenas emails de redefinição para endereços (usuários) que já estão armazenados em seu banco de dados!</strong></p>

<div class="note">
<p><strong>Nota</strong>: O sistema de redefinição de senha exige que seu site suporte e-mail, que está além do escopo deste artigo, portanto esta parte ainda não funcionará. Para permitir o teste, coloque a seguinte linha no final do seu arquivo settings.py. Isso registra todos os emails enviados ao console (para que você possa copiar o link de redefinição de senha do console).</p>

<pre class="brush: python notranslate">EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
</pre>

<p>Para mais informações, veja <a href="https://docs.djangoproject.com/en/2.1/topics/email/">Sending email</a> (Django docs).</p>
</div>

<h2 id="Testando_contra_usuários_autenticados">Testando contra usuários autenticados</h2>

<p>Esta seção analisa o que podemos fazer para controlar seletivamente o conteúdo que o usuário vê, com base em se está logado ou não.</p>

<h3 id="Testando_nos_templates">Testando nos templates</h3>

<p>Você pode obter informações sobre o usuário conectado no momento em modelos com a variável de template <code>\{{ user }}</code> (isso é adicionado ao contexto do template por padrão quando você configura o projeto como fizemos em nosso esqueleto).</p>

<p>Normalmente você primeiro testará contra a variável de template <code>\{{ user.is_authenticated }}</code> para determinar se o usuário está qualificado para ver conteúdo específico. Para demonstrar isso, em seguida, atualizaremos nossa barra lateral para exibir um link "Login" se o usuário estiver desconectado e um link "Logout" se estiverem conectados.</p>

<p>Abra o template base (<strong>/locallibrary/catalog/templates/base_generic.html</strong>) e copie o texto a seguir no bloco <code>sidebar</code>, imediatamente antes da template tag <code>endblock</code>.</p>

<pre class="brush: html notranslate">  &lt;ul class="sidebar-nav"&gt;

    ...

   <strong>{% if user.is_authenticated %}</strong>
     &lt;li&gt;User: <strong>\{{ user.get_username }}</strong>&lt;/li&gt;
     &lt;li&gt;&lt;a href="{% url 'logout'%}?next=\{{request.path}}"&gt;Logout&lt;/a&gt;&lt;/li&gt;
   <strong>{% else %}</strong>
     &lt;li&gt;&lt;a href="{% url 'login'%}?next=\{{request.path}}"&gt;Login&lt;/a&gt;&lt;/li&gt;
   <strong>{% endif %} </strong>
  &lt;/ul&gt;</pre>

<p>Como você pode ver, usamos template tags <code>if</code>-<code>else</code>-<code>endif</code> para exibir condicionalmente o texto com base em <code>\{{ user.is_authenticated }}</code> ser verdadeiro. Se o usuário estiver autenticado, sabemos que temos um usuário válido, por isso chamamos <code>\{{ user.get_username }}</code><strong> </strong>para exibir o nome deles.</p>

<p>Criamos os URLs dos links de logon e logout usando a template tag <code>url</code> e os nomes das respectivas configurações de URL. Observe também como anexamos <code>?next=\{{request.path}}</code> no final dos URLs. O que isso faz é adicionar um parâmetro de URL a seguir, contendo o endereço (URL) da página atual, ao final do URL vinculado. Após o usuário ter efetuado login/logout com sucesso, as visualizações usarão este valor "<code>next</code>" para redirecionar o usuário de volta à página em que ele clicou pela primeira vez no link de logon/logout.</p>

<div class="note">
<p><strong>Nota</strong>: Experimente! Se você estiver na página inicial e clicar em Login/Logout na barra lateral, depois que a operação for concluída, você deverá voltar à mesma página.</p>
</div>

<h3 id="Testando_nas_views">Testando nas views</h3>

<p>Se você estiver usando views baseadas em funções, a maneira mais fácil de restringir o acesso a suas funções é aplicando o decorator <code>login_required</code> à sua função view, como mostrado abaixo. Se o usuário estiver logado, seu código de exibição será executado normalmente. Se o usuário não estiver conectado, isso será redirecionado para o URL de login definido nas configurações do projeto.(<code>settings.LOGIN_URL</code>), passando o caminho absoluto atual como o <code>next</code> no parametro da URL. Se o usuário conseguir fazer login, ele retornará a esta página, mas desta vez autenticado.</p>

<pre class="brush: python notranslate">from django.contrib.auth.decorators import login_required

@login_required
def my_view(request):
    ...</pre>

<div class="note">
<p><strong>Nota:</strong> Você pode fazer o mesmo tipo de coisa manualmente testando em<code>request.user.is_authenticated</code>, mas o decorator é muito mais conveniente!</p>
</div>

<p>Da mesma forma, a maneira mais fácil de restringir o acesso a usuários logados em suas visualizações baseadas em classe é derivar de <code>LoginRequiredMixin</code>. Você precisa declarar esse mixin primeiro na lista de superclasses, antes da classe de visualização principal.</p>

<pre class="brush: python notranslate">from django.contrib.auth.mixins import LoginRequiredMixin

class MyView(LoginRequiredMixin, View):
    ...</pre>

<p>Isso tem exatamente o mesmo comportamento de redirecionamento que o decorator <code>login_required</code>. Você também pode especificar um local alternativo para redirecionar o usuário se ele não estiver autenticado (<code>login_url</code>), e um nome de parâmetro de URL em vez de "<code>next</code>" para inserir o caminho absoluto atual (<code>redirect_field_name</code>).</p>

<pre class="brush: python notranslate">class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'
</pre>

<p>Para detalhes adicionais, consulte o <a href="https://docs.djangoproject.com/en/2.1/topics/auth/default/#limiting-access-to-logged-in-users">Django docs here</a>.</p>

<h2 id="Exemplo_-_listando_os_livros_do_usuário_atual">Exemplo - listando os livros do usuário atual</h2>

<p>Agora que sabemos como restringir uma página a um usuário específico, vamos criar uma visualização dos livros que o usuário atual emprestou.</p>

<p>Infelizmente, ainda não temos como os usuários emprestarem livros! Portanto, antes que possamos criar a lista de livros, primeiro estenderemos o modelo <code>BookInstance</code> para suportar o conceito de empréstimo e usar o aplicativo Django Admin para emprestar vários livros ao nosso usuário de teste.</p>

<h3 id="Models">Models</h3>

<p>Primeiro, teremos que possibilitar que os usuários tenham um <code>BookInstance</code> emprestado (já temos um <code>status</code> e uma data <code>due_back</code>, mas ainda não temos nenhuma associação entre esse modelo e um usuário. Vamos criar um usando um campo <code>ForeignKey</code> (one-to-many). Também precisamos de um mecanismo fácil para testar se um livro emprestado está vencido.</p>

<p>Abra <strong>catalog/models.py</strong>, e importe o model <code>User</code> de <code>django.contrib.auth.models</code> (adicione isso logo abaixo da linha de importação anterior na parte superior do arquivo, para <code>User</code> estar disponível para o código subsequente que faz uso dele):</p>

<pre class="brush: python notranslate">from django.contrib.auth.models import User
</pre>

<p>Em seguida, adicione o campo <code>borrower</code> para o modelo <code>BookInstance</code>:</p>

<pre class="brush: python notranslate">borrower = models.ForeignKey(User, on_delete=models.SET_NULL, null=True, blank=True)
</pre>

<p>Enquanto estamos aqui, vamos adicionar uma propriedade que podemos chamar de nossos modelos para saber se uma instância específica de um livro está atrasada. Embora possamos calcular isso no próprio modelo, usando uma <a href="https://docs.python.org/3/library/functions.html#property">property</a> como mostrado abaixo será muito mais eficiente.</p>

<p>Adicione isso em algum lugar perto da parte superior do arquivo:</p>

<pre class="brush: python notranslate">from datetime import date</pre>

<p>Agora adicione a seguinte definição de propriedade a classe <code>BookInstance</code>:</p>

<pre class="brush: python notranslate">@property
def is_overdue(self):
    if self.due_back and date.today() &gt; self.due_back:
        return True
    return False</pre>

<div class="note">
<p><strong>Nota</strong>: Primeiro, verificamos se <code>due_back</code> está vazio antes de fazer uma comparação. Um campo <code>due_back</code> vazio faria com que o Django gerasse um erro em vez de mostrar a página: valores vazios não são comparáveis. Isso não é algo que gostaríamos que nossos usuários experimentassem!</p>
</div>

<p>Agora que atualizamos nossos modelos, precisaremos fazer novas migrações no projeto e aplicá-las:</p>

<pre class="brush: bash notranslate">python3 manage.py makemigrations
python3 manage.py migrate
</pre>

<h3 id="Admin">Admin</h3>

<p>Agora abra <strong>catalog/admin.py</strong>, e adicione o campo <code>borrower</code> para a classe <code>BookInstanceAdmin</code> em ambos os <code>list_display</code> e o <code>fieldsets</code> como mostrado abaixo. Isso tornará o campo visível na seção Admin, permitindo atribuir um <code>User</code> para um <code>BookInstance</code> quando necessário.</p>

<pre class="brush: python notranslate">@admin.register(BookInstance)
class BookInstanceAdmin(admin.ModelAdmin):
    list_display = ('book', 'status'<strong>, 'borrower'</strong>, 'due_back', 'id')
    list_filter = ('status', 'due_back')

    fieldsets = (
        (None, {
            'fields': ('book','imprint', 'id')
        }),
        ('Availability', {
            'fields': ('status', 'due_back'<strong>,'borrower'</strong>)
        }),
    )</pre>

<h3 id="Emprestando_alguns_livros">Emprestando alguns livros</h3>

<p>Agora que é possível emprestar livros para um usuário específico, vá e empreste vários <code>BookInstance</code>. Defina o campo <code>borrowed</code> para o usuário de teste, faça o <code>status</code> "On loan", e defina datas de vencimento no futuro e no passado.</p>

<div class="note">
<p><strong>Nota</strong>: Não detalharemos o processo, pois você já sabe como usar o site Admin!</p>
</div>

<h3 id="Na_view_loan">Na view loan</h3>

<p>Agora, adicionaremos uma view para obter a lista de todos os livros que foram emprestados ao usuário atual. Usaremos a mesma view de lista genérica baseada em classe com a qual estamos familiarizados, mas desta vez também importaremos e derivaremos de <code>LoginRequiredMixin</code>, para que apenas um usuário conectado possa chamar essa visualização. Também optaremos por declarar um <code>template_name</code>, em vez de usar o padrão, pois podemos ter algumas listas diferentes de registros BookInstance, com diferentes visualizações e modelos.</p>

<p>Adicione o seguinte a <strong>catalog/views.py</strong>:</p>

<pre class="brush: python notranslate">from django.contrib.auth.mixins import LoginRequiredMixin

class LoanedBooksByUserListView(LoginRequiredMixin,generic.ListView):
    """Generic class-based view listing books on loan to current user."""
    model = BookInstance
    template_name ='catalog/bookinstance_list_borrowed_user.html'
    paginate_by = 10

    def get_queryset(self):
        return BookInstance.objects.filter(borrower=self.request.user).filter(status__exact='o').order_by('due_back')</pre>

<p>Para restringir nossa consulta apenas ao objeto <code>BookInstance</code> para o usuário atual, reimplementamos <code>get_queryset()</code> como mostrado abaixo. Observe que "o" is the stored code for "on loan" (emprestado) e nós pedimos pela data <code>due_back</code> para que os itens mais antigos sejam exibidos primeiro.</p>

<h3 id="URL_conf_para_livros_on_loan_emprestado">URL conf para livros on loan (emprestado)</h3>

<p>Agora abra <strong>/catalog/urls.py</strong> e adicione um <code>path()</code> apontando para a visualização acima (você pode copiar o texto abaixo no final do arquivo).</p>

<pre class="brush: python notranslate">urlpatterns += [
    path('mybooks/', views.LoanedBooksByUserListView.as_view(), name='my-borrowed'),
]</pre>

<h3 id="Template_para_livros_on_loan_emprestado">Template para livros on loan (emprestado)</h3>

<p>Agora, tudo o que precisamos fazer para esta página é adicionar um modelo. Primeiro, crie o arquivo de modelo <strong>/catalog/templates/catalog/bookinstance_list_borrowed_user.html</strong> e forneça o seguinte conteúdo:</p>

<pre class="brush: python notranslate">{% extends "base_generic.html" %}

{% block content %}
    &lt;h1&gt;Borrowed books&lt;/h1&gt;

    {% if bookinstance_list %}
    &lt;ul&gt;

      {% for bookinst in bookinstance_list %}
      &lt;li class="{% if bookinst.is_overdue %}text-danger{% endif %}"&gt;
        &lt;a href="{% url 'book-detail' bookinst.book.pk %}"&gt;\{{bookinst.book.title}}&lt;/a&gt; (\{{ bookinst.due_back }})
      &lt;/li&gt;
      {% endfor %}
    &lt;/ul&gt;

    {% else %}
      &lt;p&gt;There are no books borrowed.&lt;/p&gt;
    {% endif %}
{% endblock %}</pre>

<p>Este modelo é muito semelhante ao que criamos anteriormente para os objetos <code>Book</code> e <code>Author</code>. A única coisa "nova" aqui é que verificamos o método que adicionamos no modelo <code>(bookinst.is_overdue</code>) e use-o para alterar a cor dos itens em atraso.</p>

<p>Quando o servidor de desenvolvimento estiver em execução, agora você poderá visualizar a lista de um usuário conectado no seu navegador em <a href="http://127.0.0.1:8000/catalog/mybooks/">http://127.0.0.1:8000/catalog/mybooks/</a>. Experimente isso com o usuário conectado e desconectado (no segundo caso, você deve ser redirecionado para a página de login).</p>

<h3 id="Adicione_a_lista_à_barra_lateral">Adicione a lista à barra lateral</h3>

<p>O último passo é adicionar um link para esta nova página na barra lateral. Colocaremos isso na mesma seção em que exibimos outras informações para o usuário conectado.</p>

<p>Abra o template base (<strong>/locallibrary/catalog/templates/base_generic.html</strong>) e adicione a linha em negrito à barra lateral, como mostrado.</p>

<pre class="brush: python notranslate"> &lt;ul class="sidebar-nav"&gt;
   {% if user.is_authenticated %}
   &lt;li&gt;User: \{{ user.get_username }}&lt;/li&gt;
<strong>   &lt;li&gt;&lt;a href="{% url 'my-borrowed' %}"&gt;My Borrowed&lt;/a&gt;&lt;/li&gt;</strong>
   &lt;li&gt;&lt;a href="{% url 'logout'%}?next=\{{request.path}}"&gt;Logout&lt;/a&gt;&lt;/li&gt;
   {% else %}
   &lt;li&gt;&lt;a href="{% url 'login'%}?next=\{{request.path}}"&gt;Login&lt;/a&gt;&lt;/li&gt;
   {% endif %}
 &lt;/ul&gt;
</pre>

<h3 id="Com_o_que_se_parece">Com o que se parece?</h3>

<p>Quando qualquer usuário estiver conectado, ele verá o link <em>My Borrowed</em> na barra lateral e a lista de livros exibida abaixo (o primeiro livro não tem data de vencimento, que é um bug que esperamos corrigir em um tutorial posterior!) .</p>

<p><img alt="Library - borrowed books by user" src="https://mdn.mozillademos.org/files/14105/library_borrowed_by_user.png" style="border-style: solid; border-width: 1px; display: block; height: 215px; margin: 0px auto; width: 530px;"></p>

<h2 id="Permissões">Permissões</h2>

<p>As permissões são associadas aos modelos e definem as operações que podem ser executadas em uma instância de modelo por um usuário que possui a permissão. Por padrão, o Django automaticamente fornece permissões de adição, alteração e exclusão para todos os modelos, o que permite que usuários com permissões executem as ações associadas através do site de administração. Você pode definir suas próprias permissões para modelos e concedê-las a usuários específicos. Você também pode alterar as permissões associadas a diferentes instâncias do mesmo modelo.</p>

<p>Testar permissões nas views e templates é muito semelhante ao teste no status de autenticação (e, na verdade, testar uma permissão também testa a autenticação).</p>

<h3 id="Models_2">Models</h3>

<p>A definição de permissões é feita na seção "<code>class Meta</code>" do modelo, usando o campo <code>permissions</code>. Você pode especificar quantas permissões você precisar em uma tupla, cada permissão sendo definida em uma tupla aninhada que contém o nome da permissão e o valor de exibição da permissão. Por exemplo, podemos definir uma permissão para permitir que um usuário marque que um livro foi retornado como mostrado:</p>

<pre class="brush: python notranslate">class BookInstance(models.Model):
    ...
    class Meta:
        ...
<strong>        permissions = (("can_mark_returned", "Set book as returned"),)  </strong> </pre>

<p>Poderíamos então atribuir a permissão a um grupo "Librarian" no site do administrador.</p>

<p>Abra <strong>catalog/models.py</strong>, e adicione a permissão como mostrado acima. Você ira precisar atualizar seus <em>migrations</em> (execute <code>python3 manage.py makemigrations</code> e <code>python3 manage.py migrate</code>) para atualizar o banco de dados apropriadamente.</p>

<h3 id="Templates">Templates</h3>

<p>As permissões do usuário atual são armazenadas em uma variável de modelo chamada <code>\{{ perms }}</code>. Você pode verificar se o usuário atual tem uma permissão específica usando o nome da variável específica no "aplicativo" associado ao Django — e.g. <code>\{{ perms.catalog.can_mark_returned }}</code> será <code>True</code> se o usuário tiver essa permissão, caso contrário,  <code>False</code>. Normalmente testamos a permissão usando a template tag <code>{% if %}</code> como mostrado:</p>

<pre class="brush: python notranslate">{% if perms.catalog.can_mark_returned %}
    &lt;!-- We can mark a BookInstance as returned. --&gt;
    &lt;!-- Perhaps add code to link to a "book return" view here. --&gt;
{% endif %}
</pre>

<h3 id="Views">Views</h3>

<p>As permissões podem ser testadas na exibição de funções usando o decorator <code>permission_required</code> ou em uma view baseada em classe usando o <code>PermissionRequiredMixin</code>. O padrão e o comportamento são os mesmos da autenticação de login, embora, é claro, você possa razoavelmente precisar adicionar várias permissões.</p>

<p>Função view decorator:</p>

<pre class="brush: python notranslate">from django.contrib.auth.decorators import permission_required

@permission_required('catalog.can_mark_returned')
@permission_required('catalog.can_edit')
def my_view(request):
    ...</pre>

<p>Um permission-required mixin para class-based views.</p>

<pre class="brush: python notranslate">from django.contrib.auth.mixins import PermissionRequiredMixin

class MyView(PermissionRequiredMixin, View):
    permission_required = 'catalog.can_mark_returned'
    # Or multiple permissions
    permission_required = ('catalog.can_mark_returned', 'catalog.can_edit')
    # Note that 'catalog.can_edit' is just an example
    # the catalog application doesn't have such permission!</pre>

<h3 id="Exemplo">Exemplo</h3>

<p>Não atualizaremos a <em>LocalLibrary </em>aqui; talvez no próximo tutorial!</p>

<h2 id="Desafie-se">Desafie-se</h2>

<p>No início deste artigo, mostramos como criar uma página para o usuário atual, listando os livros emprestados. O desafio agora é criar uma página semelhante que seja visível apenas para bibliotecários, que exiba <em>todos </em>os livros que foram emprestados e que inclua o nome de cada mutuário.</p>

<p>Você deve seguir o mesmo padrão da outra view. A principal diferença é que você precisará restringir a visualização apenas a bibliotecários. Você pode fazer isso com base no fato de o usuário ser um membro da equipe (decorator da função: <code>staff_member_required</code>, variável do template: <code>user.is_staff</code>) mas recomendamos que você use a permissão <code>can_mark_returned</code> e <code>PermissionRequiredMixin</code>, conforme descrito na seção anterior.</p>

<div class="warning">
<p><strong>Importante</strong>: Lembre-se de não usar seu superusuário para testes baseados em permissões (as verificações de permissão sempre retornam verdadeiras para os superusuários, mesmo que uma permissão ainda não tenha sido definida!). Em vez disso, crie um usuário bibliotecário e adicione o recurso necessário.</p>
</div>

<p>Quando terminar, sua página será semelhante à captura de tela abaixo.</p>

<p><img alt="All borrowed books, restricted to librarian" src="https://mdn.mozillademos.org/files/14115/library_borrowed_all.png" style="border-style: solid; border-width: 1px; display: block; height: 283px; margin: 0px auto; width: 500px;"></p>

<ul>
</ul>

<h2 id="Resumo">Resumo</h2>

<p>Excelente trabalho — Você criou um site no qual os membros da biblioteca podem fazer login e ver seu próprio conteúdo e que os bibliotecários (com as permissões corretas) podem usar para visualizar todos os livros emprestados e seus devedores. No momento, ainda estamos apenas visualizando conteúdo, mas os mesmos princípios e técnicas são usadas quando você deseja começar a modificar e adicionar dados.</p>

<p>Em nosso próximo artigo, veremos como você pode usar os formulários Django para coletar entradas do usuário, e então começar a modificar alguns dos nossos dados armazenados.</p>

<h2 id="Veja_também">Veja também</h2>

<ul>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/auth/">Autenticação de usuário no Django</a> (Django docs)</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/auth/default//">Usando o sistema (default) de Autenticação do Django</a> (Django docs)</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/class-based-views/intro/#decorating-class-based-views">Introdução a views baseadas em classe &gt; Decorating class-based views</a> (Django docs)</li>
</ul>

<p>{{PreviousMenuNext("Learn/Server-side/Django/Sessions", "Learn/Server-side/Django/Forms", "Learn/Server-side/Django")}}</p>

<h2 id="Neste_módulo">Neste módulo</h2>

<ul>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Introduction">Introdução ao Django</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/development_environment">Configurando um ambiente de desenvolvimento Django</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Tutorial_local_library_website">Tutorial Django: Website de uma Biblioteca Local</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/skeleton_website">Tutorial Django Parte 2: Criando a base do website</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Models">Tutorial Django Parte 3: Usando models</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Admin_site">Tutorial Django Parte 4: Django admin site</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Home_page">Tutorial Django Parte 5: Criando nossa página principal</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Generic_views">Tutorial Django Parte 6: Lista genérica e detail views</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Sessions">Tutorial Django Parte 7: Framework de Sessões</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Authentication">Tutorial Django Parte 8: Autenticação de Usuário e permissões</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Forms">Tutorial Django Parte 9: Trabalhando com formulários</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Testing">Tutorial Django Parte 10: Testando uma aplicação web Django</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Deployment">Tutorial Django Parte 11: Implantando Django em produção</a></li>
 <li><a rel="nofollow" title="A página ainda não foi criada.">Segurança de aplicações web Django</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/django_assessment_blog">DIY Django mini blog</a></li>
</ul>