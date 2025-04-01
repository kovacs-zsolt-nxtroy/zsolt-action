# Nexius AI GitHub Actions

Ez a **NexiusAI** számára készült egyedi GitHub Action-ök

## Tartalomjegyzék
- **[NexiusAI Build Action](#nexiusai-build-action)**: Docker image build és upload egy Docker Registry-be.
- **[NexiusAI Deploy Action](#nexiusai-deploy-action)**: Azure Container App frissítése a legújabb Docker image alapján.

---

## NexiusAI Build Action

### Leírás
A `NexiusAI Build Action` segítségével automatizálhatod a Docker image-ek build-elése, a verziózásukat és feltöltésüket a Docker Registry-be.

### Használati példa

```yaml
name: Docker Build Workflow

on: 
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Fetch code
        uses: actions/checkout@v3

      - name: Docker image build and push
        uses: nexius-learning/nexiusai-action/build@main
        with:
          DOCKER_REGISTRY_HOSTNAME: 'my-docker-registry.com'
          DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
          DOCKER_TOKEN: ${{ secrets.DOCKER_TOKEN }}
          VERSION_PREFIX: '1.0'
          IMAGE_NAME: 'my-application-image'
          IMAGE_TAG: 'latest'
```

### Bemeneti paraméterek
A `NexiusAI Build Action` inputs:

| Paraméter                  | Leírás                            | Kötelező | Alapértelmezett érték |
|----------------------------|-----------------------------------|----------|-----------------------|
| `DOCKER_REGISTRY_HOSTNAME` | A Docker registry címe            | Igen     | `''`                 |
| `DOCKER_USERNAME`          | A Docker Registry felhasználóneve | Igen     | `''`                 |
| `DOCKER_TOKEN`             | Docker token a hitelesítéshez     | Igen     | `''`                 |
| `VERSION_PREFIX`           | Verzió előtag (pl. `1.0`)         | Nem      | `1.0`                |
| `IMAGE_NAME`               | Az image neve                     | Igen     | `''`                 |
| `IMAGE_TAG`                | Az image tag                      | Igen     | `''`                 |

### Főbb lépések
- **Docker login**: Ellenőrzött belépés a Docker Registry-be.
- **Build**: Docker image építése és verziószám beállítása a `VERSION_PREFIX` alapján.
- **Tagelés**: Az image elnevezése a Registry és az `IMAGE_NAME` alapján.
- **Push**: Az elkészült image feltöltése a megadott Docker Registry-be.

---

## NexiusAI Deploy Action

### Leírás
A `NexiusAI Deploy Action` lehetővé teszi egy Azure Container App frissítését az aktuális verziójú Docker image segítségével. Ez az action automatizálja az Azure CLI parancsok futtatását, így egyszerűsíti a telepítési folyamatot.

### Használati példa

```yaml
name: Deploy Workflow

on: 
  workflow_dispatch:

jobs:
  deploy-to-azure:
    runs-on: ubuntu-latest
    steps:
      - name: Azure Container App frissítése
        uses: nexius-learning/nexiusai-action/deploy@main
        with:
          AZURE_COMMAND_LOGIN: "az login --service-principal -u ${{ secrets.AZURE_USERNAME }} -p ${{ secrets.AZURE_PASSWORD }} --tenant ${{ secrets.AZURE_TENANT }}"
          AZURE_INSTANCE_NAME: "my-container-app"
          AZURE_RESOURCE_GROUP_NAME: "my-resource-group"
          DOCKER_IMAGE_NAME: "my-docker-image"
          DOCKER_IMAGE_TAG: "latest"
```

### Inputs
A `NexiusAI Deploy Action` inputs:

| Paraméter                   | Leírás                                    | Kötelező | Alapértelmezett érték |
|-----------------------------|-------------------------------------------|----------|-----------------------|
| `AZURE_COMMAND_LOGIN`       | Parancs az Azure CLI hitelesítéséhez      | Igen     | `''`                 |
| `AZURE_INSTANCE_NAME`       | Az Azure Container App neve               | Igen     | `''`                 |
| `AZURE_RESOURCE_GROUP_NAME` | Az Azure Resource Group neve              | Igen     | `''`                 |
| `DOCKER_IMAGE_NAME`         | A frissítéshez használt Docker image neve | Igen | `''`                 |
| `DOCKER_IMAGE_TAG`          | A frissítéshez használt Docker image tag  | Igen | `''`                 |

### Főbb lépések
- **Azure CLI hitelesítés**: Az Azure eléréséhez szükséges parancs futtatása.
- **Container App frissítése**: A megadott Docker image beállítása az Azure-ban, környezeti változók frissítésével együtt.

---

## Fejlesztői tippek
1. **Érzékeny adatok kezelése**: Az olyan érzékeny adatokat, mint a `DOCKER_TOKEN` és az `AZURE_PASSWORD`, a GitHub Secrets tárolja, hogy biztosítsd azok védelmét.
2. **Hibakeresés**: Ha az action végrehajtása során hibák lépnek fel, használd a `GITHUB_ENV` változót és naplózz minden szükséges adatot.
3. **Tesztkörnyezet használata**: Mielőtt a főágon futtatnád az action-öket, teszteld azokat külön ágon vagy sandbox környezetben.
