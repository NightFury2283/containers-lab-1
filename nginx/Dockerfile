# ./nginx/Dockerfile
FROM nginx:alpine

COPY nginx.conf /etc/nginx/nginx.conf

COPY index.html /usr/share/nginx/html/index.html

#порт
EXPOSE 80

#не в фоне- daemon off
CMD ["nginx", "-g", "daemon off;"]