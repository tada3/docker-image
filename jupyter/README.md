＃ Docker image for Jupyter Notebook

```
docker build . -t jupyter --network=host \
    --build-arg uid=11000 \
    --build-arg gid=11000 \
```