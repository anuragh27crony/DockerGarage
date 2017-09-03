FROM ubuntu:16.04
RUN apt-get update --fix-missing && apt-get install -y \
build-essential \
cmake \
default-jre \
python2.7 \
python-minimal \
nodejs \
wget \
git-core

#CREATE DIRECTORIES FOR REPOS & EMSCRIPTEN FASTCOMPILER
RUN mkdir GitRepos GitRepos/myfastcomp && cd GitRepos 

#SETUP FASTCOMPILER & CLANG DIRECTORY
RUN cd GitRepos/myfastcomp && git clone https://github.com/kripken/emscripten-fastcomp && cd emscripten-fastcomp && git checkout -b fastcomp_1.12.1 1.12.1 && git clone https://github.com/kripken/emscripten-fastcomp-clang tools/clang && cd tools/clang && git checkout -b clang_1.12.1 1.12.1

# CREATE A BUILD DIRECTORY & PAREPARE MAKE FILE & START BUILDING.
RUN cd GitRepos/myfastcomp/emscripten-fastcomp && mkdir build && cd build && cmake .. -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="X86;JSBackend" -DLLVM_INCLUDE_EXAMPLES=OFF -DLLVM_INCLUDE_TESTS=OFF -DCLANG_INCLUDE_EXAMPLES=OFF -DCLANG_INCLUDE_TESTS=OFF && make

#SETUP EMSCRIPTEN REPO AND CHECKOUT PARTICULAR COMMIT (CAPABLE OF BUILDING VIDEOCONVERTER.JS)
RUN cd GitRepos && git clone https://github.com/kripken/emscripten.git && cd emscripten && git checkout -b videoConverterBuild df11c6f1fd1636a355b83a1c48b3a890596e6a32

#CREATE .emscripten FILE AND CONFIGURE PATHS.
RUN cd GitRepos/emscripten && ./emcc --help &&  sed -i "s/LLVM_ROOT.*/LLVM_ROOT='\/GitRepos\/myfastcomp\/emscripten-fastcomp\/build\/bin'/" ~/.emscripten 


#ADD PATH & CLONE VIDEO CONVERTER REPO AND START BUILDING LGPL BUILD VERSION
RUN export PATH=$PATH:/GitRepos/myfastcomp/emscripten-fastcomp/build/bin:/GitRepos/emscripten:/usr/bin/nodejs && cd GitRepos && git clone https://github.com/bgrins/videoconverter.js.git && cd videoconverter.js/build && ./build_lgpl.sh

RUN cd GitRepos && git clone git@github.com:Kagami/ffmpeg.js.git && cd ffmpeg.js && git submodule init && git submodule update --recursive
RUN export PATH=$PATH:/GitRepos/myfastcomp/emscripten-fastcomp/build/bin:/GitRepos/emscripten:/usr/bin/nodejs && cd GitRepos/ffmpeg.js && cd build/fribidi/ && ./bootstrap && ./configure && cd ../.. && make all


# RUN ls -l /tmp && pwd && cp -rf /GitRepos/videoconverter.js/build/*.js /tmp/hostfolder/ 
RUN sleep 10000