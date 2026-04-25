### Todas as respostas de uma rota(urls) são com views, porém fazer views no mesmo arquivo das rotas não é eficiente e não é recomendo, então cria-se um arquivo chamado "views.py"

### "from . import views" -> puxando as funções do arquivo views da pasta "projeto"

### "from django.http import HttpResponse" (serve para enviar um texto de resposta) 

# Criamos um novo arquivo (views.py)

- Responsável por guardar nossas views (nossas respostas/response)

- Importamos o arquivo usando from . para importar a partir da pasta "projeto" evitando importar as views de outras pastas, para modularizar em pastas diferentes

# Exemplo da criação de uma rota/url de teste:

1 Passo > Primeiro argumento do path = criando a rota de teste:
- em projeto/urls.py

        urlpatterns = [
            path('teste/')
        ]

2 Passo > Criando a resposta da rota:
- em projeto/views.py

        from django.http import HttpResponse
        
            def teste_view(request):
                return HttpResponse('Essa é a rota de teste!')

3 Passo > Segundo argumento do path = qual vai ser a resposta da rota "/teste"
- em projeto/urls.py

        urlpatterns = [
            path('teste/', teste_view)
        ]

4 Passo > Views. para usar o arquivo views.py (precisa importar antes)
- em projeto/urls.py

        urlpatterns = [
            path('teste/', views.teste_view)
        ]

# Criação da rota Home/Padrão:

- Criando rota dentro de:
- urlpatterns = [
    path( )
]

1 Passo > Criando a rota:
- em projeto/urls.py

        path('', views.index_view)

2 Passo > Criando a resposta da rota:
- em projeto/views.py

        def index_view(request):
            return HttpResponse("<h1>Bem Vindo!</h1>")

# Criação de Apps:

- A estrutura do django é completamente modularizada, ou seja, iremos separar as funcionalidades por apps-  
__a pasta "projeto" é um app, o app principal do nosso projeto__

