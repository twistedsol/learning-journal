# Cloud security labs

Small, hands-on security tools I built in public during a 30-day challenge. Each one is plain Python (standard library only, no installs), comes with sample files, has unit tests, and runs its tests on GitHub Actions. Each repo has its own README with what it checks, its limits and what I learned.

I use AI to help write the code and tests. I run everything myself, read every rule and check the sample results by hand.

## The labs

| Lab | What it does | Tests | Repo |
|---|---|---|---|
| Log detector | Reads cloud audit logs and flags root logins, brute-force attempts, public buckets and switched-off logging | 19 | [log-detector](https://github.com/twistedsol/log-detector) |
| Misconfiguration scan | Scanned deliberately vulnerable Terraform with Checkov, then fixed three findings on one S3 bucket (passed checks went from 25 to 32) | n/a | [misconfig-scan](https://github.com/twistedsol/misconfig-scan) |
| IAM policy checker | Reads AWS IAM policy files and flags risky permissions with seven rules | 32 | [iam-policy-checker](https://github.com/twistedsol/iam-policy-checker) |
| Secrets scanner | Finds passwords, cloud keys and tokens left in code, and never prints the full secret | 43 | [secrets-scanner](https://github.com/twistedsol/secrets-scanner) |
| Security headers checker | Reads a web page's security headers and flags missing or weak ones | 47 | [security-headers-checker](https://github.com/twistedsol/security-headers-checker) |
| Password policy checker | Reads a password policy file and flags weak settings such as short minimums, no lockout, no MFA and weak hashing | 34 | [password-policy-checker](https://github.com/twistedsol/password-policy-checker) |
| SSH config checker | Reads an `sshd_config` file and flags risky settings such as root login and weak ciphers | 41 | [sshd-config-checker](https://github.com/twistedsol/sshd-config-checker) |
| Dockerfile checker | Reads Dockerfiles and flags running as root, secrets in the file and `curl \| sh`, with line numbers | 46 | [dockerfile-checker](https://github.com/twistedsol/dockerfile-checker) |
| Workflow checker | Reads GitHub Actions workflows and flags loose permissions, unpinned actions and injection risks, with line numbers, and checks its own workflow | 51 | [workflow-checker](https://github.com/twistedsol/workflow-checker) |
| Docker Compose checker | Reads docker-compose files and flags privileged containers, the Docker socket, public database ports and hardcoded secrets, with line numbers | 76 | [compose-checker](https://github.com/twistedsol/compose-checker) |
| Kubernetes manifest checker | Reads Kubernetes files and flags privileged containers, host mounts, running as root, missing limits and hardcoded secrets, with line numbers | 61 | [k8s-checker](https://github.com/twistedsol/k8s-checker) |
| Cloud audit alerts (AWS) | Emails an alert within about a minute when an S3 bucket is created, a bucket policy is changed or public access protection is removed, using CloudTrail, EventBridge and SNS | none (console build, tested live) | [cloud-audit-alerts](https://github.com/twistedsol/cloud-audit-alerts) |
| Terraform S3 scan (AWS) | Scans a deliberately careless Terraform S3 bucket with Checkov, fixes it, and scans again: 6 passed and 8 failed before, 16 passed and 4 failed after. Never deployed | none (Checkov scan before and after) | [terraform-s3-lab](https://github.com/twistedsol/terraform-s3-lab) |

Cloud audit alerts is different from the rest: it is built in the AWS console, not in code, and it connects to a live cloud account. So it has no automated tests. I tested it by creating a bucket and checking that the alert email arrived.

## What they have in common

- The code labs only read files. None of them connects to a server or changes anything.
- Same command line: `--format json`, `--min-severity`, `--fail-on`, and exit codes 0 (clean), 1 (findings) and 2 (bad input).
- Sample files: a risky one, an "okay but with gaps" one, and a hardened one.
- Tests run on Python 3.10 and 3.12 on every push.
- Every README lists the limits honestly.

## What I learned across them

- A security check is only useful if it is honest about what it cannot see. A tool that reads a file cannot know what the live system really does.
- The edge cases matter most: exactly 7 versus 8 characters, the first value winning in a config file, a secret that must never be printed.
- Tests also catch my own mistakes, such as a sample file saved in the wrong folder.
- Smaller steps and checking each result with a screenshot beat rushing.
