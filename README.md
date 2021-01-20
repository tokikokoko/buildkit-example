# buildkit-example
## Usage
### No buildkit
```bash
# Build
DOCKER_BUILDKIT=0 docker build --no-cache -t bench .

# Test image
docker run --rm -p 8080:8080 bench
```

### Use buildkit
```bash
# Build
DOCKER_BUILDKIT=1 docker build --no-cache -t bench .

# Test image
docker run --rm -p 8080:8080 bench
```

