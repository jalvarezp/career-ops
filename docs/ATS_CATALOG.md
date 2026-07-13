# ATS Catalog

## Purpose

The ATS Catalog is the canonical knowledge base for provider detection in Career Ops.

It is built exclusively from **real production job URLs**, primarily discovered through LinkedIn.

Providers are identified from observed URL patterns, not vendor documentation.

---

# Detection Pipeline

```
URL
    ↓
Provider Detection
    ↓
Canonical URL
    ↓
Primary Key
    ↓
Normalized Job
```

Companies are consumers.

Providers are implementation targets.

---

# Provider Status

## UNKNOWN

Provider or backend has not yet been identified.

Research is required before implementation.

Examples:

* Ualá Careers
* Revolut Careers

---

## UNSUPPORTED

Provider identified.

URL pattern understood.

No detector/extractor implemented yet.

Examples:

* Pandapé
* Oracle Recruiting
* Eightfold
* Buk ATS

---

## SUPPORTED_MISCONFIGURED

Provider exists.

Company configuration is incorrect.

Typical causes:

* wrong slug
* invalid URL
* obsolete endpoint

Examples:

* dLocal
* Stori
* Pomelo
* Dock
* Kushki

---

## SUPPORTED_HEALTHY

Provider implemented and validated through real scanner execution.

Examples:

* Greenhouse
* Ashby
* Workday
* Amazon Jobs
* Avature
* BambooHR
* SuccessFactors
* Phenom
* Lever
* SmartRecruiters
* Workable
* Recruitee
* SolidJobs

---

# Supported Provider Evidence

## Greenhouse

Status:

SUPPORTED_HEALTHY

Patterns:

```
job-boards.greenhouse.io/{company}/jobs/{job_id}

stripe.com/jobs/listing/{slug}/{job_id}
```

Verified:

* Stripe
* EBANX
* Nubank
* Adyen
* Clara
* OKX
* Sezzle

Primary Key:

```
job_id
```

Normalize:

Remove:

* gh_src
* utm_*

---

## Ashby

Status:

SUPPORTED_HEALTHY

Patterns:

```
jobs.ashbyhq.com/{company}/{uuid}

jobs.ashbyhq.com/{company}/{uuid}/application
```

Verified:

* Kraken
* Checkout.com
* Airwallex
* Peek
* Belvo

Important:

Company slug may contain domains.

Examples:

```
kraken.com

checkout.com
```

Primary Key:

```
uuid
```

Normalize:

* remove `/application`
* remove tracking parameters

---

## Workday

Status:

SUPPORTED_HEALTHY

Patterns:

```
*.wd*.myworkdayjobs.com

/job/{slug}_{requisition_id}

/job/{slug}_{requisition_id}/apply
```

Verified:

* Visa
* BBVA

Primary Key:

```
requisition_id
```

Examples:

```
REF083146W

JR00107819
```

Normalize:

* remove `/apply`
* remove `share_id`
* remove `utm_*`

---

## Amazon Jobs

Status:

SUPPORTED_HEALTHY

Pattern:

```
amazon.jobs/en/jobs/{job_id}/{slug}
```

Primary Key:

```
job_id
```

Normalize:

Remove:

* cmpid
* utm_*
* ss

---

## Phenom + Workday

Status:

SUPPORTED_HEALTHY

Verified:

* Mastercard

Architecture:

```
Frontend
    ↓
Phenom

Backend
    ↓
Workday
```

Persist:

```
public_job_id

requisition_id
```

---

# Unsupported Providers

## Oracle Recruiting

Status:

UNSUPPORTED

Observed families:

```
*.fa.oraclecloud.com

/hcmUI/CandidateExperience
```

and

```
/en/sites/CX_1/job/{job_id}
```

Verified:

* JPMorgan Chase
* American Express

Implementation should support both URL families.

---

## Eightfold

Status:

UNSUPPORTED

Pattern:

```
{company}.eightfold.ai/careers/job/{job_id}
```

Verified:

* PayPal

Primary Key:

```
job_id
```

---

## Buk ATS

Status:

UNSUPPORTED

Pattern:

```
{company}.buk.mx/seleccions/{job_hash}/postular
```

Verified:

* Monato
* Contalink

Primary Key:

```
job_hash
```

Normalize:

Remove:

```
referrer
```

---

## Pandapé

Status:

UNSUPPORTED

Pattern:

```
{company}.pandape.computrabajo.com/Detail/{job_id}
```

Verified across multiple companies.

Primary Key:

```
job_id
```

---

# Discovery Backlog

## Ualá Careers

Pattern:

```
www.ua.la/sumate/{uuid}
```

Status:

UNKNOWN

Backend not yet identified.

---

## Revolut Careers

Pattern:

```
www.revolut.com/careers/position/{uuid}
```

Status:

UNKNOWN

Backend not yet identified.

---

# Discarded Fixtures

IBM

```
careers.ibm.com/en_US/closedjob
```

Reason:

* closed position
* no reusable identifier
* not suitable as regression evidence

---

# Repository Baseline

Canonical repository:

* Version: 1.18.0
* Branch: main
* Commit: 05bee6c4f8e111310b145ed99fdf4c678c000f83
* Provider modules: 59

Validated locally:

* `node doctor.mjs`
* `npm run scan`

Repository contents and commit SHA are the source of truth.