- no Terminal (CTRL + '):

        python manage.py startapp NomeDoApp

### Porém o app não é considerado após a criação, para isso devemos ir até:

- em projeto/settings.py

        INSTALLED_APPS = [
            'django.contrib.admin',
            'django.contrib.auth',
            'django.contrib.contenttypes',
            'django.contrib.sessions',
            'django.contrib.messages',
            'django.contrib.staticfiles',
        >>> 'NomeDoApp',
        ]
- e adicionamos o nosso app na lista
- Também devemos criar um "urls.py" dentro da pasta do nosso app para criar rotas relativas a ele

### Adicionando rotas no App:

- em NomeDoApp/urls.py

        from django.urls import path

        urlpatterns[

        ]

# App de Tarefas de exemplo:

- no Terminal (CTRL + '):

        python manage.py startapp tarefas      ->      (criando app)

- em projeto/settings.py

        INSTALLED_APPS = [
            'django.contrib.admin',
            'django.contrib.auth',
            'django.contrib.contenttypes',
            'django.contrib.sessions',
            'django.contrib.messages',
            'django.contrib.staticfiles',
            'tarefas',      ->      (adicionando app)
        ]

- em tarefas/urls.py

        from django.urls import path      ->      (importando módulo de rotas)

        urlpatterns[
            path("")      ->      (criando rota)
        ]

- em tarefas/views.py

        from django.http import HttpResponse

        def tarefas_home(request):      ->      (criando resposta da rota)
            return HttpResponse("Aqui estão suas tarefas:")

- em tarefas/urls.py

        from django.urls import path
        from . import views      ->      (importando views da pasta/app tarefas)

        urlpatterns[
            path("", views.tarefas_home)
        ]

# Agrupamento de Rotas:

- em projeto/urls.py

        from django.urls import path, include                       <<<<        Importação do include

        urlpatterns[
            path('tarefas/', include("tarefas.urls"))               <<<<        Adicionando as urls de tarefas
        ]

- a partir de agora tudo que estiver dentro das urls do app (tarefas) vão ser acessiveis a partir do tarefas/

# Outras Rotas do App Tarefas:

### Criando a Rota Adicionar (adicionar tarefas)

- em tarefas/urls.py

        from django.urls import path
        from . import views

        urlpatterns[
            path("", views.tarefas_home),
            path(adicionar/,)                                       <<<<        Rota Criada
        ]

### Criando resposta da Rota Adicionar

- em tarefas/views.py

        from django.http import HttpResponse

        def tarefas_home(request):
            return HttpResponse("Aqui estão suas tarefas:")

        def tarefas_adicionar(request):                             <<<<        Função da Resposta
            return HttpResponse("Adicione aqui suas tarefas:)       <<<<        Resposta

### Adicionando Resposta da Rota

- em tarefas/urls.py

        from django.urls import path
        from . import views

        urlpatterns[
            path("", views.tarefas_home),
            path(adicionar/, views.tarefas_adicionar)               <<<<        Mudanças Feitas
        ]
- Dessa forma a rota adicionar só é acessivel se a url for: tarefas/adicionar. Por causa do agrupamento.
- Agrupamento:

  - Todas as rotas que colocarmos dentro das urls do app de tarefas estão agrupadas, e o prefixo desse agrupamento é /tarefas.                
    Então tudo que a gente criar dentro das urls de tarefas só vão ser acessiveis por /tarefas

# Templates:

- É de se imaginar que o conteúdo das nossas páginas não irão ser textos simples como "Bem Vindo!".                     
Boa Noticia: É possível integrar HTML5 no Django

    - Como?

- em projeto/settings.py
        
                TEMPLATES = [
            {
                'BACKEND': 'django.template.backends.django.DjangoTemplates',
                'DIRS': [],      >>>      'DIRS': [BASE_DIR/'templates']
                'APP_DIRS': True,
                'OPTIONS': {
                    'context_processors': [
                        'django.template.context_processors.request',
                        'django.contrib.auth.context_processors.auth',
                        'django.contrib.messages.context_processors.messages',
                    ],
                },
            },
        ]

- O que isso muda?

    - _A 'templates' Dentro de 'DIRS: [ ]' é basicamente a localização onde o programa vai procurar pelos templates (templates html)_                          
    - _Ou seja: tudo que tiver pasta com nome templates, o programa irá considerar e procurar por templates html pela pasta_

# Criando Templates de exemplo:

- Criamos uma pasta na raiz do projeto(fora de todas as pastas) chamada "templates"
    - Dentro da pasta criamos outra pasta para nossas templates, nesse caso: 'tarefas'
        - Dentro da pasta tarefas criamos as templates html, nesse caso: home.html

- em templates/tarefas/home.html
- estrutura base de html com !

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Bem Vindo! Aqui estão suas tarefas</h1>      <<<<      Adicionamos o Texto da home.html
        </body>
        </html>

### Dessa forma, nada acontecerá. Porque?

- Porque na nossa views em tarefas/views.py, ele só faz retornar um texto.
- Pra fazer ele retornar o html, usaremos o render que ja vem no arquivo e faremos as seguintes mudanças:
    
  - tiramos o "return HttpResponse" da "def tarefas_home" e substituimos por:

        def tarefas_home(request):
            return render(request, '')      >>>      return render(request, 'tarefas/home.html')

- Como isso funciona?
    - Vale lembrar que colocamos a pasta 'templates' em:
        - projeto/settings.py: 'DIRS:' com [BASE_DIR/'templates'],
    - Toda template que ele for procurar será partir da pasta 'templates', e irá procurar a string que colocarmos dentro do render
        - E como a partir da pasta 'templates' temos a pasta 'tarefas', colocaremos 'tarefas', pois queremos as templates de 'tarefas'
        - e como queremos a home, será 'tarefas/home.html'
    - Dentro de render passamos o caminho do nosso template

### Também é possível passar variaveis Python para nosso HTML

- Obrigatoriamente um Dicionário ( com chaves -> {} )
- Cada item desse Dicionário será uma variavel que enviaremos para o html
- E usando o terceiro argumento de render, enviamos o context(contexto)

        def tarefas_home(request):
            contexto = {
                "nome":"Lu"
            }
            return render(request, 'tarefas/home.html', contexto)

### Como inserir a variavel no HTML?

- Colocaremos duas chaves e o nome da variavel (nome)
- em templates/tarefas/home.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Bem Vindo {{nome}}! Aqui estão suas tarefas</h1>      <<<<      Mudanças feitas
        </body>
        </html>


### Recapitulando:

- Ele só está pegando a variavel por que enviamos por meio do context(contexto) na nossa tarefas/views.py

# Redirecionando:

- Digamos que eu queira acessar a rota tarefas/adicionar, sem precisar digitar.
- Faremos o seguinte:

    - Em baixo do h1 criaremos a tag <.a href""></.a>   (os . são apenas para o comando não iniciar)

        - dentro de >< escreveremos o nome do "botão" >>> Adicionar Tarefa
        - dentro de "" vamos apontar pra qual rota enviar quando clicarmos, ou seja a rota do adicionar

    - Para mexer com rotas, com redirecionamento no Django, faremos o seguinte:

        - dentro do <a href""> colocaremos {% url '' %}

    - Colocamos %% por que estamos lidando com tags, que são basicamente comandos prontos do django que usaremos para determinadas funcionalidades
    - Na variavel pra http usamos duas chaves {{}} para colocar variavel, variavel sempre será com duas chaves.
    - Mas nesse caso colocamos só uma porque usaremos tags.
        - a tag url serve para criar um link dinâmico para a rota que iremos definir
        - mas antes devemos dizer onde a rota está, ou seja, {% url 'tarefas:' %}, e a rota será 'adicionar'. Ficando assim:

- em templates/tarefas/home.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Bem Vindo {{nome}}! Aqui estão suas tarefas</h1>
            <a href"{% url 'tarefas:adicionar' %}">Adicionar Tarefa</a>      <<<<      Mudanças feitas
        </body>
        </html>

- Isso causará um erro, qual o motivo? ele não encontrou a rota "adicionar". Para resolver isso, faremos o seguinte:

- No django, as nossas rotas podem ser nomeadas, e isso é extremamente recomendado por ser muito mais fácil e intuitivo de localizar elas, então:

- em tarefas/urls.py

        from django.urls import path
        from . import views

        urlpatterns[
            path("", views.tarefas_home),
            path(adicionar/, views.tarefas_adicionar, name="adicionar")      <<<<       Nome dado para Rota
        ]

- Só isso? Não, pois ainda falta dar um "namespace" para o nosso app (tarefas)
    - Pra que o nome das nossas rotas agrupadas sejam reconhecidos, precisamos dar um "appname":

- em tarefas/urls.py

        from django.urls import path
        from . import views

        app_name = "tarefas"      <<<<      appname criado

        urlpatterns[
            path("", views.tarefas_home),
            path(adicionar/, views.tarefas_adicionar, name="adicionar")
        ]

# Models:

- Estamos lidando com Django, Django é um framework backend, e no backend nós vamos mexer com banco de dados.
    - Banco de Dados = Basicamente é um sistema que vai permitir que guardemos informações dos nossos usuários
- Para mexer com banco de dados no django, iremos até a pasta models.py do nosso app

- Os models são basicamente classes que criamos, e essas classes serão traduzidas em modelos no banco de dados, então não precisaremos mexer com sql, tudo será gerenciavel pelo framework (Django)
    - Django traz muitas ferramentas prontas, então só precisaremos manusear e usar do nosso jeito.

### Criando Models de exemplo:

- Como nosso app é para Tarefas, a primeira coisa que faremos é criar um model para nossas tarefas, ou seja um modelo que represente uma tarefa, coisas que uma tarefa tem basicamente.

- em tarefas/models.py

        from django.db import models

        class TarefaModel(models.Model):
            nome = models.CharField(max_length=100)
            descricao = models.TextField(null=True, blank=True)
            completo = models.BooleanField(default=False)
            data_criacao = models.DateTimeField(auto_now_add=True)
            
            def __str__(self):
                return self.nome

- Como isso funciona?
    - Criamos uma classe para nosso banco de dados de acordo com o nosso app
        - Essa classe precisa herdar de models.Model (que importamos no inicio do código)
    - Feito isso. Criaremos os campos do nosso modelo, todas as propriedades que ele tem, as particularidades
    - Sendo eles:
        - Nome (afinal toda tarefa precisa de um nome kk)
            - nome = models.CharField(max_length=100)
                - CharField é basicamente uma string, e (max_length=100) para definir o limite de caracteres em 100
        - Descrição
            - descricao = models.TextField(null=True, blank=True)
                - TextField para textos mais longos do que apenas um nome, (null=True e blank=True) para que a descrição seja opcional, pode ser tanto null(preenchida) quanto blank(vazia)
        - Completo (pois tarefas podem ser completadas ou não, e esse campo é o que vai determinar se foi ou não)
            - completo = models.BooleanField(default=False)
                - BooleanField segue a mesma lógica de Booleanos em Python (True ou False) e (default=False) determina que o padrão é a tarefa estar incompleta
        - Data de Criação
            - data_criacao = models.DateTimeField(auto_now_add=True)
                - DateTimeField para um campo de data e (auto_now_add=True) para que a partir do momento em que nosso registro seja criado ele já registre uma data de criação automaticamente = a data que o modelo for criado será a data que irá inserar em data_criacao
        
        - Também criaremos um método dentro da nossa classe (opcional)
            - def __str__(self):
                return self.nome
                - Se tivermos uma instância dessa classe, se tiver um registro em nossa mãos e dermos um print nesse registro, essa função "__str__" será executada, retornando o nome da tarefa
                - Caso dermos print em uma lista de objetos, mostrará o nome de cada um.

#### Por padrão Django usará o Banco de Dados sqlite que é um banco de dados bem simples que funciona em um único arquivo só para testes de desenvolvimento

### Por quê não tem nada no meu Banco de Dados?

- no Django e em qualquer outro framework basicamente, iremos mexer com o que chamamos de "migrações" as migrations.

# Migrações:

- Pode ser que no desenvolvimento do seu banco de dados, se você fizer alterações ao longo do tempo (você vai fazer) você vai precisar de um sistema para versionar essas alterações, ou seja se você faz um modelo de banco de dados, atualiza o banco de dados e faz besteira, esse versionamento vai permitir que você volte atrás, sem que você precise criar tudo do zero, mantendo os seus dados, e isso é feito através de migrações

# Criação de Migrações:

- no Terminal (CTRL + '):

        python manage.py makemigrations

- Ainda não está tudo pronto porque devemos aplicar nossa migração

- no Terminal (CTRL + '):

        python manage.py migrate

# Criação de CRUD:

### Adicionar Tarefas

- No Contexto de Tarefas
- remover o return da "def tarefas_adicionar" e adicionamos render assim como em "def tarefas_home"
- em tarefas/views.py

        from django.shortcuts import render
        from django.http import HttpResponse

        def tarefas_home(request):
            contexto = {
                "nome":"Lu"
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request):
            return render(request, 'tarefas/adicionar.html')

- a rota tarefas/adicionar.html ainda não existe e não colocaremos nenhum contexto por hora, a seguir criaremos adicionar.html em templates/tarefas

- em templates/tarefas/adicionar.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Adicione suas tarefas aqui!</h1>      <<<<      Adicionamos o Texto da adicionar.html
        </body>
        </html>

# Formulários com Django:

- Criaremos um arquivo na pasta tarefas (fora de templates)
    - o arquivo será >>> forms.py

- em tarefas/forms.py

        from .models import TarefaModel                             <<<<      Imporação do Model
        from django import forms                                    <<<<      Importação do forms

        class TarefasForm(forms.ModelForm):                         <<<<      Classe do formulario
            class Meta:
                model = TarefaModel                                 <<<<      Primeira Propriedade
                fields = ['nome', 'descricao', 'completo']          <<<<      Segunda Propriedade

- Como isso funciona?
    - Faremos a importação de forms e criaremos uma classe para nosso formulario e adicionaremos (forms.ModelForm)
        - ModelForm é basicamente uma interface de criação de formulário que o django oferece, onde conseguimos criar formularios automaticamente a partir de um modelo(model) que será o modelo que criamos em (tarefas/models.py)
    - Dentro da nossa classe criaremos outra classe, a classe Meta, que é uma classe de configuração para o nosso formulario
        - E dentro da classe Meta passaremos duas propriedades:
            - o Primeiro é o Modelo que ele irá se referenciar, mas para isso precisamos importar nosso modelo antes. Para isso usamos "from .models" ou seja, a partir dessa pasta (tarefas) ele vai pegar o arquivo models e importar o TarefaModel.
                - model = TarefaModel
            - a Segunda vai servir para dizermos para nosso formulario quais campos do model ele vai considerar na hora de passar para HTML, ou seja:
                - fields = ['nome', 'descricao', 'completo']
                    - Porque não passamos a data de criação? (data_criacao)
                    - R: porque ele ja tem auto_now_add, que adiciona automaticamente


- Voltamos para views

  - importamos nosso formulrio com 'from .forms import TarefaForm'

- em tarefas/views.py

        from django.shortcuts import render
        from .forms import TarefaForm

        def tarefas_home(request):
            contexto = {
                "nome":"Lu"
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request):
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)

- Como isso funciona?
    - Damos um Contexto para tarefas_adicionar
        - dentro do contexto(dicionario) passamos o item:
        - "form":TarefaForm
            - Ou seja vamos passar nosso formulario diretamente pra HTML (adicionar.html)
    - Removemos HttpResponse e importamos forms
        - from .forms import TarefaForm

### Voltamos para Adicionar Tarefas

- Agora a rota "tarefas/adicionar.html" existe e colocamos nosso formulario como contexto nela, a seguir adicionaremos nosso formulario na template adicionar.html

- em templates/tarefas/adicionar.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Adicione suas tarefas aqui!</h1>-
            <form action="" method="post">              <<<<      Adicionamos nosso Formulario como metodo POST(Enviar Dados para Criar ou Alterar Recursos)
                {{form}}                                <<<<      Passamos a variavel do formulario  
                <input type="submit" value="Enviar">    <<<<      Criamos um Botão de Enviar
            </form>
        </body>
        </html>

- Dessa forma dará um erro, porque o Django, como é um framework ele também lida com segurança, e para os formularios funcionarem precisamos adicionar a tag csrf_token.                                             
    - ( lembrando que tags usam {% %} )

- em templates/tarefas/adicionar.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Adicione suas tarefas aqui!</h1>-
            <form action="" method="post">
                {% csrf_token %}                        <<<<      Adicionando a Tag
                {{form}}
                <input type="submit" value="Enviar">
            </form>
        </body>
        </html>

### Funciona mas não faz nada, Porque?

- Por que não foi programado para fazer nada, apenas criamos o formulario e fizemos enviar, sem dizer o que fará com o formulário.
- Dito isso, agora vem a lógica de inserção de dados
    - Quando apertamos em Enviar o HTML redireciona automaticamente pra onde está o action de <form action"">
- Como está vazio, ele redireciona para nenhum lugar

### Solução:

- em tarefas/views.py

        from django.shortcuts import render
        from .forms import TarefaForm
        from django.http import HttpRequest             <<<<      Importamos Request


        def tarefas_home(request):
            contexto = {
                "nome":"Lu"
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request:HttpRequest):     <<<<      Tipamos o request
            if request.method == "POST":                <<<<      Condição para o tratamento
                formulario = TarefaForm(request.POST)   <<<<      Tratamento
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)

- Como isso funciona?
    - Antes de mais nada vamos tipar o request do tarefas_adicionar
    - As requisições tem varios tipos, o padrão é GET, quando acessamos um site o padrão de requisição que estamos fazendo é GET. Quando enviamos um formulário o tipo de requisição muda para POST, porque estamos postando/enviando alguma coisa
        - Se o método do request for igual a POST (quer dizer que está enviando dados)
            if request.method == "POST":
    - Faremos um tratamento para ele cuidar desses dados, para salvar esses dados
        - Criaremos uma variavel para o formulario, a variavel vai receber o formulario de tarefas (TarefaForm) e entre parenteses vamos passar os dados do nosso formulario, para obter esses dados é bem simples, usaremos request.POST
            formulario = TarefaForm(request.POST)
    - Vale lembrar que request são todas as informações da nossa requisição / do nosso acesso, é basicamente isso que ele fará, os dados do formulário estão inclusos no request a partir do POST, estamos criando uma instância do nosso formulário (por assim dizer), que contenha as informações que enviamos para ele.

- Agora faremos o seguinte:

- em tarefas/views.py

        from django.shortcuts import render
        from .forms import TarefaForm
        from django.http import HttpRequest


        def tarefas_home(request):
            contexto = {
                "nome":"Lu"
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request:HttpRequest):
            if request.method == "POST":
                formulario = TarefaForm(request.POST)
                if formulario.is_valid():               <<<<      Condição (se o formulario for Válido:)
                    formulario.save()                   <<<<      Salva as Informações
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)

- Resumo do que foi feito:
  - 1 Passo: Pegamos as informações e trazemos para o nosso formulario (variavel)

        if request.method == "POST":
            formulario = TarefaForm(request.POST)

  - 2 Passo: Verificamos se ele é válido, se as informações estão em conformidades com as nossas regras

  -     if formulario.is_valid():

  - 3 Passo: Salvamos as informações no banco de dados devidamente

  -        formulario.save()

- Queremos que após salvar, ele redirecione para página inicial
    - Faremos o seguinte:

- em tarefas/views.py

        from django.shortcuts import render, redirect   <<<<      Importando Redirect
        from .forms import TarefaForm
        from django.http import HttpRequest


        def tarefas_home(request):
            contexto = {
                "nome":"Lu"
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request:HttpRequest):
            if request.method == "POST":
                formulario = TarefaForm(request.POST)
                if formulario.is_valid():
                    formulario.save()
                    return redirect("tarefas:home")     <<<<      Redirect  
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)

- return redirect"" fará a mesma regra que fizemos no HTML, ou seja, o nome do app e depois a rota
    - a rota da página inicial ainda não tem nome, então após isso:
- em tarefas/urls.py

        from django.urls import path
        from . import views

        app_name = "tarefas"

        urlpatterns[
            path("", views.tarefas_home, name="home"),      <<<     Nome Adicionado
            path(adicionar/, views.tarefas_adicionar, name="adicionar")
        ]

# Listando Objetos

- Precisamos enviar as nossas tarefas que ja estão no banco de dados para o nosso template "home.html"
    - Devemos importar o TarefaModel e adicionarmos o item:
    - "tarefas":TarefaModel.objects.all() no nosso dicionario "contexto"
        - .all() irá trazer todos os registros de tarefas para nossa chave de tarefas, nosso contexto

- em tarefas/views.py

        from django.shortcuts import render, redirect
        from .forms import TarefaForm
        from .models import TarefaModel                 <<<<      Importando Model
        from django.http import HttpRequest


        def tarefas_home(request):
            contexto = {
                "nome":"Lu",
                "tarefas":TarefaModel.objects.all()     <<<<      Usando o Model
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request:HttpRequest):
            if request.method == "POST":
                formulario = TarefaForm(request.POST)
                if formulario.is_valid():
                    formulario.save()
                    return redirect("tarefas:home")
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)

