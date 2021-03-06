---
title: Habilitación de la eliminación temporal en todas las instancias de Azure Key Vault | Microsoft Docs
description: Use este documento para adoptar la eliminación temporal para todos los almacenes de claves.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: sudbalas
ms.openlocfilehash: e512cccdbfdc56500fa7c69372ca38f59d3195c2
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97590093"
---
# <a name="soft-delete-will-be-enabled-on-all-key-vaults"></a>La eliminación temporal se habilitará en todos los almacenes de claves

> [!WARNING]
> **Cambio importante**: la posibilidad de rechazar la eliminación temporal pronto estará en desuso. Los usuarios y administradores de Azure Key Vault deben habilitar la eliminación temporal en sus almacenes de claves de forma inmediata.
>
> Para HSM administrado, la eliminación temporal está habilitada de forma predeterminada y no se puede deshabilitar.

Cuando se elimina un secreto de un almacén de claves sin protección contra la eliminación temporal, el secreto se elimina de forma permanente. Los usuarios pueden rechazar la eliminación temporal durante la creación del almacén de claves; no obstante, para proteger los secretos de la eliminación accidental o malintencionada por parte de un usuario, Microsoft pronto habilitará la protección contra la eliminación temporal en **todos** los almacenes de claves y los usuarios ya no tendrán la opción de rechazar ni desactivar la eliminación temporal.

:::image type="content" source="../media/softdeletediagram.png" alt-text="<texto alternativo>":::

Para obtener detalles completos sobre la funcionalidad de eliminación temporal, consulte [Información general sobre la eliminación temporal de Azure Key Vault](soft-delete-overview.md).

## <a name="can-my-application-work-with-soft-delete-enabled"></a>¿Puede mi aplicación funcionar con la eliminación temporal habilitada?

> [!Important] 
> **Revise atentamente la siguiente información antes de activar la eliminación temporal para los almacenes de claves.**

Los nombres de Key Vault son únicos globalmente. Los nombres de los secretos almacenados en un almacén de claves también son únicos. No podrá volver a usar el nombre de un almacén de claves o de un objeto de almacén de claves que exista en el estado de eliminación temporal. 

**Ejemplo 1**: la aplicación crea mediante programación un almacén de claves denominado "Almacén A" y, posteriormente, lo elimina. En este caso, el almacén de claves se pasará al estado de eliminación temporal. La aplicación no podrá volver a crear otro almacén de claves denominado "Almacén A" hasta que el almacén de claves se purgue del estado de eliminación temporal. 

**Ejemplo 2**: la aplicación crea una clave denominada `test key` en el almacén de claves A y, posteriormente, la elimina de dicho almacén. En este caso, la aplicación no podrá crear una clave denominada `test key` en el almacén de claves A hasta que el objeto `test key` se purgue del estado de eliminación temporal. 

Esto puede producir errores de conflicto si intenta eliminar un objeto del almacén de claves y volver a crearlo con el mismo nombre sin purgarlo primero del estado de eliminación temporal. Esto puede provocar errores en las aplicaciones o la automatización. Antes de realizar los cambios necesarios en la aplicación y la administración, póngase en contacto con el equipo de desarrollo. 

### <a name="application-changes"></a>Cambios en la aplicación

Si la aplicación supone que la eliminación temporal no está habilitada y espera que los nombres del secreto o el almacén de claves eliminados estén disponibles para su reutilización inmediata, la lógica de la aplicación tendrá que realizar los siguientes cambios para poder adoptar este cambio.

1. Eliminar el almacén de claves o el secreto original.
2. Purgar el almacén de claves o el secreto con el estado de eliminación temporal.
3. Esperar (si se vuelven a crear inmediatamente se puede producir un conflicto).
4. Volver a crear el almacén de claves con el mismo nombre.
5. Volver a intentar la implementación si la operación de creación sigue produciendo un error de conflicto de nombres; los registros DNS pueden tardar hasta 10 minutos en actualizarse en el peor de los escenarios.

### <a name="administration-changes"></a>Cambios en la administración

A las entidades de seguridad que necesitan acceso para eliminar secretos de forma permanente se les deben conceder permisos de directiva de acceso adicionales para purgar estos secretos y el almacén de claves.

Si tiene una instancia de Azure Policy en los almacenes de claves que exige que la eliminación temporal esté desactivada, esta directiva deberá deshabilitarse.  Es posible que necesite escalar este problema a un administrador que controle las directivas de Azure que se aplican a su entorno. Si no se deshabilita esta directiva, puede perder la capacidad de crear nuevos almacenes de claves en el ámbito de la directiva aplicada.

Si su organización está sujeta a requisitos legales de cumplimiento normativo y no puede permitir que los almacenes de claves y secretos eliminados permanezcan en un estado recuperable durante un período de tiempo prolongado, tendrá que ajustar el período de retención de la eliminación temporal, que se puede configurar entre 7 y 90 días, para cumplir los estándares de su organización.

