# Kubernetes API & RESTful Interface 

> Application program interface that serves kubernetes functionality through a RESTful interface & stores the state of the cluster 

- K8's resources and records of intent are stored as API objects that can be modified via RESTful calls to the API server. 

- An API defines the ruls that you must follow to communicate with different software systems.

- The process of exposing or creating an API so others can communicate.

- REST: Representational state transfer, is a software architecture that imposes conditions on how API's should work.

# RESTful API 

- Provides a guidline to manage communication on networks e.g. the internet
- Supports high performing and reliable communication at scale
- Brings visibility and cross platform portablity to any API system 

# Benefits 
- Scalability 
- Flexibility 
- Independence

# REST Architecture style

## Uniform Interface 

- Server transfers information in a standard format 
> Formatted resource is called a representation
- Format can be different from internal on server application 

1. Request should identify resources, they do so by using a uniform resource identifier. 
2. Clients have enough info in the resource representation to modify or delete. The server meets this confition by sending metadata that describes the resources further. 
3. Clients recieve info about how to process the representation; server sends self-descriptive messages that contains meta-data about how a client can use them.
4. Clients recieve info about all other related resources they need to complete the task; server sends hyperlinks in the representation so clicents can dynamically discover more resources. 

## Statelessness 
- Communication method in which server completes every client request independently of all previous requests (Helpful when needing to scale, stops resource bottleneck)
- Clients can request resources in any order, every request is stateless or isolated from other resources.
- Design constrait implies server can understand & fufil request every time.

## Layered system 
- Client can connect to other authorized intermediors between the client and server. 
- Servers can pass on requests of other servers. 
- Freedom to design RESTful web services to run on serveral servers; with multiple layers such as security, application and buisness logic;
> They all work together to fufil client requests which remain invisible to the client.

## Cacheability 
- RESTful web services support cacheing.
- Controlled by using API Responses that define themselves as cacheable or noncacheable. 

## Code on demand 
- Services can temporaly extend or customize client functionality by transferring software programming code to the client.

# The flow 

1. Client sends request 
2. Server authenticates & Checks permissions 
3. Server recieves & processes request 
4. Server returns a responses to the client; e.g. success? what did the client request.


# Client Request 

## Unique Resource Identifier 
- Typically performs identification using URL which specifies the path to the resource (resource endpoint)
- Specifies to the server what the client requires

## Method 
> Often implemented using HTTP; specifies what the server needs to do to the resources 

### GET 
- Access resources that are located at the specified URL / endpoint 
- Can cache `get` requests
- Can send parameters in RESTful API request to filter data before returning 

### POST
- Send data to the server
- Includes data representation 
- Sending the same request multiple times can duplicate the resource 

### PUT 
- Update exisiting resource on the server 
- Sending the same request multiple times has the same result.

### DELETE
- Removes resource from the server 
- Can change the servers state
- If user does not have the appropriate authentication the request will fail

## HTTP Headers
> Request headers is the metadata exchanged between client and server
- Format or request andn response
- Request status 

### Data
- REST Api requests include data for `POST`, `PUT` and others

### Parameters 
- Path parameters that specify URL / endpoint details.
- Query parameters that request more information about resources.

# Authentication Methods 
> Server must authenticate requests before sending a response; verify identity. 

- Clients must prove identity to the server to provide trust. 

## HTTP Authentication 
> Defines some authentication schemes
### Basic Authentication 
- Client sends username and password in the request header.
- Encodes with `base64` which converts the pair into a string of 64 characters for safe transmission. 

### Bearer Authentication 
- Gives access control to the token bearer. 
- Token is typically encrypted string of characters that the server generates in response to a logic request. 
- Client sends the token in request header to access resources

### API Keys 
- Server assigns a unique generated balue to first time client, when client attempts to access resources on the server, it uses the API key to identify itself.
- Less secure as client has to transmit the key making it vulnerable to network theft.

### OAUTH 
- Combines passwords and tokens for highly secure logic across any system. 
- First requests a password and then asks for additional token.

# Server Response 
## Status line
- 3 digit status code 
- `2xx` = Success
- `4xx` & `5xx` = Failure / error
- `3xx` = Redirect 

### Generic status codes 
- `200` : Generic success
- `201` : `POST` success 
- `400` : Incorrect request, can't process
- `404` : Not found

## Message body 
- Contains resource representation, based on the request and the request header

## Headers
- Contains header or metadata including; server, encoding, date and content type.
 