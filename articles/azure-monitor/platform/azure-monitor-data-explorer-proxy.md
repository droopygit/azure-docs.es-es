---
title: Consulta entre recursos en Azure Data Explorer mediante Azure Monitor
description: Use Azure Monitor para realizar consultas entre productos entre Azure Data Explorer, áreas de trabajo de Log Analytics y aplicaciones clásicas de Application Insights en Azure Monitor.
author: osalzberg
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 12/02/2020
ms.openlocfilehash: 1a35b80ceec12b378a01555f42b7a0500b8f6229
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2021
ms.locfileid: "98060459"
---
# <a name="cross-resource-query-azure-data-explorer-by-using-azure-monitor"></a>Consulta entre recursos en Azure Data Explorer mediante Azure Monitor
Azure Monitor admite las consultas entre los servicios Azure Data Explorer, [Application Insights](/azure/azure-monitor/app/app-insights-overview) y [Log Analytics](/azure/azure-monitor/platform/data-platform-logs). Después puede consultar el clúster de Azure Data Explorer mediante las herramientas de Log Analytics o Application Insights y hacer referencia al mismo en una consulta entre servicios. En el artículo se muestra cómo realizar una consulta entre servicios.

En el diagrama siguiente se muestra el flujo entre servicios de Azure Monitor:

:::image type="content" source="media\azure-data-explorer-monitor-proxy\azure-monitor-data-explorer-flow.png" alt-text="Diagrama que muestra el flujo de consultas entre un usuario, Azure Monitor, un proxy y Azure Data Explorer.":::

>[!NOTE]
> La consulta entre servicios de Azure Monitor está en versión preliminar pública. Póngase en contacto con el [equipo de servicio](mailto:ADXProxy@microsoft.com) si tiene alguna duda.

## <a name="cross-query-your-log-analytics-or-application-insights-resources-and-azure-data-explorer"></a>Consulta entre los recursos de Log Analytics o Application Insights y Azure Data Explorer

Puede ejecutar las consultas entre recursos usando herramientas de cliente que admitan consultas de Kusto. Entre los ejemplos de estas herramientas se incluyen la interfaz de usuario web de Log Analytics, Workbooks, PowerShell y la API REST.

Escriba el identificador de un clúster de Azure Data Explorer en una consulta dentro del patrón `adx`, seguido del nombre y la tabla de la base de datos.

```kusto
adx('https://help.kusto.windows.net/Samples').StormEvents
```
:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-cross-service-query-example.png" alt-text="Captura de pantalla que muestra un ejemplo de una consulta entre servicios.":::

> [!NOTE]
>* Los nombres de las bases de datos distinguen mayúsculas de minúsculas.
>* No se admite la consulta entre recursos como una alerta.

## <a name="combine-azure-data-explorer-cluster-tables-with-a-log-analytics-workspace"></a>Combinación de tablas de clúster de Azure Data Explorer con un área de trabajo de Log Analytics

Use el comando `union` para combinar las tablas de clúster con un área de trabajo de Log Analytics.

```kusto
union customEvents, adx('https://help.kusto.windows.net/Samples').StormEvents
| take 10
```
```kusto
let CL1 = adx('https://help.kusto.windows.net/Samples').StormEvents;
union customEvents, CL1 | take 10
```
:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-union-cross-query.png" alt-text="Captura de pantalla que muestra un ejemplo de consulta entre servicios con el comando &quot;union&quot;.":::

> [!Tip]
> Se permite el formato abreviado: *nombreDeClúster*/*catálogoInicial*. Por ejemplo, `adx('help/Samples')` se convierte en `adx('help.kusto.windows.net/Samples')`.

## <a name="join-data-from-an-azure-data-explorer-cluster-in-one-tenant-with-an-azure-monitor-resource-in-another"></a>Combinación de datos desde un clúster de Azure Data Explorer de un inquilino con un recurso de Azure Monitor de otro inquilino

No se admiten las consultas entre inquilinos en los servicios. Debe iniciar sesión en un solo inquilino para ejecutar la consulta que abarca ambos recursos.

Si el recurso de Azure Data Explorer está en el inquilino A y el área de trabajo de Log Analytics está en el inquilino B, use uno de los métodos siguientes:

*  Azure Data Explorer permite agregar roles para entidades de seguridad de distintos inquilinos. Agregue su identificador de usuario en el inquilino B como usuario autorizado en el clúster de Azure Data Explorer. Compruebe que la propiedad [TrustedExternalTenant](https://docs.microsoft.com/powershell/module/az.kusto/update-azkustocluster) del clúster de Azure Data Explorer contiene al inquilino B. Ejecute la consulta cruzada completamente en el inquilino B.
*  Use [Lighthouse](https://docs.microsoft.com/azure/lighthouse/) para proyectar el recurso de Azure Monitor en el inquilino A.

## <a name="connect-to-azure-data-explorer-clusters-from-different-tenants"></a>Conexión a clústeres de Azure Data Explorer desde otros inquilinos

Kusto Explorer inicia sesión automáticamente en el inquilino al que pertenece originalmente la cuenta de usuario. Para acceder a los recursos de otros inquilinos con la misma cuenta de usuario, tiene que especificar explícitamente el valor de `TenantId` en la cadena de conexión:

`Data Source=https://ade.applicationinsights.io/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName;Initial Catalog=NetDefaultDB;AAD Federated Security=True;Authority ID=TenantId`

## <a name="next-steps"></a>Pasos siguientes
* [Escritura de consultas](https://docs.microsoft.com/azure/data-explorer/write-queries)
* [Consulta de datos en Azure Monitor mediante Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/query-monitor-data)
* [Consulta de registros entre recursos en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/cross-workspace-query)
