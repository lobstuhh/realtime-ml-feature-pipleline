# realtime-ml-feature-pipleline
Real-time ML systems need low-latency access to fresh features at inference time, but many pipelines only compute features in batch (hourly/daily), leading to stale predictions. This project builds a streaming feature pipeline that ingests events in real-time, computes features on the fly, stores them in a low-latency store for serving, and exposes them via an API for model inference — demonstrating the kind of infrastructure used in production recommendation, fraud-detection, and personalization systems.

[Data Source] 
     │
     ▼
[Kafka Producer] ──publishes──▶ [Kafka Topic: raw-events]
                                        │
                                        ▼
                              [Kafka Consumer]
                                        │
                          ┌─────────────┴─────────────┐
                          ▼                            ▼
                 [Feature Computation]         [Feast Feature Store]
                          │                            │
                          ▼                            ▼
                    [Redis Cache] ◀── low-latency reads ──┘
                          │
                          ▼
                  [FastAPI Serving Layer]
                          │
                          ▼
                  [XGBoost Model Inference]
