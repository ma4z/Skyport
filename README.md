
## Overview

This API allows managing users, instances, and nodes. It requires an API key for all requests, which should be passed in the x-api-key header.

## Authentication
API Key:
* An API key is required for all endpoints. It ensures that the request is authenticated and authorized.
* Header: x-api-key

---

## User Endpoints

### GET | /api/users
* Retrieves a list of all users.
* Reponse:
  * '200': List of users.
  * '500': Error retrieving users.

### POST | /api/getUser
* Retrieves a user by email or username.
* Request Body:
  * 'type' (string): The search type, either 'email' or 'username'.
  * 'value' (string): The email or username to search for.
* Response:
  * '201': User details.
  * '400': Missing or invalid parameters, or user not found.
  * '500': Error retrieving user.

### POST | /api/users/create
* Creates a new user.
* Request Body:
  * 'username' (string): Username of the new user.
  * 'email' (string): Email of the new user.
  * 'password' (string): Password for the new user.
  * 'userId' (string, optional): User ID, generated if not provided.
  * 'admin' (boolean, optional): Whether the user has admin rights.
* Response:
  * '201': User created with details.
  * '400': Missing required parameters.
  * '409': User already exists.
  * '500': Error creating user.

## Instances Endpoints

### GET | /api/instances
* Retrieves a list of all instances.
* Response:
  * '200': List of instances.
  * '500': Error retrieving instances.

### POST | /api/instances/deploy
* Deploys a new instance.
* Request Body:
  * 'image' (string): Image to deploy.
  * 'memory' (string): Memory allocation.
  * 'cpu' (string): CPU allocation.
  * 'ports' (string): Port mapping.
  * 'nodeId' (string): Node ID where the instance will be deployed.
  * 'name' (string): Name of the instance.
  * 'user' (string): User ID owning the instance.
  * 'primary' (string): Primary port.
* Response:
  * '201': Instance created.
  * '400': Missing parameters.
  * '500': Error deploying instance.

### DELETE | /api/instance/delete
* Deletes an existing instance.
* Request Body:
  * 'id' (string): ID of the instance to delete.
* Response:
  * '201': Instance deleted.
  * '400': Missing ID or instance not found.
  * '500': Error deleting instance.

### POST | /api/getUserInstance
* Retrieves instances owned by a specific user.
* Request Body:
  * 'userId' (string): ID of the user.
* Response:
  * '200': List of user instances.
  * '400': Missing user ID or user not found.
  * '500': Error retrieving user instances.

### POST | /api/getInstance
* Retrieves details of a specific instance.
* Request Body:
  * 'id' (string): ID of the instance.
* Response:
  * '200': Instance details.
  * '400': Missing instance ID or instance not found.
  * '500': Error retrieving instance.

## Images Endpoints

### GET | /api/images
* Retrieves a list of available images.
* Response:
  * '200': List of images.
  * '500': Error retrieving images.

## Nodes Endpoints

### GET | /api/nodes
* Retrieves a list of all nodes
* Response:
  * '200': List of nodes.
  * '500': Error retrieving nodes.

### POST | /api/nodes/create
* Creates a new node.
* Request Body:
  * 'name' (string): Name of the node.
  * 'tags' (string): Tags associated with the node.
  * 'ram' (string): RAM specification.
  * 'disk' (string): Disk specification.
  * 'processor' (string): Processor specification.
  * 'address' (string): Network address of the node.
  * 'port' (string): Network port.
* Response:
  * '201': Node created with details.
  * '400': Missing parameters.
  * '500': Error creating node.

### DELETE | /api/nodes/delete
* Deletes an existing node.
* Request Body:
  * 'nodeId' (string): ID of the node to delete.
* Response:
  * '201': Node deleted.
  * '400': Invalid or missing node ID.
  * '500': Error deleting node.
---

## Error Responses

* '400': Bad Request. The request was invalid or cannot be otherwise served.
* '401': Unauthorized. API key is missing or invalid.
* '500': Internal Server Error. An error occurred on the server.







## Plugins API Documentation

### Overview

This guide details how to create and manage plugins for our system. Plugins extend and customize the core application's functionality.

### Plugin Structure

A typical plugin includes several key files:

- `manifest.json`: Metadata and configuration for the plugin.
- `index.js`: The main entry point for the plugin's code.
- `router.js`: Defines the express routes for the plugin.

### Example Plugin

Here is an example of a simple plugin:

#### `manifest.json`

```json
{
    "name": "Example Plugin",
    "version": "1.0.0",
    "description": "Description of the Example Plugin",
    "author": "privt00",
    "main": "index.js",
    "router": "example",
    "adminsidebar": {
        "example url": "/example"
    },
    "sidebar": {
        "example url": "/example"
    },
    "instancesidebar": {
        "example url": {
            "url": "/instance/%id%/example",
            "icon": "fa-solid fa-folder"
        }
    }
}
```

In the `manifest.json`, you can configure the metadata for the Skyport Plugin Manager. Key fields include:

- `"main": "index.js"`: Specifies the main file to initialize the plugin.
- `"router": "example"`: Sets a prefix for all express routes. For instance, a route defined as `"/"` will be accessible at `"/example"`.
- `"adminsidebar"`: Links to add to the Admin Sidebar, useful for plugin settings.
- `"sidebar"`: Links for the user sidebar.
- `"instancesidebar"`: Links for the instance navigation bar. You can include placeholders like `%id%` which will be replaced with the instance ID. FontAwesome icons can be used for sidebar entries.

#### `index.js`

```javascript
const express = require('express');
const router = require('./router');
const settings = require('./settings');

function register(addonManager) {
    // Plugin registration logic
    addonManager.registerSettings(settings);
    // console.log('Advanced plugin loaded!');
}

module.exports = {
    register,
    router
};
```

#### `router.js`

```javascript
const express = require('express');
const router = express.Router();
const middleware = require('./middleware');

router.use(middleware);

router.get('/', (req, res) => {
    res.render('index', { user: req.user });
});

module.exports = router;
```

#### `middleware.js`

```javascript
module.exports = function(req, res, next) {
    // Middleware logic
    console.log('Middleware executed for:', req.url);
    next();
};
```

#### `settings.js`

```javascript
module.exports = {
    option1: true,
    option2: "default"
};
```

### Conclusion

This documentation covers the basics of creating and managing plugins within the system. By following the provided structure and guidelines, you can extend the core application's functionality to suit your needs.
