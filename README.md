# Administracion-de-base-de-datos

### Comandos de Base de Datos Oracle para su administracion



## Conceptos

_**OFA:** Estandar de nomenclaturas de Oracle_

_**Algoritmo RU:** Elimina los bloques de memoria que son menos utilizados para generar espacio al Buffer Cache_

_**Full Scan:** Recorrer toda la tabla_

_**Data block:** Unidad minima de Oracle_

_**Oracle Base:** Es la ruta padre donde se instala Oracle, solo existe un Oracle Base para todas las BDs_

_**Oracle Home:** Es la ruta final donde estara un producto de Oracle_

_**SCN (Sequence Change Number):** Es el ID de cualquier transaccion de la Base de datos_

_**PFILE:** Es un archivo de texto, para que haga efecto los cambios del PFILE se tiene que reiniciar la instancia_

_**SFILE:** Es un archivo binario, es prioridad porque es Online, ⅔ de los cambios del SFILE se puede hacer Online_



## Demonios de la Base de datos (Background Process)

_**PMON:** Liberacion de recursos, registra dinamicamente el servicio de una BD al listener por defecto_

_**SMON:** Sirve para el Instance Recovery; elimina las tablas temporales que no estan en uso, ademas, desecha transacciones no commiteadas_

_**DBWR:** Baja la informacion del Buffer Cache y la manda al DataFile, desecha los bloques que no son utilizados_

_**LGWR:** Sirve para recuperacion, cada 3 segundos baja la informacion del Redo Log Buffer, o cada vez que se commitea_

_**CKPT (CHECKPOINT):** Poner la base de datos en modo consistente, sucede cuando ocurre un switch de redo log, tambian ocurre cada 1800 segundos, o cuando hay un shutdown immediate_

_**MMAN:** Trabaja con la PGA, distribuye la cantidad de memoria en la instancia_

_**MMON:** Captura una foto a nivel de instancia para ver la salud de la BD, lo guarda en un AWR, las fotos son tomadas cada 1 hora, las fotos se guardan por 8 dias, pero es modificable, recomendable que sea 31 dias

_**MMNL:** Toma la informacion del ASH que esta en el Shared Pool y lo baja al AWR, ocurre cuando el ASH esta por llenarse_

_**RECO:** Permite la conexion de una tabla de otra BD, DBLINKS, permite que se pueda usar un DBLINK_

_Las Bases de datos son inconsistentes a menos que ocurra una fase de CHECKPOINT_


## Fases de la Base de Datos

* **NO MOUNT:** Lee los parameter file, solo tiene informacion de la instancia

* **MOUNT:** Ubica el Control File y se asegura que es consistente, conoce el SCN

* **OPEN:** Lee todos los Data File + Redo Log, levanta la BD 


## Formas de bajar la Base de datos

* **SHUTDOWN NORMAL:** Espera que todos los usuarios salgan por su propia cuenta, hace CHECKPOINT

* **SHUTDOWN TRANSACTIONAL:** Espera que haga commit o rollback, hace CHECKPOINT

* **SHUTDOWN IMMEDIATE:** Bota a todos

* **SHUTDOWN ABORT:** Baja toda la BD, no espera a nadie


## Vistas






### Pre-requisitos 📋

_Que cosas necesitas para instalar el software y como instalarlas_

```
Da un ejemplo
```
