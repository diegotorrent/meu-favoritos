# meu-favoritos
Projeto hospedado no Firebase. 

→ Definir o escopo (Conceituar o proposito do projeto)

    → Lista de sites favoritos decentralizada, ou seja, independente do navegador.

→ Definir as funcionalidades

    → Mensagem de boas vindas;
    → Autenticacao por contas google;
    → Regras de acesso ao Realtime Database limitado ao indice do email do usuario;
    → Listar favoritos;
    → Incluir favorito;
    → Editar favorito;
    → Remover favorito;

→ Definir as tecnologias

    → JavaScript
    → Firebase
    → GitHub

→ Criar o Projeto Firebase

    → Hosting
    → Authentication (Google)
    → Real Time Database

→ Definir Landpage

    → Mensagem de boas vindas;
    → Autenticacao;
    → Real time database usando o email como indice;
    → Cadastro de favoritos;
    → Listagem de favoritos;

→ Real time Database Rules

    → Configurar Rules

# Realtime Database

## Rules

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
