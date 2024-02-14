## SSV_NODE
This repository contains instructions for setting up the SSV_NODE environment.

### Clone the repository:
```
git clone https://github.com/goooodnes/SSV_NODE.git
cd SSV_NODE
```

Alternative chekpoint if standart checkpoint is blocked
```
lighthouse:
  --checkpoint-sync-url=https://checkpoint-sync.holesky.ethpandaops.io/
```

### Create a password file:
```
echo "<MY_OPERATOR_PASSWORD>" >> password
```

### Key pair generation and encryption:
```
docker run --name ssv-node-key-generation -v "$(pwd)/password":/password -it "bloxstaking/ssv-node:latest" /go/bin/ssvnode generate-operator-keys --password-file=password && docker cp ssv-node-key-generation:/encrypted_private_key.json ./encrypted_private_key.json && docker rm ssv-node-key-generation
```

### Start the network:
```
docker-compose up -d
```

* Make sure to replace <MY_OPERATOR_PASSWORD> with your actual operator password. Additionally, ensure that you have Docker and Docker Compose installed before running the commands.

### Launch DKG ceremony:
```
docker run --restart unless-stopped --name ssv_dkg -p 3030:3030 -v "$HOME/SSV_NODE":/data -it "bloxstaking/ssv-dkg:latest" /app start-operator --configPath /data/operator.yaml
```
