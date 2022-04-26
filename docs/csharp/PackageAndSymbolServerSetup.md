# Adding new NuGet source:
1. In Visual Studio, go to **Tools > NuGet Package Manager > Package Manager Settings**. 

![](images/Aspose.Words.54e4b746-25ab-4f57-b21d-de4b2f6ea9d3.001.png)

1. Under **NuGet Package Manager >** **Package Sources**. Hit the “+” button, and type in the name and source (http://oa-utility:81/nuget/aggregated/) on the bottom. Hit **Update**, then **OK**. ![](images/Aspose.Words.54e4b746-25ab-4f57-b21d-de4b2f6ea9d3.002.png)

# Enabling Symbol/Source Server
1. In Visual Studio, go to **Debug > Options**.

![](images/Aspose.Words.54e4b746-25ab-4f57-b21d-de4b2f6ea9d3.003.png)

1. Uncheck **Enable Just My Code**, and check **Enable source server support**

![](images/Aspose.Words.54e4b746-25ab-4f57-b21d-de4b2f6ea9d3.004.png)

1. Click **Symbols** in the left, and click the folder icon to add a new server, http://oa-utility:81/symbols/aggregated. 

![](images/Aspose.Words.54e4b746-25ab-4f57-b21d-de4b2f6ea9d3.005.png)

1. You should now be able to step into NuGet packages when debugging.

![](images/Aspose.Words.54e4b746-25ab-4f57-b21d-de4b2f6ea9d3.006.png)
