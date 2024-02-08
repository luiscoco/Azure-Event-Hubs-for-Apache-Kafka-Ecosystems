# How to use Azure Event Hubs with Apache Kafka (Confluent.Kafka library) 

For additional info about this sample see these github repos: 

https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/dotnet

https://github.com/Azure/azure-event-hubs-for-kafka/tree/master

## 1. Create an EventHub with Azure CLI commands

We create an Azure Event Hubs namespace named "mykafkaeventhub" in your resource group "myRG", located in "West Europe", with the pricing tier "Standard", and then to create a new Event Hub named "eventhubtest" inside this namespace, you can follow these steps using Azure CLI commands. 

Ensure we have Azure CLI installed and we are logged in to your Azure account through the CLI.

### 1.1. Create an Event Hubs Namespace

We use the az eventhubs namespace create command to create a new Event Hubs namespace

```
az eventhubs namespace create --name mykafkaeventhub --resource-group myRG --location westeurope --sku Standard
```

This command creates a namespace named "mykafkaeventhub" in the "myRG" resource group, located in "West Europe", with a "Standard" pricing tier

### 1.2. Create an Event Hub

After the namespace is created, we can create an Event Hub named "eventhubtest" within this namespace using the az eventhubs eventhub create command

```
az eventhubs eventhub create --name eventhubtest --namespace-name mykafkaeventhub --resource-group myRG
```

This command creates an Event Hub named "eventhubtest" in the previously created namespace "mykafkaeventhub".

We make sure we have the necessary permissions to create resources in the specified resource group and region

If you encounter any errors, double-check the command syntax and ensure your Azure CLI is logged in to your Azure account.

## 2. Create a .NET8 console application

## 3. Load the project dependencies/libraries


## 4. Configure the App.config file


## 5. Create the certificate (cacert.pem)

Install Perl in Window from this URL: https://strawberryperl.com/

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/bd0991ae-1589-41d6-97b4-31dba3d5a731)

Git clone the following repo: https://github.com/puppetlabs/puppet-ca-bundle/tree/main

Run **Perl** command line

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/2532f67c-8357-4eca-9d44-f162aa52bc3f)

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/525604ff-b9b9-4206-9d62-2ddc90186d04)

Run this perl command to download the certificates bundle from Mozilla 

```
C:\Users\luisc\Downloads\puppet-ca-bundle-main\puppet-ca-bundle-main>perl mk-ca-bundle.pl -f
```

Rename the bundle file from **ca-bundle.crt** to **cacert.pem**

```
C:\Users\luisc\Downloads\puppet-ca-bundle-main\puppet-ca-bundle-main>ren ca-bundle.crt cacert.pem
```

Copy the **cacert.pem** file to the project root

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/bbc70297-174f-42a3-9a7c-e63c2a71275e)

## 6. Create the Worker class

## 7. Create the Program.cs file


## 8. Run and test the application




