# Specification: Ballerina GraphQL Library

_Owners_: @shafreenAnfar @DimuthuMadushan @ThisaruGuruge @MohamedSabthar  
_Reviewers_: @shafreenAnfar @ThisaruGuruge @DimuthuMadushan @ldclakmal  
_Created_: 2022/01/06  
_Updated_: 2023/01/24  
_Edition_: Swan Lake  

## Introduction

This is the specification for the GraphQL standard library of the [Ballerina](https://ballerina.io)[ language](https://ballerina.io), which provides GraphQL server functionalities to produce GraphQL APIs and GraphQL client functionalities to communicate with GraphQL APIs.

The GraphQL library specification has evolved and may continue to evolve in the future. The released versions of the specification can be found under the relevant GitHub tag.

If you have any feedback or suggestions about the library, start a discussion via a [GitHub issue](https://github.com/ballerina-platform/ballerina-standard-library/issues) or in the [Discord server](https://discord.gg/ballerinalang). Based on the outcome of the discussion, the specification and implementation can be updated. Community feedback is always welcome. Any accepted proposal, which affects the specification is stored under `/docs/proposals`. Proposals under discussion can be found with the label `type/proposal` on GitHub.

The conforming implementation of the specification is released and included in the distribution. Any deviation from the specification is considered a bug.

## Contents

1. [Overview](#1-overview)
2. [Components](#2-components)
    * 2.1 [Listener](#21-listener)
        * 2.1.1 [HTTP Listener](#211-http-listener)
        * 2.1.2 [WebSocket Listener](#212-websocket-listener)
        * 2.1.3 [Initializing the Listener Using Port Number](#213-initializing-the-listener-using-port-number)
        * 2.1.4 [Initializing the Listener using an HTTP Listener](#214-initializing-the-listener-using-an-http-listener)
        * 2.1.5 [Listener Configuration](#215-listener-configuration)
    * 2.2 [Service](#22-service)
        * 2.2.1 [Service Type](#221-service-type)
        * 2.2.2 [Service Base Path](#222-service-base-path)
        * 2.2.3 [Service Declaration](#223-service-declaration)
        * 2.2.4 [Service Object Declaration](#224-service-object-declaration)
        * 2.2.5 [Service Configuration](#225-service-configuration)
    * 2.3 [Parser](#23-parser)
    * 2.4 [Engine](#24-engine)
    * 2.5 [Client](#25-client)
        * 2.5.1 [Initializing the Client](#251-initializing-the-client)
        * 2.5.2 [Executing Operations](#252-executing-operations)
        * 2.5.3 [Client Data Binding](#253-client-data-binding)
        * 2.5.4 [Client Configuration](#254-client-configuration)
3. [Schema Generation](#3-schema-generation)
    * 3.1 [Root Types](#31-root-types)
        * 3.1.1 [The `Query` Type](#311-the-query-type)
        * 3.1.2 [The `Mutation` Type](#312-the-mutation-type)
        * 3.1.3 [The `Subscription` Type](#313-the-subscription-type)
    * 3.2 [Wrapping Types](#32-wrapping-types)
        * 3.2.1 [`NON_NULL` Type](#321-non_null-type)
        * 3.2.2 [`LIST` Type](#322-list-type)
    * 3.3 [Resource Methods](#33-resource-methods)
        * 3.3.1 [Resource Accessor](#331-resource-accessor)
        * 3.3.2 [Resource Name](#332-resource-name)
        * 3.3.3 [Hierarchical Resource Path](#333-hierarchical-resource-path)
    * 3.4 [Remote Methods](#34-remote-methods)
        * 3.4.1 [Remote Method Name](#341-remote-method-name)
    * 3.5 [Documentation](#35-documentation)
4. [Types](#4-types)
    * 4.1 [Scalars](#41-scalars)
        * 4.1.1 [Int](#411-int)
        * 4.1.2 [Float](#412-float)
        * 4.1.3 [String](#413-string)
        * 4.1.4 [Boolean](#414-boolean)
    * 4.2 [Objects](#42-objects)
        * 4.2.1 [Record Type as Object](#421-record-type-as-object)
        * 4.2.2 [Service Type as Object](#422-service-type-as-object)
    * 4.3 [Unions](#43-unions)
    * 4.4 [Enums](#44-enums)
    * 4.5 [Input Types](#45-input-types)
        * 4.5.1 [Input Union Types](#451-input-union-types)
        * 4.5.2 [Input Objects](#452-input-objects)
        * 4.5.3 [Default Values](#453-default-values)
    * 4.6 [Interfaces](#46-interfaces)
        * 4.6.1 [Interfaces Implementing Interfaces](#461-interfaces-implementing-interfaces)
5. [Directives](#5-directives)
   * 5.1 [@skip](#51-skip)
   * 5.2 [@include](#52-include)
   * 5.3 [@deprecated](#53-deprecated)
6. [File Upload](#6-file-upload)
    * 6.1 [File Upload Endpoint](#61-file-upload-endpoint)
        * 6.1.1 [`graphql:Upload` Type](#611-graphqlupload-type)
            * 6.1.1.1 [`fileName` Field](#6111-filename-field)
            * 6.1.1.2 [`mimeType` Field](#6112-mimetype-field)
            * 6.1.1.3 [`encoding` Field](#6113-encoding-field)
            * 6.1.1.4 [`byteStream` Field](#6114-bytestream-field)
        * 6.1.2 [Writing a File Upload Resolver](#612-writing-a-file-upload-resolver)
7. [Errors](#7-errors)
    * 7.1 [Error Detail Record](#71-error-detail-record)
        * 7.1.1 [Message](#711-message)
        * 7.1.2 [Locations](#712-locations)
        * 7.1.3 [Path](#713-path)
    * 7.2 [Service Error Handling](#72-service-error-handling)
        * 7.2.1 [Returning Errors and Nil Values](#721-returning-errors-and-nil-values)
    * 7.3 [Client Error Handling](#73-client-error-handling)
        * 7.3.1 [Request Error](#731-request-error)
            * 7.3.1.1 [HTTP Error](#7311-http-error)
            * 7.3.1.2 [Invalid Document Error](#7312-invalid-document-error)
        * 7.3.2 [Payload Binding Error](#732-payload-binding-error)
8. [Context Object](#8-context-object)
    * 8.1 [Set Attribute in Context](#81-set-attribute-in-context)
    * 8.2 [Get Context Attribute](#82-get-context-attribute)
    * 8.3 [Remove Attribute from Context](#83-remove-attribute-from-context)
    * 8.4 [Accessing the Context](#84-accessing-the-context-object)
    * 8.5 [Resolving Field Value](#85-resolving-field-value)
9. [Field Object](#9-field-object)
    * 9.1 [Field Object Methods](#91-field-object-methods)
        * 9.1.1 [Get Field Name](#911-get-field-name)
        * 9.1.2 [Get Field Alias](#912-get-field-alias)
        * 9.1.3 [Get Field Path](#913-get-field-path)
        * 9.1.4 [Get Subfield Names](#914-get-subfield-names)
        * 9.1.5 [Get Field Type](#915-get-field-type)
    * 9.2 [Accessing the Field Object](#92-accessing-the-field-object)
10. [Annotations](#10-annotations)
    * 10.1 [Service Configuration](#101-service-configuration)
        * 10.1.1 [Max Query Depth](#1011-max-query-depth)
        * 10.1.2 [Auth Configurations](#1012-auth-configurations)
        * 10.1.3 [Context Initializer Function](#1013-context-initializer-function)
        * 10.1.4 [CORS Configurations](#1014-cors-configurations)
        * 10.1.5 [GraphiQL Configurations](#1015-graphiql-configurations)
        * 10.1.6 [Service Level Interceptors](#1016-service-level-interceptors)
        * 10.1.7 [Introspection Configurations](#1017-introspection-configurations)
11. [Interceptors](#11-interceptors)
    * 11.1 [Interceptor Service Object](#111-interceptor-service-object)
    * 11.2 [Writing an Interceptor](#112-writing-an-interceptor)
    * 11.3 [Execution](#113-execution)
        * 11.3.1 [Service Level Interceptors](#1131-service-level-interceptors)
12. [Security](#12-security)
    * 12.1 [Service Authentication and Authorization](#121-service-authentication-and-authorization)
        * 12.1.1 [Declarative Approach](#1211-declarative-approach)
            * 12.1.1.1 [Basic Authentication - File User Store](#12111-basic-authentication---file-user-store)
            * 12.1.1.2 [Basic Authentication - LDAP User Store](#12112-basic-authentication---ldap-user-store)
            * 12.1.1.3 [JWT Authentication](#12113-jwt-authentication)
            * 12.1.1.4 [OAuth2](#12114-oauth2)
        * 12.1.2 [Imperative Approach](#1212-imperative-approach)
            * 12.1.2.1 [Basic Authentication - File User Store](#12121-basic-authentication---file-user-store)
            * 12.1.2.2 [Basic Authentication - LDAP User Store](#12122-basic-authentication---ldap-user-store)
            * 12.1.2.3 [JWT Authentication](#12123-jwt-authentication)
            * 12.1.2.4 [OAuth2](#12124-oauth2)
    * 12.2 [Client Authentication and Authorization](#122-client-authentication-and-authorization)
        * 12.2.1. [Basic Authentication](#1221-basic-authentication)
        * 12.2.2. [Bearer Token Authentication](#1222-bearer-token-authentication)
        * 12.2.3. [Self-Signed JWT Authentication](#1223-self-signed-jwt-authentication)
        * 12.2.4. [OAuth2](#1224-oauth2)
            * 12.2.4.1 [Client Credentials Grant Type](#12241-client-credentials-grant-type)
            * 12.2.4.2 [Password Grant Type](#12242-password-grant-type)
            * 12.2.4.3 [Refresh Token Grant Type](#12243-refresh-token-grant-type)
            * 12.2.4.4 [JWT Bearer Grant Type](#12244-jwt-bearer-grant-type)
    * 12.3 [SSL/TLS and Mutual SSL](#123-ssltls-and-mutual-ssl)
        * 12.3.1 [Listener](#1231-listener)
            * 12.3.1.1 [SSL/TLS](#12311-ssltls)
            * 12.3.1.2 [Mutual SSL](#12312-mutual-ssl)
        * 12.3.2 [Client](#1232-client)
            * 12.3.2.1 [SSL/TLS](#12321-ssltls)
            * 12.3.2.2 [Mutual SSL](#12322-mutual-ssl)
13. [Tools](#13-tools)
    * 13.1 [GraphiQL Client](#131-graphiql-client)

## 1. Overview

The Ballerina language provides first-class support for writing network-oriented programs. The GraphQL standard library uses these language constructs and creates the programming model to produce/consume GraphQL APIs.

The GraphQL standard library is designed to work with [GraphQL specification](https://spec.graphql.org). There are two main approaches when writing GraphQL APIs. The schema-first approach and the code-first approach. The Ballerina GraphQL standard library uses the code-first first approach to write GraphQL APIs. This means no GraphQL schema is required to create a GraphQL service.

In addition to functional requirements, this library deals with none functional requirements such as security. Each requirement is discussed in detail in the coming sections.

## 2. Components

This section describes the components of the Ballerina GraphQL package. To use the Ballerina GraphQL package, a user must import the Ballerina GraphQL package first.

###### Example: Importing the GraphQL Package

```ballerina
import ballerina/graphql;
```

### 2.1 Listener

#### 2.1.1 HTTP Listener
Since the GraphQL spec does not mandate an underlying client-server protocol, a GraphQL implementation can use any protocol underneath. The Ballerina GraphQL package, like most of the other implementations, uses HTTP as the protocol. The Ballerina GraphQL listener is using an HTTP listener to listen to incoming GraphQL requests through HTTP.

#### 2.1.2 WebSocket Listener
If the schema contains the `Subscription` type (as described in [Subscription Type](#313-the-subscription-type)), The GraphQL listener will establish a new WebSocket listener to listen to incoming subscription requests.

In Ballerina, WebSocket is used as the communication protocol for GraphQL subscriptions as it is capable of dispatching data continuously while maintaining a persistent connection. Ballerina GraphQL utilizes the `graphql-transport-ws` (graphql-ws) WebSocket sub-protocol in subscriptions. If a WebSocket connection is established with the `graphql-transport-ws` sub-protocol, all subscription responses will be formatted according to the standard message structure outlined in the [specification](https://github.com/enisdenjo/graphql-ws/blob/master/PROTOCOL.md).

A standard response includes JSON fields for `type`, `id`, and `payload`. The `type` field specifies the message type of the response. The `id` field is used to uniquely identify the client. The `payload` field includes the GraphQL response returned from the GraphQL engine. If a subscription request is sent without the `graphql-transport-ws` sub-protocol then the WebSocket handshake fails with an error.

>**Note:** The Ballerina GraphQL subscription includes a default implementation for sending `ping` messages over a graphql-ws connection, and periodically checking for `pong` messages in response. By default, these messages are sent and checked every 15 seconds. If a `pong` message is not received within this time frame, the service will close the WebSocket connection. 

A Ballerina GraphQL listener can be declared as described below, honoring the Ballerina generic [listener declaration](https://ballerina.io/spec/lang/2021R1/#section_9.2.1).

#### 2.1.3 Initializing the Listener Using Port Number
If a GraphQL listener requires to be listening to a port number, that port number must be provided as the first parameter of the listener constructor.

###### Example: Initializing the Listener Using Port Number
```ballerina
listener graphql:Listener graphqlListener = new (9090);
```

#### 2.1.4 Initializing the Listener using an HTTP Listener
If a GraphQL listener requires to listen to the same port as an existing [`http:Listener`](https://github.com/ballerina-platform/module-ballerina-http/blob/master/docs/spec/spec.md/#21-listener) object, that `http:Listener` object must be provided as the first parameter of the listener constructor.

###### Example: Initializing the Listener using an HTTP Listener
```ballerina
listener http:Listener httpListener = new (9090);
listener graphql:Listener graphqlListener = new (httpListener);
```

>**Note:**  The client does not need to explicitly create a `websocket:Listener` for subscriptions. If there is a subscription resolver in a service, the `graphql:Listener` will initiate a `websocket:Listener` over the same port used in the `http:Listener`.

#### 2.1.5 Listener Configuration

Since the GraphQL listener uses the `http:Listener` and the `websocket:Listener` as the underlying listeners, some additional configurations can be passed to these listeners, when creating a `graphql:Listener`. These configurations are defined in the `graphql:ListenerConfiguration` record.

###### Example: Listener Configuration

```ballerina
listener graphql:Listener graphqlListener = = new (9090, timeout = 10);
```

>**Note:** If the GraphQL service includes subscription operations, the `httpVersion` of the `graphql:ListenerConfiguration` must be either `"1.0"` or `"1.1"`. Otherwise, this will cause a runtime error when attaching the service to the listener.

### 2.2 Service

The `service` represents the GraphQL schema in the Ballerina GraphQL package. When a service is attached to a GraphQL listener, it is considered a GraphQL service. When a service is identified as a GraphQL service, it will be used to [Generate the Schema](#3-schema-generation). Attaching the same service to multiple listeners is not allowed, and will cause a compilation error.

###### Example: Service

```ballerina
service /graphql on new graphql:Listener(9090) {

}
```

In the above [example](#example-service), a GraphQL service is attached to a GraphQL listener. This is syntactic sugar to declare a service and attach it to a GraphQL listener.

#### 2.2.1 Service Type

The following distinct service type provided by the Ballerina GraphQL package can be used by the users. It can be referred to as `graphql:Service`. Since the language support is yet to be implemented for the service typing, service validation is done using the Ballerina GraphQL compiler plugin.

```ballerina
public type Service distinct service object {

};
```

#### 2.2.2 Service Base Path

The base path is used to discover the GraphQL service to dispatch the requests. identifiers and string literals can be used as the base path, and it should be started with `/`. The base path is optional and if not provided, will be defaulted to `/`. If the base path contains any special characters, they should be escaped or defined as string literals.

###### Example: Base Path

```ballerina
service hello\-graphql on new graphql:Listener(9090) {

}
```

#### 2.2.3 Service Declaration

The [service declaration](https://ballerina.io/spec/lang/2021R1/#section_10.2.2) is syntactic sugar for creating a service. This is the mostly-used approach for creating a service.

###### Example: Service Declaration

```ballerina
service graphql:Service /graphql on new graphql:Listener(9090) {

}
```

#### 2.2.4 Service Object Declaration

A service can be instantiated using the service object. This approach provides full control of the service life cycle to the user. The listener life cycle methods can be used to handle this.

###### Example: Service Object Declaration

```ballerina
graphql:Service graphqlService = service object {
    resource function get greeting() returns string {
        return "Hello, world!";
    }
}

public function main() returns error? {
    graphql:Listener graphqlListener = check new (9090);
    check graphqlListener.attach(graphqlService, "graphql");
    check graphqlListener.'start();
    runtime:registerListener(graphqlListener);
}
```

>**Note:** The service object declaration is only supported when the service object is defined in global scope. If the service object is defined anywhere else, the schema generation will fail. This is due to a known current limitation in the Ballerina language.

#### 2.2.5 Service Configuration

The `graphql:ServiceConfiguration` annotation can be used to provide additional configurations to the GraphQL service. These configurations are described in the [Service Configuration](#101-service-configuration) section.

### 2.3 Parser
The Ballerina GraphQL parser is responsible for parsing the incoming GraphQL documents. This will parse each document and then report any errors. If the document is valid, it will return a syntax tree.

>**Note:** The Ballerina GraphQL parser is implemented as a separate module and is not exposed outside the Ballerina GraphQL package.

### 2.4 Engine

The GraphQL engine acts as the main processing unit in the Ballerina GraphQL package. It connects all the other components in the Ballerina GraphQL service together.

When a request is received by the GraphQL Listener, it dispatches the request to the GraphQL engine, where it extracts the document from the request, then passes it to the parser. Then the parser will parse the document and return an error (if there is any) or the syntax tree to the engine. Then the engine will validate the document against the generated schema, and then if the document is valid, the engine will execute the document.

### 2.5 Client

The GraphQL client can be used to connect to a GraphQL service and retrieve data. This client currently supports the `Query` and `Mutation` operations. The Ballerina GraphQL client uses HTTP as the underlying protocol to communicate with the GraphQL service.

#### 2.5.1 Initializing the Client

The `graphql:Client` init method requires a valid URL and optional configuration to initialize the client.

```ballerina
graphql:Client graphqlClient = check new (“http://localhost:9090/graphql”, {timeout: 10});
```
#### 2.5.2 Executing Operations

The graphql client provides `execute` API to execute graphql query and mutation operations. The `execute` method of `graphql:Client` takes a GraphQL document as the required argument and sends a request to the specified backend URL seeking a response. Further, the execute method could take the following optional arguments.

- `variables` - A map containing the GraphQL variables. All the variables that may be required by the graphql document can be set via this `variables` argument.
- `operationName` - The GraphQL operation name. If the document has more than one operation, then each operation must have a name. A single GraphQL request can only execute one operation; the operation name must be set if the document has more than one operation. Otherwise, the GraphQL server responds with an error.
- `headers` - A map containing headers that may be required by the graphql server to execute each operation.

The method definition of the `execute` API is given below.

```ballerina
remote isolated function execute(string document, map<anydata>? variables = (), string? operationName = (),
    map<string|string[]>? headers = (), typedesc<GenericResponseWithErrors|record {}|json> targetType = <>)
    returns targetType|ClientError ;
```

#### 2.5.3 Client Data Binding

When sending a GraphQL request to a GraphQL server using the Ballerina GraphQL client, the response can be data-bound. That means the user can define the expected shape of the GraphQL response by defining a type. The data type defined by the user should be a subtype of `graphql:GenericResponseWithErrors|record{}|json`. Otherwise, the data binding fails with an error.

>**Note:** It is recommended to use the `graphql:GenericResponseWithErrors` or any subtype of it when retrieving a response using the `graphql:Client` using the `execute` method.

When defining the expected type, nullable fields should be defined as a union of the field type and nil (`()`). A [Payload Binding Error](#732-payload-binding-error) can occur otherwise.

###### Example: Handle Response with Data Binding

This example shows how to data-bind the response to a pre-defined type. Note how the `age` field is defined as a nullable field.

```ballerina
type ProfileResponseWithErrors record {|
    *graphql:GenericResponseWithErrors;
    record {|Profile profile;|} data;
|};

type Profile record {|
    string name;
    int? age;
|};

public function main() returns error? {
    graphql:Client graphqlClient = check new ("localhost:9090/graphql");
    string document = "{ profile(id: 100) {name age} }";
    ProfileResponseWithErrors response = check graphqlClient->execute(document);
    string name = response.data.profile.name;
    io:println(name);
}
```

The `execute` method can return errors when retrieving a response from a GraphQL API. For information about handling errors, check the section [Client Error Handling](#73-client-error-handling)

#### 2.5.4 Client Configuration

The `graphql:Client` uses `http:Client` as its underlying implementation; this `http:Client` can be configured by providing the `graphql:ClientConfiguration` as an optional parameter via the `graphql:Client` init method.

## 3. Schema Generation

The GraphQL schema is generated by analyzing the Ballerina service attached to the GraphQL listener. The Ballerina GraphQL package will walk through the service and the types related to the service to generate the complete GraphQL schema.

When an incompatible type is used inside a GraphQL service, a compilation error will be thrown.

### 3.1 Root Types
Root types are a special set of types in a GraphQL schema. These types are associated with an operation, which can be done on the GraphQL scheme. There are three root types.

- `Query`
- `Mutation`
- `Subscription`

#### 3.1.1 The `Query` Type

The `Query` type is the main root type in a GraphQL schema. It is used to query the schema. The `Query` must be defined for a GraphQL schema to be valid. In Ballerina, the service itself is the schema, and each `resource` method with the `get` accessor inside a GraphQL service is mapped to a field in the root `Query` type.

###### Example: Adding a Field to the `Query` Type

```ballerina
service on new graphql:Listener(9090) {
    resource function get greeting() returns string {
        return "Hello, World!";
    }
}
```

>**Note:** Since the `Query` type must be defined in a GraphQL schema, a Ballerina GraphQL service must have at least one resource method with the `get` accessor. Otherwise, the service will cause a compilation error.

#### 3.1.2 The `Mutation` Type

The `Mutation` type in a GraphQL schema is used to mutate the data. In Ballerina, each `remote` method inside the GraphQL service is mapped to a field in the root `Mutation` type. If no `remote` method is defined in the service, the generated schema will not have a `Mutation` type.

###### Example: Adding a Field to the `Mutation` Type

```ballerina
service on new graphql:Listener(9090) {
    remote function setName(string name) returns string {
        //...
    }
}
```

As per the [GraphQL specification](https://spec.graphql.org/June2018/#sec-Mutation), the `Mutation` type is expected to perform side effects on the underlying data system. Therefore, the mutation operations should be executed serially. This is ensured in the Ballerina GraphQL package. Each remote method invocation in a request is done serially, unlike the resource method invocations, which are executed in parallel.

#### 3.1.3 The `Subscription` Type

The `Subscription` type in a GraphQL schema is used to continuously fetch data from a GraphQL service. In Ballerina, each `resource` method with the `subscribe` accessor inside a GraphQL service is mapped to a field in the root `Subscription` type. If the `resource` method has the `subscribe` accessor, it must return a `stream`. Otherwise, the compilation error will occur.

###### Example: Adding a Field to the `Subscription` Type

```ballerina
service on new graphql:Listener(9090) {
    resource function subscribe greetings() returns stream<stream> {
        return ["Hello", "Hi", "Hello World!"].toStream();
    }
}
```

### 3.2 Wrapping Types

Wrapping types are used to wrap the named types in GraphQL. A wrapping type has an underlying named type. There are two wrapping types defined in the GraphQL schema.

#### 3.2.1 `NON_NULL` Type

`NON_NULL` type is a wrapper type to denote that the resulting value will never be `null`. Ballerina types do not implicitly allow `nil`. Therefore, each type is inherently a `NON_NULL` type until specified explicitly otherwise. If a type is meant to be a nullable value, it should be unionized with `nil`.

>**Note:** `nil` (represented by `()`) is the Ballerina's version of `null`.

In the following example, the type of the `name` field is `String!`. This means the `String` type is wrapped by the `NON_NULL` type.

###### Example: NON_NULL Type
```ballerina
service on new graphql:Listener(9090) {
    resource function get name returns string {
        return "Walter White";
    }
}
```

To make it a nullable type, it should be unionized with `?`. The following example shows the field `name` of the type `String`. This means the `name` field can have a `null` value.

###### Example: Nullable Type
```ballerina
service on new graphql:Listener(9090) {
    resource function get name returns string? {
        return "Walter White";
    }
}
```

>**Note:** `?` is syntactic sugar for `|()`.

#### 3.2.2 `LIST` Type

The list type represents a list of values of another type. Therefore, `LIST` is considered a wrapping type. In Ballerina, a `LIST` type is defined using an array. The following represents a field called `names` of the type of `LIST` of `String!` type.

###### Example: LIST Type
```ballerina
service on new graphql:Listener(9090) {
    resource function get names() returns string[] {
        return ["Walter White", "Jesse Pinkman"];
    }
}
```

### 3.3 Resource Methods

Resource methods are a special kind of method in Ballerina. In the Ballerina GraphQL package, `resource` methods are used to define GraphQL object fields. The `resource` methods in a GraphQL service are validated at the compile-time.

#### 3.3.1 Resource Accessor

The only allowed accessors in Ballerina GraphQL resource are, `get` and `subscribe`. Any other accessor usage will result in a compilation error.

###### Example: Resource Accessor

```ballerina
resource function get greeting() returns string {
    // ...
}
```

###### Counter Example: Resource Accessor

```ballerina
resource function post greeting() returns string {
    // ...
}
```

#### 3.3.2 Resource Name

As the `resource` methods are mapped to a field of a GraphQL `Object` type, the resource name represents the name of the corresponding field.

###### Example: Resource Name

```ballerina
resource function get greeting() returns string {
    // ...
}
```

In the above example, the resource represents a field named `greeting` of type `String!`. Check [Types Section](#4-types) for more information on types and fields.

#### 3.3.3 Hierarchical Resource Path

GraphQL represents the data as a hierarchical structure. Ballerina resources provide different ways to define this structure. Hierarchical resource paths are one approach, which is also the simplest way.

The path of a resource can be defined hierarchically so that the schema generation can generate the types using the hierarchical path. When a service has resources with hierarchical resource paths, the first path segment and each intermediate path segment of a resource represent an `Object` type field. The GraphQL type represented by the return type of the `resource` method is assigned to the field represented by the leaf-level (final) path segment. Each intermediate type has the same name as the path segment. Therefore, the field name and the type name are the same for the intermediate path segments.

###### Example: Hierarchical Resource Path

```ballerina
service graphql:Service on new graphql:Listener(9090) {
    resource function get profile/address/number() returns int {
        return 308;
    }

    resource function get profile/address/street() returns string {
        return "Negra Arroyo Lane";
    }

    resource function get profile/address/city() returns string {
        return "Albuquerque";
    }

    resource function get profile/name() returns string {
        return "Walter White";
    }

    resource function get profile/age() returns int {
        return 52;
    }
}
```

In the above example shows how to use hierarchical resource paths to create a hierarchical data model. When the schema is generated using this service, the root `Query` operation has a single field, `profile`, as it is the only path segment at the top level. The type of this field is also `profile`, which is an `Object` type. This object type has three fields: `address`, `name`, and `age`. The type of the `address` field is also `Object` as it is an intermediate path segment (i.e. has child path segments). The name of this object type is `address`. It has three fields: the `number` (type `Int!`), the `street` (type `String!`), and the `city` (type `String!`). The `name` field is of type `String!`, and the `age` field is of type `Int!`. Check [Types Section](#4-types) for more information on types and fields.

### 3.4 Remote Methods

The `remote` methods are used to define the fields of the `Mutation` type in a GraphQL schema. Remote methods are validated at the compile-time.

>**Note:** The `resource` and `remote` methods are called __*resolvers*__ in GraphQL terminology. Therefore, in this spec, sometimes the term __*resolver*__ is used to refer `resource` and `remote` methods.

#### 3.4.1 Remote Method Name

The name of the `remote` method is the name of the corresponding GraphQL field in the `Mutation` type.

### 3.5 Documentation

A GraphQL schema can have documentation for the types, fields, enums, schema, etc.

In Ballerina, the Ballerina doc comments can be used to add documentation for the generated schema. Each comment belonging to a field, argument, or enum will be applied to the particular GraphQL schema member.

###### Example: Documentation

```ballerina
# Service to query people database.
service on new graphql:Listener(9090) {

    # Returns a profile with the given ID.
    #
    # + id - The ID of the profile
    # + return - The profile with the given ID
    resource function get profile(int id) returns Profile {
        // ...
    }

    # Represents a profile.
    #
    # + id - The ID of the profile
    # + name - The name of the profile
    # + age - The age of the profile
    public type Profile record {|
        int id;
        string name;
        int age;
    |};
}
```

This will generate the documentation for all the fields of the `Query` type including the field descriptions of the `Profile` type.

>**Note:** When a field or an argument name contains Unicode characters or any other escape characters, they are unescaped when generating the schema.

###### Example: Escaping Characters
```ballerina
service on new graphql:Listener(9090) {
    resource function get 'type(string 'version) returns string {
        return "";
    }

    resource function get name(string \u{0076}ersion) returns string {
        return "";
    }
}
```

The above code will generate the following schema:

```graphql
type Query {
    type(version: String!): String!
    name(version: String!): String!
}
```

## 4. Types

GraphQL type system is represented using a hierarchical structure. Type is the fundamental unit of any GraphQL schema.

### 4.1 Scalars

Scalar types represent primitive leaf values in the GraphQL type system. The following built-in types are supported in the Ballerina GraphQL package. Scalar values are represented by the primitive types in Ballerina.

#### 4.1.1 Int
The `Int` type is represented using the `int` type in Ballerina.

#### 4.1.2 Float
The `Float` type is represented using the `float` type in Ballerina.

>**Note:** When used as an input value type, both integer and float values are accepted as valid inputs.

#### 4.1.3 String
The `String` type is represented using the `string` type in Ballerina. It can represent Unicode values.

#### 4.1.4 Boolean
The `Boolean` type is represented using the `boolean` type in Ballerina.

Apart from the above types, the `decimal` type can also be used inside a GraphQL service, which will create the `Decimal` scalar type in the corresponding GraphQL schema.

### 4.2 Objects

Objects represent the intermediate levels of the type hierarchy. Objects can have a list of named fields, each of which has a specific type.

In Ballerina, a GraphQL object type can be represented using either a service type or a record type.

#### 4.2.1 Record Type as Object

A Ballerina record type can be used as an Object type in GraphQL. Each record field is mapped to a field in the GraphQL object and the type of the record field will be mapped to the type of the corresponding GraphQL field.

###### Example: Record Type as Object
```ballerina
service on new graphql:Listener(9090) {
    resource function get profile() returns Profile {
        return {name: "Walter White", age: 52};
    }
}

type Profile record {|
    string name;
    int age;
|};
```

>**Note:** Even though anonymous record types are supported in Ballerina, they cannot be used as types in a GraphQL schema. This is because a type in a GraphQL schema must have a name. Therefore, if an anonymous record is used in a GraphQL service, it will result in a compilation error.

>**Note:** When a record field is defined as an optional field, the Ballerina GraphQL engine identifies it as a nullable field. Even though it is supported, it is recommended to use Ballerina nilable types instead of optional fields to define nullable fields in GraphQL.

>**Hint:** Open records are supported in GraphQL services, but they do not make sense in the context of GraphQL since a GraphQL type cannot have dynamic fields. Therefore, it is recommended to use closed records in GraphQL services unless it is absolutely needed.

#### 4.2.2 Service Type as Object

A Ballerina service type can be used as an `Object` type in GraphQL. Similar to the `Query` type, each resource method inside a service type represents a field of the object.

Since GraphQL only allows mutations at the top level and the `remote` methods are used to represent the `Mutation` type, any service type used to represent a GraphQL `Object` cannot have `remote` methods inside them.

>**Note:** As per the GraphQL spec, only the root type can have `Mutation`s. Therefore, defining `remote` methods inside subsequent object types does not make any sense.

The `resource` methods in these service types can have input parameters. These input parameters are mapped to arguments in the corresponding field.

>**Note:** The service types representing an `Object` can be either `distinct` or non-distinct type. But if a service type is used as a member of a `Union` type, they must be `distinct` service classes.

###### Example: Service Type as Object

