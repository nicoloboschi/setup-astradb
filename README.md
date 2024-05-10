# `@nicoloboschi/setup-astradb` Github action

This action creates a [DataStax AstraDB](https://www.datastax.com/products/datastax-astra) database.
To clean up the database, use the `@nicoloboschi/cleanup-astradb` [action](https://github.com/nicoloboschi/cleanup-astradb).

## Action Inputs
| Input name  | Description                                                                               	                     | Required 	 | Default Value |
|-------------|-----------------------------------------------------------------------------------------------------------------|------------|---------------|
| token       | Astra DB application token. It needs to have enough permissions to create/delete databases in the organization. | true       |               |
| name        | Name of the database to create. If not provided, a name will be generated automatically.                        | false      |               |
| name-prefix | Name prefix in case `name` is not set.                                                                          | false      |               |
| region      | Region whereas to create the database. (aws,gcp,azure)                                                          | true       |               |
| cloud       | Cloud provider whereas to create the database.                                                                  | true       |               |
| env         | Astra DB environment. (ENV, PROD).                                                                              | false      | PROD          |


## Action Outputs
| Output name  | Description                           |
|--------------|---------------------------------------|
| id           | Full URL API endpoint of the database |
| api-endpoint | Database ID.                          |
| name         | Database name.                        |

## Example

```yml
name: Test application that uses AstraDB

on: [ pull_request ]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v3
      - uses: nicoloboschi/setup-astradb@v1
        id: astradb
        with:
          token: ${{ secrets.ASTRA_DB_TOKEN }}
          name: my-database
          region: us-east-2
          cloud: aws
      - run: |
          echo "Database ID is ${{ steps.astradb.outputs.id }}"
          echo "Database API endpoint is ${{ steps.astradb.outputs.api-endpoint }}"
      - uses: nicoloboschi/cleanup-astradb@v1
        with:
          token: ${{ secrets.ASTRA_DB_TOKEN }}
          name: my-database
```