# Museum Dataset Service

A dedicated microservice for managing and serving museum collection datasets, including CSV-based datasets like the National Gallery of Art Open Data and future museum integrations.

## Purpose

Centralize dataset management, processing, and search functionality for multiple museum collections, providing a unified API for consuming artwork data across different sources.

## Architecture

```
dataset-service/
├── Handles CSV-based datasets (NGA, future museums)
├── Provides unified search across all datasets
├── Manages dataset synchronization and updates
└── Serves artwork data via REST API
```

## Key Features

- **Multi-Dataset Support**: Handle CSV-based datasets and future museum integrations
- **Unified Search**: Search across all datasets with a single API
- **CSV Processing**: Download, parse, and index CSV datasets from GitHub
- **Image Resolution**: Resolve IIIF image URLs and other image sources
- **Scheduled Sync**: Automatic daily updates for datasets
- **REST API**: Clean, consistent API for consuming artwork data

## API Endpoints

### Dataset-Specific
- `GET /api/datasets/[museum]/search?q=query&limit=20` - Search artworks
- `GET /api/datasets/[museum]/artwork/:id` - Get single artwork
- `POST /api/datasets/[museum]/sync` - Trigger CSV sync (admin only)

### Unified
- `GET /api/datasets/unified/search?q=query` - Search across all datasets

## Data Flow

1. **Sync**: Download CSV files from source (GitHub, APIs, etc.)
2. **Parse**: Parse CSV files into structured data
3. **Index**: Store in database with proper relationships
4. **Serve**: Provide via REST API to consuming applications

## Integration

Consuming applications (like art-history.app) make HTTP requests to the dataset service:

```typescript
// Example: Fetch [museum] artwork
const artwork = await fetch(`${DATASET_SERVICE_URL}/api/datasets/[museum]/artwork/12345`);

// Example: Search across all datasets
const results = await fetch(`${DATASET_SERVICE_URL}/api/datasets/unified/search?q=monet`);
```

## Benefits

- **Separation of Concerns**: Dataset logic isolated from application logic
- **Scalability**: Scale dataset processing independently
- **Reusability**: Multiple applications can consume the same dataset API
- **Centralized Management**: All dataset operations in one place
- **Independent Updates**: Update datasets without deploying main applications

## Technology Stack

- **Framework**: Next.js or Express
- **Database**: PostgreSQL (Neon or Supabase)
- **CSV Parsing**: papaparse or native Node.js
- **Image Handling**: IIIF support, image URL resolution

## Deployment

- Can be deployed as separate service (Vercel.)
- Can share database with main app or use separate database
- Supports both shared and isolated deployment models

## Migration Path

This service can be created after initial dataset integration:
1. Start with direct integration in main app
2. Extract dataset logic when adding 3rd+ dataset
3. Refactor existing integrations to use service API
4. Both approaches can coexist during migration
