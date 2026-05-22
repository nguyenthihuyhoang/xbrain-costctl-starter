# costctl — checklist run log

Date: 2026-05-22  
Workspace: `xbrain-costctl-starter/`  
OS: Windows (PowerShell)

## README Quickstart checklist

### 2) Install dependencies
- Not re-run in this log (environment already had dependencies installed).
- Repo includes: `requirements.txt`, `requirements-dev.txt`, `Makefile`.

### 3) Confirm baseline tests
Command:

```powershell
python -m pytest -q
```

Result:

```text
25 passed in 30.73s
```

### 4) Confirm `--help` works
Command:

```powershell
python .\costctl.py --help
```

Result (excerpt):

```text
usage: costctl [-h] [--region REGION] <command> ...

positional arguments:
  <command>
    list           list resources, filter by tag / missing-tag
    cost           show cost of resources matching tag
    terminate      terminate/delete one resource (asks confirmation)
    tag            add/update tags on one resource
    clean          (stretch) bulk terminate resources by tag
    idle           (stretch) find idle EC2 by CPU avg
    migrate-gp3    (stretch) gp2 -> gp3 EBS migration

options:
  -h, --help       show this help message and exit
  --region REGION  AWS region (default: $AWS_REGION or us-east-1)
```

Also verified subcommand help:

```powershell
python .\costctl.py list --help
python .\costctl.py terminate --help
```

### (Optional) Run against AWS account
README suggests configuring creds:

```bash
aws configure
./costctl.py list ec2
```

Attempted (PowerShell), with metadata fetch disabled:

```powershell
$env:AWS_EC2_METADATA_DISABLED="true"
python .\costctl.py list ec2
```

Result:

```text
botocore.exceptions.ClientError: An error occurred (AuthFailure) when calling the DescribeInstances operation: AWS was not able to validate the provided access credentials
```

Notes:
- CLI itself runs, but AWS credentials in this environment were not accepted by AWS at runtime.
- Fix by setting valid `AWS_ACCESS_KEY_ID`/`AWS_SECRET_ACCESS_KEY` (and optional `AWS_SESSION_TOKEN`) or running `aws configure` for a real account.

### Re-try after `aws configure` (success)

Verified identity:

```powershell
aws sts get-caller-identity
```

```text
Account: 444203688599
Arn: arn:aws:sts::444203688599:assumed-role/WSParticipantRole/Participant
```

Generated real sample outputs (saved under `sample_output/`):

- `sample_output/list_ec2_2026-05-22.txt`
- `sample_output/list_ec2_missing_app_2026-05-22.txt`
- `sample_output/cost_TaskIO_2026-05-22.txt`

## Environment
- Python: `Python 3.12.6`
