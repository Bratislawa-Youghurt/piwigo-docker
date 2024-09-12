## Containerized Piwigo with MariaDB database in Azure

# How to setup on Azure
1. Create a Resource Group
3. Create a Storage Account for photo storage, settings:
    - Primary Service: Azure Blob Storage
    - Primary Workload: Other
    - Performance: Standard
    - Redundancy: RA-GRS (GRS+make read access available in case of regional unavailability)
4. Create a new file share in the storage account
5. Create a Web App using template NGINX settings (settings will be corrected later)
6. Mount the created file shares to the web app using Storage Mounts, settings:
    - Storage type: Azure files
    - Protocol: SMB
    - Mount paths: /piwigostorage, /piwigoconfig, /mariadbstorage
7. Add custom domain to Web App
    - Domain provider: All other domain services
    - TLS Certificate: App Service Managed Certificate
    - Domain: gallery.your-domain.com
    - Copy the CNAME and TXT records to your DNS (DNS Zone in Azure->DNS Management->Record Sets)
    - Validate, should pass, then Add
8. Web App environment variables
    - MYSQL_PASSWORD=yourpassword
    - MYSQL_ROOT_PASSWORD=yourrootpassword
    - WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
9. Go to Deployment Center in Azure
    - Source: Container Registry
    - Container type: Docker Compose
    - Registry source: Docker Hub
    - Repository access: public
    - Paste the docker-compose.yml content to Config field
    - Save and go to the app address and wait for it to load (will probably take a while)
10. Piwigo database configuration
    - Host: mariadb (the name of your db container)
    - User, password and db name you set up earlier

# How to run locally
1. Clone repo
2. (Optional) Edit docker-compose-local.yml credentials etc
3. Run `docker compose up -d -f docker-compose-local.yml`
4. Navigate to localhost on your browser
5. Do piwigo setup with your credentials