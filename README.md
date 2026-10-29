### hi, i'm tanish

[email](mailto:tanxena26@gmail.com) / [resume](https://tanxena.in/resume) / [linkedin](https://www.linkedin.com/in/tanxena) / [github](https://github.com/tanxena)

i'm a software developer at Xpurto, an AI construction-tech startup based in the US. i build products end to end there, from backend and web app to deployment, and i'm also the sole ML engineer. i also dive into Go projects about correctness: what happens when a process dies halfway through a write, or when a stream has to pass through a proxy without being buffered. i graduated from Netaji Subhas University of Technology (NSUT, erstwhile NSIT) in 2026 and live in Delhi.

#### what i do at Xpurto

i've shipped three products, owning each one from the backend to the models and the deployment:

- camera-based attendance to replace entry/exit biometrics. it reads the number on each worker's vest and stitches sightings from multiple cameras into hours worked and idle time. that's 100+ data points per worker per day, where an entry/exit scan gives 2.
- blueprint takeoff: YOLOv8 with SAHI finds walls, doors, windows and symbols in construction drawings, about 90% faster than a person doing it by hand.
- a material calculator you talk to, using Whisper and an LLM on top of the takeoff's wall data.

i built the multi-tenant web app they run in (Next.js, FastAPI, MongoDB, Redis, S3, Keycloak SSO) and the Kubernetes deployment through ArgoCD and GitHub Actions, and i got the API image build down from 25 minutes to 2.5. the vision side is 8 YOLOv8 models plus PaddleOCR, served on TensorRT.

#### side projects

[matchd](https://github.com/tanxena/matchd) is an order matching engine in Go, the core of an exchange. a single thread matches buy and sell orders by price and then by arrival time. the hard part is not losing anything. every order is written to a write-ahead log first, and the engine doesn't confirm it or publish the trade until that write is safely on disk. if the process dies, it restarts from the last snapshot, replays the log, and ends up in exactly the same state. to prove that, a test kills it with SIGKILL mid-run, 50 times over, and it has never lost an order it confirmed. matching takes 1.5 µs at p50 and 15 µs at p99. with disk writes grouped every millisecond, it handles 54k orders a second.

[llmgw](https://github.com/tanxena/llmgw) is a proxy that sits between your app and any OpenAI-compatible API, so you point the OpenAI SDK at it and nothing else changes. it passes streamed tokens through as they arrive, limits each API key by requests and tokens, and switches to a backup provider when one fails. it only retries before any output has reached the client, so a user never sees an answer restart halfway. at 6,000 requests a second it had zero errors and added 0.5 ms at p50. with 200 streams open at once, it adds under 0.9 ms before the first token at p99. i also profiled its memory and found that one 32 KiB buffer per stream was half of everything a stream used. cutting it to 4 KiB brought each stream from 188 to 138 KiB, and a side-by-side benchmark of both versions confirmed that latency didn't change.

earlier, my team won Smart India Hackathon 2023 with a [forensic logging system](https://github.com/tanxena/IT-System-Log-Analyser) for CRPF that chains log batches together on IPFS so tampering shows.

#### tools

Go, Python, TypeScript, C++, PyTorch, YOLO, TensorRT, OpenCV, FastAPI, Next.js, MongoDB, Postgres, Redis, Kubernetes, ArgoCD, Docker, Prometheus
