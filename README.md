# Jerkar Addin for Spring boot

The goal of this project is to provide a Jerkar addin so you can build Spring Boot application with minimal effort.
This allow to go farer in the no XML / script experience cause you won't need to edit any XML (Maven pom) or gradle script at all : <strong>only Java code</code> so easy to debug in your favorite IDE.
The way Jerkar works is also very coherent with Spring Boot philosophy (Just running main methods to get things done).

## Principle

### Writing the build class

Just make your build class extending JkSpringbootBuild as the above exemple :

```
import org.jerkar.addin.springboot.JkSpringModules.Boot;
import org.jerkar.addin.springboot.JkSpringbootBuild;
import org.jerkar.api.depmanagement.JkDependencies;
import org.jerkar.tool.JkInit;

@JkImport({ "org.jerkar:addin-spring-boot:0.1-SNAPSHOT"})
public class Build extends JkSpringbootBuild {

    @Override
    protected JkDependencies dependencies() {
	     return JkDependencies.builder()
		      .on(Boot.STARTER)
		      .on(Boot.STARTER_TEST, TEST).build();
    }
    
    public static void main(String[] args) {
	     JkInit.instanceOf(Build.class, args).doDefault();
    }
}
```

Running this class performs :

* Compilation and test run
* Generation of the original binary jar along its sources jar
* Generation of the executable jar
* Generation of the executable war file (if WEB-INF present)


### Writing extra run class

You can also add other runner classes beside the build class to perform other tasks: 

```
import org.jerkar.tool.JkInit;

class RunApplication {

    public static void main(String[] args) {
        Build build = JkInit.instanceOf(Build.class, args);
	     build.doCompile();
	     build.run();
    }

}
```

 This class compile the code an run the application upon the compiled code and declared dependencies.
 
 ##Adding extra dependencies
 
 Springboot addin perform provides class constants to declare most of dependencies. 
 It adds great comfort when picking some Spring dependencies.
 
 
 ```
import org.jerkar.addin.springboot.JkSpringModules.Boot;
import org.jerkar.addin.springboot.JkSpringbootBuild;
import org.jerkar.api.depmanagement.JkDependencies;
import org.jerkar.tool.JkInit;

@JkImport({ "org.jerkar:addin-spring-boot:0.1-SNAPSHOT"})
public class Build extends JkSpringbootBuild {

    @Override
    protected JkDependencies dependencies() {
	     return JkDependencies.builder()
		      .on(Boot.STARTER)
		      .on(Fwk.JDBC)
		      .on(Data.MONGODB)
		      .on(Data.COMMONS)
		      .on(Security.CORE)
		      .on(Mobile.DEVICE)
		      .on(JkPopularModules.GUAVA, "18.0")
		      .on(Boot.STARTER_TEST, TEST).build();
    }
    
    public static void main(String[] args) {
	     JkInit.instanceOf(Build.class, args).doDefault();
    }
}
```
