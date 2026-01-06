FROM node:16.13.0

WORKDIR /mydir

EXPOSE 5000

COPY example-frontend/ .

RUN npm install 

RUN npm install -g serve


# RUN npm audit fix 

RUN npm run build  

CMD ["serve", "-s", "-l", "5000", "build"]