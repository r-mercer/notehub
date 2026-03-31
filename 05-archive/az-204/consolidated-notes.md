# AZ-204 Consolidated Notes

> This file consolidates miscellaneous notes from across the AZ-204 archive. See subdirectories for detailed topic notes.

## Key Study Insights

### CosmosDB Consistency Levels
- Session: monotonic reads/writes, good for user-context apps. All reads see own writes.
- Strong: no uncommitted reads, highest write latency, lowest read throughput.
- Bounded Staleness: consistent prefix + staleness window. Low write latency, global order.
- Eventual: no ordering guarantee, highest read throughput and availability.
- Consistent Prefix: guarantees prefix of updates are seen.

Throughput is inversely proportional to predictability. Eventual = max throughput, Strong = FIFO order.

### OAuth 2.0 Flows
- **Auth Code**: redirection-based, for installed apps accessing web APIs.
- **Client Credentials**: web service uses own credentials (not impersonating a user).
- **On-Behalf-Of (OBO)**: app invokes service that calls another service/API.
- **Implicit**: redirection-based, client capable of interacting with user-agent.
- Session, Auth Code, OBO, and Implicit delegate user permission. Client Credentials does not.

### Azure Container Products
- **Container Instances**: isolated simple operations, build jobs, automations. Fast, persistent.
- **Container Apps**: closer to Functions, good for microservices or containerizing Functions.
- **Container Groups**: group containers sharing storage/networking (like docker-compose).

### Change Feed Processor Components
1. Monitored Container - source data
2. Lease Container - state coordination across workers
3. Delegate - custom logic to process changes
4. Compute Instance - VM/pod/App Service hosting the processor

### SAS Tokens
Format: `?sp=r&st=datetime&se=datetime&sv=ver&sr=b&sig=string`
- sp = permissions (add, create, delete, list, read, write)
- st/se = start/end time
- sv = API version
- sr = resource type (b=blob)
- sig = signature

### Graph API
`{verb} https://graph.microsoft.com/{version}/{resource}?{query}`
- version = 1.0 or beta
- Use delegated perms for interactive apps, app perms for app-to-app

## Questions to Revisit
- When to use App Config vs Key Vault?
- Difference between App Service Plan vs Environment?
- JSON format for blob storage rules?
- Staging slot settings inheritance behavior?
