## scAtteR server

The server component of the *scAtteR* system as described in the paper, "Characterizing Distributed Mobile Augmented Reality Applications at the Edge".

### Dependencies

  - `OpenCV` - tested with version 4.1
  - `CUDA` - tested with version 10.1
  - `FALCONN` - CPU LSH
  - `CudaSift` - CUDA version of SIFT with detection, extraction, matching
  - `Eigen` - Dense data structure
  - `VLFeat` - CPU GMM training

### Installation/Running

Prerequisites are for OpenCV and CUDA to be pre-installed before compiling the code

```sh
cd lib/cudasift 
sed -i 's/executable/library/g' CMakeLists.txt
cmake -D CUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-12.1 -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release .
make
cd ../../src/build
make
./server service[primary/sift/encoding/lsh/matching]
```

```sh
/home/ar_server/src/build/server primary & /home/SidecarQueue -entry=true -p=50001 -next=172.18.0.3:50002 -sidecar=0.0.0.0:5000
/home/ar_server/src/build/server sift & /home/SidecarQueue -p=50002 -next=127.0.0.1:50003 -sidecar=localhost:5000
/home/ar_server/src/build/server encoding & /home/SidecarQueue -entry=true -exit=false -p=50003  -next=0.0.0.0:50004 -sidecar=localhost:5000

```


### Deployment specifications

The servers were ran on a machine with the following specs:

- CPU: Intel Core i7-8700 3.20GHz x 12
- GPU: GeForce RTX 2080 Ti
- Memory: 32GB
- OS: Ubuntu 18.04.3 LTS

Plus the program was compiled with the following CUDA/GCC/G++ compilers:

- CUDA: 10.1
- GCC: 7.4.0
- G++: 7.4.0

### Testing notes

- 181019: Combined the cache processes and the recognition processes into one "server" - caching now appears to work 

- 031019: Disabled the cache server since somewhere in the caching pipeline, the cache server would crash. 



