# Deploy log

## 2026-10-13: dev environment live (account 822491881190, us-east-1)

* Bootstrap applied (state bucket `unfold-tfstate-822491881190`, lock table, plan/apply/images roles; the account's existing GitHub OIDC provider is reused).
* `envs/dev` applied: 72 resources, images `7d3d0bb` (api, worker, exporter) from ECR.
* New-account limits met on the first apply: Lambda memory capped at 3008 MB, concurrency quota 10 (reserved concurrency off: `api_memory_mb=3008`, `api_reserved_concurrency=-1`).
* Frontend: Vercel project `unfold-roan`, env `API_INTERNAL_URL`, `PROXY_HMAC_SECRET`, `UPLOAD_BUCKET_HOST`, `PUBLIC_WEB_ORIGIN`. The first secret (base64) did not verify in Vercel; a hex secret replaced it.
* Smoke through `https://unfold-roan.vercel.app`: session 201; GitHub project `psf/requests` (1 070 chunks) analysed on Fargate in 7 min 36 s, status `succeeded`, guide ready (15 min read); chat answer with evidence citations in 69 s (cold start included); no ECS task running afterwards (idle check).
* Unsigned direct calls to the Function URL return 403 (signed-only in production).

Not yet run: failure drill (kill a task, DLQ), cleanup/expiry check, Cost Explorer after 24-48 h, ZIP upload through the browser, PDF export, prod environment.
