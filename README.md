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
   -
   -
   -

   Disadvantages:
   -
   -
   -
5.
6.
7.
8.
9.
10.
