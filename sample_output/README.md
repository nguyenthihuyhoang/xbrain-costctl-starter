# sample_output/

This folder should contain **real outputs** captured from running `costctl`
against your workshop AWS account.

If your repo still has `*_example.txt` files, replace them with real runs and
delete the example files before submission.

## How to produce real samples

```bash
./costctl.py list ec2 > sample_output/list_ec2_$(date +%F).txt
./costctl.py list ec2 --missing-tag Application > sample_output/list_ec2_missing_app_$(date +%F).txt
./costctl.py cost --tag Application=<your-app> --days 7 > sample_output/cost_<your-app>_$(date +%F).txt
```

Windows / PowerShell equivalent:

```powershell
$d = Get-Date -Format 'yyyy-MM-dd'
python .\costctl.py list ec2 | Out-File -Encoding utf8 "sample_output\list_ec2_$d.txt"
python .\costctl.py list ec2 --missing-tag Application | Out-File -Encoding utf8 "sample_output\list_ec2_missing_app_$d.txt"
python .\costctl.py cost --tag Application=<your-app> --days 7 | Out-File -Encoding utf8 "sample_output\cost_<your-app>_$d.txt"
```

The trainer will `git clone` your repo, follow the README, and expect to
reproduce roughly these outputs (allowing for natural drift in timestamps and
resource lists between snapshots).

## Extra samples (optional)

It's fine (often helpful) to include additional real outputs for other resource
types or broader tags, for example:

- `./costctl.py list s3`
- `./costctl.py list volume`
- `./costctl.py list rds`
- `./costctl.py cost --tag CostCenter=<group> --days 90`

Note: `costctl cost` filters by a **cost allocation tag**. Some AWS charges may
not show up under a given tag (or at all) unless that tag is activated in AWS
Billing and the resources/services support tag-based allocation.

## Anti-pattern

Don't paste fabricated output. If `costctl list ec2` against your account
returns 0 rows, commit that — it's a valid output. Don't invent fake instance
IDs to make the sample look "interesting".
