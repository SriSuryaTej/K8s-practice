FROM maven:3-jdk-11
RUN git clone https://github.com/SriSuryaTej/openmrs-core.git && cd openmrs-core && mvn clean package
WORKDIR openmrs-core/webapp
CMD ["mvn","jetty:run"]