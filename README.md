# iam-policy-analyzer

Browser-based analyzer for AWS IAM policies. Paste a policy document, get back a list of findings: privilege escalation paths, overly broad grants, missing conditions, and trust policy issues.

**Live demo:** https://0xelitesystem.github.io/iam-policy-analyzer/

Single HTML file. No build step, no dependencies, no network calls. Open `index.html` or use the hosted version.

## Use

1. Paste an IAM policy or trust policy JSON document, or click one of the Sample buttons.
2. Click **Analyze**.
3. Work through the findings by severity, from critical down to info.
4. Fix the policy and run it again until the findings you care about are gone.

## Why this exists

IAM policies are easy to over-grant and hard to review in a pull request. This tool applies a static rule set to the JSON you paste and flags escalation paths, wildcards, and missing conditions. It is one HTML file with inline CSS and JavaScript: no account, no tracking, no analytics, no external scripts or fonts, and it works offline. MIT licensed, so you can fork it, self-host it, or read every line.

## What it checks

- **Privilege escalation actions**, `iam:PassRole`, `iam:CreateAccessKey`, `lambda:UpdateFunctionCode`, `ec2:RunInstances`, `cloudformation:CreateStack`, and 30+ other actions known to enable lateral movement when granted on `Resource: *`.
- **Wildcards**, `Action: "*"`, `Resource: "*"`, and service-level wildcards like `s3:*` flagged with severity scaled by the action's blast radius.
- **Missing conditions**, Sensitive actions granted without `aws:SourceIp`, `aws:MultiFactorAuthPresent`, `aws:SecureTransport`, or `aws:PrincipalOrgID` constraints.
- **Trust policies**, `Principal: "*"` in trust documents, missing `aws:SourceAccount` / `aws:SourceArn` on cross-account roles, and confused-deputy patterns.
- **NotAction / NotResource**, Flagged because they allow more than they appear to.

## Severity scale

| Tag | Meaning |
|-----|---------|
| critical | Direct privilege escalation path or full account compromise possible |
| high | Sensitive permission with no scoping; common attacker target |
| medium | Broad grant that should be narrowed but doesn't directly escalate |
| low | Hygiene issue (missing condition, deprecated pattern) |
| info | Informational, e.g. unrecognized action |

## What this is NOT

Not a replacement for AWS Access Analyzer or Cloudsplaining. It does not simulate policy evaluation, expand managed policies, or check against actual account state. It reads the JSON you paste and applies a static rule set. Use it for fast review of policy diffs in pull requests, not as a sole source of truth.

## Privacy

Everything runs in the browser. The policy you paste never leaves the page. No analytics, no network requests after page load, and the policy is never stored. The only thing written to storage is your light or dark theme choice, saved in localStorage under the key `theme`.

## Run locally

```bash
git clone https://github.com/0xelitesystem/iam-policy-analyzer
cd iam-policy-analyzer
```

Then open `index.html` in any modern browser. Or serve the folder with `python -m http.server 8000` and visit http://localhost:8000.

## Build

No build step. The whole tool is one `index.html` with no dependencies, so there is nothing to install or compile.

## Samples

Three sample policies are loaded via the buttons in the header: a privilege-escalation policy, an overly broad S3 grant, and a permissive cross-account trust policy.

## Related repositories

Part of a 10-repo security audit set.

Browser-based audit tools:
- [terraform-security-linter](https://github.com/0xelitesystem/terraform-security-linter)
- [kubernetes-manifest-security-scanner](https://github.com/0xelitesystem/kubernetes-manifest-security-scanner)
- [session-cookie-auditor](https://github.com/0xelitesystem/session-cookie-auditor)
- [regex-redos-checker](https://github.com/0xelitesystem/regex-redos-checker)

Reference collections:
- [incident-response-runbooks](https://github.com/0xelitesystem/incident-response-runbooks)
- [ai-llm-security-audit](https://github.com/0xelitesystem/ai-llm-security-audit)
- [api-security-audit-checklist](https://github.com/0xelitesystem/api-security-audit-checklist)
- [secrets-leak-response-runbook](https://github.com/0xelitesystem/secrets-leak-response-runbook)
- [threat-modeling-worksheets](https://github.com/0xelitesystem/threat-modeling-worksheets)

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. See [LICENSE](LICENSE).
