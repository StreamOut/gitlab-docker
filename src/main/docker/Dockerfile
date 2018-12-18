FROM openjdk:8u131-jre-alpine

WORKDIR /usr/src/app
COPY @project.build.finalName@.@project.packaging@ .

EXPOSE 8080

ENTRYPOINT ["java"]
CMD ["-jar", "@project.build.finalName@.@project.packaging@"]