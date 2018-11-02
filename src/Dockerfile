FROM alpine:edge
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" | tee -a /etc/apk/repositories
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" | tee -a /etc/apk/repositories
RUN apk update
RUN apk upgrade
RUN apk add emscripten-fastcomp emscripten emscripten-optimizer \
    emscripten-libs-asmjs emscripten-libs-wasm make autoconf autoconf-archive \
    automake darkhttpd git glib-dev glib markdown
RUN adduser -D -h /home/dosbox dosbox dosbox
RUN git clone https://github.com/dreamlayers/em-dosbox.git /home/dosbox/em-dosbox/
RUN chown -R dosbox:dosbox /home/dosbox/em-dosbox/
USER dosbox
WORKDIR /home/dosbox/em-dosbox/
RUN ./autogen.sh
RUN emconfigure ./configure --with-sdl2 --host=asmjs
RUN emmake make
RUN cp /etc/emscripten.cfg /home/dosbox/.emscripten
USER dosbox
WORKDIR /home/dosbox/em-dosbox/src
#COPY MIPS.COM bigtop/
COPY BIGTOP.IMG bigtop/
COPY dosbox.conf bigtop/
COPY bigtop.map bigtop/
RUN ./packager.py bigtop bigtop BIGTOP.IMG
CMD darkhttpd /home/dosbox/em-dosbox/src/ --index bigtop.html --no-listing
