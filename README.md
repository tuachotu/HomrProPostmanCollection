# HomePro API Postman Collection

This directory contains a complete Postman collection for the HomePro Backend API with automated Firebase authentication and environment configurations for local and dev environments.

## Files

- `HomePro_API.postman_collection.json` - Main Postman collection with all API endpoints
- `Local.postman_environment.json` - Local development environment
- `Dev.postman_environment.json` - Development server environment
- `homepro-api-contract.yaml` - OpenAPI specification
- `CLAUDE.md` - Claude Code guidance file

## Setup Instructions

### 1. Import into Postman

1. Open Postman
2. Click **Import** in the top left
3. Drag and drop or select the following files:
   - `HomePro_API.postman_collection.json`
   - `Local.postman_environment.json`
   - `Dev.postman_environment.json`

### 2. Configure Firebase Authentication

To use the automatic Firebase authentication, you need to configure your Firebase credentials in the environment variables:

#### Get Firebase Web API Key

1. Go to your Firebase Console
2. Click on Project Settings (gear icon)
3. Scroll down to "Your apps" section
4. If you don't have a web app, create one
5. Copy the `apiKey` value from the Firebase config

#### Set Environment Variables

For each environment (Local and Dev), set the following variables:

**Required Variables:**
- `firebase_api_key` - Your Firebase Web API Key
- `firebase_email` - Email address for a test user in your Firebase project
- `firebase_password` - Password for the test user

**Auto-managed Variables (don't set these manually):**
- `firebase_token` - Auto-generated JWT token
- `firebase_token_expiry` - Token expiration timestamp

**Environment-specific Variables:**
- `protocol` - http (local) or https (dev)
- `host` - localhost (local) or your dev server hostname
- `port` - 2107 (local) or 443 (dev)

### 3. Configure Test Data Variables

After creating resources through the API, update these variables for testing:

- `user_id` - Your user UUID (get from login response)
- `home_id` - Home UUID (get after creating a home)
- `item_id` - Home item UUID (get after creating an item)
- `photo_id` - Photo UUID (get after uploading a photo)

## How Firebase Authentication Works

The collection includes an automatic Firebase authentication system:

1. **Pre-request Script**: Before each API call, the collection checks if a valid Firebase token exists
2. **Token Refresh**: If the token is expired or missing, it automatically calls Firebase Auth API to get a new token
3. **Token Storage**: The token is stored in environment variables and reused for subsequent requests
4. **Automatic Headers**: The Bearer token is automatically added to all requests

## API Endpoint Categories

### Authentication
- **User Login** - Validates Firebase JWT and returns user profile

### Homes
- **Get Homes** - Retrieve user's homes with statistics
- **Create Home** - Create a new home (one per user restriction)
- **Delete Home** - Delete home with cascading deletes

### Home Items
- **Get Home Items** - List items with filtering options
- **Create Home Item** - Add appliances, utility controls, rooms, etc.
- **Delete Home Item** - Remove item and associated photos

### Photos
- **Get Photos** - Retrieve photos by home or home item
- **Upload Photo** - Upload photo for home item (multipart/form-data)
- **Delete Photo** - Remove photo from database and S3

### Support Requests
- **Get Support Requests** - List support requests with filtering
- **Create Support Request** - Submit new service request

## Usage Tips

### 1. Start with Authentication
1. Set up your Firebase credentials in the environment
2. Run the **User Login** request to verify authentication works
3. Copy the `user_id` from the response to your environment variables

### 2. Create Test Data
1. Create a home using **Create Home**
2. Copy the `home_id` from response to environment
3. Create items using **Create Home Item** examples
4. Upload photos using **Upload Photo** request

### 3. Testing Different Item Types
The collection includes examples for different home item types:
- **Appliance**: Kitchen refrigerator with brand/model data
- **Emergency Utility**: Water shutoff valve with location details
- **Room**: Master bedroom with size/features

### 4. Photo Testing
1. Use the **Upload Photo** request with a real image file
2. Copy the returned `photo_id` to your environment
3. Test **Get Photos** to see pre-signed URLs
4. Test **Delete Photo** for cleanup

## Environment Switching

- **Local Environment**: For testing against `localhost:2107`
- **Dev Environment**: For testing against your development server

Switch environments using the dropdown in the top-right of Postman.

## Troubleshooting

### Authentication Issues
- Verify `firebase_api_key` is correct
- Ensure the test user exists in your Firebase project
- Check that the user's email is verified in Firebase
- Look at Postman Console for authentication error details

### API Errors
- Check the API server is running on the expected host/port
- Verify CORS configuration allows your requests
- Review the OpenAPI spec for required request format

### Token Expiry
The Firebase tokens are automatically refreshed 1 minute before expiry. If you see auth errors:
1. Clear the `firebase_token` and `firebase_token_expiry` variables
2. Run any request to trigger a new token generation

## API Documentation

Refer to `homepro-api-contract.yaml` for complete API documentation including:
- Request/response schemas
- Authentication requirements
- Error codes and responses
- Business rules and constraints