- Feito isso iremos mostrar nossas tarefas da seguinte maneira:

- em templates/tarefas/home.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Bem Vindo {{nome}}! Aqui estão suas tarefas</h1>
            <table border="1">                                              <<<< Começo da Tabela
                <thead>                                                     <<<< Começo da Cabeça da Tabela
                    <th>Nome</th>                                           <<<< Campo na Cabeça com Nome
                    <th>Descrição</th>                                      <<<< Campo na Cabeça com Descrição
                    <th>Situação</th>                                       <<<< Campo na Cabeça com Situação
                    <th>Data de Criação</th>                                <<<< Campo na Cabeça com Data
                </thead>                                                    <<<< Fim da Cabeça
                <tbody>                                                     <<<< Corpo da Tabela
                    {% for tarefa in tarefas %}                             <<<< Começo do For
                        <tr>                                                <<<< Começo dos campos para cada Tarefa
                            <td>{{tarefa.nome}}</td>                        <<<< Campo com dado nome para cada Tarefa
                            <td>{{tarefa.descricao}}</td>                   <<<< Campo com dado descricao para cada Tarefa
                            <td>{{tarefa.completo}}</td>                    <<<< Campo com dado completo para cada Tarefa
                            <td>{{tarefa.data_criacao}}</td>                <<<< Campo com dado data para cada Tarefa
                        </tr>                                               <<<< Fim dos campos para cada Tarefa
                    {% endfor %}                                            <<<< Fim do For
                </tbody>                                                    <<<< Fim do Corpo da Tabela
            </table>                                                        <<<< Fim da Tabela
            <a href"{% url 'tarefas:adicionar' %}">Adicionar Tarefa</a>
        </body>
        </html>

