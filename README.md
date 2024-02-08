# How to use Azure Event Hubs with Apache Kafka (Confluent.Kafka library) 

For additional info about this sample see these github repos: 

https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/dotnet

https://github.com/Azure/azure-event-hubs-for-kafka/tree/master

## 1. Create an EventHub with Azure CLI commands

We create an Azure Event Hubs namespace named "mykafkaeventhub" in your resource group "myRG", located in "West Europe", with the pricing tier "Standard", and then to create a new Event Hub named "eventhubtest" inside this namespace, you can follow these steps using Azure CLI commands. 

Ensure we have Azure CLI installed and we are logged in to your Azure account through the CLI.

Before starting with Azure CLI we run the log in command

```
az login
```

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/76492bd7-b992-4b56-959f-5df408449918)

### 1.1. Create an Event Hubs Namespace

We use the az eventhubs namespace create command to create a new Event Hubs namespace

```
az eventhubs namespace create --name mykafkaeventhub --resource-group myRG --location westeurope --sku Standard
```

This command creates a namespace named "mykafkaeventhub" in the "myRG" resource group, located in "West Europe", with a "Standard" pricing tier

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/fcab5614-5c95-4de1-9f03-cce3396078d8)

### 1.2. Create an Event Hub

After the namespace is created, we can create an Event Hub named "eventhubtest" within this namespace using the az eventhubs eventhub create command

```
az eventhubs eventhub create --name eventhubtest --namespace-name mykafkaeventhub --resource-group myRG
```

This command creates an Event Hub named "eventhubtest" in the previously created namespace "mykafkaeventhub".

We make sure we have the necessary permissions to create resources in the specified resource group and region

If you encounter any errors, double-check the command syntax and ensure your Azure CLI is logged in to your Azure account.

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/39f5b713-dd4f-4c2f-8215-e64488b67d25)

### 1.3. We get the EventHubNameSpace connection string and FQDN

We obtain the **connection string** and the **Fully Qualified Domain Name (FQDN)** for an Azure Event Hubs namespace, we can use the az eventhubs namespace authorization-rule keys list command

This command retrieves the keys and connection strings for a specific authorization rule within the namespace

By default, every Event Hubs namespace has a built-in rule named **RootManageSharedAccessKey** that has manage, send, and listen claims.

Here's how we can retrieve the connection string and FQDN for our Event Hubs namespace "mykafkaeventhub" in the resource group "myRG":

**Get Namespace Connection String**:

```
az eventhubs namespace authorization-rule keys list ^
    --resource-group myRG ^
    --namespace-name mykafkaeventhub ^
    --name RootManageSharedAccessKey ^
    --query primaryConnectionString ^
    -o tsv
```

This command will output the primary connection string for the namespace, which includes the Shared Access Signature (SAS) key

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/eaeef73b-92ea-4000-80cf-fb5c5e182ee2)

**Get Namespace FQDN**:

While the Azure CLI doesn't directly provide a command that only outputs the FQDN, the FQDN is part of the connection string. The connection string format is typically:

```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

So, we can identify the FQDN from the Endpoint part of the connection string. However, if we want to specifically retrieve details about the namespace, including its FQDN, you can use the az eventhubs namespace show command and then parse the output for the serviceBusEndpoint or directly inspect the JSON output:

```
az eventhubs namespace show --name mykafkaeventhub --resource-group myRG --query serviceBusEndpoint -o tsv
```

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/22b3e46f-c58b-4c6f-bd89-847a28981b33)

This command will give you the serviceBusEndpoint value, which includes the FQDN of your Event Hubs namespace

Remember, the actual host part of the serviceBusEndpoint URL is the FQDN you're interested in

## 2. Create a .NET8 console application

We run Visual Studio 2022 Community Edition and we create a new project

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/d2299a2b-86de-44c8-ad00-430c4db89f4a)

We select the console C# project template

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/9e8636cc-c52b-488e-9c8d-6cf3db1709ab)

We input the project name and location

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/fbd97ca2-5d0a-49e0-bc11-ba8553024205)

We select the .NET8 framework

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/353589fa-36c0-4776-8468-aa695f4c51cf)

## 3. Load the project dependencies/libraries

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/5a797434-6810-40be-85bb-3e0809ef5d65)

## 4. Configure the App.config file

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/c04ebcd6-b6b1-420a-b57f-9725ee481ca2)

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <add key="EH_FQDN" value="mykafkaeventhub.servicebus.windows.net:9093"/>
    <add key="EH_CONNECTION_STRING" value="Endpoint=sb://mykafkaeventhub.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=F0W/prbI9uVCExHveAgQADfegnIZ7Ahwe+AEhLmbI14="/>
    <add key="EH_NAME" value="eventhubtest"/>
    <add key="CONSUMER_GROUP" value="$Default"/>
    <add key="CA_CERT_LOCATION" value=".\cacert.pem"/>
  </appSettings>
</configuration>
```

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

