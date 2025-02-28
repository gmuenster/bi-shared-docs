---
title: "Analysis Services client libraries | Microsoft Docs"
description: Download and learn how client libraries are necessary for client applications and tools to connect to Analysis Services.
ms.date: 8/13/2021
ms.prod: sql
ms.technology: analysis-services
ms.custom:
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
monikerRange: "asallproducts-allversions || azure-analysis-services-current || power-bi-premium-current || >= sql-analysis-services-2016"
---

# Analysis Services client libraries

[!INCLUDE[ssas-appliesto-sqlas-all-aas-pbip](includes/ssas-appliesto-sqlas-all-aas-pbip.md)]

Client libraries are necessary for client applications and tools to connect to Analysis Services. Microsoft client applications like Power BI Desktop, Excel, SQL Server Management Studio (SSMS), and Analysis Services projects extension for Visual Studio install all three client libraries and update them along with regular application updates. Custom client applications also require client libraries are installed. Client libraries are updated monthly. Before downloading, be sure to see [Known issues](#known-issues).

## Download the latest

### Windows Installer  

|Download  | Version  |
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    15.1.105.16     |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |     15.1.105.16        |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   19.24.4.0    |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    19.24.4.0      |

### NuGet packages

Analysis Services Management Objects (AMO) and ADOMD client libraries are available as installable packages from [NuGet.org](https://www.nuget.org/). It's recommended you migrate to NuGet references instead of using Windows Installer.

Starting Feb. 2021, versions of [.NET Core](/dotnet/core/about) packages equivalent to the AMO and ADOMD client packages are also available. There are, however, a few scenarios not-supported by the .NET Core versions. To learn more, see [Known issues](#known-issues) later in this article.

NuGet package assemblies AssemblyVersion follow semantic versioning: MAJOR.MINOR.PATCH. NuGet references load the expected version even if there is a different version in the GAC (resulting from MSI install). PATCH is incremented for each release. AMO and ADOMD versions are kept in-sync.

#### AMO and ADOMD

|Package  | Version  |
|---------|---------|
|[AMO](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/)    |    19.24.4.0     |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.retail.amd64/)     |   19.24.4.0       |

#### AMO and ADOMD .Net Core

|Package  | Version  |
|---------|---------|
|[AMO](https://www.nuget.org/packages/Microsoft.AnalysisServices.NetCore.retail.amd64)    |    19.24.4.0      |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.NetCore.retail.amd64)     |   19.24.4.0        |

## Minimum required versions

Transport Layer Security (TLS) protocol version 1.0/1.1 will be [deprecated](/azure/active-directory/fundamentals/whats-new#november-2020) in Azure Active Directory by June 30, 2021. Going forward, TLS 1.2 and higher is required. TLS 1.2 is not supported in earlier versions of the Analysis Services client libraries. Newer client library versions include support for TLS 1.2 and higher as well as other important security enhancements.

To minimize risk and potential security vulnerabilities, beginning June 30th, 2021, enhanced security in Azure Analysis Services and Power BI require the following or higher versions:

|Client lib  |File version  | Version  |
|---------|---------|---------|
|MSOLAP     |   2018.151.61.21       |    15.1.61.21       |
|AMO     |   15.1.61.21        |    19.12.3.0      |
|ADOMD     |   15.1.61.21        |    19.12.3.0      |

## Known issues

#### AMO and ADOMD .Net Core 

Supported scenarios include connections to Azure Analysis Services, Power BI Premium, and SQL Server Analysis Services. TCP based connectivity is supported for Windows computers only.

Interactive login with Azure Active Directory is supported for Windows computers only. The .NET Core Desktop runtime is required.

Dependency in MSAL requires version 4.6.0 and higher.

Version 19.14.0 of the .Net Core client libraries introduced preview support for Self-Contained publishing (both in a published directory, and in single-file mode), as well as support for consumption by .NET 5.0 projects. A number of issues related to connectivity to SQL Server Analysis Services that were identified in the previous release have been fixed.

Version 19.12.7.2 of the .Net Core client libraries introduced support for SQL Server Analysis Services. Lower preview versions only supported Azure Analysis Services and Power BI datasets.

#### AMO

Version 19.12.3.0 of the AMO client library introduces a new enumeration, **Microsoft.AnalysisServices.DataType**. However, the previous enumeration, **Microsoft.AnalysisServices.Tabular.DataType** still exists. If your code references the previous enumeration simply as **DataType** in a code file with statements to both namespaces (**Microsoft.AnalysisServices**, **Microsoft.AnalysisServices.Tabular**), due to the ambiguity, you could get an error when compiling. To resolve the error, fully qualify the reference to the enumeration.

## Understanding client libraries

Analysis Services utilizes three client libraries. ADOMD.NET and Analysis Services Management Objects (AMO) are managed client libraries. And Analysis Services OLE DB Provider (MSOLAP DLL) is a native client library. Typically, all three are installed at the same time.

Microsoft client applications like Power BI Desktop and Excel install all three client libraries and update them when new versions are available. Depending on the version or frequency of updates, some client libraries may not be the latest versions required by Azure Analysis Services and Power BI. The same applies to custom applications or other interfaces such as AsCmd, TOM, ADOMD.NET. These applications require manually or programmatically installing the libraries. The client libraries for manual installation are included in SQL Server feature packs as distributable packages. However, these client libraries are tied to the SQL Server version and may not be the latest. Make sure you always install the latest, downloadable from this article.  

### Client library types

#### Analysis Services OLE DB Provider (MSOLAP)

 Analysis Services OLE DB Provider (MSOLAP) is the native client library for Analysis Services database connections. It's used indirectly by both ADOMD.NET and AMO, delegating connection requests to the data provider. You can also call the OLE DB Provider directly from application code.  
  
 The Analysis Services OLE DB Provider is installed automatically by most tools and client applications used to access Analysis Services databases. It must be installed on computers used to access Analysis Services data.  
  
 OLE DB providers are often specified in connection strings. An Analysis Services connection string uses a different nomenclature to refer to the OLE DB Provider: MSOLAP.\<version>.dll.

#### AMO  

 AMO is a managed client library used for server administration and data definition. It's installed and used by tools and client applications. For example, SQL Server Management Studio (SSMS) uses AMO to connect to Analysis Services. A connection using AMO is typically minimal, consisting of `"data source=\<servername>"`. After a connection is established, you use the API to work with database collections and major objects. Both Visual Studio and SSMS use AMO to connect to an Analysis Services instance.  

#### ADOMD

 ADOMD.NET is a managed data client library used for querying Analysis Services data. It's installed and used by tools and client applications.
  
 When connecting to a database, the connection string properties for all three libraries are similar. Almost any connection string you define for ADOMD.NET by using  [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](/dotnet/api/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring#Microsoft_AnalysisServices_AdomdClient_AdomdConnection_ConnectionString) also works for AMO and the Analysis Services OLE DB Provider (MSOLAP). To learn more, see [Connection string properties](instances/connection-string-properties-analysis-services.md).

## Check installed versions

### OLEDDB (MSOLAP)  

1. Go to `C:\Program Files\Microsoft Analysis Services\AS OLEDB\`. If you have more than one folder, choose the higher number.
1. Right-click **msolap.dll** > **Properties** > **Details**. Check the **Product version** property. Note: If the filename is msolap140.dll, it's older than latest version and should be upgraded.

    ![MSOLAP Client library details dialog](media/client-libraries/msolap-details.png)

### AMO

1. Go to `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\`. If you have more than one folder, choose the higher number.
1. Right-click **Microsoft.AnalysisServices** > **Properties** > **Details**.  

    ![AMO Client library details dialog](media/client-libraries/amo-details.png)

### ADOMD

1. Go to `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\`. If you have more than one folder, choose the higher number.
1. Right-click **Microsoft.AnalysisServices.AdomdClient** > **Properties** > **Details**.  

    ![ADOMD Client library details dialog](media/client-libraries/adomd-details.png)

## Manually update

Client libraries are typically installed and updated automatically along with tools and client applications that use them. However, in some cases client libraries may not be updated automatically and each must be manually updated. To update manually, download and run the Windows Installer (.msi) package for each client library.

### To download and update

1. Click:
    - [Download MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576) or [Download MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)
    - [Download AMO](https://go.microsoft.com/fwlink/?linkid=829578)
    - [Download ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)

1. In **Downloads**, click a Windows Installer Package to run Setup.
1. In Setup, click **Next**.
1. Read the license agreement. If you agree, select **I accept the terms in the license agreement**, and then click **Next**.
1. Click, **Install**.
1. When completed, click **Finish**.
