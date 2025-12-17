<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2024 The Linux Foundation
-->

# 🔑 JSON Variable Key/Value Lookup

Action to perform a lookup from a JSON string containing a simple array of
key/value pairs.

## json-key-value-lookup-action

## Usage Example

Pass a JSON string as an input to the action, along with the key to lookup.
The action will return the corresponding value, using JQ to pass the JSON.

```yaml
- name: "Get ticket reference from JSON lookup table"
  uses: lfreleng-actions/json-key-value-lookup-action@main
  with:
      json: ${{ inputs.issue_id_lookup_json }}
      key: ${{ env.key }}
```

## Example JSON Variable

Provide JSON as an array containing simple key/value pairs, as shown below.

```json
[
    { "key": "dependabot[bot]", "value": "CIMAN-33" },
    { "key": "John Doe", "value": "CIMAN-1000" }
]
```

In the example above, the repository has a variable configured called:

- `ISSUE_ID_LOOKUP_JSON`

Set/define this under:

[ GitHub Repository ] -> Settings -> Secrets and variables -> Actions -> Variables

## Inputs

<!-- markdownlint-disable MD013 -->

| Variable Name   | Required | Default | Description                                           |
| --------------- | -------- | ------- | ----------------------------------------------------- |
| json            | True     |         | JSON dictionary containing key/value pairs            |
| key             | True     |         | Key to use to extract corresponding value             |
| default         | False    | None    | Default value in the event lookup fails               |
| silent          | False    | False   | Suppresses output of returned values                  |
| summary_output  | False    | False   | Displays summary output in GitHub step summary        |
| exit_on_failure | False    | True    | Exit with error on failure (when no default provided) |

<!-- markdownlint-enable MD013 -->

## Outputs

| Variable Name | Description                                            |
| ------------- | ------------------------------------------------------ |
| value         | The value corresponding to the key (provided as input) |

## Requirements/Dependencies

Ensure validated JSON (in the format specified above) is in the variable.
The action performs validation before the lookup and will otherwise throw
an error.

## Notes

If the key is not represented in the lookup table, the action will exit with an
error. This behaviour will result in the calling action/workflow failing.
Prevent this by setting a default return value, allowing upstream code to take
alternative paths.
