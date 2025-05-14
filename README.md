# NexiusAI GitHub Actions

Ez a repository két GitHub Action-t tartalmaz az Azure Container Apps szolgáltatáshoz való build és deploy folyamatokhoz.

## NexiusAI Build Action

Ez az action Docker image-eket épít és feltölti azokat az Azure Container Registry-be.

### Bemeneti paraméterek

| Paraméter             | Leírás                                                                                 | Kötelező |
|-----------------------|----------------------------------------------------------------------------------------|----------|
| AZURE_CLIENT_ID       | Azure szolgáltatásnév kliens azonosítója                                               | Igen |
| AZURE_CLIENT_SECRET   | Azure szolgáltatásnév titkos kulcsa                                                    | Igen |
| AZURE_TENANT_ID       | Azure tenant azonosító                                                                 | Igen |
| AZURE_SUBSCRIPTION_ID | Azure előfizetés azonosító                                                             | Igen |
| AZURE_REGISTRY_NAME   | Azure Container Registry neve                                                          | Igen |
| BUILD_EXTRA_PARAMETER | Itt lehet megtadni extra paramétereket a buid-nek. vigyázz, nincs szintaxis ellenörzés | Nem |
| IMAGE_NAME            | Docker image neve                                                                      | Igen |
| IMAGE_TAG             | Docker image tag                                                                       | Igen |
| IMAGE_POSTFIX         | Opcionális utótag az image tag-hez                                                     | Nem |
| DOCKER_FILE           | Dockerfile elérési útja (alapértelmezett: 'Dockerfile')                                | Nem |
| DOCKER_TARGET         | Docker build Target opció                                                              | Nem |



## NexiusAI Deploy Action

Ez az action telepíti a Docker image-et az Azure Container Apps szolgáltatásba.

### Bemeneti paraméterek

| Paraméter | Leírás | Kötelező |
|-----------|--------|----------|
| AZURE_CLIENT_ID | Azure szolgáltatásnév kliens azonosítója | Igen |
| AZURE_CLIENT_SECRET | Azure szolgáltatásnév titkos kulcsa | Igen |
| AZURE_TENANT_ID | Azure tenant azonosító | Igen |
| AZURE_SUBSCRIPTION_ID | Azure előfizetés azonosító | Igen |
| AZURE_REGISTRY_NAME | Azure Container Registry neve | Igen |
| AZURE_INSTANCE_NAME | Azure Container App példány neve | Igen |
| AZURE_RESOURCE_GROUP_NAME | Azure erőforráscsoport neve | Igen |
| IMAGE_NAME | Docker image neve | Igen |
| IMAGE_TAG | Docker image tag | Igen |
| IMAGE_POSTFIX | Opcionális utótag az image tag-hez | Nem |

## NexiusAI Image Copy

Ez az action a megadott Docker image-et átmásolja az Azure Container Registry-ből egy másik Azure Container Registry-be.

### Bemeneti paraméterek

| Paraméter                    | Leírás                                          | Kötelező |
|------------------------------|-------------------------------------------------|----------|
| SOURCE_AZURE_CLIENT_ID       | Forrás Azure szolgáltatásnév kliens azonosítója | Igen |
| SOURCE_AZURE_CLIENT_SECRET   | Forrás Azure szolgáltatásnév titkos kulcsa      | Igen |
| SOURCE_AZURE_TENANT_ID       | Forrás Azure tenant azonosító                   | Igen |
| SOURCE_AZURE_SUBSCRIPTION_ID | Forrás Azure előfizetés azonosító               | Igen |
| SOURCE_AZURE_REGISTRY_NAME   | Forrás Azure Container Registry neve            | Igen |
| TARGET_AZURE_CLIENT_ID       | Cél Azure szolgáltatásnév kliens azonosítója    | Igen |
| TARGET_AZURE_CLIENT_SECRET   | Cél Azure szolgáltatásnév titkos kulcsa      | Igen |
| TARGET_AZURE_TENANT_ID       | Cél Azure tenant azonosító                   | Igen |
| TARGET_AZURE_SUBSCRIPTION_ID | Cél Azure előfizetés azonosító               | Igen |
| TARGET_AZURE_REGISTRY_NAME   | Cél Azure Container Registry neve            | Igen |
| IMAGE_NAME | Docker image neve                               | Igen |

## Előfeltételek

- Azure előfizetés
- Azure Container Registry
- Azure Container Apps szolgáltatás
- Azure szolgáltatásnév megfelelő jogosultságokkal

## Biztonság

A titkos értékeket (AZURE_CLIENT_ID, AZURE_CLIENT_SECRET, stb.) mindig GitHub Secrets-ként tárolja és használja!

## Verziókezelés

A deploy action automatikusan hozzáad egy VERSION környezeti változót a konténerhez a GitHub run number értékével.