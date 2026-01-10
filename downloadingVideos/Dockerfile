# GET YOUR OS
FROM ubuntu:25.04


# SETUP YOUR DIRECTORY
WORKDIR /mydir


# WE NEED TO GET THE TOOL
# BUT BEFORE THIS WE NEED TO INSTALL DEPENDENCIES/APP REQUIRED
RUN <<EOF
apt update && apt install -y curl 
apt update && apt install -y python3 ffmpeg
curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
chmod a+rx /usr/local/bin/yt-dlp  
apt update && apt install bash
EOF


# NOW TO RUN THIS 
# CMD ["/usr/local/bin/yt-dlp", "param1"]
ENTRYPOINT [ "/usr/local/bin/yt-dlp" ]
# ENTRYPOINT [ "/bin/sh" ] 
CMD ["https://www.youtube.com/watch?v=Aa55RKWZxx"]