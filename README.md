# DynamoDB-DIO
Repositório do Projeto sobre Amazon DynamoDB

🚀 **Serviços Utilizados**

- Amazon DynamoDB
- AWS CLI (Command Line Interface) para execução de comandos

## 01 - Comandos para Execução do Projeto

### 1.1 - Criar uma Tabela
Para criar uma tabela no DynamoDB, execute o seguinte comando:

```
aws dynamodb create-table \
    --table-name Music \
    --attribute-definitions \
        AttributeName=Artist,AttributeType=S \
        AttributeName=SongTitle,AttributeType=S \
    --key-schema \
        AttributeName=Artist,KeyType=HASH \
        AttributeName=SongTitle,KeyType=RANGE \
    --provisioned-throughput \
        ReadCapacityUnits=10,WriteCapacityUnits=5
``` 

1.2 - Para inserir um item na tabela Music, utilize o comando abaixo:

```
aws dynamodb put-item \
    --table-name Music \
    --item file://itemmusic.json
```

1.3 - Para inserir múltiplos itens de uma vez, use o comando batch-write-item:

```
aws dynamodb batch-write-item \
    --request-items file://batchmusic.json
```

1.4 - Para modificar um item, edite o arquivo itemmusic.json usando o editor nano:

```
nano itemmusic.json
```

Após modificar o item, execute o comando abaixo para atualizar a tabela:

```
aws dynamodb put-item \
    --table-name Music \
    --item file://itemmusic.json
```

1.5 - Sair do Editor nano
Para sair do nano, siga os passos abaixo:

Pressione Ctrl + X (isso inicia o processo de saída).

Se houver alterações no arquivo, o nano perguntará se deseja salvar:

Pressione Y (Yes) para salvar.

Pressione N (No) para sair sem salvar.

Se você pressionar Y para salvar, será solicitado o nome do arquivo. Pressione Enter para manter o nome original.

1.6 - Para pesquisar um item pelo nome do artista, utilize o comando query:

```
aws dynamodb query \
    --table-name Music \
    --key-condition-expression "Artist = :artist" \
    --expression-attribute-values '{":artist":{"S":"Iron Maiden"}}'
```

Você também pode fazer a pesquisa para outro artista:

```
aws dynamodb query \
    --table-name Music \
    --key-condition-expression "Artist = :artist" \
    --expression-attribute-values '{":artist":{"S":"David Guetta (feat. Sia)"}}'
```

1.7 - Para criar um índice secundário global (GSI) baseado no título do álbum, execute o seguinte comando:

```
aws dynamodb update-table \
    --table-name Music \
    --attribute-definitions AttributeName=AlbumTitle,AttributeType=S \
    --global-secondary-index-updates \
    "[{\"Create\":{\"IndexName\": \"AlbumTitle-index\",\"KeySchema\":[{\"AttributeName\":\"AlbumTitle\",\"KeyType\":\"HASH\"}],\"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5},\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

1.8 - Pesquise pelo Índice Secundário Baseado no Título do Álbum utilize o comando:

```
aws dynamodb query \
    --table-name Music \
    --index-name AlbumTitle-index \
    --key-condition-expression "AlbumTitle = :name" \
    --expression-attribute-values '{":name":{"S":"Brave Now World"}}'
```

Outro exemplo de pesquisa pelo índice secundário:

```
aws dynamodb query \
    --table-name Music \
    --index-name AlbumTitle-index \
    --key-condition-expression "AlbumTitle = :name" \
    --expression-attribute-values '{":name":{"S":"Nothing But The Beat"}}'
```