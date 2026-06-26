# SDLC Policy Template

## What This Is

The Software Development Life Cycle (SDLC) Policy defines how security is integrated into every phase of software development. It's the document that ensures security isn't a "final check before release" but is built in from the requirements phase. This is a SOC 2 CC8.2 control (Change Management) area.

## What It Covers

- SDLC phases: Requirements → Design → Development → Testing → Deployment → Maintenance
- Security activities per phase (threat modeling, SAST, peer review, pen testing)
- Secure coding standards
- Environment separation (dev/staging/prod)
- Production data restrictions in lower environments
- Outsourced development requirements

## Gotchas People Get Wrong

**1. "We do peer review" is not the same as "we do security peer review."** Most code reviews focus on functionality and style. Security review requires looking for specific vulnerability patterns (OWASP Top 10, auth bypass, injection). Either train all reviewers on security review or have a security specialist review high-risk changes.

**2. Threat modeling sounds intimidating but doesn't have to be.** For a SaaS application, a 2-hour whiteboard session using STRIDE or a simple data flow diagram with trust boundaries provides 80% of the value. Don't let "threat modeling" become a 40-page document nobody reads.

**3. "Production data must not be used in test environments" is violated constantly.** Developers copy production databases to staging to debug issues. The policy needs teeth: if you must use production-like data, it must be anonymized. Provide a tool or script that sanitizes production data for test use. If you only say "don't do it" without providing an alternative, people will do it anyway.

**4. SAST tools generate noise.** Running a SAST scanner and dumping 500 findings on developers is counterproductive. Tune the tool, suppress false positives, and integrate findings into the developer workflow (IDE plugins, PR comments). Developers should see 5 actionable findings, not 500.

**5. Environment separation is about blast radius, not security theater.** Separate AWS accounts (or at minimum separate VPCs) for dev/staging/prod are ideal. If cost forces you to use one account, use separate IAM roles with strict boundaries. A developer's staging credentials should not work in production, period.

## Implementation Advice

- **Start with the CI/CD pipeline.** SAST, dependency scanning, and secret detection in the pipeline are the highest-leverage security improvements. They run automatically on every commit and don't require developer behavior change.
- **Provide a data anonymization tool.** If developers need production-like data for testing, give them a one-command tool that scrubs PII. Make doing the right thing easier than doing the wrong thing.
- **Annual secure coding training with real examples.** Use your own codebase's past vulnerabilities as training material. "Remember that SQL injection we shipped last year? Here's how the code review should have caught it."
- **The SDLC policy and the Change Management policy should reference each other.** SDLC covers how code is built. Change Management covers how it's deployed. Together they form a complete control.
