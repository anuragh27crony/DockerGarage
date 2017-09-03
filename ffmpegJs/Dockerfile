FROM ubuntu:16.04
RUN apt-get update --fix-missing && apt-get install -y \
build-essential \
cmake \
default-jre \
python2.7 \
nodejs \
wget \
git-core

#CREATE DIRECTORIES FOR REPOS & EMSCRIPTEN FASTCOMPILER
RUN mkdir GitRepos GitRepos/myfastcomp && cd GitRepos 

#SETUP FASTCOMPILER & CLANG DIRECTORY
RUN cd GitRepos/myfastcomp && git clone https://github.com/kripken/emscripten-fastcomp && cd emscripten-fastcomp && git checkout -b fastcomp_1.12.1 1.12.1 && git clone https://github.com/kripken/emscripten-fastcomp-clang tools/clang && cd tools/clang && git checkout -b clang_1.12.1 1.12.1

# CREATE A BUILD DIRECTORY & PAREPARE MAKE FILE & START BUILDING.
RUN cd GitRepos/myfastcomp/emscripten-fastcomp && mkdir build && cd build && cmake .. -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="X86;JSBackend" -DLLVM_INCLUDE_EXAMPLES=OFF -DLLVM_INCLUDE_TESTS=OFF -DCLANG_INCLUDE_EXAMPLES=OFF -DCLANG_INCLUDE_TESTS=OFF && make

#SETUP EMSCRIPTEN REPO
RUN cd GitRepos && git clone https://github.com/kripken/emscripten.git && git checkout -b videoConverterBuild df11c6f1fd1636a355b83a1c48b3a890596e6a32

#CREATE .emscripten FILE AND CONFIGURE PATHS.
RUN cd GitRepos/emscripten && ./emcc --help


#CLONE VIDEO CONVERTER REPO AND START BUILDING
# RUN cd GitRepos && git clone https://github.com/bgrins/videoconverter.js.git && cd videoconverter.js/build && ./build_lgpl.sh
#/home/anuragm/GitRepos/emscripten:/home/anuragm/GitRepos/fastcomp/emscripten-fastcomp/build/bin:/usr/bin/nodejs

RUN sleep 10000