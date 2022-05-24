# update-static-data-in-azure-table-storage

Update static data (csv file) to azure table storage

## Usage

```yaml
name: CI
on:
  push:
    branches:
      - main
    paths:
      - 'staticFiles/**.csv'
jobs:
  upload:
    name: Uploading ${{ github.event.inputs.tableName || inputs.tableName }} to ${{ github.event.inputs.environment || inputs.environment }}
    runs-on: ubuntu-latest
    environment:
      name: ${{ github.event.inputs.environment || inputs.environment }}
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
          
      - name: Get changed files
        id: changed-files
        uses: tj-actions/changed-files@v20.1
        with:
          separator: ","
          files: |
            *.csv
            
      - uses: farmerconnect/update-static-data-in-azure-table-storage@v2.0.2
        with:
          connection_string: ${{ secrets.AzureStorageConnectionString }}
          csv_file_paths: ${{ steps.changed-files.outputs.all_changed_files }}
```

## Inputs

|               Input               |          type          | required |                description                 |
|:---------------------------------:|:----------------------:|:--------:|:------------------------------------------:|
|         connection_string         |        `string`        |  `true`  | Connection string from azure table storage |
|           csv_file_paths          |        `string`        |  `true`  | Csv file paths separated by comma          |
