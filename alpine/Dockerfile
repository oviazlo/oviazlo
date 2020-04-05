# simple alpine-based image for testing purposes
#
# user password is the same as username
#
# non-root user is in "oviazlo" group
# --> host directories to be mounted inside container should have 744 permission
#
# usage:
# $ docker build -t <imageName> .
# $ docker run -it --name <containerName> \
#   --mount type=bind,source=<hostDir>,target=<containerDir> \
#   <imageName>

FROM alpine:latest

ARG USER=docker
ENV HOME /home/$USER

RUN apk add --no-cache bash sudo vim

# add new user
RUN adduser -D --shell /bin/bash $USER \
        && echo "$USER:$USER" | chpasswd \
        && echo "$USER ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$USER \
        && chmod 0440 /etc/sudoers.d/$USER

# add user to standart groups
RUN addgroup oviazlo; addgroup sudo; \
    for u in $(ls /home); do for g in disk lp floppy audio cdrom dialout video \
                                      netdev games users oviazlo sudo; \
    do addgroup $u $g; done;done;

USER $USER
WORKDIR $HOME
CMD ["/bin/bash"]
