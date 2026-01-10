# GET YOUR MINIMAL OS
FROM ubuntu:latest


# INSTALL NECESSARY DEPENDENCIES
RUN <<EOF
DEBIAN_FRONTEND=noninteractive apt update && apt install -y curl   
EOF


# WORKDIR
WORKDIR /usr/src/app

# COPY THE SCRIPT FILE
COPY script.sh .

# CHANGE FILE PERMISSIONS
# RUN chmod a+x script.sh

# NOW WE HAVE ENTRY POINT FOR RUNNING COMMAND
ENTRYPOINT [ "./script.sh" ]


# COMMAND FOR ARGUMENTS
CMD ["helsinki.fi"]