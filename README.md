# tomcat-deployment-externalizing--config

## Application Structure


Step1 :  

 Create context xml file and, Then name of the web application, context file name both should match.
 
Step2 : 

Define the following configuration in context xm file.

```
<!-- tomcat/conf/Catalina/localhost ,Place this context file (<<war-file-name>.xml) -->

<Context path="currencyConvertion-api" ,
	docBase="/Users/admin/spring-cloud-config/currency-conversion-service/currencyConvertion-api.war" useHttpOnly="true">
	
	<WatchedResource>WEB=INF/web.xml</WatchedResource>
	<Manager className="org.apache.catalina.session.StandardManager" ,secureRandomClass="java.security.SecureRandom"/>
	<CookieProcessor className="org.apache.tomcat.util.http.LegacyCookieProcessor" />
	
	<Resources
		className="org.apache.catalina.webresources.StandardRoot">
		<PreResources
			className="org.apache.catalina.webresources.DirResourceSet"
			base="/User/admin/admin/spring-cloud-config/currency-conversion-service/config"
			webAppMount="/WEB-INF/classes">
		</PreResource>
	</Resource>
	
	<!-- Tomcat Datasource configuration -->
	<Resource name="jdbc/currencyService" auth="Container"
		type="java.sql.DataSource" , maxActive="100" , maxIdle="30" ,
		maxWait="100" , username="db-user" , password="db-password" ,
		url="jdbc:oracle:thin:@localhost:1521:xe" ,
		driverClassName="oracle.jdbc.driver.OracleDriver" >
	</Resource>
	
	<!-- 	
	 Tomcat Datasource configuration Reading the values from the environment variables 	
	 export DB_USER_NAME=db-user
	 export DB_PASSWORD=db-password
	 export DB_URL=jdbc:oracle:thin:@localhost:1521:xe	
	->
	<!-- <Resource name="jdbc/currencyService" auth="Container"
		type="java.sql.DataSource" , maxActive="100" , maxIdle="30" ,
		maxWait="100" , username="${DB_USER_NAME}" , password=${DB_PASSWORD} ,
		url="${DB_URL}" , driverClassName="oracle.jdbc.driver.OracleDriver" >
	</Resource>
	-->
	
	<!-- Externalizing the libraries -->
	<Resource>
		
		<PreResources
			className="org.apache.catalina.webresources.DirResourceSet"
			base="/User/admin/admin/spring-cloud-config/currency-conversion-service/lib"
			webAppMount="/WEB-INF/lib">
		</PreResource>
	
	<!-- 
	"Base" is the path to the external resources and "webAppMount" is where you want to mount these resources.
	 -->
	</Resource>

	<JarScanner scanClassPath="false"/>
	<JarScanner scanBootstrapClassPath="false"/>
	<JarScanner scanManifest="false"/>
</Context>
```



