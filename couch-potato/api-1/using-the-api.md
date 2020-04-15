# Using the API

The Couch Potato API is a RESTFul API.

A RESTful API -- also referred to as a RESTful web service or REST API -- is based on representational state transfer \(REST\) technology, an architectural style and approach to communications often used in web services development.

This documentation assumes prior knowledge of using \(consuming\) a REST API.

Each API endpoint is carefully described in the [API Reference](api-reference/), along with code examples written in Typescript. Successful response messages or messages are returned by all API calls and documented in the [Objects](api-reference/objects.md) and [Error Codes](api-reference/error-codes.md) sections of this document.

For easy reference, client side error codes \(400\) are given sub-codes as well.

All the functionality of the Couch Potato web application can be reproduced purely through the API making it easy to create your own applications and data proxies.

API calls that change data or create and post BOS incidents \(triggers\) also update the Couch Potato MySQL database so that the changes are reflected on the web application if it's being used.

This allows for a kind of hybrid development and implementation of Couch Potato. For example, you could use your own interface combined with the ``[`add_game`]() API to create all new games and send the trigger to BOS, but then use the Couch Potato web application to start games and add scores.

The most important API calls to become familiar with are the five core functions that create BOS messages and send them as triggers to BOS. These five calls are:

1. \`\`[`add_game`]() - adds a new game and sends a `create` trigger to BOS.
2. \`\`[`start_game`]() - starts a game and sends an `in_progress` trigger to BOS.
3. \`\`[`add_score`]() - adds scores and send a `result` trigger to BOS.
4. \`\`[`finish_game`]() - finishes a game and sends a `finish` trigger to BOS.
5. \`\`[`cancel_game` ]()- cancels a game that is either not started or in progress and sends a `canceled` trigger to BOS.

