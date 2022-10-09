# syntax=docker/dockerfile:1.4

# * Arguments
ARG \
    BASE_IMAGE_NAME="${BASE_IMAGE_NAME}" \
    BASE_IMAGE_REPOSITORY="${BASE_IMAGE_REPOSITORY}" \
    BASE_IMAGE_TAG="${BASE_IMAGE_TAG}" \
    BUILDER_IMAGE_NAME="${BUILDER_IMAGE_NAME}" \
    BUILDER_IMAGE_REPOSITORY="${BUILDER_IMAGE_REPOSITORY}" \
    BUILDER_IMAGE_TAG="${BUILDER_IMAGE_TAG}" \
    BUILD_DATE="${BUILD_DATE}" \
    NS_MODULES_VERSION="${NS_MODULES_VERSION}" \
    NS_VERSION="${NS_VERSION}" \
    RL_JSON_VERSION="${RL_JSON_VERSION}" \
    TCLLIB_VERSION="${TCLLIB_VERSION}" \
    TCL_VERSION="${TCL_VERSION}" \
    TDOM_VERSION="${TDOM_VERSION}" \
    THREAD_VERSION="${THREAD_VERSION}" \
    XOTCL_VERSION="${XOTCL_VERSION}"

# * Build software
FROM ${BUILDER_IMAGE_REPOSITORY}/${BUILDER_IMAGE_NAME}:${BUILDER_IMAGE_TAG} as build

# ** Environmen
ENV NS_VERSION="${NS_VERSION}" \
    NS_MODULES_VERSION="${NS_MODULES_VERSION}" \
    RL_JSON_VERSION_VERSION="${RL_JSON_VERSION_VERSION}" \
    TCLLIB_VERSION="${TCLLIB_VERSION}" \
    TCL_VERSION="${TCL_VERSION}" \
    TDOM_VERSION="${TDOM_VERSION}" \
    THREAD_VERSION="${THREAD_VERSION}" \
    XOTCL_VERSION="${XOTCL_VERSION}"

# ** Build
WORKDIR /
RUN mkdir /workspaces
COPY builds /builds
RUN  /bin/bash /builds/all-build.sh

# * From
FROM ${BASE_IMAGE_REPOSITORY}/${BASE_IMAGE_NAME}:${BASE_IMAGE_TAG}
ARG \
    BASE_IMAGE_NAME \
    BASE_IMAGE_REPOSITORY \
    BASE_IMAGE_TAG \
    BUILDER_IMAGE_NAME \
    BUILDER_IMAGE_REPOSITORY \
    BUILDER_IMAGE_TAG \
    BUILD_DATE \
    NS_MODULES_VERSION \
    NS_VERSION \
    RL_JSON_VERSION \
    TCLLIB_VERSION \
    TCL_VERSION \
    TDOM_VERSION \
    THREAD_VERSION \
    XOTCL_VERSION

# ** Copy naviserver and locales en_US and ru_RU
WORKDIR /
COPY --from=build /usr/local/ns /usr/local/ns/
COPY --from=build /usr/lib/locale/en_US /usr/lib/locale/en_US
COPY --from=build /usr/lib/locale/en_US.utf8 /usr/lib/locale/en_US.utf8
COPY --from=build /usr/lib/locale/ru_RU /usr/lib/locale/ru_RU
COPY --from=build /usr/lib/locale/ru_RU.utf8 /usr/lib/locale/ru_RU.utf8

# ** Labels
# hadolint ignore=DL3048
LABEL \
    maintainer="Oupfiz V <oupfiz5@yandex.ru>" \
    org.opencontainers.image.created="${BUILD_DATE}" \
    org.opencontainers.image.authors="Oupfiz V (Five)" \
    org.opencontainers.image.url="https://chiselapp.com/user/oupfiz5/repository/alt-naviserver-s6/home" \
    org.opencontainers.image.documentation="https://chiselapp.com/user/oupfiz5/repository/alt-naviserver-s6/wiki" \
    org.opencontainers.image.source="https://chiselapp.com/user/oupfiz5/repository/alt-naviserver-s6/brlist" \
    org.opencontainers.image.version="0.0.1d" \
    org.opencontainers.image.revision="" \
    org.opencontainers.image.vendor="" \
    org.opencontainers.image.licenses="" \
    org.opencontainers.image.ref.name="" \
    org.opencontainers.image.title="NaviServer on AltLinux base docker image using s6-overlay" \
    org.opencontainers.image.description="NaviServer on AltLinux base docker image using s6-overlay" \
    org.opencontainers.image.base="${BASE_IMAGE_REPOSITORY}/${BASE_IMAGE_NAME}:${BASE_IMAGE_TAG}" \
    custom.package.version.naviserver="${NS_VERSION}" \
    custom.package.version.naviserver_modules="${NS_MODULES_VERSION}" \
    custom.package.version.rl_json="${RL_JSON_VERSION}" \
    custom.package.version.tcl="${TCL_VERSION}" \
    custom.package.version.tcllib="${TCLLIB_VERSION}" \
    custom.package.version.tdom="${TDOM_VERSION}" \
    custom.package.version.thread="${THREAD_VERSION}" \
    custom.package.version.xotcl="${XOTCL_VERSION}"

# ** Run
# hadolint ignore=DL3008,DL3015
RUN export LANG="en_US.UTF-8" \
    && export LC_ALL="en_US.UTF-8" \
    && mkdir /var/lib/apt/lists/partial \
    && apt-get update \
    && apt-get -y install libssl-devel libpq5-devel wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists \
          /var/cache/apt/archives/*.rpm  \
          /var/cache/apt/*.bin \
    && groupadd nsadmin \
    && useradd -g nsadmin nsadmin \
    && chown -R  nsadmin:nsadmin /usr/local/ns

# ** Copy s6 and naviserver configurations
COPY rootfs/etc /etc/
COPY rootfs/usr/local/ns/conf /usr/local/ns/conf

# ** Expose
EXPOSE 8080

# ** Environment
Env NS_CONF="/usr/local/ns/conf/nsd-config.tcl"

# ** Workdir
WORKDIR /usr/local/ns

# ** Entrypoint
ENTRYPOINT ["/init"]

# ** Cmd
CMD []
