# Security Policy

## Supported Versions

Security fixes are applied to the latest published version.

| Version | Supported |
| ------- | --------- |
| 1.x     | Yes       |
| < 1.0   | No        |

## Reporting a Vulnerability

Please do not report security vulnerabilities through public GitHub issues.

Contact the maintainer privately through the contact information available on the repository owner's GitHub profile.

Include the following information when possible:

- Clear description of the vulnerability.
- Affected file, section, or version.
- Steps to reproduce the issue.
- Potential security impact.
- Suggested mitigation or fix.
- A minimal proof of concept, when safe to share.

Do not include real credentials, production secrets, personal data, confidential source code, or sensitive customer information in the report.

## Scope

This policy covers:

- The contents of `SKILL.md`.
- Instructions that could encourage unsafe code or insecure architecture.
- Security weaknesses in examples or validation commands.
- Unsafe recommendations related to authentication, authorization, secrets, data access, or external integrations.
- Accidental exposure of sensitive information in the repository.

## Out of Scope

The following are generally outside the scope of this policy:

- Vulnerabilities in third-party libraries, frameworks, operating systems, or developer tools.
- General feature requests or documentation improvements.
- Disagreements with architectural recommendations.
- Incorrect configuration in a consuming project.
- Issues requiring access to systems or data outside this repository.

Third-party vulnerabilities should be reported to the maintainers of the affected project.

## Response Process

The maintainer will:

1. Review the report as soon as reasonably possible.
2. Confirm whether the issue affects this repository.
3. Assess the severity and potential impact.
4. Prepare a correction or mitigation when appropriate.
5. Publish an update or security notice when necessary.

Please allow reasonable time for investigation and remediation before publicly disclosing the issue.

## Responsible Use

This skill provides engineering guidance and does not guarantee that software built with it is secure.

Users remain responsible for:

- Reviewing generated or modified code.
- Validating authentication and authorization controls.
- Protecting credentials and sensitive data.
- Running appropriate tests and security checks.
- Reviewing dependencies and deployment configuration.
- Confirming that production systems meet their own security requirements.

## Security Disclaimer

The maintainer does not guarantee that the recommendations in this skill are complete, current, or suitable for every system.

Security decisions should be reviewed according to the risk, regulatory requirements, architecture, and operational context of each project.
