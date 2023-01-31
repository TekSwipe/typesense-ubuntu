## Set up typesense with digital ocean ubuntu server

# Step 1: Download & Install Typesense

Once you have logged into your ubuntu server on digital ocean, run the following commands to download and install typesense

Download typsense
```shell
curl -O https://dl.typesense.org/releases/0.24.0/typesense-server-0.24.0-linux-amd64.tar.gz
```

Install and run
```shell
tar -xzf typesense-server-0.24.0-linux-amd64.tar.gz
```

Now we should have typesense installed.

This is an optional command we can run to see if typesense has installed correctly
```shell
curl http://localhost:8108/health
```