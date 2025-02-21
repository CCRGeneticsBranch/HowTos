# Setting Up Gen3 Client

Gen3 client (`gen3`) is a command-line tool for interacting with Gen3 data commons. This guide provides instructions to download, install, and configure the Gen3 client on your system.

## Prerequisites

Ensure you have the following installed on your system:

- [Git](https://git-scm.com/downloads)
- [Python 3.x](https://www.python.org/downloads/)

## Download and Install Gen3 Client

### Option 1: Using cURL (Recommended)

```sh
curl -O https://raw.githubusercontent.com/uc-cdis/cdis-data-client/master/gen3-client
chmod +x gen3-client
mv gen3-client /usr/local/bin/gen3
```

### Option 2: Using Git

```sh
git clone https://github.com/uc-cdis/cdis-data-client.git
cd cdis-data-client
go build
mv gen3 /usr/local/bin/
```

## Verify Installation

Run the following command to verify that the Gen3 client is installed correctly:

```sh
gen3 --help
```

You should see the list of available commands.

## Configure Gen3 Client

1. **Obtain API Credentials**

   - Log in to the Gen3 data commons you wish to access. For CCDI data >> https://nci-crdc.datacommons.io/login
   - Navigate to your profile settings.
   - Download your API key (`cred.json`).

2. **Configure Gen3 Client with API Key**

   ```sh
   gen3 auth login --profile=<profile_name> --cred=path/to/cred.json
   ```

   Replace `<profile_name>` with a name for your profile and `path/to/cred.json` with the actual path to your credential file.

3. **Verify Authentication**

   ```sh
   gen3 auth verify
   ```

   If successful, the command will confirm that you are authenticated.

## Basic Usage

- **Download Manifest and Convert to JSON**

  From the [CCDI Explore page](https://ccdi.cancer.gov/explore), download the manifest CSV file containing the list of files to be downloaded. Convert this CSV to a JSON file containing only the object IDs (which start with `dg.4DFC`).

- **Convert CSV to JSON Using Python**

  Use the following Python script to convert the manifest CSV file to JSON format:

  ```python
  #!/usr/bin/env python
  import csv
  import json
  import sys

  # Read IDs from csv and remove prefix
  def read_ids_from_csv(file_path):
      with open(file_path, 'r') as csvfile:
          reader = csv.reader(csvfile)
          next(reader, None)
          ids = [row[0].replace('drs://nci-crdc.datacommons.io/', '') for row in reader]
      return ids

  # Convert to json format
  def convert_to_json(ids):
      json_data = [{"object_id": id} for id in ids]
      return json_data

  # Output json to file
  def output_json_to_file(json_data, output_file):
      with open(output_file, 'w') as jsonfile:
          json.dump(json_data, jsonfile, indent=2)

  csv_file_path = sys.argv[1]
  output_json_file = sys.argv[2]

  # Read IDs from csv, remove prefix, and convert to json
  ids = read_ids_from_csv(csv_file_path)
  json_data = convert_to_json(ids)

  # Output JSON
  output_json_to_file(json_data, output_json_file)
  ```

  Example JSON file:

  ```json
  [
    {
      "object_id": "dg.4DFC/33aa9d2e-4a75-4ba6-test-test9"
    },
    {
      "object_id": "dg.4DFC/487d2064-e9eb-4181-test-testb4"
    },
    {
      "object_id": "dg.4DFC/f331a053-ba25-49d1-test-test9306"
    }
  ]
  ```

- **Run the Download Script**

  Save and execute the following shell script to download multiple files using `gen3-client`:

  ```sh
  #!/bin/bash

  cd /data/khanlab/projects/CCDI-MCI_data

  yes | HOME= ./gen3-client download-multiple \
          --profile=VG_CCDI \
          --manifest=CCDI_PBBKTB_manifest.json \
          --download-path=./Gen3_downloads/PBBKTB_RNAseq/ \
          --skip-completed=true
  ```

## Troubleshooting

- If you encounter permission errors, ensure that your credentials are correctly configured.
- Use `gen3 --help` for detailed command options.
- Check the official documentation at [Gen3 Client GitHub](https://github.com/uc-cdis/cdis-data-client).

## Conclusion

You have successfully installed and configured the Gen3 client. You can now interact with Gen3 data commons using the command line. Happy coding!

Example JSON file:

```json
[
  {
    "object_id": "dg.4DFC/33aa9d2e-4a75-4ba6-test-test9"
  },
  {
    "object_id": "dg.4DFC/487d2064-e9eb-4181-test-testb4"
  },
  {
    "object_id": "dg.4DFC/f331a053-ba25-49d1-test-test9306"
  }
]
```

- **Run the Download Script**

  Save and execute the following shell script to download multiple files using `gen3-client`:

  ```sh
  #!/bin/bash

  cd /path/to/json/file/gen3

  yes | HOME= ./gen3-client download-multiple \
          --profile=VG_CCDI \
          --manifest=manifest.json \
          --download-path=./Gen3_downloads/ \
          --skip-completed=true
  ```

## Troubleshooting

- If you encounter permission errors, ensure that your credentials are correctly configured.
- Use `gen3 --help` for detailed command options.
- Check the official documentation at [Gen3 Client GitHub](https://github.com/uc-cdis/cdis-data-client).

## Conclusion

You have successfully installed and configured the Gen3 client. You can now interact with Gen3 data commons using the command line. Happy coding!
