## Start services
```docker-compose up```

## Launch a shell in the container (default user)
```docker-compose exec nifi bash```

## One-time configuration to wire NiFi & Registry
```
cd /home/nifi

# default nifi docker image doesn't have registry settings, let's add those
cp .nifi-cli.nifi.properties .nifi-cli.registry.properties

# 'nifi' and 'registry' are service hostnames in the docker-compose image
sed -i s/nifi:8080/registry:18080/g .nifi-cli.registry.properties

# tell CLI where our properties file is so we don't have to type them every time
cd /opt/nifi/nifi-toolkit-current/bin
./cli.sh session set nifi.reg.props /home/nifi/.nifi-cli.registry.properties 

# link nifi & registry in 1 line (because who needs a UI with a dozen clicks?)
./cli.sh  nifi create-reg-client --registryClientName "Local Registry" --registryClientUrl http://registry:18080
```


## Quickly import the flow into registry and deploy to nifi
```
./cli.sh demo quick-import --input file:///tmp/flow_tx_enrichment.json
```

TIP: --input can be any URL, e.g. use https:// and a full path to raw flowfile json on github
```https://raw.githubusercontent.com/aperepel/nifi-workshop-2019/master/workshop-flow.json```

## Launch a root shell in the 'nifi' container
```docker exec -it --user root nifi bash```

## Copy from local to the 'nifi' container
```docker cp ./local-file.txt nifi:/tmp```

## Copy from 'nifi' container to local
```docker cp nifi:/tmp/remote-file.txt ./local-file.txt```
