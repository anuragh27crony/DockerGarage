FROM ubuntu:16.04
RUN apt-get update --fix-missing && apt-get install -y \
build-essential \
cmake \
default-jre \
python2.7 \
nodejs \
wget \
git-core
RUN mkdir GitRepos && cd GitRepos && mkdir myfastcomp && cd myfastcomp && git clone https://github.com/kripken/emscripten-fastcomp && cd emscripten-fastcomp && git clone https://github.com/kripken/emscripten-fastcomp-clang tools/clang && mkdir build
RUN cd GitRepos/myfastcomp/emscripten-fastcomp/build && cmake .. -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="X86;JSBackend" -DLLVM_INCLUDE_EXAMPLES=OFF -DLLVM_INCLUDE_TESTS=OFF -DCLANG_INCLUDE_EXAMPLES=OFF -DCLANG_INCLUDE_TESTS=OFF && make
RUN ls -l
RUN sleep 10000
