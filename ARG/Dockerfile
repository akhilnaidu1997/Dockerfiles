ARG VERSION
FROM almalinux:${VERSION:-9}
ARG COURSE="DevOps" \
    TRAINER="Shiva" 
ENV course=${COURSE}
RUN echo "Trainer: ${TRAINER},course: ${COURSE}, ${VERSION}"
CMD ["sleep","1000"]