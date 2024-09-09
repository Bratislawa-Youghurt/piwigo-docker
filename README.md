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
6. Mount the created file share to the web app using Storage Mounts, settings:
    - Storage type: Azure files
    - Protocol: SMB
    - Mount path: /piwigostorage
7. Add custom domain to Web App
    - Domain provider: All other domain services
    - TLS Certificate: App Service Managed Certificate
    - Domain: gallery.your-domain.com
    - Copy the CNAME and TXT records to your DNS (DNS Zone in Azure->DNS Management->Record Sets)
    - Validate, should pass, then Add

# How to run locally
1. Clone repo
2. (Optional) Edit docker-compose.yml credentials etc
3. Run `docker compose up -d`
4. Navigate to localhost on your browser
5. Do piwigo setup with your credentials