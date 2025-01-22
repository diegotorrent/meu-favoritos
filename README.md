###### README.md - Brazilian Portuguese (PT-BR)
##### Author: Diego Ferry Torrent
##### [**Linkedin**: https://www.linkedin.com/in/dft/](https://www.linkedin.com/in/dft/)
###### Creation Date: 2025-01-20
----

# meu-favoritos

1. **Visão Geral do Sistema**

    A aplicação será um sistema simples de favoritos, onde o usuário se autentica via Google, armazena seus links favoritos e pode visualizá-los de maneira personalizada. A hospedagem será feita no Firebase Hosting e a persistência dos dados no Firebase Realtime Database.

1. **Arquitetura do Sistema**

    A arquitetura é centrada em três componentes principais:

    * Frontend (UI): Interface do usuário construída com HTML, CSS e JavaScript.  
    * Autenticação (Firebase Auth): Responsável pela autenticação do usuário via Google.
    * Backend (Firebase Realtime Database): Armazenamento dos dados de favoritos dos usuários, utilizando o email como índice único.

    **Componentes e suas responsabilidades**

    * Frontend (HTML/CSS/JS):
      > Apresentar uma interface limpa e simples.
      > 
      > Exibir mensagem de boas-vindas após autenticação.
      > 
      > Listar os favoritos salvos.
      > 
      > Permitir ao usuário adicionar novos favoritos (nome, URL).
      > 
      > Responsável por interagir com a Firebase Auth para realizar o login do usuário e a Firebase Realtime Database para salvar e recuperar favoritos.

    * Firebase Authentication:
      > Gerenciar a autenticação via Google.
      > 
      > Após o login, gerar um token de identificação (ID do usuário) que será usado como chave para o Firebase Realtime Database.
      > 

    * Firebase Realtime Database:
      > Armazenar dados de favoritos do usuário.
      > 
      > A chave principal será o email ou uid do usuário, de forma que cada usuário tenha acesso somente aos seus próprios dados.
      > 
      ```
        {
          "users": {
            "user_email_or_uid": {
              "bookmarks": {
                "bookmark_id": {
                  "title": "My Favorite Website",
                  "url": "https://www.example.com"
                }
              }
            }
          }
        }
      ```

1. **Fluxo de Trabalho da Aplicação**

    **Fluxo 1: Acesso ao Sistema**

    1. O usuário acessa a página inicial.
    1. O sistema verifica se o usuário está autenticado.
        * Se o usuário não estiver autenticado, ele verá um botão de "Login com Google".
        * Se o usuário estiver autenticado, o sistema irá carregar a página com a lista de favoritos e uma mensagem de boas-vindas personalizada (usando o nome ou email do usuário).

    **Fluxo 2: Autenticação**

    1. O usuário clica no botão "Login com Google".
    1. O Firebase Auth solicita permissão para autenticar via conta Google.
    1. Após a autenticação, o Firebase Auth retorna os dados do usuário (ex.: nome, email, foto de perfil).
    1. O sistema grava o uid do usuário (que é único para cada conta Google) para realizar operações de leitura/escrita no Realtime Database.

    **Fluxo 3: Exibição dos Favoritos**

    1. O sistema busca no Firebase Realtime Database os favoritos do usuário (utilizando seu uid ou email).
    1. Exibe os favoritos em formato de lista.
    1. Caso o usuário não tenha favoritos, exibe uma mensagem como "Nenhum favorito encontrado".
    
    **Fluxo 4: Adicionar Novo Favorito**

    1. O usuário pode adicionar um novo favorito através de um formulário simples.
    1. O formulário solicita o título e a URL do favorito.
    1. Ao enviar o formulário, os dados são armazenados no Firebase Realtime Database na coleção do usuário.

    **Fluxo 5: Logout**

    1. O usuário pode fazer logout clicando em um botão de "Logout".
    1. Ao fazer logout, o sistema deve limpar qualquer informação da sessão do usuário e redirecionar para a tela de login.

1. **Estrutura de Dados do Firebase Realtime Database**

    A estrutura sugerida para armazenar os favoritos seria organizada da seguinte maneira:
    ```
    {
      "users": {
        "user_email_or_uid": {
          "bookmarks": {
            "bookmark_id_1": {
              "title": "My Favorite Site",
              "url": "https://www.example.com"
            },
            "bookmark_id_2": {
              "title": "Another Site",
              "url": "https://www.another.com"
            }
          }
        }
      }
    }
    ```
    
    * users: Coleção que armazena os dados dos usuários. A chave para cada usuário será o email ou uid (ID gerado pelo Firebase Auth).
    * bookmarks: Subcoleção de cada usuário, onde os favoritos serão armazenados.
    * bookmark_id: Cada favorito terá um identificador único gerado automaticamente ou baseado na URL.
    * title: Título do favorito.
    * url: URL do favorito.

1. **Considerações de Segurança e Acessos**

    ###### **Regras de Segurança do Firebase Realtime Database:**

    Deve-se garantir que cada usuário só consiga acessar seus próprios dados. Uma regra básica de segurança seria:
   
    ```
    {
      "rules": {
        "users": {
          "$encodedEmail": {
            ".read": "auth != null && auth.token.email_verified == true && auth.token.email.toLowerCase().replace('@','').replace('.','') === $encodedEmail",
            ".write": "auth != null && auth.token.email_verified == true  && auth.token.email.toLowerCase().replace('@','').replace('.','') === $encodedEmail"
          }
        }
      }
    }
    ```
    
    Isso garante que apenas o próprio usuário poderá ler ou escrever em seu banco de dados.

    ###### **Autenticação:**
   
    O Firebase Auth deve ser configurado para permitir login apenas via Google.
   
    Caso o sistema precise de mais segurança, é possível incluir verificação de e-mails ou implementar múltiplos métodos de autenticação, mas inicialmente a autenticação Google atende bem.

1. **Componentes do Frontend (HTML/CSS/JS)**

    HTML:
    * Tela de login (com botão de autenticação Google).
    * Tela de favoritos (listagem de favoritos do usuário com possibilidade de adicionar um novo favorito).

    CSS:
    * Design simples e responsivo para garantir boa usabilidade em diferentes dispositivos.
    * Layout claro para a exibição dos favoritos e fácil navegação.

    JavaScript:
    * Firebase SDK: Para integração com Firebase Auth e Realtime Database.
    * Eventos: Para autenticação do usuário, carregamento de favoritos, e adição de novos favoritos.

1. **Tecnologias Utilizadas**

    * Frontend: HTML, CSS, JavaScript.
    * Backend: Firebase (Authentication e Realtime Database).
    * Hospedagem: Firebase Hosting.

1. **Cronograma e Estimativa de Tempo**

    O cronograma pode ser divido nas seguintes etapas:

    ###### **Configuração do Firebase: 1-2 horas**
    1. Configuração do Firebase Auth e Realtime Database.
    1. Definição das regras de segurança.

    ###### **Desenvolvimento do Frontend: 3-5 horas**
    1. Desenvolvimento das telas de login e listagem de favoritos.
    1. Implementação de funcionalidade de adicionar novos favoritos.
    
    ###### **Integração Firebase: 2-3 horas**
    1. Integração da autenticação com Firebase Auth.
    1. Integração da leitura e escrita de favoritos no Realtime Database.
    
    ###### **Testes e Validação: 2-3 horas**
    1. Testar login/logout, exibição de favoritos, adicionar novos favoritos, e regras de segurança.

----
        
