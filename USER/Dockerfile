FROM almalinux:9
RUN dnf install nginx -y
RUN useradd akhil 
USER akhil
CMD ["nginx", "-g", "daemon off;"]