# product-sync-integration-3-layer
Product Sync &amp; Notification Integration is a MuleSoft application that fetches product data from a public API, applies business rules using Choice and DataWeave, publishes messages to Anypoint MQ, processes them asynchronously, upserts records into Salesforce, runs on a scheduler, and includes retry and error handling for reliability.

This project follows a 3-Layer Architecture:

1️⃣ Experience Layer</br>
2️⃣ Process Layer</br>
3️⃣ System Layer

 1️⃣ Experience Layer – API Exposure
The Experience Layer – API Exposure exposes an HTTP POST endpoint to receive client requests, invokes the Process Layer, returns the final response, and handles errors using components such as HTTP Listener, Flow Reference, Until Successful (retry mechanism), and a Global Error Handler.

<img width="369" height="285" alt="image" src="https://github.com/user-attachments/assets/d1f0aaa4-63eb-4b2d-83f9-10cfd1f1883f" />
<img width="408" height="252" alt="image" src="https://github.com/user-attachments/assets/8d0748f7-23b2-437c-82ee-d80ee1d34646" />



 2️⃣ Process Layer – Business Logic
The Process Layer – Business Logic fetches product data from an external API, applies conditional rules (Price > 100 → Premium, Price ≤ 100 → Standard), counts the total products fetched, publishes each product to the VM queue, returns a summary response, and runs automatically every 30 minutes using a Scheduler, utilizing components such as HTTP Request, Choice Router, For Each, VM Publish, and Scheduler.

<img width="553" height="328" alt="image" src="https://github.com/user-attachments/assets/5dea8de5-7b2c-47ca-ba2b-3c48557572ce" />
<img width="1057" height="462" alt="image" src="https://github.com/user-attachments/assets/01d28bb8-ada0-48fc-bf52-5ca97c37857a" />
<img width="1293" height="357" alt="image" src="https://github.com/user-attachments/assets/7af3d500-52d6-4caa-bdaf-aebbb8292125" />



 3️⃣ System Layer – Salesforce Integration
The System Layer – Salesforce Integration listens to the VM queue, transforms product data into Salesforce format, upserts records into the External_Product__c object using an External ID to prevent duplicates, retries up to five times on failure using Until Successful, and routes failed messages to a Dead Letter Queue (DLQ) via a VM listener.

<img width="822" height="519" alt="image" src="https://github.com/user-attachments/assets/b8ca99bb-791e-4e70-89e8-3c299f35066b" />
<img width="795" height="326" alt="image" src="https://github.com/user-attachments/assets/b0a51a1c-9047-4b7f-a479-5aca7cf5b39b" />


