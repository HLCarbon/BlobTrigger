# Azure Blob Trigger Function

## How It Works
1. Triggered by new blob upload
2. Processes document using Document Intelligence
3. Stores JSON results in result container

### Usage Scenarios
- **Receipt Processing**: Upload receipts (PDF/Images) to automatically extract:
  - Merchant information
  - Transaction date and time
  - Total amount
  - Tax details
  - Line items
  - Payment method

- **Integration Examples**:
  - Connect to accounting software to automate expense entry
  - Feed into expense management systems
  - Store in databases for financial reporting
  - Trigger downstream processes based on extracted data

- **Automation Flow**:
  1. Upload receipt to source container
  2. Function automatically processes and extracts data
  3. JSON result contains structured receipt data
  4. Use the JSON output for further processing or storage

## Technical Implementation
- Uses Azure Functions isolated process model (.NET 8)
- When a file is added to the container, receives blob content as byte array to handle binary files
- Converts byte array to MemoryStream for Document Intelligence processing
- Uses prebuilt receipt model for standardized extraction
- Implements managed identity for secure authentication
- Serializes results with indented JSON formatting

## Configuration

### Required Settings
- `BlobContainerName`: Source container for documents
- `ResultBlobContainerName`: Container for JSON results
- `DocumentIntelligenceEndpoint`: Document Intelligence service endpoint
- `storage-account-blueledger-connection-string`: Storage account connection

### Managed Identity Permissions
- Storage Blob Data Contributor role on storage account
- Cognitive Services User role on Document Intelligence

### Key Vault Integration
Store these values in Key Vault:
- `DocumentIntelligenceEndpoint`
- `storage-account-blueledger-connection-string`

Format: `@Microsoft.KeyVault(SecretUri=https://your-keyvault.vault.azure.net/secrets/secret-name)` 