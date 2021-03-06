---
title: 'Tutorial: Integración de Azure Active Directory con Velpic SAML | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Velpic SAML.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8328fc5582fa9ebe38a23e5916ac0c3127575529
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166104"
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Tutorial: Integración de Azure Active Directory con Velpic SAML

En este tutorial, aprenderá a integrar Velpic SAML con Azure Active Directory (Azure AD).

La integración de Velpic SAML con Azure AD le proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a Velpic SAML.
- Puede permitir que los usuarios inicien sesión automáticamente en Velpic SAML (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el Portal de administración de Azure

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Velpic SAML, necesita lo siguiente:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Velpic SAML

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No debe usar el entorno de producción, a menos que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar Velpic SAML desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-velpic-saml-from-the-gallery"></a>Agregar Velpic SAML desde la galería
Para configurar la integración de Velpic SAML en Azure AD, es preciso agregar Velpic SAML desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Velpic SAML desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo del **[Portal de administración de Azure](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Haga clic en el botón **Agregar** situado en la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Velpic SAML**.

    ![Creación de un usuario de prueba de Azure AD](./media/velpicsaml-tutorial/tutorial_velpicsaml_search.png)

1. En el panel de resultados, seleccione **Velpic SAML** y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Velpic SAML utilizando un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Velpic SAML para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Velpic SAML.

Esta relación de vínculo se establece mediante la asignación del valor de **nombre de usuario** en Azure AD como valor de **Username** (Nombre de usuario) en Velpic SAML.

Para configurar y probar el inicio de sesión único de Azure AD con Velpic SAML, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Velpic SAML](#creating-a-velpic-saml-test-user)**: para tener un homólogo de Britta Simon en Velpic SAML que esté vinculado a su representación en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en el Portal de administración de Azure y configurará el inicio de sesión único en la aplicación Velpic SAML.

**Para configurar el inicio de sesión único de Azure AD con Velpic SAML, realice los pasos siguientes:**

1. En el Portal de administración de Azure, en la página de integración de aplicaciones de **Velpic SAML**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo**, seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

1. Escriba los datos en la sección **Dominio y direcciones URL de Velpic SAML**:

    ![Configurar inicio de sesión único](./media/velpicsaml-tutorial/tutorial_velpicsaml_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba el valor como: `https://<sub-domain>.velpicsaml.net`

    b. En el cuadro de texto **Identificador**, pegue el valor de **"Single sign on URL"** (URL de inicio de sesión único), `https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Tenga en cuenta que la URL de inicio de sesión se la proporcionará el equipo de Velpic SAML y el valor del identificador estará disponible cuando configure el complemento de SSO en Velpic SAML. Tiene que copiar ese valor de la página de la aplicación Velpic SAML y pegarlo aquí.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo XML en el equipo.

    ![Configurar inicio de sesión único](./media/velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/velpicsaml-tutorial/tutorial_general_400.png)

1. En la sección Configuración de Velpic SAML, haga clic en Configurar Velpic SAML para abrir la ventana Configurar inicio de sesión. Copie el Id. de entidad de SAML de la sección Referencia rápida.

1. En otra ventana del explorador web, inicie sesión como administrador en su sitio de la compañía de Velpic SAML.

1. Haga clic en la pestaña **Manage** (Administrar) y vaya a la sección **Integration** (Integración), donde tiene que hacer clic en el botón **Plugins** (Complementos) para crear el complemento Sign-In (Inicio de sesión).

    ![Complemento](./media/velpicsaml-tutorial/velpic_1.png)

1. Haga clic en el botón **"Add plugin"** (Agregar complemento).
    
    ![Complemento](./media/velpicsaml-tutorial/velpic_2.png)

1. Haga clic en el icono **SAML** de la página Add Plugin (Agregar complemento).
    
    ![Complemento](./media/velpicsaml-tutorial/velpic_3.png)

1. Escriba el nombre del nuevo complemento SAML y haga clic en el botón **"Add"** (Agregar).

    ![Complemento](./media/velpicsaml-tutorial/velpic_4.png)

1. Especifique los detalles de la manera siguiente:

    ![Complemento](./media/velpicsaml-tutorial/velpic_5.png)

     a. En el cuadro de texto **Name** (Nombre), escriba el nombre del complemento SAML.

    b. En el cuadro de texto **Issuer URL** (URL del emisor), pegue el **Id. de entidad de SAML** que copió de la ventana **Configurar inicio de sesión** de Azure Portal.

    c. En **Provider Metadata Config** (Config. de metadatos del proveedor), cargue el archivo XML de metadatos que descargó de Azure Portal.

    d. También puede habilitar aprovisionamiento Just-In-Time de SAML activando la casilla **"Auto create new users"** (Crear nuevos usuarios automáticamente). Si no existe ningún usuario de Velpic y no se habilita esta marca, el inicio desde Azure no se producirá. Si se habilita la marca, el usuario se aprovisionará automáticamente en Velpic en el momento de inicio de sesión. 

    e. Copie la **Single sign on URL** (URL de inicio de sesión) en el cuadro de texto y péguela en Azure Portal.
    
    f. Haga clic en **Save**(Guardar).

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en el Portal de administración de Azure llamado Britta Simon.

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo del **Portal de administración de Azure**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/velpicsaml-tutorial/create_aaduser_01.png) 

1. Vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios** para mostrar la lista de usuarios.
    
    ![Creación de un usuario de prueba de Azure AD](./media/velpicsaml-tutorial/create_aaduser_02.png) 

1. En la parte superior del diálogo, haga clic en **Agregar** para abrir el diálogo **Usuario**.
 
    ![Creación de un usuario de prueba de Azure AD](./media/velpicsaml-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/velpicsaml-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-velpic-saml-test-user"></a>Creación de un usuario de prueba de Velpic SAML

Este paso no suele ser necesario porque la aplicación admite aprovisionamiento de usuarios Just-In-Time. Si el aprovisionamiento automático de usuarios no se habilita, puede llevarse a cabo la creación manual de usuarios tal y como se describe a continuación.

Inicie sesión como administrador en su sitio de la compañía de Velpic SAML y realice los pasos siguientes:
    
1. Haga clic en la pestaña Manage (Administrar), vaya a la sección Users (Usuarios) y haga clic en el botón New (Nuevo) para agregar usuarios.

    ![agregar usuario](./media/velpicsaml-tutorial/velpic_7.png)

1. En el cuadro de diálogo **"Create New User"** (Crear nuevo usuario), realice los pasos siguientes.

    ![user](./media/velpicsaml-tutorial/velpic_8.png)
    
     a. En el cuadro de texto **First Name** (Nombre), escriba el nombre de Britta Simon.

    b. En el cuadro de texto **Last Name** (Apellido), escriba el apellido de Britta Simon.

    c. En el cuadro de texto **User Name** (Nombre de usuario), escriba el nombre del usuario de Britta Simon.

    d. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de la cuenta de Britta Simon.

    e. El resto de la información es opcional, puede rellenarla si es necesario.
    
    f. Haga clic en **GUARDAR**.  

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Velpic SAML.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Velpic SAML, siga estos pasos:**

1. En el Portal de administración de Azure, abra la vista de aplicaciones, vaya a la vista de directorio y vaya a **Aplicaciones empresariales**. A continuación, haga clic en **All applications** (Todas las aplicaciones).

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Velpic SAML**.

    ![Configurar inicio de sesión único](./media/velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

1. Al hacer clic en el icono de Velpic SAML del panel de acceso, debería entrar en la página de inicio de sesión de la aplicación Velpic SAML. Debe ver el botón **"Log In With Azure AD"** (Iniciar sesión con Azure AD) en la página de inicio de sesión.

    ![Complemento](./media/velpicsaml-tutorial/velpic_6.png)

1. Haga clic en el botón **"Log In With Azure AD"** (Iniciar sesión con Azure AD) para iniciar sesión en Velpic con su cuenta de Azure AD.


## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/velpicsaml-tutorial/tutorial_general_203.png

