---
layout: single
title: "Hybrid Cloud Extension (HCX) Lab Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---
# Introducción

En este ejercicio aprenderá acerca de Hybrid Cloud Extension (HCX), principalmente esta es una herramienta, incluida con VMware Cloud on AWS, que le permitirá migrar cargas de trabajo a VMware Cloud on AWS y reducir significativamente el tiempo y la complejidad de la migración de cargas de trabajo en la esfera de la nube pública.

## ¿Qué es Hybrid Cloud Extension (HCX)?

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX1.jpg)

Hybrid Cloud Extension abstrae los recursos on-premises y los ubicados en la nube presentándolos a las aplicaciones como una nube híbrida continua. Hybrid Cloud Extension provee interconexiones a múltiples sitios, de alto rendimiento, seguras y optimizadas. La hibridación se da gracias a la abstracción y a las interconexiones. Sobre esta hibridación, Hybrid Cloud Extension facilita movilidad de aplicaciones segura y simple a través de las plataformas vSphere en el centro de datos y las Nubes VMware. Hybrid Cloud Extension es un servicio multi-sitio, multi-nube que facilita la operación de una verdadera nube híbrida.

### Características de Hybrid Cloud Extension

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX2.jpg)

#### Any-to-Any vSphere Cloud App Mobility

* Movimiento rápido de cargas de trabajo existentes desde una plataforma vSphere al SDDC
* Reducir el tiempo de planeación inicial para costos y análisis de recursos
* Acelerar la adopción de nube y evitar cambios importantes en el ambiente on-premise

#### Continuidad de Negocio con un Menor TCO

* Reconfiguración de direcciones IP y MAC no es necesario
* No es necesario modificar las VMs existentes en el ambiente
* Hybrid Cloud Extension brinda migración warm y cold en lote y bidireccional
* Hybrid Cloud Extension simplifica su modelo operacional

#### Diseñado para la Seguridad

* Asegure un control altamente seguro para las nubes privadas y publicas
* Proteja recursos con capacidades de recuperación de desastre
* La DMZ híbrida de Hybrid Cloud Extension permite portabilidad de las redes empresariales y las prácticas seguras en la nube
* Las políticas de seguridad migran junto con las aplicaciones

#### Hibridación de Infraestructura de Alto Rendimiento

* Optimización WAN incluida que se adapta a las necesidades de los casos de uso híbridos
* Hybrid Cloud Extension brinda enrutamiento ágil e inteligente
* Balanceo de tráfico garantizado por políticas
* Múltiples modelos de migración de VM (incluyendo vMotion, warm y cold) facilita la migración a y desde la nube sin ningún cambio

## Configurar HCX

Haga click en el enlace a continuación para ensayar los pasos de como instalar y configurar HCX en su ambiente local on-premises.

[HCX Install and Configuration](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX-OnPrem-Installation.htm){:target="_blank"}

## HCX - Migración usando vMotion

Ahora que ya está familiarizado en como instalar y configurar HCX, vamos a realizar una migracion en vivo de una máquina virtual utilizando vMotion hacia VMware Cloud on AWS.

### Iniciar Sesión a vCenter On-Prem

Le hemos proveído un vCenter on-prem con máquinas virtuales para migrar. Dependiendo de su número asignado de estudiante ## favor selecione la máquina virtual respectiva para migrar.

Desde su escritorio Horizon (desktop.vmc.ninja) abra Google Crhome y acceda a este vCenter on-prem.

**Nota: Refiera a la página Student Access para iniciar una sesión al escritorio virtual Horizon si es necesario https://vmc-field-team.github.io/student-access/**

![HCX01](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx01.jpg)

