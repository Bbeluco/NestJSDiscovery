# NestJSDiscovery
This repo has the goal to store all my discoveries about NestJS framework

# What is NestJS?
It is a framework that group other frameworks (like express and jest). It goal is to facilitate the backend development creating certain patterns and also dealing with boilerplates (like create default structure for controllers, services, etc).

## Organization
NestJS cli offer the possibility to create a default organization that can be followed by others developers. This helps other devs to keep the source-code organized.

Another thing that NestJS helps a lot is related to linters and formatters, by default prettier and eslint is setup to help developers.

### Linters X Formatters
- Formatter = The main goal is to keep the code formated (tab, code line width, blank spaces, etc)
- Linter = The main goal is to catch bugs executing a static analysis over the code

## Stacks
By default, NestJS starts with express configured, but it also give you the possibility to change for fastify.

# Controllers
It keep the same logic as spring boot, ASP.NET MVC, etc. The controller is responsible for dealing with users request and return any kind of response.

## Response manipulating
NestJS has 2 different ways to manipulate responses
- **Standard (recommended)**: Object/array is serialized to JSON. Primitive types (number, string, boolean) is just send back without serialization
- **Library-specific**: As the name indicates it uses the lib (eg. Express) response. You can do this by using @Res() decorator at your controller

## Scopes
NestJS works sharing the same instance across requests (singleton). Keep this in mind if you want to connect to databases.

## DTOs
Its very important in POST (or any other method that commonly uses body request) that a DTO is required to be created

*What is a DTO?*
Object that defines how data will be sent over the network

It's recomended that we use **classes** instead of typescript interfaces. Why? Because classes are native from ES6, interface is exclusive from TypeScript and TS just exists before runtime, so if we use interfaces during runtime NestJS wont refer those schemas.

# Providers
Providers are so important to NestJS. Their main goal is basically 2:
 1. Make classes injectable
 2. Give special behaviour based on tag

Note that those already exists in Spring Boot framework. This strategy shows us 2 principles for code organization

### Inversion of Control (IoC)
Lets imagine that we're going to use a database inside a class called "Users". If we expect a specific type of database your code will be something like this


```java
    public class User {
        private MysqlDatabase db;

        public Users() {
            this.db = new MysqlDatabase();
        }
    } 
```

Note in this case that if we want to change our MySQL for any other DB we have to change a bunch of things (like db variable type and instance). **To avoid this problem we can use IoC** so we **rely on abstractions over concret implementations**

Using this concept our code will become this:

```java
    public interface Database {
        persist(String data);
    }

    public class User {
        private Database db;

        public Users(Database db) {
            this.db = db;
        }
    }
```

So, we as *User* class only have to consume this DB instance from the entity that is trying to instanciate us. This makes rooms for the second concept super important.

### Dependency Injection (DI)
If IoC is sending responsability for another layer in our application, DI is basically fills what IoC needs. So to use that approch we can write the following code

```java
    public interface Database {
        void persist(String data);
    }

    public class User {
        private Database db;

        public Users(Database db) {
            this.db = db;
        }
    }

    public class MySQLDatabase implements Database {
        public void persist(String data) {
            System.out.println(data);
        }
    }

    public class EntryPoint {
        public static void main(String[] args) {
            MySQLDatabase db = new MySQLDatabase();
            new User(db);
        }
    }
```

## Scope (lifetime)
The doc says that everytime that the application is bootstrapped every provider is instanciated and just shut down when application goes down too. I belive that we have to pay attention at that part bc this can use a lot of resources just to keep up something that is not being used 100% of the time.


# Modules
Looks like its just a way that NestJS internal keep the files organized.
Reading more about it on the internet we discovered that this @Modules() thing exists because NestJS copies Angular even in that aspect. In fact, modules in Angular have their benefit ([Lazy load](https://www.alura.com.br/artigos/como-lazy-loading-pode-melhorar-desempenho-aplicacao-angular) being the most important one). But for Backend porpouse it does not make any sense to use it. You can set everything in the same app module. Obviously your application will be a little bit more slow in when startup but as BE is shared over the users this problem will disapear soon enough

# Middleware
Functions that is called before router handler. Lets imagine a security in a club, before you enter the party you need to pass the guard first.


# Exception filters
Every exception unhandle by application is catch by these filters

# Pipes
Has the same goal as Angular pipes, they try validate and format our request before it gets in the controller. You can deal with pipes in 3 different ways.

Using NestJS pipes (useful when your dealing with @Query() or @Path())
Zod lib (useful when you have a complex schema that you want guarantee that fits your contract)
class-validator lib (Works like Sprint Boot with their validation over the DTO's expected fields) **RECOMMENDED in my context**