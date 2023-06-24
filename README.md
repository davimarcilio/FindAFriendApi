
# Find A Friend API

API do projeto [Find A Friend](https://github.com/davimarcilio/findAFriend)

## Stack utilizada

- 🚀 Desenvolvido com [Node](https://nodejs.org/en)
- ✅ Validação com [ZOD](https://zod.dev/)
- 🪧 Rotas com [Fastify](https://www.fastify.io/)
- 📨 Comunicação com APIs utilizando [Axios](https://axios-http.com/ptbr/docs/intro)
- 🎲 Banco de Dados com [Prisma ORM](https://www.prisma.io/)
- 🎲 Padronização de código com [Eslint](https://eslint.org/)

## Rodando localmente

Clone o projeto

```bash
  git clone git@github.com:davimarcilio/findAFriendApi.git
```

Entre no diretório do projeto

```bash
  cd findAFriendApi
```

Instale as dependências

```bash
  npm install
```

Inicie o banco de dados

```bash
  npx prisma migrate dev
```

Popule o banco de dados

```bash
  npx prisma db seed
```

Inicie a aplicação

```bash
  npm run dev
```


## Variáveis de Ambiente

Para rodar esse projeto, você vai precisar adicionar as seguintes variáveis de ambiente no seu .env

`JWT_SECRET=GCC-FIND-A-FRIEND`

`APP_URL=http://localhost:3333`

`DATABASE_URL="file:./dev.db"`



## Funcionalidades

- Cadastro de organização
- Login de organização
- Consulta de PETs
- Integrado com google maps dinamicamente
- Extremamente validado com zod e react-hook-form
- etc...


# Documentação API

## Listar Estados

Nessa rota você irá receber a listagem de todos os estados do Brasil.

<aside>

🗂️  **[ GET ]** /location/states

</aside>

### Respostas

Aqui são os dados retornados da API em todas as situações que podem ocorrer.

- **Sucesso (Status 200)**
    
    ```json
    {
    	"states": [
    		{
    			"sigla": "RO",
    			"name": "Rondônia",
    		},
    		{
    			"sigla": "AC",
    			"name": "Acre",
    		}
    	]
    }
    ```
    
<hr>

## Listar Cidades por estado

Nessa rota vamos listar todas as cidades de um estado especificado na requisição.

<aside>

🗂️ **[ GET ]** /location/citys/:UF

</aside>

### Route params

São os parâmetros que enviamos diretamente na rota de requisição para a API.

> `:UF`  -  Obrigatório
> 

> O parâmetro de `UF` deve ser informado para seja possível filtrar e buscar todas as cidades de um estado do Brasil.

Ex.: `/location/citys/SP`
> 

### Respostas

Aqui são os dados retornados da API em todas as situações que podem ocorrer.

- **Sucesso (Status 200)**
    
    ```json
    {
    	"citys": [
    		{
    			"name": "Adamantina",
    			"code": "3500105"
    		},
    		{
    			"name": "Adolfo",
    			"code": "3500204"
    		},
    		{
    			"name": "Aguaí",
    			"code": "3500303"
    		}
    	]
    }
    ```
    
- **Sigla inválida (Status 404)**
    
    ```json
    {
    	"error": "Sigla de UF inválida"
    }
    ```
    
<hr>

## Listar pets

Nessa rotas vamos conseguir buscar todos os pets que estão disponíveis para adoção em uma cidade.

<aside>

🗂️ **[ GET ]** /pets/:city

</aside>

### Route params

São os parâmetros que enviamos diretamente na rota de requisição para a API.

> `:city`  -  Obrigatório
> 

> O parâmetro de `city` de ser informado como nome da cidade onde está sendo realizada a busca de um pet.

Ex.: `/pets/São Paulo`
> 

### Query Params

São parâmetros que enviamos na requisição para realizar algum tipo de filtro na listagem que vem da API e geralmente são opcionais.

> `age`  -  Opcional
> 

> O parâmetro `age` pode ser informado para realizar um filtro na idade do pet.

Os valores aceitos para o filtro de `age` são:

- `cub` (Filhote)
- `adolescent` (Adolescente)
- `elderly` (Adulto / Idoso)
> 

> `energy`  -  Opcional
> 

> O parâmetro `energy` pode ser informado para realizar um filtro no nível de energia do pet.

Os níveis de energia vão e `1` até `5`.
> 

> `independence`  -  Opcional
> 

> O parâmetro `independence` pode ser informado para realizar um filtro no nível de independência do pet em relação ao seu tutor.

Os valores aceitos para o filtro de `independence` são:

- `low` (Baixo)
- `medium` (Médio)
- `high` (Alto)
> 

> `size`  -  Opcional
> 

> O parâmetro `size` pode ser informado para realizar um filtro no porte do pet.

Os valores aceitos para o filtro de `size` são:

- `small` (Pequeno)
- `medium` (Mediano)
- `big` (Grande)
> 

> `type`  -  Opcional
> 

### Respostas

Aqui são os dados retornados da API em todas as situações que podem ocorrer.

- **Sucesso (Status 200)**
    
    ```json
    {
    	"pets": [
    		{
    			"id": "4d6281f6-5c6d-4dc8-be9c-07e344e33d49",
    			"name": "Yoda",
    			"description": "Um companheiro para todas as horas",
    			"city": "São Paulo",
    			"age": "adolescent",
    			"energy": 5,
    			"size": "small",
    			"independence": "low",
    			"type": "cat",
    			"photo": "yoda.jpeg",
    			"orgId": "30ab4c94-593c-4a5b-8249-54364ef77612",
    			"photo_url": "http://localhost:3333/images/yoda.jpeg"
    		}
    	]
    }
    ```
    



<hr>


## Coordenadas por cep

<aside>

🗂️ **[ GET ]** /location/coordinates/:cep

</aside>

### Route Params

São os dados que precisamos enviar na rota da API para poder buscar a Geolocalização da ORG via CEP.

> `:cep`  -  Obrigatório
> 

> O parâmetro de `cep` deve ser informado para seja possível buscar as coordenadas de uma ORG

Ex.: `/location/coordinates/01310‑902`
> 

### Respostas

- **Sucesso (Status 200)**
    
    ```json
    {
    	"address": "Avenida Paulista, 52",
    	"coordinates": {
    		"latitude": "-23.5707253",
    		"longitude": "-46.6445287"
    	}
    }
    ```
    
- **Cep inválido (404)**
    
    ```json
    {
    	"error": "Não foi possível buscar as coordenadas"
    }
    ```
    
<hr>

## Detalhes do pet

Nessa rota iremos buscar todos os detalhes e um pet e a qual ORG ele está relacionado.

<aside>

🗂️ ******[ GET ]****** /pets/show/:pet_id

</aside>

### Route Params

São os dados que precisamos enviar na rota para que possamos buscar por um pet em específico

> `:pet_id`  -  Obrigatório
> 

> O parâmetro de `pet_id` deve ser informado para seja possível buscar os dados do Pet correspondente àquele ID.

Ex.: `/pets/show/d1d4cc7c-92b2-44ea-bd13-4815476bfd1d`
> 

### Respostas

- **Sucesso (200)**
    
    ```json
    {
    	"pet": {
    		"id": "137d9eb5-aae2-4aa2-958a-525ec830dde9",
    		"name": "Caramelinho",
    		"description": "Um doguinho para quem tem muito amor para dar",
    		"city": "Sao Paulo",
    		"age": "cub",
    		"energy": 3,
    		"size": "medium",
    		"independence": "high",
    		"type": "dog",
    		"photo": "caramelinho.jpeg",
    		"orgId": "30ab4c94-593c-4a5b-8249-54364ef77612",
    		"org": {
    			"id": "30ab4c94-593c-4a5b-8249-54364ef77612",
    			"nome": "Adote Pets",
    			"address": "Avenida Paulista, 52",
    			"cep": "01310‑900",
    			"whatsappNumber": "+558699999999"
    		},
    		"photo_url": "http://localhost:3333/images/caramelinho.jpeg"
    	}
    }
    ```
    
- **Pet não encontrado (404)**
    
    ```json
    {
    	"error": "Pet não encontrado"
    }
    ```
    
<hr>

## Galeria de Fotos do Pet

Nessa rota vamos buscar todas as fotos que foram cadastradas para um pet.

<aside>

🗂️ **[ GET ]** /pets/gallery/:pet_id

</aside>

### Route params

São os parâmetros que precisamos informar para a API identificar o Pet e buscar as imagens.

> `:pet_id`  -  Obrigatório
> 

> O parâmetro de `pet_id` deve ser informado para seja possível buscar a galeria de fotos do Pet correspondente àquele ID.

Ex.: `/pets/gallery/94f3c2fb-806a-4624-b24e-88b925581dce`
> 

### Respostas

- **Sucesso (200)**
    
    ```json
    {
    	"pet_gallery": [
    		{
    			"id": "ed55c494-1369-4b40-b54e-a835dadddc5e",
    			"image": "tigrao-2.jpeg",
    			"petId": "94f3c2fb-806a-4624-b24e-88b925581dce",
    			"photo_url": "http://localhost:3333/images/tigrao-2.jpeg"
    		},
    		{
    			"id": "27489470-4c9d-4f20-95d9-eb0e8882226f",
    			"image": "tigrao.jpeg",
    			"petId": "94f3c2fb-806a-4624-b24e-88b925581dce",
    			"photo_url": "http://localhost:3333/images/tigrao.jpeg"
    		},
    		{
    			"id": "9b2ed77b-ae40-43e5-ba4c-f095bc9506b3",
    			"image": "tigrao-1.jpg",
    			"petId": "94f3c2fb-806a-4624-b24e-88b925581dce",
    			"photo_url": "http://localhost:3333/images/tigrao-1.jpg"
    		}
    	]
    }
    ```
    
<hr>

## Requisitos de adoção do Pet

Nessa rota vamos buscar todos os requisitos necessários para adoção de um pet.

<aside>

🗂️ **[ GET ]** /pets/adoption-requirements/:pet_id

</aside>

### Route params

São os parâmetros que precisamos informar para a API identificar o Pet e requisitos de adoção.

> `:pet_id`  -  Obrigatório
> 

> O parâmetro de `pet_id` deve ser informado para seja possível buscar os requisitos necessários para a adoção do Pet correspondente àquele ID.

Ex.: `/pets/adoption-requirements/94f3c2fb-806a-4624-b24e-88b925581dce`
> 

### Respostas

- **Sucesso (200)**

<hr>
## Cadastrar ORG

Nessa rota vamos poder criar uma nova que pode acessar o painel administrativo e cadastrar pets.

<aside>

🗂️ **[ POST ]** /orgs

</aside>

### Request Body

São as informações envidas no corpo da requisição para que seja possível a criação da ORG.

> `name`  -  Obrigatório
> 

> O parâmetro de `name` é uma string e faz referência ao nome da ORG.

Ex.: `"name": "Nova Org"`
> 

> `email`  -  Obrigatório
> 

> O parâmetro de `name` é uma string e faz referência ao e-mail que a ORG irá utilizar para acessar a aplicação.

Ex.: `"email": "nova.org@email.com"`
> 

> `cep`  -  Obrigatório
> 

> O parâmetro de `cep` é uma string e vamos utilizar para podermos buscar a Geolocalização da ORG.

Ex.: `"cep": "69000-000"`
> 

> `address`  -  Obrigatório
> 

> O parâmetro de `address` é uma string e faz referência ao endereço da ORG.

Ex.: `"address": "Rua da ORG, 123"`
> 

> `whatsappNumber`  -  Obrigatório
> 

> O parâmetro de `whatsappNumber` é uma string e faz referência ao número de WhatsApp onde o usuário entrará em contato com a ORG.
Ele deve seguir o padrão que o Whatspp exige.

Ex.: `"whatsappNumber": "+5511999999999"`
> 

> `password`  -  Obrigatório
> 

> O parâmetro de `password` é uma string e faz referência à senha que a ORG utilizará para acessar a aplicação

Ex.: `"password": "123456"`
> 

> `passwordConfirm`  -  Obrigatório
> 

> O parâmetro de `passwordConfirm` é uma string e precisa ter o mesmo valor informado no campo de `password`.

Ex.: `"passwordConfirm": "123456"`
> 

- **Sucesso (201)**
    
    Resposta com corpo vazio.
    
- **ORG já cadastrada (400)**
    
    ```json
    {
    	"error": "Já existe uma ORG com este e-mail"
    }
    ```
    
- **Senhas diferentes (400)**
    
    ```json
    {
    	"error": "As senhas não conferem"
    }
    ```
    
- **ORG não cadastrada (400)**
    
    ```json
    {
    	"error": "Não foi possível cadastrar a ORG"
    }
    ```
    
<hr>


## Criar sessão (Login)

Nessa rota vamos realizar a autenticação de uma ORG aplicação, recebendo um token JTW para realizar requisições autenticadas.

<aside>

🗂️ **[ POST ]** /auth/sessions

</aside>

### Request Body

São as informações envidas no corpo da requisição para que autenticar uma ORG.

> `email`  -  Obrigatório
> 

> O parâmetro de `email` é uma string e faz referência ao e-mail informado no momento do cadastro da ORG.

Ex.: `"email": "nova.org@email.com"`
> 

> `password`  -  Obrigatório
> 

> O parâmetro de `password` é uma string e precisa ser informado para validar as credenciais da ORG, para que a autenticação seja realizada

Ex.: `"password": "123456"`
> 
- **Sucesso (200)**
    
    ```json
    {
    	"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIzMGFiNGM5NC01OTNjLTRhNWItODI0OS01NDM2NGVmNzc2MTIiLCJpYXQiOjE2Nzk5NDc2MTB9.eiKubHLQeD0kYinFs_qJWgUrVovVR03sIk7ouLDfX4A",
    	"org": {
    		"id": "30ab4c94-593c-4a5b-8249-54364ef77612",
    		"nome": "Adote Pets",
    		"email": "adote_pets@email.com",
    		"address": "Avenida Paulista, 52",
    		"cep": "01310‑900",
    		"whatsappNumber": "+558699999999"
    	}
    }
    ```
    
- **Erro de credenciais (401)**
    
    ```json
    {
    	"error": "Credenciais inválidas"
    }
    ```
    
<hr>

## Refresh token

Nessa rota vamos poder revalidar o token JWT da ORG que está logado caso a sessão da mesma tiver expirado.

<aside>

🗂️ **[ PATCH ]** /auth/refresh-token

</aside>

### Respostas

- **Sucesso (200)**
    
    ```json
    {
    	"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIzMGFiNGM5NC01OTNjLTRhNWItODI0OS01NDM2NGVmNzc2MTIiLCJpYXQiOjE2ODA1MjYxNDksImV4cCI6MTY4MDUyNjQ0OX0.oXjhJrH63lKMr4_c9ht1ShyP1aXkZQ4K6dIsm5NzY3c"
    }
    ```
    
- **Falha na revalidação (401)**
    
    ```json
    {
    	"error": "Erro ao revalidar o token"
    }
    ```

<hr>
    

## Cadastrar Pet

Nessa rota vamos poder adicionar um novo pet que esteja disponível para adoção.

<aside>

🗂️ **[ POST ]** /pets

</aside>

### Request Body

Nessa rota o formato do corpo da requisição é um `multipart/form-data`, por isso é muito importante que seja feito esse tratamento pelo lado do front-end.

> `name`  -  Obrigatório
> 

> O parâmetro de `name` é uma string e faz referência ao nome do Pet.

Ex.: `"name": "Yoda"`
> 

> `age`  -  Obrigatório
> 

> O parâmetro de `age` é uma string e precisa ser informado com um dos valores abaixo:
- `cub` (Filhote)
- `adolescent` (Adolescente)
- `elderly` (Adulto / Idoso)

Ex.: `"age": "adolescent"`
> 

> `description`  -  Obrigatório
> 

> O parâmetro de `description` é uma string e faz referência a uma breve descrição do Pet.

Ex.: `"description": "Um companheiro para todas as horas."`
> 

> `energy`  -  Obrigatório
> 

> O parâmetro de `energy` é uma string e precisa ser informado com um valor entre 1 e 5 (valores inteiros).

Ex.: `"energy": "4"`
> 

> `independence`  -  Obrigatório
> 

> O parâmetro de `independence` é uma string e precisa ser informado com um dos valores abaixo:
- `low` (Baixo)
- `medium` (Médio)
- `high` (Alto)

Ex.: `"independence": "medium"`
> 

> `size`  -  Obrigatório
> 

> O parâmetro de `size` é uma string e precisa ser informado com um dos valores abaixo:
- `small` (Pequeno)
- `medium` (Mediano)
- `big` (Grande)

Ex.: `"size": "medium"`
> 

> `type`  -  Obrigatório
> 

> O parâmetro de `type` é uma string e precisa ser informado com um dos valores abaixo:
- `cat` (Gato)
- `dog` (Cachorro)

Ex.: `"type": "cat"`
> 

> `adoptionRequirements`  -  Obrigatório
> 

> O parâmetro de `adoptionRequirements` é uma string e precisa ser informado em um formato de array.

Ex.: `"type": "['espaço amplo para brincar', 'Muito amor para dar']"`
> 

> `images`  -  Obrigatório
> 

> O parâmetro de `images` são as fotos que irão para a galeria e para a Thumbnail do pet. Podem ser selecionadas até 6 fotos para o Pet.
> 

### Headers

Para essa rota precisamos informar um cabeçalho de autorização, contendo um Bearer Token e esse token é o que recebemos tanto no momento do login ou no refresh token. 

Exemplo: 

```json
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIzMGFiNGM5NC01OTNjLTRhNWItODI0OS01NDM2NGVmNzc2MTIiLCJpYXQiOjE2ODA1NDM4MDksImV4cCI6MTY4MDcxNjYwOX0.qSt5P4X2MGQuuD9_iL0uOBkZL8tN7RrUKZUhvopHNqU
```

### Respostas

- **Sucesso (201)**
    
    Resposta com corpo vazio
    
- **ORG não encontrada (404)**
    
    ```json
    {
    	"error": "ORG não encontrada"
    }
    ```
    
- **Requisitos de adoção não informados (400)**
    
    ```json
    {
    	"error": "É necessário no mínimo 1 requisito de adoção"
    }
    ```
    
- **Galeria vazia (400)**
    
    ```json
    {
    	"error": "É necessário no mínimo 1 imagem do pet"
    }
    ```
    
- **Falha na criação (400)**
    
    ```json
    {
    	"error": "Não foi possível cadastrar o Pet"
    }
    ```
    
- **Sessão Expirada (401)**
    
    ```json
    {
    	"error": "token.invaid"
    }
    ```
## Referência

 - [GCC Find A Friend API](https://efficient-sloth-d85.notion.site/API-FindAFriend-c9275383751f463b8a43137eed9087e8)

## Licença

[MIT](https://choosealicense.com/licenses/mit/)