- Como isso funciona?

    - Criamos uma tabela "<.table>" em baixo do <.h1> em home.html para ficar visualmente apresentável
    - (não é obrigatório, tabela é puramente do HTML, só para ficar mais facil de entender e visualizar, mas também poderiamos usar paragrafos <.p> ou coisa do tipo)
      - Dentro da tabela criamos um "<.thead>" e dentro dele criamos quatro campos "<.th>":
        - Nome, Descrição, Situação e Data de Criação
    - Após os 4 campos, no corpo da nossa table "<.tbody>" usamos um for
      - for é uma tag, então devemos usar {% %} e para o fim do for usamos endfor
    - Para cada tarefa em tarefas ele irá criar uma linha com um dado ( dados são variaveis então usamos {{ }} )
        - linha == <.tr>
        - dado == <.td>
    - Nesse caso cada linha terá um Nome, uma Descrição, se está Completo ou Incompleto e a Data de Criação
- _Opcional: no começo da tabela <.table> é possivel adicionar bordas com <.table border="1">_
- _OBS: estará em inglês e com horário de UTC, para mudar basta ir em projeto/settings.py e procurar por:_
  - LANGUAGE_CODE = 'en-us' -> 'pt-br'
  
  - TIME_ZONE = 'UTC' -> 'America/Sao_Paulo'

