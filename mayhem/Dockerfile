# Copyright 2018 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder
RUN sed -i s|snapshot.ubuntu.com|archive.ubuntu.com|g /etc/apt/sources.list /etc/apt/sources.list.d/*.list 2>/dev/null || true
RUN apt-get update && apt-get install -y mercurial python-is-python3
RUN pip3 install numpy
RUN git clone --depth=1 https://bitbucket.org/jpommier/pffft.git $SRC/pffft
WORKDIR pffft
COPY mayhem/build.sh $SRC
# TODO(alessiob): Move the fuzzing source code to pffft upstream.
COPY mayhem/generate_seed_corpus.py $SRC/pffft
COPY mayhem/pffft_fuzzer.cc $SRC/pffft

ENV FUZZING_LANGUAGE=c++ SANITIZER=address FUZZING_ENGINE=libfuzzer ARCHITECTURE=x86_64
RUN compile
RUN chmod +x /out/pffft_complex_fuzzer
