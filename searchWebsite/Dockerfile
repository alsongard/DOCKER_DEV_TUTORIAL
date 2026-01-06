# get minimal versionof alpine
FROM alpine:latest


# install bash
RUN apk update && apk add bash
RUN apk update && apk add curl


# SET WORKING DIRECTORY
WORKDIR /usr/src/app

# COPY YOUR SCRIPT OR FILE
COPY script.sh .


# COMMAND TO EXECUTE WHEN CREATE CONTIANER
CMD ["./script.sh"]
