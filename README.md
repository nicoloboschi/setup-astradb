# `@nicoloboschi/setup-astradb` Github action

This action creates a [DataStax AstraDB](https://www.datastax.com/products/datastax-astra) database.

## Action Inputs
| Input name | Description                                                                               	                     | Required 	 | Default Value |
|------------|-----------------------------------------------------------------------------------------------------------------|------------|---------------|
| token      | Astra DB application token. It needs to have enough permissions to create/delete databases in the organization. | true       |               |
| name       | Name of the database to create.                                                                                 | true       |               |
| region     | Region whereas to create the database. (aws,gcp,azure)                                                          | true       |               |
| cloud      | Cloud provider whereas to create the database.                                                                  | true       |               |
| env        | Astra DB environment. (ENV, PROD).                                                                              | false      | PROD          |


## Action Outputs
| Output name  | Description                           |
|--------------|---------------------------------------|
| id           | Full URL API endpoint of the database |
| api-endpoint | Database ID.                          |

## Example

```yml
name: Test application that uses AstraDB

on: [pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
    - uses: actions/checkout@v3
    - uses: nicoloboschi/setup-astradb@v1
      with:
        token: ${{ secrets.ASTRA_DB_TOKEN }}
        name: my-database
        region: us-east-2
        cloud: aws
```