## <a name="procedures"></a>Procedimientos

### <a name="audit-your-key-vaults-to-check-if-soft-delete-is-enabled"></a>Auditoría de los almacenes de claves para comprobar si la eliminación temporal está habilitada

1. Inicie sesión en el Portal de Azure.
2. Busque "Azure Policy".
3. Seleccione "Definiciones".
4. En Categoría, seleccione "Key Vault" en el filtro.
5. Seleccione la directiva "Key Vault should have soft delete enabled" (El almacén de claves debe tener habilitada la eliminación temporal).
6. Haga clic en "Asignar".
7. Establezca el ámbito de la suscripción.
8. Asegúrese de que el efecto de la directiva se establece en "Auditar".
9. Seleccione "Revisar y crear".
10. Un examen completo de su entorno puede tardar hasta 24 horas en realizarse.
11. En la hoja Azure Policy, haga clic en "Cumplimiento".
12. Seleccione la directiva que ha aplicado.

Ya debería poder filtrar y ver cuáles de sus almacenes de claves tienen habilitada la eliminación temporal (recursos compatibles) y cuáles no (recursos no compatibles).

### <a name="turn-on-soft-delete-for-an-existing-key-vault"></a>Activación de la eliminación temporal para un almacén de claves existente

1. Inicie sesión en el Portal de Azure.
2. Busque su instancia de Key Vault.
3. En Configuración, seleccione "Propiedades".
4. En Eliminación temporal, seleccione el botón de radio correspondiente a "Enable recovery of this vault and its objects" (Habilitar la recuperación de este almacén y de sus objetos).
5. Establezca el período de retención para la eliminación temporal.
6. Seleccione "Guardar".

### <a name="grant-purge-access-policy-permissions-to-a-security-principal"></a>Concesión de permisos de directiva de acceso de depuración a una entidad de seguridad

1. Inicie sesión en el Portal de Azure.
2. Busque su instancia de Key Vault.
3. Seleccione "Directivas de acceso" en Configuración.
4. Seleccione la entidad de servicio a la que le gustaría conceder acceso.
5. En cada lista desplegable de los permisos de clave, secreto y certificado, desplácese hacia abajo hasta "Privileged Operations" (Operaciones con privilegios) y seleccione el permiso "Purgar".

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="does-this-change-affect-me"></a>¿Me afecta este cambio?

Si ya tiene activada la eliminación temporal o si no elimina y vuelve a crear objetos del almacén de claves con el mismo nombre, es probable que no observe ningún cambio en el comportamiento del almacén de claves.

Si tiene una aplicación que elimina y vuelve a crear objetos del almacén de claves con las mismas convenciones de nomenclatura con frecuencia, tendrá que realizar cambios en la lógica de la aplicación para mantener el comportamiento esperado. Consulte la sección "¿Cómo debo responder ante cambios importantes?" anterior.

### <a name="how-do-i-benefit-from-this-change"></a>¿Cómo puedo aprovechar este cambio?

La protección contra la eliminación temporal proporcionará a su organización una capa adicional de protección frente a la eliminación accidental o malintencionada. Como administrador del almacén de claves, puede restringir el acceso a los permisos de recuperación y de purga.

Si un usuario elimina accidentalmente un almacén de claves o un secreto, puede concederle permisos de acceso para recuperar el secreto sin crear un riesgo de que elimine permanentemente el secreto o el almacén de claves. Este proceso de autoservicio minimizará el tiempo de inactividad en el entorno y garantizará la disponibilidad de los secretos.

### <a name="how-do-i-find-out-if-i-need-to-take-action"></a>¿Cómo puedo averiguar si es necesario realizar alguna acción?

Siga los pasos descritos anteriormente en la sección titulada "Auditoría de los almacenes de claves para comprobar si la eliminación temporal está habilitada". Este cambio afectará a cualquier almacén de claves que no tenga activada la eliminación temporal. Pronto habrá disponibles otras herramientas para ayudar a realizar auditorías y este documento se actualizará.

### <a name="what-action-do-i-need-to-take"></a>¿Qué acción debo llevar a cabo?

Asegúrese de que no tiene que realizar cambios en la lógica de la aplicación. Una vez que lo haya comprobado, active la eliminación temporal en todos los almacenes de claves.

### <a name="by-when-do-i-need-to-take-action"></a>¿Cuándo debo realizar alguna acción?

Para asegurarse de que las aplicaciones no se vean afectadas, active la eliminación temporal en los almacenes de claves lo antes posible.

## <a name="next-steps"></a>Pasos siguientes

- Póngase en contacto con nosotros si tiene alguna pregunta sobre este cambio en [akvsoftdelete@microsoft.com](mailto:akvsoftdelete@microsoft.com).
- Lea [Introducción a la eliminación temporal](soft-delete-overview.md).
