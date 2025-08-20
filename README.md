# N8N-E-commerce-Order-Processor
Built a workflow to automate online store order processing by integrating Webhook, Google Sheets, and Slack. Implemented inventory validation, conditional logic for stock issues, automated email alerts, packing list generation, and team notifications—reducing manual effort and streamlining fulfillment.

## ⚙️ Prerequisites

Before you start, make sure you have:  
🛠️ **n8n:** self-hosted or cloud version    
📦 **Postman:** to simulate new order webhook triggers  
🤖 **OpenRouter API:** for sending feedback text to an LLM and getting sentiment analysis results  
📑 **Google Sheets:** to store inventory data    ([Inventory Database](https://github.com/Hafsa-Ali/N8N---E-commerce-Order-Processor/commit/9e28e474808e94fac29d0d5044c371476909f98b))    
📬 **Email credentials:** SMTP or Gmail node in n8n  
💬 **Messaging app integration:** Slack/Discord/MS Teams    

## 📝 Sample Input

This workflow is triggered whenever a **new order** is received.  
The input JSON contains order details such as order ID, customer name, and ordered items.  
*💡 Reminder: Paste this code in your Postman → Body → Raw*
### Example
```json
{
  "orderId": "ORD-3002",
  "customer": "Name of your choice",
  "items": [
    { "productId": "P005", "product": "Bluetooth Headphones", "quantity": 25 }
  ]
}
```
## Workflow
![image](https://github.com/user-attachments/assets/71d14b23-d422-4733-ba32-35afdfd8e991)  

## 📦 Order Fulfillment Workflow

| Step | Node | Purpose |
|------|------|---------|
| 1 | **Webhook – New Order Received** | Trigger – receives new order details (orderId, customer, items, quantity). |
| 2 | **Split Out** | Splits order items array so each product can be processed individually. |
| 3 | **Check Inventory (Google Sheets)** | Looks up product stock in inventory database. ([Inventory Database](https://github.com/Hafsa-Ali/N8N---E-commerce-Order-Processor/commit/9e28e474808e94fac29d0d5044c371476909f98b)) |
| 4 | **If (Stock Check)** | Compares requested quantity with available stock. |
| 5 | **Append Row (TRUE branch)** | Adds order details to **Fulfillment Queue** sheet for processing. ([Fullfillment Sheet](https://github.com/Hafsa-Ali/N8N---E-commerce-Order-Processor/commit/9e28e474808e94fac29d0d5044c371476909f98b)) |
| 6 | **Send Slack Message (TRUE branch)** | Notifies fulfillment team with order + packing list. |
| 7 | **Send Gmail (FALSE branch)** | Sends alert email to Inventory Manager if stock is too low. |

✅ **TRUE branch** → Order added to queue + team notified  
❌ **FALSE branch** → Inventory Manager alerted for restock