1. Habra Google Chrome y escriba https://vcenter-workshop.workshop.set.local/ui para el URL.
2. Escriba su número de estudiante (student#@set.local) para credenciales
3. Escriba su contraseña asignada
4. Haga click en **Login** para continuar

### Migración de una máquina virtual a la nube

![HCX02](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02.jpg)

1. Haga click en la flecha para expander **Datacenter**.
2. Haga click en la flecha para expander **VSAN-Cluster**.
3. Haga click en la flecha para expander **Migrate** resource pool.
4. Haga click en la máquina virtual **Student##**.
5. Tome nota de la dirección IP para poder hacer un ping en otro ejercicio.

![HCX02-1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02-1.jpg)

1. Haga click derecho en el ícono de Windows en la parte inferior izquierda de su escritorio.
2. Haga click en **Command Prompt**.

![HCX02-2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx02-2.jpg)

1. En caso que no apuntó la dirección IP de su máquina virtual.
2. Escriba en la ventana de command prompt **ping 172.60.2.xxxxx**.

![HCX03](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx03.jpg)

1. Regrese a su consola de vCenter y haga click derecho en su máquina virtual **Student##**.
2. Apunte el mouse sobre **Hybridity Actions**.
3. Haga click en **Migrate to the Cloud**.

![HCX04](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx04.jpg)

Para migrar la máquina virtual a VMware Cloud on AWS tendrá que escoger el destino para cosas como la carpeta, resource pool y datastore.

**Nota: Asegúrese de seleccionar la carpeta y resource pool Migrate para asegurarse que pueda encontrar la misma máquina virtual cuando la migre de regreso a on-prem.**

1. Haga click en **Specify Destination Folder** y seleccione la carpeta **Migrate**.
2. Haga click en **Specify Destination Container** y seleccione **Migrate**.
3. Haga click en **Select Storage** y seleccione el datastore **WorkloadDatastore**.
4. Haga click en **Select Migration Type** y seleccione **Cloud Motion with vSphere Replication**.
    No modifique el **schedule failover** para asegurar que la migración de la máquina ocurra de inmediato.
5. Haga click en **Next** para validar su selección.

![HCX05](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx05.jpg)

1. Verifique que la validación fué exitosa.
2. Haga click en **Finish** para migrar su máquina virtual a VMware Cloud on AWS.

### Chequear el Progreso de la Migración

![HCX06](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx06.jpg)

1. Haga click en **Menu**.
2. Haga click en **HCX**.

![HCX07](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx07.jpg)

El Dashboard le demuestra información general de cuántas máquinas virtuales se han migrado, están migrándose, y han sido agendadas para ser migradas.

1. Haga click en **Migration** en el panel izquierdo.

![HCX08](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx08.jpg)

1. Tome nota del progreso de la migración con vMotion.
2. Haga click en el botón de **Refresh** para actualizar el progreso.

![HCX09](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx09.jpg)

Mientras la migración esté en progreso, examine la respuesta del ping.

1. Haga click en la ventana de **Command Prompt** para regresar a la prueba de ping.
2. Observe la prueba de ping que dejamos corriendo en un ejercicio anterior y fíjese que no se ha caído.

**Nota: Asegúrese que la migración sea exitosa antes de continuar con el siguiente paso.**

Una vez la máquina virtual se haya migrado exitosamente a VMware Cloud on AWS, vamos a tomar la misma máquina y la migraremos de regreso a nuestro vCenter on-prem.

### Migración de Máquina Virtual de Regreso al Centro de Datos On-prem

Utilizaremos vMotion para migrar la máquina virtual de regreso al vCenter on-prem. Note que esto es una operación que se realiza en serie y dependiendo de cuantas máquinas virtuales estén siendo migradas con vMotion a la misma vez, puede que tome unos minutos para completar.

![HCX010](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx010.jpg)

1. Verifique que la máquina virtual haya sido migrada exitosamente al SDDC en VMware Cloud on AWS.
2. Haga click en el botón **Migrate Virtual Machines**.

![HCX011](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx011.jpg)

1. Haga click en **Reverse Migration** para cambiar la vista al vCenter de VMware Cloud on AWS.
2. Haga click en el resource pool **Migrate** para mostrar las máquinas virtuales que fueron migradas.
3. Haga click en la máquina que le corresponde **Student##**.
4. Haga click en **Specify Destination Folder** y seleccione la carpeta **Migrate**.
5. Haga click en **Specify Destination Container** y seleccione el resource pool **Migrate**.
6. Haga click en **Select Storage** y seleccione el datastore **vsanDatastore**.
7. Haga click en **Select Migration Type** y seleccione **vMotion**.
8. Haga click en **Next** para validar su selección.

![HCX012](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx012.jpg)

1. Verifique que la validación sea exitosa.
2. Haga click en **Finish** para migrar su máquina virtual de regreso al vCenter on-prem.

![HCX013](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx013.jpg)

1. Tome nota del progreso de la migración con vMotion.
2. Haga click en el botón de **Refresh** para actualizar el progreso.

![HCX014](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx014.jpg)

**Paso Opcional**

1. Haga click en el **Command Prompt** para regresar a la prueba de ping.
2. Observe la prueba de ping que dejamos corriendo en un ejercicio anterior y fíjese que no se ha caído mientras la máquina se migra hacia el vCenter on-prem.
3. Use ctrl-C para cancelar el ping.

### Otros métodos de migración

Tómese la libertad de probar otros tipos de migración. Use la misma máquina virtual **Student##** que le fue asignada y siga los mismos pasos pero en vez de hacer la migración con vMotion, trate de utilizar **Bulk Migration**.

![HCX015](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/hcx015.jpg)

#### Cloud Motion with vSphere Replication

Esta última opción proporciona cero tiempos de inactividad para la movilidad de la carga de trabajo desde el origen hasta el destino. Primero, los discos de carga de trabajo se replican en el sitio de destino. La replicación se maneja utilizando la replicación de vSphere incorporada en HCX. Este proceso depende de la cantidad de datos y el ancho de banda de red disponible. Una vez que se completa la sincronización de datos, HCX inicia un vMotion para completar la migración. vMotion migra la carga de trabajo al sitio de destino y sincroniza solo los datos restantes (delta) y el estado de la memoria de la carga de trabajo. Hay una opción para programar una ventana de mantenimiento para el swithcover de sincronización de datos de vMotion, de lo contrario sucede de inmediato.

#### Bulk Migration

Bulk Migration crea una nueva máquina virtual en el sitio de destino. Esto puede ser local o VMC y retiene el UUID de la carga de trabajo. Luego usa vSphere Replication para copiar un snapshot de la carga de trabajo desde el sitio de origen al lugar de destino mientras la carga de trabajo aún está encendida. En este caso, un snapshot es un punto del tiempo del estado del disco de carga de trabajo, pero no es el snapshot de vSphere tradicional. Bulk Migration es administrada por el proxy de puerta de enlace en la nube a través del HCX Interconnect. Durante la sincronización de datos, no hay interrupción de las cargas de trabajo. La sincronización de datos depende de la cantidad de datos y el ancho de banda disponible. Existe una opción para programar una ventana de mantenimiento para la conmutación (switchover); de lo contrario, la conmutación se realiza de inmediato. Una vez que se completa la sincronización de datos inicial, tiene lugar una conmutación (a menos que esté programada). Las cargas de trabajo del sitio de origen están inactivas y se cierran aprovechando VMware Tools. Si VMware Tools no está disponible, HCX le solicitará que apague la(s) carga(s) de trabajo para iniciar el cambio. Durante el proceso de cambio, se produce una sincronización delta basada en el seguimiento de bloque modificado (CBT) para sincronizar los cambios desde el snapshot original. Las cargas de trabajo en el sitio de destino comenzarán a encenderse una vez que se complete la sincronización de datos (incluidos los cambios de datos delta). Se han realizado comprobaciones para garantizar que los recursos estén disponibles para encender las cargas de trabajo. Si una carga de trabajo de destino no puede encenderse debido a los recursos, la carga de trabajo de origen se volverá a encender.

#### vMotion "Live Migration"

HCX es compatible con el vMotion que conocemos y amamos hoy. Las cargas de trabajo se migran en vivo sin tiempo de inactividad similar a Cloud Motion con vSphere Replication. vMotion no debe utilizarse para migrar cientos de cargas de trabajo o cargas de trabajo con grandes cantidades de datos. En su lugar, use Cloud Motion con vSphere Replication o Bulk Migration. Por lo general, una red vMotion debe configurarse y enrutarse al host vSphere de destino, en este caso, el tráfico de vMotion es manejado por el Cloud Gateway del HCX Interconnect para vMotion entre nubes. vMotion a través de HCX encapsula y encripta todo el tráfico desde el origen al destino, eliminando la complejidad de la red de enrutamiento a la nube.

HCX tiene una opción incorporada para conservar las direcciones MAC de las cargas de trabajo. Si esta opción no está marcada, las cargas de trabajo tendrán un MAC diferente en el sitio de destino. Las cargas de trabajo deben estar en compatibilidad (hardware) versión 9 o mayor y 100 Mbps o superior del ancho de banda debe estar disponible. Con vMotion y la migración bidireccional, es importante tener en cuenta la Compatibilidad de vMotion mejorada (EVC). La buena noticia aquí es que HCX también maneja EVC. Las cargas de trabajo se pueden migrar sin problemas y, una vez que se reinicie, se heredarán las características de la CPU del clúster de destino. Esto permite un vMotion entre nubes entre diferentes versiones de chipset (por ejemplo, Sandy Bridge to Skylake) pero dentro de la misma familia de CPU (por ejemplo, Intel). **Además, es importante tener en cuenta que vMotion se realiza de manera serializada.** Solo se produce un vMotion a la vez y pone en cola las cargas de trabajo restantes hasta que se completa el vMotion actual.