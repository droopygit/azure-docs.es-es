---
title: 'Tutorial: Integración de Azure Active Directory con Mimecast Personal Portal | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Mimecast Personal Portal.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/21/2020
ms.author: jeedes
ms.openlocfilehash: ad7b5b97149d38b64d75f5a02cd0aa776893e832
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92522571"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mimecast-personal-portal"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Mimecast Personal Portal

En este tutorial, aprenderá a integrar Mimecast Personal Portal con Azure Active Directory (Azure AD). Al integrar Mimecast Personal Portal con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a Mimecast Personal Portal.
* Permitir que los usuarios inicien sesión automáticamente en Mimecast Personal Portal con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Mimecast Personal Portal

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Mimecast Personal Portal admite el SSO iniciado por **SP** .
* Una vez que haya configurado Mimecast Personal Portal, puede aplicar el control de sesión, que protege en tiempo real su organización frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Incorporación de Mimecast Personal Portal desde la galería

Para configurar la integración de Mimecast Personal Portal en Azure AD, deberá agregar esta solución desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory** .
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones** .
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación** .
1. En la sección **Add from the gallery** (Agregar desde la galería), escriba **Mimecast Personal Portal** en el cuadro de búsqueda.
1. Seleccione **Mimecast Personal Portal** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-mimecast-personal-portal"></a>Configuración y prueba del inicio de sesión único de Azure AD para Mimecast Personal Portal

Configure y pruebe el inicio de sesión único de Azure AD con Mimecast Personal Portal mediante un usuario de prueba llamado **B.Simon** . Para que el inicio de sesión único funcione, es necesario establecer una vinculación entre un usuario de Azure AD y el usuario relacionado de Mimecast Personal Portal.

