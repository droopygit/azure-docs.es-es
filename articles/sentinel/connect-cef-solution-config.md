---
title: Configuración de la solución de seguridad para conectar datos de CEF a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo configurar la solución de seguridad para conectar datos de CEF a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: fec3f25c4b401ff7c3bc73d249b716b9c12e6529
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96548552"
---
# <a name="step-2-configure-your-security-solution-to-send-cef-messages"></a>PASO 2: Configurar la solución de seguridad para enviar mensajes CEF

En este paso, realizará los cambios de configuración necesarios en la solución de seguridad para enviar registros al agente de CEF.

## <a name="configure-a-solution-with-a-connector"></a>Configuración de una solución con un conector

Si la solución de seguridad ya tiene un conector existente, use las instrucciones específicas del conector como se indica a continuación:

- [AI Vectra Detect](connect-ai-vectra-detect.md)
- [Check Point](connect-checkpoint.md)
- [Cisco](connect-cisco.md)
- [Reveal(x) de ExtraHop](connect-extrahop.md)
- [F5 ASM](connect-f5.md)  
- [Productos de Forcepoint](connect-forcepoint-casb-ngfw.md)
- [Fortinet](connect-fortinet.md)
- [AMS de Illusive Networks](connect-illusive-attack-management-system.md)
- [One Identity Safeguard](connect-one-identity.md)
- [Palo Alto Networks](connect-paloalto.md)
- [Trend Micro Deep Security](connect-trend-micro.md)
- [Zscaler](connect-zscaler.md)
## <a name="configure-any-other-solution"></a>Configurar cualquier otra solución

Si no existe un conector para su solución de seguridad específica, use las siguientes instrucciones genéricas para reenviar los registros al agente de CEF.

1. Vaya al artículo de configuración específico para conocer los pasos para configurar la solución para enviar mensajes CEF. Si su solución no aparece, debe establecer estos valores en el dispositivo para que envíe los registros necesarios en el formato necesario al agente de Syslog de Azure Sentinel, según el agente de Log Analytics. Puede modificar estos parámetros en su dispositivo, siempre y cuando modifique también el demonio de Syslog en el agente de Azure Sentinel.
    - Protocolo = TCP
    - Puerto = 514
    - Formato = CEF
    - Dirección IP: Asegúrese de enviar los mensajes CEF a la dirección IP de la máquina virtual que ha dedicado para tales efectos.

   Esta solución admite RFC 3164 o RFC 5424 de Syslog.

1. Para buscar eventos CEF en Log Analytics, escriba `CommonSecurityLog` en la ventana de consulta.

1. Diríjase al PASO 3: [validación de conectividad](connect-cef-verify.md).

> [!NOTE]
> **Cambio del origen del campo TimeGenerated**
>
> - De forma predeterminada, el agente de Log Analytics rellena el campo *TimeGenerated* en el esquema con la hora en la que el agente recibió el evento del demonio de Syslog. Como resultado, la hora a la que se generó el evento en el sistema de origen no se registra en Azure Sentinel.
>
> - Sin embargo, puede ejecutar el siguiente comando que descargará y ejecutará el script `TimeGenerated.py`. Este script configura el agente de Log Analytics para rellenar el campo *TimeGenerated* con la hora original del evento en su sistema de origen, en lugar de usar la hora a la que lo recibió el agente.
>
>    ```bash
>    wget -O TimeGenerated.py https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/TimeGenerated.py && python TimeGenerated.py {ws_id}
>    ```

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar dispositivos CEF a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](./tutorial-detect-threats-built-in.md).