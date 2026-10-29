### hi, i'm tanish

i'm a software developer at Xpurto, a construction-tech startup based in the US. i build products end to end there, from backend and web app to deployment, and i'm also the only one working on ML. evenings go into Go projects about correctness: what happens when a process dies halfway through a write, or when a stream has to pass through a proxy without being buffered. i graduated from NSUT in 2026 and live in Delhi.

#### what i do at Xpurto

i've shipped three products, owning each one from the backend to the models and the deployment:

- camera-based attendance to replace entry/exit biometrics. it reads the number on each worker's vest and stitches sightings from multiple cameras into hours worked and idle time. that's 100+ data points per worker per day, where an entry/exit scan gives 2.
- blueprint takeoff: YOLOv8 with SAHI finds walls, doors, windows and symbols in construction drawings, about 90% faster than a person doing it by hand.
- a material calculator you talk to, using Whisper and an LLM on top of the takeoff's wall data.

i built the multi-tenant web app they run in (Next.js, FastAPI, MongoDB, Redis, S3, Keycloak SSO) and the Kubernetes deployment through ArgoCD and GitHub Actions, and i got the API image build down from 25 minutes to 2.5. the vision side is 8 YOLOv8 models plus PaddleOCR, served on TensorRT.

#### side projects

[matchd](https://github.com/tanxena/matchd) is an exchange-style order matching engine in Go. matching was the easy part. the real work was durability: every order goes into a checksummed write-ahead log, and nobody hears about a trade until it's on disk. after a crash, the engine replays the log on top of a snapshot and rebuilds exactly the same state. i killed it with SIGKILL 50 times mid-run and it never lost an acknowledged order. matching takes 1.5 µs at p50 and 15 µs at p99, and it handles 54k orders a second when fsyncs are batched per millisecond.

[llmgw](https://github.com/tanxena/llmgw) is a gateway you can drop in front of any OpenAI-compatible API. it streams tokens straight through, enforces rate limits per key, and fails over to another upstream, but only before the first byte has gone out, so a client never sees a stream restart. it handled 6,000 requests a second with zero errors, adding half a millisecond at p50. with 200 streams open, it adds under 0.9 ms at p99 before the first byte. profiling showed a single copy buffer was half of each stream's memory, and shrinking it saved about 50 KiB per stream. the first benchmark after the fix suggested it had hurt latency. a rerun in reverse order on a quiet machine showed the "regression" was my laptop doing other things. [decision 017](https://github.com/tanxena/llmgw/blob/main/docs/decisions/017-buffer-size-ab.md) has the details.

earlier, my team won Smart India Hackathon 2023 with a [forensic logging system](https://github.com/tanxena/IT-System-Log-Analyser) for CRPF that chains log batches together on IPFS so tampering shows.

#### on building with AI

most of my code gets written by Claude Code now, so the job is making sure it's right. i write the spec first. after the code is done, a fresh session that didn't write it reviews it against that spec, and i settle any disagreements. benchmarks i run myself. a few things only got caught that way, like benchmark processes pinned to two cores while the Go runtime still assumed eight. the specs and review notes are committed in each repo.

#### tools

Go, Python, TypeScript, C++, PyTorch, YOLO, TensorRT, OpenCV, FastAPI, Next.js, MongoDB, Postgres, Redis, Kubernetes, ArgoCD, Docker, Prometheus

#### get in touch

[email](mailto:tanxena26@gmail.com) / [resume](https://tanxena.in/resume) / [linkedin](https://www.linkedin.com/in/tanxena) / [github](https://github.com/tanxena)
