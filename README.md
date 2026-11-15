# Simple-Jenkins-Pipeline-for-CI-CD

A Flask application packaged and validated by a Jenkins pipeline. The app exposes
`/` for its greeting and `/health` for a health check.

## Run tests

```sh
python3 -m pip install -r requirements.txt
python3 -m pytest tests/ -v
```

The Jenkins pipeline installs the dependencies, runs the test suite, creates a
build artifact, and performs a deployment simulation.
