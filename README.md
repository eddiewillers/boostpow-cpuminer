# CPU Miner for Boost

This c++ program depends on the Gigamonkey repository and is designed to pull in all its dependencies via conan. It shall also be pulished as a conan package so that other c++ programs can easily pull it in as a dependency where needed.

For distribution it is meant that this repository may be built for mac, linux, and windows, as well as packaged for Docker. Ideally also there will be an apt package repository.

## Run from docker

This is the easiest way to use BoostMiner. Docker needs to be installed. For linux users, be sure to follow the [post install](https://docs.docker.com/engine/install/linux-postinstall/) instructions. Otherwise, the following instructions need to be run with `sudo`, which is not the recommended way to use docker.

1. Download the latest master branch of this software. 
2. `docker build -t boostpow .` (This should take a while.)
3. `docker run boostpow ./BoostMiner help` This will show the help message for the mining software.
   See heading *Usage* below. 

## Installation

We use conan package manager to install the dependencies and build the applicatoin
```bash
conan config set general.revisions_enabled=True

conan remote add proofofwork https://conan.pow.co/artifactory/api/conan/conan
```
Is needed to add the correct repository to conan.

`conan install . -r=proofofwork` will install gigamonkey and its dependencies from artifactory

`conan build .` will output the BoostMiner program compiled for your platform

## Usage

```
./BoostMiner help
Input should be
	<method> <args>... --<option>=<value>...
where method is
	spend      -- create a Boost script.
	redeem     -- mine and redeem an existing boost output.
	mine       -- call the pow.co API to get jobs to mine.
For method "spend" provide the following as options or as arguments in order
	content    -- hex for correct order, hexidecimal for reversed.
	difficulty -- a positive number.
	topic      -- (optional) string max 20 bytes.
	data       -- (optional) string, any size.
	address    -- (optional) If provided, a boost contract output will be created. Otherwise it will be boost bounty.
For method "redeem", provide the following as options or as arguments in order
	txid       -- txid of the tx that contains this output.
	index      -- index of the output within that tx.
	wif        -- private key that will be used to redeem this output.
	script     -- (optional) boost output script, hex or bip 276.
	value      -- (optional) value in satoshis of the output.
	address    -- (optional) your address where you will put the redeemed sats.
	              If not provided, addresses will be generated from the key.
For method "mine", provide the following as options or as arguments in order
	key        -- WIF or HD private key that will be used to redeem outputs.
	address    -- (optional) your address where you will put the redeemed sats.
	              If not provided, addresses will be generated from the key.
additional available options are
	api_host          -- Host to call for Boost API. Default is pow.co
	threads           -- Number of threads to mine with. Default is 1.
	min_profitability -- Boost jobs with less than this sats/difficulty will be ignored.
	max_difficulty    -- Boost jobs above this difficulty will be ignored.
	fee_rate          -- Sats per byte of the final transaction.
	                     If not provided we get a fee quote from Gorilla Pool.
```



