---
ms.openlocfilehash: 40d2f957ce115b43a1dcc138b86e05ec9cc47384
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2021
ms.locfileid: "98060622"
---
Este tutorial requiere los siguientes recursos de Azure:

* IoT Hub
* Cuenta de almacenamiento
* Cuenta de Azure Media Services
* Máquina virtual Linux en Azure, con el [entorno de ejecución de Azure IoT Edge](../../../../../iot-edge/how-to-install-iot-edge.md) instalado

Para este inicio rápido, se recomienda usar el [script de configuración de recursos de Live Video Analytics](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) para implementar los recursos necesarios en su suscripción de Azure. Para hacerlo, siga estos pasos:

1. Abra [Azure Cloud Shell](https://shell.azure.com).
1. Si es la primera vez que usa Cloud Shell, se le pedirá que seleccione una suscripción para crear una cuenta de almacenamiento y un recurso compartido de Microsoft Azure Files. Seleccione **Create storage** (Crear almacenamiento) para crear una cuenta de almacenamiento para la información de la sesión de Cloud Shell. Esta cuenta de almacenamiento es independiente de la que creará el script para usarla con su cuenta de Azure Media Services.
1. En el menú desplegable del lado izquierdo de la ventana de Cloud Shell, seleccione el entorno **Bash**.

    ![Selector de entorno](../../../media/quickstarts/env-selector.png)
1. Ejecute el siguiente comando:

    ```
    bash -c "$(curl -sL https://aka.ms/lva-edge/setup-resources-for-samples)"
    ```
    
    Tras completar correctamente el script, debería ver todos los recursos necesarios en la suscripción.
1. Después de que finalice el script, seleccione las llaves para exponer la estructura de carpetas. Verá algunos archivos en el directorio *~/clouddrive/lva-sample*. Archivos de interés en este inicio rápido:

     * * **~/clouddrive/lva-sample/edge-deployment/.env**: este archivo contiene las propiedades que utiliza Visual Studio Code para implementar módulos en un dispositivo perimetral.
     **~/clouddrive/lva-sample/appsetting.json**: Visual Studio Code utiliza este archivo para ejecutar el código de ejemplo.
     
    Necesitará estos archivos al configurar el entorno de desarrollo en Visual Studio Code en la sección siguiente. Es posible que desee copiarlos en un archivo local por el momento.
    
    ![Configuración de la aplicación](../../../media/quickstarts/clouddrive.png)

> [!TIP]
> Si tiene problemas con los recursos de Azure que se crean, consulte la *[guía de solución de problemas](../../../troubleshoot-how-to.md#common-error-resolutions)* * para resolver algunos de los problemas más comunes que pueden surgir.