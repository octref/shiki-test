# Specification: Ballerina gRPC Library

_Owners_: @shafreenAnfar @daneshk @BuddhiWathsala @MadhukaHarith92 @dilanSachi  
_Reviewers_: @shafreenAnfar @daneshk @dilanSachi  
_Created_: 2021/12/05   
_Updated_: 2022/10/14  
_Edition_: Swan Lake  

## Introduction
This is the specification for the gRPC standard library of [Ballerina language](https://ballerina.io/), which provides APIs for gRPC client and server implementation.

The gRPC library specification has evolved and may continue to evolve in the future. The released versions of the specification can be found under the relevant GitHub tag.

If you have any feedback or suggestions about the library, start a discussion via a GitHub issue or in the [Discord server](https://discord.gg/ballerinalang). Based on the outcome of the discussion, the specification and implementation can be updated. Community feedback is always welcome. Any accepted proposal which affects the specification is stored under `/docs/proposals`. Proposals under discussion can be found with the label `type/proposal` in GitHub.

The conforming implementation of the specification is released and included in the distribution. Any deviation from the specification is considered a bug.

## Contents
1. [Overview](#1-overview)
2. [gRPC command line interface (CLI)](#1-grpc-command-line-interface-cli)
3. [Protocol buffers to Ballerina data mapping](#3-protocol-buffers-to-ballerina-data-mapping)
4. [gRPC communication](#4-grpc-communication)
   * 4.1. [Simple RPC](#41-simple-rpc)
   * 4.2. [Server streaming RPC](#42-server-streaming-rpc)
   * 4.3. [Client streaming RPC](#43-client-streaming-rpc)
   * 4.4. [Bidirectional streaming RPC](#44-bidirectional-streaming-rpc)
5. [gRPC security](#51-authentication-and-authorization)
   * 5.1. [Authentication and authorization](#51-authentication-and-authorization)
      * 5.1.1. [Declarative approach](#511-declarative-approach)
         * 5.1.1.1. [Service - basic auth - file user store](#5111-service---basic-auth---file-user-store)
         * 5.1.1.2. [Service - basic auth - LDAP user store](#5112-service---basic-auth---ldap-user-store)
         * 5.1.1.3. [Service - JWT auth](#5113-service---jwt-auth)
         * 5.1.1.4. [Service - OAuth2](#5114-service---oauth2)
         * 5.1.1.5. [Client - basic auth](#5115-client---basic-auth)
         * 5.1.1.6. [Client - bearer token auth](#5116-client---bearer-token-auth)
         * 5.1.1.7. [Client - self-signed JWT auth](#5117-client---self-signed-jwt-auth)
         * 5.1.1.8. [Client - OAuth2](#5118-client---oauth2)
      * 5.1.2 [Imperative Approach](#512-imperative-approach)
         * 5.1.2.1. [Service - basic auth - file user store](#5121-service---basic-auth---file-user-store)
         * 5.1.2.2. [Service - basic auth - LDAP user store](#5122-service---basic-auth---ldap-user-store)
         * 5.1.2.3. [Service - JWT auth](#5123-service---jwt-auth)
         * 5.1.2.4. [Service - OAuth2](#5124-service---oauth2)
         * 5.1.2.5. [Client - basic auth](#5125-client---basic-auth)
         * 5.1.2.6. [Client - bearer token auth](#5126-client---bearer-token-auth)
         * 5.1.2.7. [Client - self-signed JWT auth](#5127-client---self-signed-jwt-auth)
         * 5.1.2.8. [Client - OAuth2](#5128-client---oauth2)
6. [gRPC utility functions](#6-grpc-utility-functions)
   * 6.1. [gRPC deadline](#61-grpc-deadline)
   * 6.2. [gRPC compression](#62-grpc-compression)
   * 6.3. [gRPC access and trace logs](#63-grpc-access-and-trace-logs)
   * 6.4. [gRPC retry](#64-grpc-retry)
7. [gRPC Server Reflection](#7-grpc-server-reflection)


