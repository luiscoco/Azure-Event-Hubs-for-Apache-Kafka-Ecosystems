# How to use Azure Event Hubs with Apache Kafka (Confluent.Kafka library) 

For additional info about this sample see these github repos: 

https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/dotnet

https://github.com/Azure/azure-event-hubs-for-kafka/tree/master

## 1. Create an EventHub in Azure Portal

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




