# Base OS/JDK image to use
FROM adoptopenjdk/openjdk11:ubuntu-slim

# Metadata
LABEL Version="1.0" \
      Author="Bhaskar.S" \
      Email="bswamina@polarsparc.com"

# Enviornment variable(s)
ENV DRILL_VERSION=1.20.2 \
    DRILL_USER=drill \
    DRILL_GID=1000 \
    DRILL_UID=1000 \
    DRILL_HOME=/opt/drill \
    DRILL_PID_DIR=/opt/drill \
    DRILL_LOG_DIR=/opt/drill/log \
    DRILL_CONF_DIR=/opt/drill/conf \
    DRILL_DATA_DIR=/data

# Install required core dependencies
RUN apt-get -qq update && \
    apt-get install -y ca-certificates -qq && \
    apt-get install -y wget -qq && \
    apt-get clean

# Prepare Apache Drill environment - user, group, directories
RUN mkdir -p "$DRILL_HOME" "$DRILL_LOG_DIR" "$DRILL_CONF_DIR" "$DRILL_DATA_DIR" && \
    groupadd -r "$DRILL_USER" --gid="$DRILL_GID" && \
    useradd -r -g "$DRILL_USER" --uid="$DRILL_UID" -d "$DRILL_HOME" "$DRILL_USER" && \
    chown -R "$DRILL_USER":"$DRILL_USER" "$DRILL_HOME" && \
    chown -R "$DRILL_USER":"$DRILL_USER" "$DRILL_DATA_DIR"

# Install Apache Drill software
RUN wget -q "https://www.apache.org/dist/drill/$DRILL_VERSION/apache-drill-$DRILL_VERSION.tar.gz" -P /tmp && \
    tar -xzf "/tmp/apache-drill-$DRILL_VERSION.tar.gz" -C /tmp && \
    mv "/tmp/apache-drill-$DRILL_VERSION/"* "$DRILL_HOME" && \
    rm -rf "/tmp/apache-drill-$DRILL_VERSION.tar.gz" "/tmp/apache-drill-$DRILL_VERSION"

# Set the Home directory
WORKDIR $DRILL_HOME

# Expose the desired network ports
EXPOSE 8047 31010 31011 31012

# Set the PATH variable
ENV PATH=$PATH:$DRILL_HOME/bin

# Define volume mount point(s)
VOLUME $DRILL_CONF_DIR
VOLUME $DRILL_LOG_DIR
VOLUME $DRILL_DATA_DIR
