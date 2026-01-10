FROM  golang:1.16.15-bullseye

EXPOSE 8080

WORKDIR /mydir

COPY example-backend/ .

RUN go build

RUN go test .


CMD [ "./server" ]