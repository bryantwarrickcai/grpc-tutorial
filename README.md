## Module 8
### Reflection
1. Unary:
   - 1 request from the client
   - 1 response from the server
   - Single message exchange  

   Server Streaming:
   - 1 request from the client
   - Multiple responses as a stream from the server
   - Server sends data progressively over time  

   Bi-directional Streaming:
   - Multiple requests from the client
   - Multiple responses from the server
   - Allows the client and server to communicate inside the same connection, allowing for real-time communication.  

   Scenarios:
   - Unary: simple queries, such as fetching user details (client sends request with user ID, and server responds with user details (name, email, etc.))
   - Server streaming: things that require live updates, such as live sport scores, requesting the status of data transfer or processing, and file downloading (splitting the file into multiple chunks).
   - Bi-directional streaming: video conferencing, chat application, collaborative editing.
2. Authentication ensures that the server and client can verify each other's identity. Some things that can be used include mutual TLS (mTLS) to authenticate both the server and the client and JWT/OAuth2 tokens to be validated in middleware. With authorization, it controls the list of things that a user can do, based on their roles. For authentication, considerations include implementing role-based access control (RBAC) to enforce access controls based on the user's roles and permissions in the JWT or other tokens. Data encryption ensures that data is encrypted and cannot be read by attackers. Considerations include using strong TLS encryption for all communications and using end-to-end encryption (E2EE).
3.  
   - Error propagation: Errors in one part of the stream may propagate and disrupt the entire stream. Therefore, properly handling errors without terminating the entire stream is crucial and requires careful design.
   - Concurrency management: Rust's ownership model and strict borrowing rules makes it challenging to manage multiple concurrent streams. It is crucial to implement proper handling of asynchronous tasks without causing data races or deadlocks.
   - Order of messages: Ensuring the order of messages is important and requires careful handling, especially when multiple concurrent streams are involved.
   - Testing and debugging: Asynchronous bidirectional streaming may be challenging to perform unit tests on. Integration tests with mock clients and servers are often needed.
4. Advantages:
   - Ease of integration with other Tokio features: Using Tokio's `ReceiverStream` allows seamless integration with Tokio's `mpsc` channels, and there is no need to manually implement `Stream`, as it automatically converts `tokio::sync::mpsc::Receiver` into a `Stream`.
   - Conrurency: Multiple tasks can send data concurrently without blocking the stream.
   - Error handling: `ReceiverStream` can handle closed channels by ending the stream without a panic.

   Disadvantages:
   - Fixed channel capacity: If a bounded channel is used, when the buffer is full, producers may block, potentially causing delays. Using unbounded channels avoids blocking, but risks memory exhaustion.
   - Single consumer limitation: Each `Receiver` can only have a single consumer.
   - Memory overhead: Depending on the buffer size, it may reduce unnecessary memory overhead if the buffer is large and underutilized.
5.  
   - Each of the three services (`PaymentService`, `TransactionService`, and `ChatService`) should each be placed in separate modules or files.
   - Shared module if multiple services use similar or identical data structures
   - Centralize service configurations (such as services or timeouts)
   - Client logic (creating connection, sending request, and handling streams) can be abstracted into a reusable module, reducing redundant code.
   - Use a common error handling utility module for handling gRPC-specific errors.
6.  
   - Add input validation for `user_id` and `amount` to check that the entered data is in a valid format.
   - Sanitize input to prevent injection attacks.
   - Provide more detailed error messages
   - Distinguish between transient errors (such as network issues) and permanent errors.
   - Add a retry method for transient errors.
   - Define more custom error types for different types of errors.
   - Use data encryption
7. The adoption of gRPC as a communication protocol improves the performance and efficiency. gRPC uses Protobuf, which is faster than XML or JSON. gRPC also uses HTTP/2 multiplexing, which allows multiple concurrent requests over a single TCP connection, reducing latency and improving throughput. In terms of interoperability, gRPC automatically generates client and server code in multiple programming languages, making it easier to build polyglot systems. However, interoperability between non-gRPC systems require additional adapters or gateways. Also, gRPC relies on HTTP/2, which is not fully supported in all browsers.
8. Advantages:
   - Multiplexing: HTTP/2 allows multiple requests and responses to be sent simultaneously over a single connection. This is more efficient than HTTP/1.1, which requires multiple connections for each request-response pair.
   - Header compression: HTTP/2 uses header compression, which improves efficiency for APIs with repetitive headers. This is more efficient than HTTP/1.1, which sends headers as plain text, increasing size of the requests.
   - Binary: HTTP/2 uses binary instead of text-based like HTTP/1.1, making it more efficient in terms of parsing, reducing overhead and improving performance.
   - Server push: HTTP/2 allows servers to proactively send resources to clients before they are requested (although this is rarely used in REST APIs). HTTP/1.1 requires explicit requests for each resource.

   Disadvantages:
   - Complexity: HTTP/2 is more complex to implement than HTTP/1.1.
   - Compatibility: Some legacy systems may not fully support HTTP/2.
   - Not human-readable: The binary format makes debugging harder, as specialized tools like Wireshark is needed.
9. REST APIs are not inherently designed for real-time communications, so workarounds are needed, such as WebSockets or SSE. gRPC is specifically built for real-time communication with streaming, reducing the need for additional protocols and workarounds.  
   REST APIs have a higher latency due to the need of parsing XML/JSON and HTTP overhead. On the other hand, gRPC uses binary serialization and multiplexed streams; therefore, it has a lower latency.
10. gRPC and ProtoBuf uses `.proto` files that defines and enforces the data structures of the request and response. This ensures data validation as it performs compile-time check, as each field is strongly typed. With REST + JSON, since JSON is flexible, it allows more loosely structured data. This may lead to inconsistencies and runtime errors if the data structures are not validated and well-documented.  
    ProtoBuf uses a binary format, so it results in a smaller payloads compared to JSON (which is a text-based format). This reduces bandwidth usage and results in a better performance. On the other hand, JSON is text-based, leading to larger payloads. It is more human-readable, but less efficient in terms of transmission size and speed.  
    ProtoBuf handles backward compatibility well by allowing optional fields. This is useful when adding new fields, so that it does not break existing clients. Field tags are also used for implementation to ensure that the data remains correctly interpreted even if the field order changes. On the other hand, REST + JSON's versioning is less structured than ProtoBuf. Therefore, ensuring backward compatibility requires a custom implementation.  
    gRPC and ProtoBuf automatically generates client and server code in multiple languages, which ensures consistency and reduces boilerplate code. JSON does not provide any code generation, so manual coding is required.
