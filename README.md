# Criar um Repositório no GitHub

## Passo a Passo:

1. Crie um novo repositório no GitHub chamado **"app-deploy-demo"**.
2. Adicione um arquivo `index.html` com o seguinte conteúdo:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplicação Demo</title>
</head>
<body>
    <h1>Aplicação em Docker com GitHub Actions</h1>
    <p>Este é um exemplo de aplicação simples para demonstrar o deploy usando Docker e GitHub Actions.</p>
</body>
</html>
```

3. Adicione um arquivo `Dockerfile` com o seguinte conteúdo:

```dockerfile
# Use a imagem oficial do Node.js como base
FROM node:16

# Crie o diretório de trabalho
WORKDIR /app

# Copie o arquivo package.json e instale as dependências
COPY package.json /app
RUN npm install

# Copie os arquivos do projeto
COPY . /app

# Exponha a porta 8080
EXPOSE 8080

# Comando para iniciar a aplicação
CMD ["npm", "start"]
```

---

## Configurar o GitHub Actions

### Passo a Passo:

1. No repositório, vá para a aba **"Actions"**.
2. Escolha o template **"Simple workflow"**.
3. Substitua o conteúdo do arquivo `.github/workflows/main.yml` pelo seguinte:

```yaml
name: Deploy com Docker

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Clonar o repositório
        uses: actions/checkout@v3

      - name: Configurar Docker
        uses: docker/setup-buildx-action@v2

      - name: Fazer login no Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Construir e enviar a imagem
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: usuario/minha-aplicacao:latest
```

4. Commit as alterações.

---

## Executar o Deploy

### Passo a Passo:

1. Peça para os alunos fazerem um **push** do código para o repositório.
2. No GitHub, vá para a aba **"Actions"** e acompanhe a execução do workflow.
3. Quando o workflow for concluído, a aplicação estará rodando em um contêiner Docker.

---

## Testar a Aplicação

### Passo a Passo:

1. No terminal, execute o comando:

```bash
docker ps
```

2. Verifique o **ID** do contêiner e execute:

```bash
docker exec -it <container_id> bash
```

---

## Feedback e Discussão

### Perguntas para Discussão:

- O que acontece se o deploy falhar? Como podemos melhorar o processo?
- Quais são as vantagens de usar **CI/CD** e **Docker**?
- Como podemos monitorar a aplicação em produção?

---

## Material de Apoio

- **Repositório GitHub**: Disponibilize um repositório modelo para os alunos seguirem.
- **Documentação**: Um guia rápido com os comandos Docker e exemplos de workflow do GitHub Actions.
- **Gravação da Aula**: Se possível, grave a aula e disponibilize para os alunos revisarem.

---


