FROM nginx:latest
LABEL maintainer="fLoveARTh flovearth@protonmail.com"
ENV KULLANICI="flovearth"
ARG RENK
RUN apt-get update && apt-get install -y curl htop wget
WORKDIR /gecici
ADD https://wordpress.org/latest.tar.gz .
WORKDIR /usr/share/nginx/html
COPY html/${RENK} .
HEALTHCHECK --interval=30s --retries=3 --start-period=10s --timeout=30s CMD curl -f http://localhost/ || exit 1
CMD ["./script.sh"]