```csharp
//Copyright (c) Microsoft Corporation. All rights reserved.
//Copyright 2016-2017 Confluent Inc., 2015-2016 Andreas Heider
//Licensed under the MIT License.
//Licensed under the Apache License, Version 2.0
//
//Original Confluent sample modified for use with Azure Event Hubs for Apache Kafka Ecosystems

using Confluent.Kafka;

namespace EventHubsForKafkaSample
{
    class Worker
    {
        public static async Task Producer(string? brokerList, string? connStr, string? topic, string? cacertlocation)
        {
            try
            {
                var config = new ProducerConfig
                {
                    BootstrapServers = brokerList,
                    SecurityProtocol = SecurityProtocol.SaslSsl,
                    SaslMechanism = SaslMechanism.Plain,
                    SaslUsername = "$ConnectionString",
                    SaslPassword = connStr,
                    SslCaLocation = cacertlocation,
                    //Debug = "security,broker,protocol"        //Uncomment for librdkafka debugging information
                };
                using (var producer = new ProducerBuilder<long, string>(config).SetKeySerializer(Serializers.Int64).SetValueSerializer(Serializers.Utf8).Build())
                {
                    Console.WriteLine("Sending 10 messages to topic: " + topic + ", broker(s): " + brokerList);
                    for (int x = 0; x < 10; x++)
                    {
                        var msg = string.Format("Sample message #{0} sent at {1}", x, DateTime.Now.ToString("yyyy-MM-dd_HH:mm:ss.ffff"));
                        var deliveryReport = await producer.ProduceAsync(topic, new Message<long, string> { Key = DateTime.UtcNow.Ticks, Value = msg });
                        Console.WriteLine(string.Format("Message {0} sent (value: '{1}')", x, msg));
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine(string.Format("Exception Occurred - {0}", e.Message));
            }
        }

        public static void Consumer(string? brokerList, string? connStr, string? consumergroup, string? topic, string? cacertlocation)
        {
            var config = new ConsumerConfig
            {
                BootstrapServers = brokerList,
                SecurityProtocol = SecurityProtocol.SaslSsl,
                SocketTimeoutMs = 60000,                //this corresponds to the Consumer config `request.timeout.ms`
                SessionTimeoutMs = 30000,
                SaslMechanism = SaslMechanism.Plain,
                SaslUsername = "$ConnectionString",
                SaslPassword = connStr,
                SslCaLocation = cacertlocation,
                GroupId = consumergroup,
                AutoOffsetReset = AutoOffsetReset.Earliest,
                BrokerVersionFallback = "1.0.0",        //Event Hubs for Kafka Ecosystems supports Kafka v1.0+, a fallback to an older API will fail
                //Debug = "security,broker,protocol"    //Uncomment for librdkafka debugging information
            };

            using (var consumer = new ConsumerBuilder<long, string>(config).SetKeyDeserializer(Deserializers.Int64).SetValueDeserializer(Deserializers.Utf8).Build())
            {
                CancellationTokenSource cts = new CancellationTokenSource();
                Console.CancelKeyPress += (_, e) => { e.Cancel = true; cts.Cancel(); };

                consumer.Subscribe(topic);

                Console.WriteLine("Consuming messages from topic: " + topic + ", broker(s): " + brokerList);

                while (true)
                {
                    try
                    {
                        var msg = consumer.Consume(cts.Token);
                        Console.WriteLine($"Received: '{msg.Message.Value}'");
                    }
                    catch (ConsumeException e)
                    {
                        Console.WriteLine($"Consume error: {e.Error.Reason}");
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine($"Error: {e.Message}");
                    }
                }
            }
        }
    }
}
```

## 7. Create the Program.cs file

```csharp
using System.Configuration;
using EventHubsForKafkaSample;

string? brokerList = ConfigurationManager.AppSettings["EH_FQDN"];
string? connectionString = ConfigurationManager.AppSettings["EH_CONNECTION_STRING"];
string? topic = ConfigurationManager.AppSettings["EH_NAME"];
string? caCertLocation = ConfigurationManager.AppSettings["CA_CERT_LOCATION"];
string? consumerGroup = ConfigurationManager.AppSettings["CONSUMER_GROUP"];

Console.WriteLine("Initializing Producer");
Worker.Producer(brokerList, connectionString, topic, caCertLocation).Wait();
Console.WriteLine();
Console.WriteLine("Initializing Consumer");
Worker.Consumer(brokerList, connectionString, consumerGroup, topic, caCertLocation);
Console.ReadKey();
```

## 8. Run and test the application

We verify the application output

![image](https://github.com/luiscoco/Azure-Event-Hubs-for-Apache-Kafka-Ecosystems/assets/32194879/408dbf2b-96de-402e-ac20-7ee9c9d3237d)