### Vale lembrar que o tarefas dentro do for é o mesmo que enviamos por meio do contexto:

- em tarefas/views.py

        def tarefas_home(request):
            contexto = {
                "nome":"Lu",
                "tarefas":TarefaModel.objects.all()     <<<<      Enviando "Tarefas" por meio do contexto
            }
            return render(request, 'tarefas/home.html', contexto)

# Removendo Objetos

- Criaremos uma rota para Remover
- em tarefas/urls.py

        from django.urls import path
        from . import views

        app_name = "tarefas"

        urlpatterns = [
            path("", views.tarefas_home, name="home")
            path("adicionar/", views.tarefas_adicionar, name="adicionar")
            path("remover/<int:id>", views.tarefas_remover, name="remover") <<<<
        ]

- Quando a gente for remover um item, precisamos informar qual item queremos remover, e o que usaremos para determinar isso é o id
    - id é um campo que tem no banco de dados que representa seu item, cada item, cada registro terá um id diferente, um id único e com esse id iremos dizer qual item vamos remover:
  - remover/<.int:id> 

- Feito isso iremos criar a resposta dessa rota com o segundo argumento do id
- em tarefas/views.py

        from django.shortcuts import render, redirect
        from .forms import TarefaForm
        from .models import TarefaModel
        from django.http import HttpRequest


        def tarefas_home(request):
            contexto = {
                "nome":"Lu",
                "tarefas":TarefaModel.objects.all()
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request:HttpRequest):
            if request.method == "POST":
                formulario = TarefaForm(request.POST)
                if formulario.is_valid():
                    formulario.save()
                    return redirect("tarefas:home")
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)
        
        def tarefas_remover(request:HttpRequest, id):             <<<<
            ...

