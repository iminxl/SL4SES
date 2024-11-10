# Swarm Learning for Temporal and Spatial Series Data in Energy Systems

Here is the code for "Swarm Learning for Temporal and Spatial Series Data in Energy Systems: A Decentralized Collaborative Learning Design Based on Blockchain." This repository includes machine learning and blockchain applications, with specific setup and installation instructions for the Swarm Learning blockchain application on macOS, based on Fisco Bcos.

## Installing Fisco Bcos

### 1. Install macOS dependencies
```bash
brew install openssl@1.1 curl wget
```

### 2. Create working directory
```bash
cd ~ && mkdir -p fisco && cd fisco
```

### 3. Download chain-building script (version 3.1 recommended)
```bash
curl -#LO https://github.com/FISCO-BCOS/FISCO-BCOS/releases/download/v3.11.0/build_chain.sh && chmod u+x build_chain.sh
```
**Note**: If GitHub access is slow, use the following command:
```bash
curl -#LO https://gitee.com/FISCO-BCOS/FISCO-BCOS/releases/download/v3.11.0/build_chain.sh && chmod u+x build_chain.sh
```

### 4. Generate a single-group, 4-node FISCO chain
```bash
bash build_chain.sh -l 127.0.0.1:4 -p 30300,20200
```

### 5. Start all nodes
```bash
bash nodes/127.0.0.1/start_all.sh
```

### 6. Install Java (Java 14 recommended)

### 7. Download the console
```bash
cd ~/fisco && curl -LO https://github.com/FISCO-BCOS/console/releases/download/v3.1.0/download_console.sh && bash download_console.sh
```

### 8. Copy console configuration files
```bash
cp -n console/conf/config-example.toml console/conf/config.toml
```

### 9. Configure console certificates
With SSL enabled, copy the generated SDK certificates for console use:
```bash
cp -r nodes/127.0.0.1/sdk/* console/conf
```

### 10. Launch the console to test the setup
```bash
cd ~/fisco/console && bash start.sh
```
In the console, type `exit` to exit.

## Compile Swarm Contract and Create Blockchain Application Project

### 1. Compile the Smart Contract
Place `Swarm.sol` in `~/fisco/console/contracts/solidity/` and generate the Java contract class:
```bash
cd ~/fisco/console/
bash contract2java.sh solidity -s contracts/solidity/Swarm.sol -p org.fisco.bcos.swarm.contract
```

### 2. Install IntelliJ IDE and configure the project

### 3. Configure SDK certificates
```bash
cd ~/fisco
mkdir -p swarm-app-2.0/src/test/resources/conf
cp -r nodes/127.0.0.1/sdk/* swarm-app-2.0/src/test/resources/conf
cp -r nodes/127.0.0.1/sdk/* swarm-app-2.0/src/main/resources/conf
```

### 4. Import compiled contract Java class into project
```bash
cd ~/fisco
cp console/contracts/sdk/java/org/fisco/bcos/swarm/contract/Swarm.java asset-app-3.0/src/main/java/org/fisco/bcos/swarm/contract/Swarm.java
```

### 5. Build the project
```bash
cd ~/fisco/swarm-app-2.0
./gradlew build
```

### 6. Deploy Swarm.sol contract
```bash
cd dist
bash asset_run.sh deploy 2 100 ~/Desktop/1.txt
```
Where `1.txt` contains account names. For example:
```
A1
A2
B1
B2
C1
D1
