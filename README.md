# `@nicoloboschi/setup-astradb` Github action

This action creates a [DataStax AstraDB](https://www.datastax.com/products/datastax-astra) database.
To clean up the database, use
the `@nicoloboschi/cleanup-astradb` [action](https://github.com/nicoloboschi/cleanup-astradb).

Related actions:

- `@nicoloboschi/cleanup-astradb` [action](https://github.com/nicoloboschi/cleanup-astradb): Delete a specific AstraDB
  database.
- `@nicoloboschi/cleanup-astradb-env` [action](https://github.com/nicoloboschi/cleanup-astradb-env): Delete databases
  based on their last usage time.

## Action Inputs

| Input name  | Description                                                                               	                     | Required 	 | Default Value    |
|-------------|-----------------------------------------------------------------------------------------------------------------|------------|------------------|
| token       | Astra DB application token. It needs to have enough permissions to create/delete databases in the organization. | true       |                  |
| region      | Region whereas to create the database. (aws,gcp,azure)                                                          | true       |                  |
| cloud       | Cloud provider whereas to create the database.                                                                  | true       |                  |
| env         | Astra DB environment. (ENV, PROD).                                                                              | false      | PROD             |
| name        | Name of the database to create. If not provided, a name will be generated automatically.                        | false      |                  |
| name-prefix | Name prefix in case `name` is not set.                                                                          | false      |                  |
| vector      | Whether to create a Vector database. (true, false)                                                              | false      | true             |
| keyspace    | Database default keyspace. Defaults to 'default_keyspace'.                                                      | false      | default_keyspace |

## Action Outputs

| Output name  | Description                            |
|--------------|----------------------------------------|
| id           | Database ID.                           |
| api-endpoint | Full URL API endpoint of the database. |
| name         | Database name.                         |

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
      - name: Setup AstraDB database
        uses: nicoloboschi/setup-astradb@v1
        id: astradb
        with:
          token: ${{ secrets.ASTRA_DB_TOKEN }}
          region: us-east-2
          cloud: aws
      - run: |
          echo "Database ID is ${{ steps.astradb.outputs.id }}"
          echo "Database API endpoint is ${{ steps.astradb.outputs.api-endpoint }}"
      - name: Cleanup AstraDB database
        uses: nicoloboschi/cleanup-astradb@v1
        if: ${{ always() }}
        with:
          token: ${{ secrets.ASTRA_DB_TOKEN }}
          name: ${{ steps.astra.outputs.name }}
```