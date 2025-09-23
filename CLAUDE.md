# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains the OpenAPI 3.0.3 specification for the HomePro Backend API, a comprehensive home management system built with Scala 3.5.0 and Akka HTTP.

## Architecture

The API specification documents a backend system with the following key components:

- **Backend**: Scala 3.5.0 with Akka HTTP framework
- **Database**: PostgreSQL with direct SQL queries and HikariCP connection pooling
- **Authentication**: Firebase Admin SDK with JWT token validation
- **Storage**: AWS S3 for photo storage with context-based organization
- **Database Features**: JSONB support for flexible metadata storage

## API Structure

The API is organized into the following main functional areas:

### Authentication (`/api/users/login`)
- Firebase JWT-based authentication
- User profile retrieval with role management

### Home Management (`/api/homes`)
- One home per user restriction (current business rule)
- Home creation, retrieval, and deletion with cascading operations
- Statistics tracking (items, photos, emergency items)
- Ownership and role-based access control

### Home Items (`/api/homes/{homeId}/items`)
- Flexible item categorization (room, utility_control, appliance, structural, etc.)
- Emergency item flagging
- JSONB metadata storage for item-specific attributes
- Photo association and counting

### Photo Management (`/api/photos`)
- S3 integration with context-based storage paths:
  - Home Item Photos: `{home_item_id}/{filename}`
  - Home Photos: `{home_id}/{filename}`
  - User Photos: `{user_id}/{filename}`
- Multipart/form-data uploads
- Pre-signed URL generation for secure access
- Cascading delete operations

### Support Requests (`/api/support-requests`)
- Service request lifecycle management
- Priority and status tracking
- Expert assignment capabilities

## Key Business Rules

1. **One Home Restriction**: Users are currently limited to owning one home (will be relaxed in future)
2. **Photo Context Constraint**: Photos must have exactly one of: home_id, home_item_id, or user_id
3. **Cascading Deletes**: Deleting homes/items removes all associated photos from both database and S3
4. **Role-based Authorization**: Strict ownership verification for all operations

## CORS Configuration

All endpoints support CORS with:
- Restricted origins: `http://localhost:3000` and `https://home-owners.tech`
- Comprehensive header support for web browser compatibility
- Preflight request handling via OPTIONS methods

## Default Server Configuration

- Protocol: HTTP (development) / HTTPS (production)
- Host: localhost (configurable)
- Port: 2107 (configurable)
- Examples include common development and production configurations

## Working with the API Specification

This repository contains only the OpenAPI specification file. When working with this specification:

1. **Validation**: Ensure all schema definitions are consistent with the described backend implementation
2. **Examples**: Maintain realistic examples that reflect actual API behavior
3. **Documentation**: Keep descriptions comprehensive for both internal and external API consumers
4. **Versioning**: This is version 1.0.0 - follow semantic versioning for changes

## Development Workflow

### Git Usage
This repository contains:
- OpenAPI specification (`homepro-api-contract.yaml`)
- Postman collection with automated Firebase authentication
- Environment configurations for local and dev testing
- Comprehensive documentation

### Common Commands
Since this is a documentation and testing repository:
- No build commands required
- No test runners (testing done via Postman)
- No linting setup (YAML/JSON validation via editors)

### File Structure
```
├── homepro-api-contract.yaml           # OpenAPI 3.0.3 specification
├── HomePro_API.postman_collection.json # Complete Postman collection
├── Local.postman_environment.json      # Local development environment
├── Dev.postman_environment.json        # Development server environment
├── README.md                           # Setup and usage instructions
├── CLAUDE.md                          # This file
└── .gitignore                         # Git ignore patterns
```

### Security Notes
- Environment files contain placeholder values for sensitive data
- Firebase credentials should be set locally in Postman, not committed
- Pre-request scripts handle authentication automatically

## Related Systems

This API specification is designed for integration with:
- Frontend applications (React-based web interface)
- Mobile applications
- Third-party integrations via the documented endpoints
- Postman collections for API testing and development