# Diagnose_EDA

The following EDA diagnosys require the following playbook per step:

. 01_check_activation_pod.yml (Step 1) — lists activation pods, checks whether each referenced activation-secret-N actually exists, and surfaces pull-related events. This is the one that pinpoints the FailedToRetrieveImagePullSecret cause.

. 02_fix_de_registry_credential.yml (Fix 1A) — creates the Decision Environment Container Registry credential, attaches it to the DE, and restarts the activation. The only state-changing playbook.

. 03_check_event_stream_arrival.yml (Steps 2–3) — reads events_received, last_event_received_at, and the test-mode test_headers/test_content capture via the API.

. 04_bisect_event_stream_auth.yml (Step 4) — fires bare token / Bearer / custom header at the stream and prints the three status codes; whichever is 2xx wins.

. 05_collect_eda_logs.yml (Step 7) — OpenShift pod logs plus containerized journalctl --user and execution-plane podman ps.

. 06_check_versions.yml (Step 7) — EDA + gateway API reachability and the installed automation-eda-controller RPM.

. 07_audit_decision_environments.yml: read-only — lists every EDA Decision Environment via /api/eda/v1/decision-environments/ and flags any whose image_url matches stale_regex