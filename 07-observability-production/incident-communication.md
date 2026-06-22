# Production Incident Communication

Production incident communication is about explaining impact, current status, mitigation, and next steps clearly.

For senior backend interviews, do not only say:

```text
I will rollback.
```

Rollback may be the right mitigation, but the interviewer also wants to hear how you communicate and coordinate.

## Incident Flow

Use this sequence:

```text
1. Acknowledge the incident.
2. Assess customer/business impact.
3. Stop the bleeding.
4. Communicate current status.
5. Mitigate with rollback, feature flag, config change, or traffic shift.
6. Monitor recovery.
7. Share next update time.
8. Do root cause analysis after service is stable.
9. Add preventive actions.
10. Share postmortem if needed.
```

## First Priorities

When something breaks in production:

```text
impact first
mitigation second
root cause third
prevention fourth
```

Do not spend too long finding perfect root cause while users are still impacted.

Stop the bleeding first.

## What To Communicate

For non-technical stakeholders, include:

```text
what happened
who is affected
current status
mitigation being applied
workaround if any
ETA or next update time
```

Avoid deep technical detail unless they ask for it.

## Non-Technical Status Update Template

Use this format:

```text
We are currently seeing <issue> affecting <users/feature>.
The issue started around <time>.
The team has identified <current understanding>.
We are mitigating by <rollback/disable feature/fix config>.
Current user impact is <impact>.
Workaround: <workaround if available>.
Next update: <time>.
```

Example:

```text
We are currently seeing failures in the checkout flow affecting some users.
The issue started around 11:10 AM after the latest release.
The team has identified that the release is causing validation failures.
We have rolled back the release and are monitoring recovery.
Users can retry checkout after a few minutes.
Next update will be shared in 30 minutes.
```

## Technical Incident Checklist

Use this internally with engineers:

```text
Check logs.
Check metrics.
Check traces.
Check recent deploys.
Check dependency health.
Check feature flags/config changes.
Identify blast radius.
Rollback or mitigate.
Monitor error rate and latency.
Confirm recovery.
Start root cause analysis.
Add prevention.
```

## Good Interview Answer

Question:

> If you released something and it failed, how would you inform a non-technical person and handle next steps?

Answer:

> First I would acknowledge the incident and assess the impact: which feature is failing, how many users are affected, and when it started. If the issue is linked to my release, I would stop the bleeding by rolling back, disabling the feature flag, or applying a safe mitigation. For non-technical stakeholders, I would communicate in simple language: what is affected, current status, what we are doing, workaround if available, and when the next update will come. After recovery, I would do root cause analysis and share preventive actions like better tests, staged rollout, alerts, or validation checks.

## After Resolution Update

Use this:

```text
The issue has been resolved.
Impact was <summary>.
Mitigation was <rollback/fix/config change>.
Systems are healthy and metrics have returned to normal.
Root cause analysis is in progress.
We will share preventive actions after the postmortem.
```

Example:

```text
The checkout issue has been resolved.
Some users saw payment failures between 11:10 AM and 11:28 AM.
We rolled back the faulty release and confirmed error rates returned to normal.
Root cause analysis is in progress.
We will add preventive actions after the postmortem.
```

## Postmortem Structure

Keep it blameless.

```text
Summary
Impact
Timeline
Root cause
What went well
What went poorly
Action items
Owners
Due dates
```

Example preventive actions:

```text
add automated regression test
add alert on error rate
add dashboard for affected flow
use staged rollout/canary
use feature flag kill switch
add runbook
improve deployment checklist
```

## Interview Line

> In production incidents, I focus first on user impact and mitigation, then communication, then root cause and prevention. I avoid overloading non-technical stakeholders with implementation details, but I keep them updated on impact, current status, workaround, and next update time.

## Common Mistakes

```text
Only saying rollback.
Not mentioning user impact.
Not giving next update time.
Trying to explain too much technical detail to non-technical people.
Doing root cause analysis before mitigation while users are still affected.
Blaming a person instead of describing system/process gaps.
```

