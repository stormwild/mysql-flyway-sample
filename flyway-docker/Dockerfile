# Step 1: Use Amazon Corretto as the base image for Java 17
FROM amazoncorretto:17-alpine AS flyway

# Step 2: Install necessary packages (removed curl as it's no longer needed)
RUN apk --no-cache add bash openssl

# Verify the Java version
RUN java -version

# Step 3: Set working directory
WORKDIR /flyway

# Step 4: Specify the Flyway version
ARG FLYWAY_VERSION=10.19.0

# Step 5: Download Flyway using ADD instruction (Docker best practice)
ADD https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/${FLYWAY_VERSION}/flyway-commandline-${FLYWAY_VERSION}-linux-x64.tar.gz flyway.tar.gz

# Step 6: Extract and setup Flyway, then list contents
RUN tar -xzf flyway.tar.gz --strip-components=1 && \
    rm flyway.tar.gz && \
    chmod -R a+r /flyway && \
    chmod a+x /flyway/flyway && \
    ls -al /flyway

# Step 6: Set environment variables
ENV PATH="/flyway:${PATH}"

# Step 7: Set entrypoint
ENTRYPOINT ["/flyway/flyway"]
CMD ["-?"]
