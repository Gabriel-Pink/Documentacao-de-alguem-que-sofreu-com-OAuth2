
Os exemplos citados se referem à API fornecido pela plataforma [Bling](https://www.bling.com.br)

- Crie um app [AQUI](https://www.bling.com.br/cadastro.aplicativos.php#/list). Com isso, será gerado algumas informações importantes, como ***Client ID*** e o ***Client Secret***. Isso gerará um link de convite: como este, por exemplo:
  
  `https://www.bling.com.br/Api/v3/oauth/authorize?response_type=code&client_id=[INFORMAÇÃO PRIVADA]&state=[INFORMAÇÃO PRIVADA]`
  
  Ao acessar o link, você é direcionado para página de login onde você pode utilizar suas credenciais de usuário da plataforma que você está acessando.
  
  Fazendo o login, você é redirecionado para a **URI de Callback** que você programou mais cedo.
  
  ***Importante***: Neste processo, utilize uma URI de Callback que você consiga recuperar as informações. Minha recomendação é o [webhook.site](https://webhook.site). 
  
  O que é retornado após a requisição:
  
```
code: (SHA1 hash)
state: (MD5 hash)
```
  
  ***Importante***: Utilizaremos o retorno de `code` no próximo bloco e eles tem prazo de até 1 minuto para serem utilizados.

- Mas isso não é tudo, agora temos que gerar o **Access Token**. Para isso, você vai precisar, realizar um POST Request para:
  
  `https://api.bling.com.br/Api/v3/oauth/token`
  
  Passando a seguinte headers:
  
  `Authorization: Basic base64(clientID:clienteSecret)`
  
  Através do URL Encoded do body, deve ser passado os valor:
  
  `grant_type: authorization_code`
  `code: valor SHA1 do processo anterior`
  
  


  
  Com isso, é retornado essas informações para você:
  

```json
{
	"access_token": "CHAVE",
	"expires_in": 21600,
	"token_type": "Bearer",
	"scope": "SCOPOS",
	"refresh_token": "CHAVE_REFRESH"
}
```


| Termo              | Detalhamento                                                                      |
| ------------------ | --------------------------------------------------------------------------------- |
| Client App         | Aplicativo que fará uso dos dados das contas dos usuários (conta Bling).          |
| Authorization Code | Código enviado ao _Client App_ quando um usuário autoriza acesso aos dados.       |
| Access Token       | _Token_ utilizado para requisição do recurso dos usuários.                        |
| Refresh Token      | _Token_ utilizado para requisitar um novo `access_token`, quando o mesmo expirar. |
