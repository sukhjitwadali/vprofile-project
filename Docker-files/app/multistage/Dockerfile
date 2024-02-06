# Use openjdk:11 as the base image for the build stage
FROM openjdk:11 AS BUILD_IMAGE

# Update package index and install Maven
RUN apt-get update && apt-get install -y maven

# Clone the Git repository
RUN git clone https://github.com/devopshydclub/vprofile-project.git

# Change working directory to the cloned repository and build the Maven project
WORKDIR /vprofile-project
RUN git checkout docker && mvn clean install

# Switch to the final stage with Tomcat
FROM tomcat:9-jre11

# Remove default Tomcat webapps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the WAR artifact built in the previous stage to the Tomcat webapps directory
COPY --from=BUILD_IMAGE /vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Expose port 8080
EXPOSE 8080

# Set the command to start Tomcat
CMD ["catalina.sh", "run"]

