### Overview
This is a collection of postman requests (in json) to use when testing authentication endpoints in preperation to connect them to SchulCloud or related systems.

### Why Postman
Postman offers us a way to test each authentication flow without having to interact with the actual SchulClolud system code. 

### How To Use
1. Download Postman or use it in the browser here: [Postman](https://www.postman.com)
2. Create an account or use it anonymously - an account offers multiple benefits such as a place to store collections and environments secuyrely.
3. Import the collection: Go to File >> Import and select the json files in the Postman repository.
4. Configure the environment: Configure id's, secrets, usernames etc. based on the details shared by the endpoint provider (identity provider, etc.) on the collection name, i.e. the parent.
5. Connect the environment with the collection by selecting the environment as active on the top right in the Postman UI.
6. Ensure that each operation in the collection (GET, POST, ...) inherits the authorization from the parent.

### Additional Information
1. Integrated Console: Postman offers an integrated console that could be used for advanced troubleshooting. This can be found when navigating to View >> Show Postman Console.