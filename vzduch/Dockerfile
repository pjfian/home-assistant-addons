ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

FROM mcr.microsoft.com/dotnet/sdk:7.0-jammy-arm64v8 AS build

ARG VZDUCH_VERSION


RUN wget -O vzduch.tar.xz https://github.com/pjfian/vzduch-dotek/archive/refs/tags/1.5.1.tar.gz \
    && tar xvzf vzduch.tar.xz -C /usr/src/ \
    && cd /usr/src/vzduch-dotek-1.5.1

WORKDIR /usr/src/vzduch-dotek-1.5.1

RUN dotnet restore
RUN dotnet publish -c release -o /app -r linux-arm64 --self-contained true --no-restore /p:PublishTrimmed=true /p:PublishSingleFile=true

# final stage/image
FROM $BUILD_FROM

LABEL Mike <ozczecho@yahoo.com>

COPY --from=build /app .
EXPOSE 5353

COPY data/run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
