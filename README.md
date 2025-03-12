# desfio-de-projetos-1

# Projeto DIO - Indexação e Consulta com Azure Cognitive Search

## Descrição do Projeto

Este projeto demonstra o uso do **Azure Cognitive Search** com **AI Search** para indexar e realizar consultas sobre dados relacionados ao desafio da DIO. A aplicação enriquece os dados utilizando habilidades cognitivas (AI Skills) e proporciona uma experiência de busca avançada, agregando valor ao portfólio e demonstrando competências técnicas em cloud e inteligência artificial.

## Índice

- [Introdução](#introdução)
- [Configuração do Serviço no Azure](#configuração-do-serviço-no-azure)
- [Criação do Índice](#criação-do-índice)
- [Indexação dos Dados](#indexação-dos-dados)
- [Aplicação de Habilidades Cognitivas](#aplicação-de-habilidades-cognitivas)
- [Realização de Consultas](#realização-de-consultas)
- [Insights e Aprendizados](#insights-e-aprendizados)
- [Possibilidades e Futuras Expansões](#possibilidades-e-futuras-expansões)
- [Como Contribuir](#como-contribuir)
- [Licença](#licença)

## Introdução

Neste projeto, exploramos como utilizar o Azure Cognitive Search para transformar uma simples aplicação de busca em uma solução robusta e inteligente. Ao indexar e enriquecer os dados com AI Skills, podemos extrair insights valiosos e melhorar a experiência do usuário durante as consultas.

## Configuração do Serviço no Azure

1. **Criação do Serviço:**
   - Acesse o [Azure Portal](https://portal.azure.com/).
   - Crie um recurso do **Azure Cognitive Search**.
   - Anote o `endpoint` e a `chave de API` gerados, pois serão necessários para as requisições.

2. **Pré-Requisitos:**
   - Conta ativa no Azure.
   - Instalação do [Azure CLI](https://docs.microsoft.com/pt-br/cli/azure/install-azure-cli).
   - Ambiente de desenvolvimento configurado (por exemplo, Python, Node.js).

## Criação do Índice

O índice define a estrutura dos dados a serem indexados. Nele, você especifica os campos que serão pesquisáveis, filtráveis e classificáveis.

### Exemplo de Schema de Índice (JSON):

```json
{
  "name": "desafio-dio-index",
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "filterable": true
    },
    {
      "name": "titulo",
      "type": "Edm.String",
      "searchable": true
    },
    {
      "name": "descricao",
      "type": "Edm.String",
      "searchable": true
    },
    {
      "name": "dataCriacao",
      "type": "Edm.DateTimeOffset",
      "filterable": true,
      "sortable": true
    }
  ]
}
```

## Indexação dos Dados
Após criar o índice, é preciso enviar os documentos para o Azure Cognitive Search. Isso pode ser feito utilizando a API REST ou os SDKs da Microsoft.

### Exemplo de Upload de Documento Usando Python:
```python
import requests
import json

# Substitua pelos seus valores
service_name = "<nome-do-seu-servico>"
api_key = "<sua-chave-de-api>"
index_name = "desafio-dio-index"

url = f"https://{service_name}.search.windows.net/indexes/{index_name}/docs/index?api-version=2020-06-30"
headers = {
    "Content-Type": "application/json",
    "api-key": api_key
}

data = {
    "value": [
        {
            "@search.action": "upload",
            "id": "1",
            "titulo": "Projeto DIO: Portfólio de Desafios",
            "descricao": "Aplicação que demonstra a integração do Azure Cognitive Search para indexação e consulta de dados.",
            "dataCriacao": "2025-03-12T00:00:00Z"
        }
    ]
}

response = requests.post(url, headers=headers, json=data)
print(json.dumps(response.json(), indent=4))
```
## Aplicação de Habilidades Cognitivas
Para enriquecer os dados, podemos configurar um skillset que aplica transformações durante a indexação, como a extração de entidades do texto.

### Exemplo Simplificado de Skillset (JSON):
```json
{
  "name": "desafio-skillset",
  "description": "Skillset para enriquecer documentos do desafio DIO.",
  "skills": [
    {
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
      "name": "#EntityRecognition",
      "context": "/document/descricao",
      "inputs": [
        {
          "name": "text",
          "source": "/document/descricao"
        }
      ],
      "outputs": [
        {
          "name": "entities",
          "targetName": "recognizedEntities"
        }
      ]
    }
  ]
}
```

## Realização de Consultas
Com os dados indexados e enriquecidos, é possível realizar consultas utilizando a API de busca. As consultas podem ser simples ou incluir filtros, facetas e ordenação para refinar os resultados.

### Exemplo de Consulta Simples via API REST:
GET https://<nome-do-seu-servico>.search.windows.net/indexes/desafio-dio-index/docs?search=portfólio&api-version=2020-06-30
api-key: <sua-chave-de-api>

## Insights e Aprendizados
Durante o desenvolvimento deste projeto, alguns dos principais aprendizados foram:

Integração com Cloud: Entendimento aprofundado sobre a criação e configuração de serviços no Azure.
Enriquecimento de Dados com AI: Aplicação prática de habilidades cognitivas para extrair entidades e enriquecer os dados.
Boas Práticas de Indexação: Aprendizado sobre a estruturação dos dados para facilitar buscas avançadas.
Documentação e Versionamento: Importância de manter um repositório bem documentado e organizado, facilitando a colaboração e futuras manutenções.
Possibilidades e Futuras Expansões
Desenvolvimento de Interface de Busca: Criar um frontend interativo para consulta dos dados.
Implementação de Novos AI Skills: Explorar outras habilidades cognitivas, como análise de sentimentos e extração de insights contextuais.
Integração com Outras Ferramentas: Conectar o serviço a sistemas de monitoramento ou visualização de dados para análises mais avançadas.
Como Contribuir
Se você deseja contribuir para este projeto, siga os passos abaixo:

Faça um fork deste repositório.
Crie uma branch para sua feature:
git checkout -b feature/nova-funcionalidade
Realize commits com mensagens descritivas:
git commit -m 'Adiciona nova funcionalidade'
Envie sua branch para o repositório remoto:
git push origin feature/nova-funcionalidade
Abra um Pull Request para revisão.
