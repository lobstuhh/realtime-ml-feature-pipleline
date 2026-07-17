# realtime-ml-feature-pipleline
Real time ML systems need low latency access to fresh features at inference time, but many pipelines only compute features in batch (hourly/daily), leading to stale predictions. This project builds a streaming feature pipeline that ingests events in real time, computes features on the fly, stores them in a low latency store for serving, and exposes them via an API for model inference demonstrating the kind of infrastructure used in production recommendation, fraud detection, and personalization systems.

1. Events (simulated real world data) get published to Kafka
2. A consumer picks them up and computes features (e.g., rolling averages, counts, ratios)
3. Features get written to both Feast (for versioning/governance) and Redis (for fast reads)
4. A FastAPI endpoint serves those features + runs them through an XGBoost model for real time predictions
