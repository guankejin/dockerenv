ARG REDIS_IMAGE_REPO
ARG REDIS_IMAGE_TAG

FROM ${REDIS_IMAGE_REPO}:${REDIS_IMAGE_TAG}

VOLUME /data

EXPOSE 6379

#CMD ["redis-server", "/usr/local/etc/redis/redis.conf"]
CMD ["redis-server"]
