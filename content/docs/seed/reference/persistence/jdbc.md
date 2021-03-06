# Introduction

Seed JDBC persistence support enables your application to interface with any relational database through the JDBC API. Note that :
* This version works with JRE 6 and 7
* This support allows you to get your connections from any Datasource, JNDI or programmatically created.

# DataSource providers

When using a non JNDI datasource, we recommend the use of pooled datasource through a DataSourceProvider defined in the configuration. We currently support 3 DataSource providers :
* [HikariCP](http://brettwooldridge.github.io/HikariCP/) with `HikariDataSourceProvider`
* [Commons DBCP](http://commons.apache.org/proper/commons-dbcp/) with `DbcpDataSourceProvider`
* [C3P0](http://www.mchange.com/projects/c3p0/) with `C3p0DataSourceProvider`

We also provide a test oriented DataSource that gives connection directly from the driver. Use `PlainDataSourceProvider` or do not specify a provider.

In case you want to use another data source, you can create your own `DataSourceProvider`

    public class SomeDataSourceProvider implements DataSourceProvider {
    
        @Override
        public DataSource provideDataSource(String driverClass, String url, String user, String password, Properties jdbcProperties) {
            SomeDataSource sds = new SomeDataSource();
            sds.setDriverClass(driverClass);
            sds.setJdbcUrl(url);
            sds.setUser(url);
            sds.setPassword(user);
            sds.setProperties(jdbcProperties);
            return sds;
        }
    
    }
    
You will be able to declare it in your configuration as `SomeDataSourceProvider`

Note that if you want to use one of the 3 data sources described above, you will have to add the dependency yourself as they have been marked as optional.


# Configuration

You can configure the support with properties in one of your \*.props files

Declare you list of data source names you will be configuring later :

    org.seedstack.seed.persistence.jdbc.datasources = datasource1, datasource2, ...
    
Configure each data source separately. Notice the use of the keyword *property* to specify any property that will be used by the datasource as specific configuration

    [org.seedstack.seed.persistence.jdbc.datasource.datasource1]
    provider = HikariDataSourceProvider
    driver = org.hsqldb.jdbcDriver
    url = jdbc:hsqldb:mem:testdb1
    user = sa
    password =
    property.specific.jdbc.prop = value
    property.prop.for.datasource = value

If your app server declares a JNDI datasource :

    [org.seedstack.seed.persistence.jdbc.datasource.datasource2]
    context = java:comp/env/jdbc/my-datasource
    
# JDBC Connection

To get a JDBC connection in your code
    
    public class MyRepository {

        @Inject
        private Connection connection;

        public void updateStuff(int id, String bar){
            try{
                String sql = "INSERT INTO FOO VALUES(?, ?)";
                PreparedStatement statement = connection.prepareStatement(sql);
                statement.setInt(1, id);
                statement.setString(2, bar);
                statement.executeUpdate();
            } catch(SqlException e){
                throw new SomeRuntimeException(e, "message");
            }
        }
    }
    
Any interaction with this connection will have to be realized inside a **transaction**. Refer to the Transaction support [documentation](#!/seed-doc/transaction) for more detail.

Below is an example using the annotation-based transaction demarcation (notice the data source name in `@Jdbc` annotation)

    public class MyService {

        @Inject
        private MyRepository myRepository;

        @Transactional
        @Jdbc("datasource1")
        public void doSomethingRelational() {
            myRepository.updateStuff(1, "bar");
        }
    }

Note that if you only use one data source, you do not need to specify its name in the annotation. Also note that if the only persistence support in your classpath is the Jdbc support, you do not need to specify the annotation Jdbc.
In both cases, whenever you add a data source or a support, you will have to specify **all** the `@Transactional` annotated methods.