- Por hora deixamos isso de lado, vamos configurar o botão para redirecionar

### Adicionar o Botão para Remover na Página Inicial

- Dentro do nosso for, afinal, queremos um botão em cada tarefa, adicionaremos um <.a href=""><./a> e <.td> para não ficar solto(em cima da tabela)
  - O nome do botão dentro de >< e a rota para remover dentro do href "" (com a tag url) mas também precisaremos passar o id
- em templates/tarefas/home.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Bem Vindo {{nome}}! Aqui estão suas tarefas</h1>
            <table border="1">                                             
                <thead>                                                   
                    <th>Nome</th>                                          
                    <th>Descrição</th>                                    
                    <th>Situação</th>                                   
                    <th>Data de Criação</th>                   
                </thead>                                             
                <tbody>                                          
                    {% for tarefa in tarefas %}                             
                        <tr>                                                
                            <td>{{tarefa.nome}}</td>                       
                            <td>{{tarefa.descricao}}</td>                   
                            <td>{{tarefa.completo}}</td>                    
                            <td>{{tarefa.data_criacao}}</td>    
                            <td><a href="{% url 'tarefas:remover' tarefa:id %}">Remover</a></td>    <<<<        
                        </tr>                                               
                    {% endfor %}                                            
                </tbody>                                                    
            </table>                                                        
            <a href"{% url 'tarefas:adicionar' %}">Adicionar Tarefa</a>
        </body>
        </html

- Resumindo: ele irá redirecionar para a rota remover com o id da tarefa

### Voltando para criação da rota

- Vamos importar o "get_object_or_404" e adicionar no metodo tarefas_remover

  - Ele irá tentar obter um item do nosso banco de dados e se ele não encontrar dará o erro 404
  - Primeiro informamos de onde ele irá tentar tirar o item "TarefaModel" e depois o item "id"

