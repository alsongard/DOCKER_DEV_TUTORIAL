# Building First Image

Now we know to execute a script you need to have Operating System
Therefore we need to get an os:
- we can use linux alpine version or ubuntu(large)


# Alpine Linux is a security-oriented, lightweight Linux distribution based on musl libc and busybox.
FROM apline:3.21


# Use /usr/src/app as your working directory: All command will be executed in this directory
WORKDIR /usr/src/app


# Copy file hello.sh to working directory
copy hello.sh .

# modify file permission
chmod u+x hello.sh

# Command to execute when running docker
CMD ["./hello.sh"]



