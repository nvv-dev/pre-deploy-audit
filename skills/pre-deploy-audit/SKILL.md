---
name: pre-deploy-audit
description: Use before building a release, deploying a service or site, or promoting from staging to production. Checks for secrets, unsafe defaults and missing configuration, and prepares a deploy with a health check and a ready rollback. Does not deploy by itself.
---

# Pre-deploy audit

Catch the problems that pass every test but break or expose production. This skill prepares and checks; the deploy itself goes through the project's normal path (CI/CD, GitOps, store console) with human approval.

## 1. Blockers (any one stops the release)

- Secrets in the repository or the build artifact: `.env` files, private keys, tokens, service-account JSON, keystores. Check the diff since the last release and the packaged artifact, not just the source tree.
- Debug or test settings reachable in the release build: test API keys or ad unit IDs, debug flags, verbose logging of personal data, QA panels or reset buttons.
- Services that should be internal listening on `0.0.0.0` instead of `127.0.0.1` or the private network.
- Required environment variables or config keys referenced in code but missing from the deployment config.
- Database migrations that are destructive or not backwards compatible with the version still running during rollout.
- Version not bumped where the platform requires it (mobile `versionCode`, package version, image tag).

## 2. Warnings (release can proceed if the human accepts them)

- Dependencies with known high/critical vulnerabilities (from the project's scanner).
- Missing or failing health endpoint.
- No monitoring or alert for the new feature's failure mode.
- Large bundle or image size increase versus the previous release.

## 3. Deploy plan

Write it before deploying:

```
Target: <environment> · Version: <x.y.z / commit>
Order: staging first → health check → production
Health check: <URL/command> must return <expected> within <time>
Rollback: <exact command or console action>, tested on: <date or "not tested">
Watch after deploy: <errors, latency, key metric> for <duration>
```

## Output

Blockers found (or "none"), warnings with the human's decision, and the deploy plan. If a blocker is found, stop and report; do not work around it.

## Known failures

_Record real mistakes here with a date and the rule that prevents them._