- em tarefas/views.py

        from django.shortcuts import render, redirect, get_object_or_404       <<<<       Importando
        from .forms import TarefaForm
        from .models import TarefaModel
        from django.http import HttpRequest


        def tarefas_home(request):
            contexto = {
                "nome":"Lu",
                "tarefas":TarefaModel.objects.all()
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request:HttpRequest):
            if request.method == "POST":
                formulario = TarefaForm(request.POST)
                if formulario.is_valid():
                    formulario.save()
                    return redirect("tarefas:home")
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)
        
        def tarefas_remover(request:HttpRequest, id):
            tarefa = get_object_or_404(TarefaModel, id=id)       <<<<
            tarefa.delete()
            return redirect("tarefas":home)

- tarefa = get_object_or_404(TarefaModel, id=id)
  - Ele vai tentar procurar na tabela de tarefas algum item que tenha o id igual ao id que estaremos passando para ele ao clicar no botão, o id do item que queremos exluir, se ele encontrar tudo certo, se não, vai dar o erro 404
- tarefa.delete()
  - Irá deletar se encontrar o item com o id que pedirmos
- return redirect("tarefas":home)
  - Após a exclusão irá redirecionar para a página inicial

# Editando Objetos

- Inicialmente faremos a mesma coisa que fizemos para remover

    - Adicionaremos um <.td> com <.a href=""><./a><./td>
    - Novamente, o nome do botão dentro do >< do href e a rota para remover dentro do href "" (com a tag url) e novamente precisaremos passar o id

- em templates/tarefas/home.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Bem Vindo {{nome}}! Aqui estão suas tarefas</h1>
            <table border="1">                                             
                <thead>                                                   
                    <th>Nome</th>                                          
                    <th>Descrição</th>                                    
                    <th>Situação</th>                                   
                    <th>Data de Criação</th>                   
                </thead>                                             
                <tbody>                                          
                    {% for tarefa in tarefas %}                             
                        <tr>                                                
                            <td>{{tarefa.nome}}</td>                       
                            <td>{{tarefa.descricao}}</td>                   
                            <td>{{tarefa.completo}}</td>                    
                            <td>{{tarefa.data_criacao}}</td>
                            <td><a href="{% url 'tarefas:editar' tarefa:id %}">Editar</a></td>    <<<<
                            <td><a href="{% url 'tarefas:remover' tarefa:id %}">Remover</a></td>       
                        </tr>                                               
                    {% endfor %}                                            
                </tbody>                                                    
            </table>                                                        
            <a href"{% url 'tarefas:adicionar' %}">Adicionar Tarefa</a>
        </body>
        </html

- Resumindo: ele irá redirecionar para a rota editar com o id da tarefa
    - Agora criaremos a rota editar, ja que assim ele irá redirecionar para uma página não existente

- em tarefas/urls.py

        from django.urls import path
        from . import views

        app_name = "tarefas"

        urlpatterns = [
            path("", views.tarefas_home, name="home")
            path("adicionar/", views.tarefas_adicionar, name="adicionar")
            path("remover/<int:id>", views.tarefas_remover, name="remover")
            path("editar/<int:id>", views.tarefas_editar, name="editar")        <<<<
        ]

- Agora criaremos a template da rota editar, copiando e colando o adicionar.html pois só precisaremos mudar algumas coisas

- em templates/tarefas/editar.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Editar Tarefa</h1>          <<<<
            <form action="" method="POST">
                {% csrf_token %}
                {{form}}
                <input type="submit" value="Enviar"
            </form>
        </body>
        </html>

- A seguir criaremos nossa resposta da rota, nossa view editar

- em tarefas/views.py

        from django.shortcuts import render, redirect, get_object_or_404
        from .forms import TarefaForm
        from .models import TarefaModel
        from django.http import HttpRequest


        def tarefas_home(request):
            contexto = {
                "nome":"Lu",
                "tarefas":TarefaModel.objects.all()
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request:HttpRequest):
            if request.method == "POST":
                formulario = TarefaForm(request.POST)
                if formulario.is_valid():
                    formulario.save()
                    return redirect("tarefas:home")
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)
        
        def tarefas_remover(request:HttpRequest, id):
            tarefa = get_object_or_404(TarefaModel, id=id)
            tarefa.delete()
            return redirect("tarefas":home)

        def tarefas_editar(request:HttpRequest, id):                <<<<
            tarefa = get_object_or_404(TarefaModel, id=id)          <<<<
            formulario = TarefaFrom(instance=tarefa)                <<<<
            context = {                                             <<<<
                'form':formulario                                   <<<<
            }                                                       <<<<
            return render(request, 'tarefas/editar.html', context)  <<<<

- Como isso funciona?
  - Fizemos igual foi com remover
    - pegamos o objeto da tarefa com get_object_or_404 e passamos o argumento TarefaModel, porque queremos um item do banco das tarefas, junto com o id usando id=id
  - Apos pegar o objeto ele irá criar um formulario usando TarefaForm, instânciando as tarefas
    - Ou seja: iremos criar um formulario novo porem esse formulario ja estará com os dados inseridos, e esses dados inseridos estão vindo da nossa tarefa que pegamos com id
  - A seguir pegaremos o context(contexto) que será nosso formulario e iremos passar ele para html

- Dessa forma, quando clicarmos em editar, ele ja irá trazer as informações automaticamente, da nossa tarefa no caso, isso porque passamos a instância dessas informações para o "def tarefas_editar" com "formulario = TarefaForm(instance=tarefa)", sendo assim, o formulario que ele envia para o html ja está com as informações para editarmos

