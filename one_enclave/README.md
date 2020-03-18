# One Enclave Attestation

The server host process is what drives the enclave app. It is responsible for managing the lifetime of the enclave and invoking enclave ECALLs but should be considered an untrusted component that is never allowed to handle plaintext secrets intended for the enclave.

![Remote Attestation](images/one_enclave_attestation_sample_details.jpg)

## Build and run

You must have CMake and protobuf installed.
Then install gRPC following the guide here https://github.com/grpc/grpc/blob/v1.27.0/BUILDING.md
Last tested version with this sample is 1.27

### CMake

Requirements:
- Requirements from [OpenEnclave](https://github.com/openenclave/openenclave/tree/0.8.2)
- virtualenv (or equivalent)
- Python3

To run the sample you have to get openenclave-curl library in this directory first. Please git clone the repository and follow the build steps in the openenclave-curl/README.md:
```bash
git clone git@github.com:openenclave/openenclave-curl.git
git checkout c430d3e
```

For server:
```bash
cd one_enclave
mkdir build && cd build
cmake ..
make run
```

For client:
```bash
cd one_enclave
cd client
./setup.sh
python3 client.py
```
Note: make sure to have started the server before running the client.