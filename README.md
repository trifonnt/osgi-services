# Sample OSGi application using Apache CXF and OpenJPA

## Clone GIT repository
git clone https://github.com/trifonnt/osgi-services.git github-trifonnt-osgi-services
cd github-trifonnt-osgi-services/


## Build
mvn package


## Import in Eclipse
File -> Import... -> Maven -> Existing Maven Projects


## Start Apache Karaf
cd <KARAF_HOME>
bim/karaf


## In Karaf 3.0.1 shell
feature:repo-add cxf 3.0.1
feature:install http cxf jpa openjpa transaction jndi jdbc
install -s mvn:org.hsqldb/hsqldb/2.3.2
install -s mvn:com.fasterxml.jackson.core/jackson-core/2.4.0
install -s mvn:com.fasterxml.jackson.core/jackson-annotations/2.4.0
install -s mvn:com.fasterxml.jackson.core/jackson-databind/2.4.0
install -s mvn:com.fasterxml.jackson.jaxrs/jackson-jaxrs-base/2.4.0
install -s mvn:com.fasterxml.jackson.jaxrs/jackson-jaxrs-json-provider/2.4.0


## Deploy (locally)
KARAF_HOME=/data/progs/apache-karaf/3.0.4

cp module-data/target/module-data-*.jar $KARAF_HOME/deploy/
cp module-services/target/module-services-*.jar $KARAF_HOME/deploy/
cp module-jax-rs/target/module-jax-rs-*.jar $KARAF_HOME/deploy/


### In Karaf shell - verify deployment of our bundles
karaf@root()> list

karaf@root()> log:display
 org.apache.aries.jpa.container - 1.0.2 | There are no providers available.


## From command line
 - Create new person:   	
   curl http://localhost:8181/cxf/api/people -iX POST -d "firstName=Tom&lastName=Knocker&email=a@b.com"
 - Query for person by e-mail:
   curl http://localhost:8181/cxf/api/people/a@b.com
 - Query all people:
   curl http://localhost:8181/cxf/api/people
 - Delete person by e-mail:
   curl http://localhost:8181/cxf/api/people/a@b.com -X DELETE