- A Seguir faremos a mesma coisa que fizemos para adicionar:

- em tarefas/views.py

        from django.shortcuts import render, redirect, get_object_or_404
        from .forms import TarefaForm
        from .models import TarefaModel
        from django.http import HttpRequest


        def tarefas_home(request):
            contexto = {
                "nome":"Lu",
                "tarefas":TarefaModel.objects.all()
            }
            return render(request, 'tarefas/home.html', contexto)

        def tarefas_adicionar(request:HttpRequest):
            if request.method == "POST":
                formulario = TarefaForm(request.POST)
                if formulario.is_valid():
                    formulario.save()
                    return redirect("tarefas:home")
            contexto = {
                "form":TarefaForm
            }
            return render(request, 'tarefas/adicionar.html', contexto)
        
        def tarefas_remover(request:HttpRequest, id):
            tarefa = get_object_or_404(TarefaModel, id=id)
            tarefa.delete()
            return redirect("tarefas":home)

        def tarefas_editar(request:HttpRequest, id):             
            tarefa = get_object_or_404(TarefaModel, id=id)
            if request.method == "POST"                                         <<<<
                formulario = TarefaForm(request.POST, instance=tarefa)          <<<<
                if formulario.is_valid():                                       <<<<
                    formulario.save()                                           <<<<
                    return render(request, 'tarefas:home')                      <<<<
            formulario = TarefaFrom(instance=tarefa)               
            context = {                                        
                'form':formulario                                
            }                                                      
            return render(request, 'tarefas/editar.html', context)    

- Como isso funciona?

-       if request.method == "POST"

    - Se ele ja tiver inserido as informaçoes enviadas, ele irá fazer o processo a seguir:
- 
        formulario = TarefaForm(request.POST, instance=tarefa)

    - Criando outro formulario porém com 2 argumentos:
     - O primeiro sendo data, ou seja = request.POST (que são os dados que o usuário inseriu para atualizar) 
     - O segundo sendo instance, para dizermos para ele qual dado ele está representando (que é o dado que queremos editar, ou seja, tarefa)
  - A seguir faremos a mesma validação que fizemos com adicionar
    
-       if formulario.is_valid()                                       
            formulario.save()
    
    - Se for válido ele salvará as informações

-       return render(request, 'tarefas:home')
    
    - Redireciona para home

- Para Resumir: a nossa rota de editar funciona da seguinte maneira: ele acessa, envia o id por meio do url, e esse id é enviado para o parâmetro id em "def tarefas_editar(request:HttpRequest, __id__):"
  - Após isso ele irá tentar obter o objeto da tarefa por meio do id, e se não encontrar dará o erro 404 (Not Found) caso contrário ele irá pegar o formulário, vai enviar para html para nós conseguirmos editar, quando editarmos e apertamos em enviar, ele vai voltar para a mesma view "tarefas = get_object_or_404" mas irá cair no if, o if request.method == "POST"
    - um método é POST quando enviamos dados de um formulario
  - Quando for POST, ele criará um formulario "novo" com 2 argumentos usando "formulario = TarefaForm(request.POST, instance=tarefa)", sendo eles os dados que ele enviou para o formulario, e a instância de tarefas que ele estará modificando, e se esse formulario for válido, se os dados que ele inseriu forem válidos, se estiver em conformidade com as nossas regras, ele vai salvar o formulario, ou seja vai salvar no banco de dados as informações alteradas e vai redirecionar para nossa rota principal, a home

# Finalizando com um toque para deixar mais bonito

- Do jeito que está em situação ele irá exibir True ou False, para deixar mais bonito iremos fazer o seguinte:
  - Colocamos chaves com a tag if tarefa.completo e a mensagem que exibira caso seja True, e a seguir adicionamos a tag else para exibir a mensagem caso seja False 

- em templates/tarefas/home.html

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
            <h1>Bem Vindo {{nome}}! Aqui estão suas tarefas</h1>
            <table border="1">                                             
                <thead>                                                   
                    <th>Nome</th>                                          
                    <th>Descrição</th>                                    
                    <th>Situação</th>                                   
                    <th>Data de Criação</th>                   
                </thead>                                             
                <tbody>                                          
                    {% for tarefa in tarefas %}                             
                        <tr>                                                
                            <td>{{tarefa.nome}}</td>                       
                            <td>{{tarefa.descricao}}</td>                   
                            <td>{% if tarefa.completo %}Completo{% else %}Incompleto{% endif %}</td>            <<<<        
                            <td>{{tarefa.data_criacao}}</td>
                            <td><a href="{% url 'tarefas:editar' tarefa:id %}">Editar</a></td>
                            <td><a href="{% url 'tarefas:remover' tarefa:id %}">Remover</a></td>       
                        </tr>                                               
                    {% endfor %}                                            
                </tbody>                                                    
            </table>                                                        
            <a href"{% url 'tarefas:adicionar' %}">Adicionar Tarefa</a>
        </body>
        </html
