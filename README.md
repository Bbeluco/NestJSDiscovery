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