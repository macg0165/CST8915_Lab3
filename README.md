# Lab 3 - CST8915 Full-stack Cloud-native Development
**Name:** Angus MacGillivary<br>
**Course:** CST8915<br>
**Section:** 11 <br> 
**Lab:** 3<br>
**Date:** October 7, 2026<br>

## Note on Azure Static Web Apps
Due to the limited regions on my Azure For Students subscription (\["westus","mexicocentral","norwayeast","northcentralus","denmarkeast"\]), I was not able to use Azure Static Web Apps for this lab.  As a result, I had to deploy the store front service on a VM in an identical manner to what I did in lab 2, the only change being the environment variables for the product and order services now point to their Azure Web App Service domains.

## Service Codebases
[Product Service (Python)](https://github.com/macg0165/product-service-py)<br>
[Order Service](https://github.com/macg0165/order-service)<br>
[Store Front](https://github.com/macg0165/store-front)<br>

## Video Demo
[Link](https://www.youtube.com/watch?v=dr1Yj31-GBk)

**Note: The product service is demonstrated using the network traffic in DevTools**

## Reflection Questions
~~**i. What challenges did you encounter when configuring environment variables in the GitHub Actions workflow?**~~ <br>
Not applicable<br><br>

**ii. How does deploying microservices on Azure Web App Service differ from running them locally?**
Azure Web App Service is a managed PaaS that can be used to automatically deploy apps from a codebase.  While running the services locally involves managing one's own environment, Azure Web Apps allows one to deploy without thinking about configurations at the environment and infrastructure level.  In the case of the product service, I used the FastAPI command to run the server, which had to be added manually to the startup commands.  I was personally surprised that no other intervention was needed on my part to deploy the app.  Web Apps automatically discovers the listening port for the app, so that variable didn't need to be configured.  In summary, Azure Web App Service is a useful tool for pushing code to production with minimal headache.

**iii. Why is it important to use environment variables for configurations in a cloud environment?**
Aside from the general usefulness of environment variables for managing apps, it's important to use them in a cloud environment because it's the only lever for configuring the app once it's containerized and deployed.  Any configuration changes can be made on the cloud console if they are stored as environment variables.  Hardcoded config would need to be modified by changing the source repository, starting a new GitHub Actions workflow and Azure deployment for what might only be a sing value change.

## Challenges

The prior mentioned configuration for the product service posed a challenge.  I also added the port to the environment variables before realizing Azure Web Service automatically maps any listening ports to the HTTP (80) and HTTPS(443) defaults.  Finally, there was some trouble with the NSG settings for the RabbitMQ VM, because Web App Service uses so many IP addresses.  I fetched the list using `azure webapp show` in the Azure CLI and added them all to one inbound rule.