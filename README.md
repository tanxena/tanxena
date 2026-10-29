### Hi, I'm Tanish

I'm a software developer at Xpurto, an AI construction-tech startup based in the US. I build products end to end there, from backend and web app to deployment, and I'm also the sole ML engineer. I also dive into Go projects about correctness: what happens when a process dies halfway through a write, or when a stream has to pass through a proxy without being buffered. I graduated from Netaji Subhas University of Technology (NSUT, erstwhile NSIT) in 2026 and live in Delhi.

#### What I do at Xpurto

I've shipped three products, owning each one from the backend to the models and the deployment:

- Camera-based attendance to replace entry/exit biometrics. It reads the number on each worker's vest and stitches sightings from multiple cameras into hours worked and idle time. That's 100+ data points per worker per day, where an entry/exit scan gives 2.
- Blueprint takeoff: YOLOv8 with SAHI finds walls, doors, windows and symbols in construction drawings, about 90% faster than a person doing it by hand.
- A material calculator you talk to, using Whisper and an LLM on top of the takeoff's wall data.

I built the multi-tenant web app they run in (Next.js, FastAPI, MongoDB, Redis, S3, Keycloak SSO) and the Kubernetes deployment through ArgoCD and GitHub Actions, and I got the API image build down from 25 minutes to 2.5. The vision side is 8 YOLOv8 models plus PaddleOCR, served on TensorRT.

#### Side projects

[matchd](https://github.com/tanxena/matchd) is an exchange-style order matching engine in Go. Matching was the easy part. The real work was durability: every order goes into a checksummed write-ahead log, and nobody hears about a trade until it's on disk. After a crash, the engine replays the log on top of a snapshot and rebuilds exactly the same state. I killed it with SIGKILL 50 times mid-run and it never lost an acknowledged order. Matching takes 1.5 µs at p50 and 15 µs at p99, and it handles 54k orders a second when fsyncs are batched per millisecond.

[llmgw](https://github.com/tanxena/llmgw) is a gateway you can drop in front of any OpenAI-compatible API. It streams tokens straight through, enforces rate limits per key, and fails over to another upstream, but only before the first byte has gone out, so a client never sees a stream restart. It handled 6,000 requests a second with zero errors, adding half a millisecond at p50. With 200 streams open, it adds under 0.9 ms at p99 before the first byte. Profiling showed a single copy buffer was half of each stream's memory, and shrinking it saved about 50 KiB per stream. The first benchmark after the fix suggested it had hurt latency. A rerun in reverse order on a quiet machine showed the "regression" was my laptop doing other things. [Decision 017](https://github.com/tanxena/llmgw/blob/main/docs/decisions/017-buffer-size-ab.md) has the details.

Earlier, my team won Smart India Hackathon 2023 with a [forensic logging system](https://github.com/tanxena/IT-System-Log-Analyser) for CRPF that chains log batches together on IPFS so tampering shows.

#### Tools

Go, Python, TypeScript, C++, PyTorch, YOLO, TensorRT, OpenCV, FastAPI, Next.js, MongoDB, Postgres, Redis, Kubernetes, ArgoCD, Docker, Prometheus

#### Get in touch

Email me at tanxena26@gmail.com, find me on [LinkedIn](https://www.linkedin.com/in/tanxena), or see more at [tanxena.in](https://tanxena.in/).