Para configurar y probar el inicio de sesión único de Azure AD con Mimecast Personal Portal, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Mimecast Personal Portal](#configure-mimecast-personal-portal-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Mimecast Personal Portal](#create-mimecast-personal-portal-test-user)** , para tener un usuario correspondiente a B.Simon en Mimecast Personal Portal que esté vinculado a la representación de usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Mimecast Personal Portal** , busque la sección **Manage** (Administrar) y seleccione el **inicio de sesión único** .
1. En la página **Seleccione un método de inicio de sesión único** , elija **SAML** .
1. En la página **Configurar el inicio de sesión único con SAML** , haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Basic SAML Configuration** (Configuración básica de SAML), si desea configurar la aplicación en modo iniciado por IDP, realice los pasos siguientes:

    a. En el cuadro de texto **Identificador** , escriba una dirección URL con el siguiente patrón:

    | Region  |  Value | 
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Estados Unidos   | `https://us-api.mimecast.com/sso/<accountcode>`|
    | Sudáfrica    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Australia       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Internacional        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    > [!NOTE]
    > Encontrará el valor `accountcode` en Mimecast Personal Portal, en **Account** > **Settings** > **Account Code** (Cuenta > Configuración > Código de cuenta). Anexe el `accountcode` al identificador.

    b. En el cuadro de texto **URL de respuesta** , escriba una dirección URL:

    | Region  |  Value |
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Estados Unidos   | `https://us-api.mimecast.com/login/saml`|
    | Sudáfrica    | `https://za-api.mimecast.com/login/saml`|
    | Australia       | `https://au-api.mimecast.com/login/saml`|
    | Internacional        | `https://jer-api.mimecast.com/login/saml`|

1. Si quiere volver a configurar la aplicación en modo iniciado por **SP** :

    En el cuadro de texto **URL de inicio de sesión** , escriba una dirección URL:

    | Region  |  Value |
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Estados Unidos   | `https://us-api.mimecast.com/login/saml`|
    | Sudáfrica    | `https://za-api.mimecast.com/login/saml`|
    | Australia       | `https://au-api.mimecast.com/login/saml`|
    | Internacional        | `https://jer-api.mimecast.com/login/saml`|

1. Haga clic en **Save** (Guardar).

1. En la página **Configurar el inicio de sesión único con SAML** , en la sección **Certificado de firma de SAML** , haga clic en el botón de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory** , **Usuarios** y **Todos los usuarios** .
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario** , siga estos pasos:
   1. En el campo **Nombre** , escriba `B.Simon`.  
   1. En el campo **Nombre de usuario** , escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña** .
   1. Haga clic en **Crear** .

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a Mimecast Personal Portal mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones** .
1. En la lista de aplicaciones, seleccione **Mimecast Personal Portal** .
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos** .

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario** . A continuación, en el cuadro de diálogo **Agregar asignación** , seleccione **Usuarios y grupos** .

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos** , seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** , seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación** , haga clic en el botón **Asignar** .

## <a name="configure-mimecast-personal-portal-sso"></a>Configuración del inicio de sesión único en Mimecast Personal Portal

1. Abra una nueva ventana del explorador web e inicie sesión en Mimecast Administration Console.

1. Vaya a **Administration** > **Services** > **Applications** (Administración > Servicios > Aplicaciones).

    ![Captura de pantalla que muestra la ventana de Mimecast con la opción Applications (Aplicaciones) seleccionada.](./media/mimecast-personal-portal-tutorial/services.png)

1. Haga clic en la pestaña **Authentication Profiles** (Perfiles de autenticación).
    
    ![Captura de pantalla que muestra la pestaña Application (Aplicación) con la opción Authentication Profiles (Perfiles de autenticación) seleccionada.](./media/mimecast-personal-portal-tutorial/authentication-profiles.png)

1. Haga clic en la pestaña **New Authentication Profile** (Nuevo perfil de autenticación).

    ![Captura de pantalla que muestra la pestaña New Authentication Profile (Nuevo perfil de autenticación) seleccionada.](./media/mimecast-personal-portal-tutorial/new-authenticatio-profile.png)

1. Proporcione una descripción válida en el cuadro de texto **Description** (Descripción) y seleccione la casilla **Enforce SAML Authentication for Mimecast Personal Portal** (Aplicar la autenticación SAML a Mimecast Personal Portal).

    ![Captura de pantalla que muestra la pestaña New Authentication Profile (Nuevo perfil de autenticación) seleccionada.](./media/mimecast-personal-portal-tutorial/selecting-personal-portal.png)

1. En la página de **configuración de SAML para Mimecast Personal Portal** , realice los pasos siguientes:

    ![Captura de pantalla que muestra dónde seleccionar la aplicación de la autenticación de SAML para la consola de administración.](./media/mimecast-personal-portal-tutorial/sso-settings.png)

    a. En **Provider** (Proveedor), seleccione **Azure Active Directory** en el menú desplegable.

    b. En el cuadro de texto **Metadata URL** (Dirección URL de metadatos), pegue el valor de **App Federation Metadata URL** (Dirección URL de metadatos de federación de aplicaciones) que copió de Azure Portal.

    c. Haga clic en **Import** . Después de importar la dirección URL de los metadatos, los campos se rellenarán automáticamente, sin necesidad de realizar ninguna acción.

    d. Asegúrese de desactivar las casillas **Use Password protected Context** (Usar contexto protegido por contraseña) y **Use Integrated Authentication Context** (Usar contexto de autenticación integrada).

    e. Haga clic en **Save** (Guardar).

### <a name="create-mimecast-personal-portal-test-user"></a>Creación de un usuario de prueba en Mimecast Personal Portal

1. Abra una nueva ventana del explorador web e inicie sesión en Mimecast Administration Console.

1. Vaya a **Administration** > **Directories** > **Internal Directories** (Administración > Directorios > Directorios Internos).

    ![Captura de pantalla que muestra la configuración de SAML de Mimecast Personal Portal, donde puede escribir los valores descritos.](./media/mimecast-personal-portal-tutorial/internal-directories.png)

1. Seleccione su dominio si se menciona más abajo. En caso contrario, cree un dominio nuevo haciendo clic en **New Domain** (Nuevo dominio).

    ![Captura de pantalla que muestra la ventana de Mimecast con la opción Internal Directories (Directorios internos) seleccionada.](./media/mimecast-personal-portal-tutorial/domain-name.png)

1. Haga clic en la pestaña **New Address** (Nueva dirección).

    ![Captura de pantalla que muestra el dominio seleccionado.](./media/mimecast-personal-portal-tutorial/new-address.png)

1. Proporcione la información de usuario necesaria en la página siguiente:

    ![Captura de pantalla que muestra la página donde puede especificar los valores descritos.](./media/mimecast-personal-portal-tutorial/user-information.png)

    a. En el cuadro de texto **Email Address** (Dirección de correo electrónico), escriba la dirección de correo electrónico del usuario como `B.Simon@yourdomainname.com`.

    b. En el cuadro de texto **Global name** (Nombre global), escriba el **nombre completo** del usuario.

    c. En los cuadros de texto **Password** (Contraseña) y **Confirm Password** (Confirmar contraseña), escriba la contraseña del usuario.

    d. Seleccione la casilla **Force Change at Login** (Forzar cambio al iniciar de sesión).

    e. Haga clic en **Save** (Guardar).

    f. Para asignar roles al usuario, haga clic en **Role Edit** (Editar rol) y asigne el rol necesario al usuario con arreglo a las necesidades de su organización.

    ![Captura de pantalla que muestra la configuración de direcciones, donde puede seleccionar Role Edit (Editar rol).](./media/mimecast-personal-portal-tutorial/assign-role.png)


## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Mimecast Personal Portal del panel de acceso, debería iniciar sesión automáticamente en la versión de Mimecast Personal Portal para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Prueba de Mimecast Personal Portal con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Protección de Mimecast Personal Portal con controles y visibilidad avanzados](/cloud-app-security/proxy-intro-aad)