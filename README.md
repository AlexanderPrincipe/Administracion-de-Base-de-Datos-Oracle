# Administracion-de-base-de-datos

### Comandos de Base de Datos Oracle para su administracion



## Conceptos

- **OFA:** Estandar de nomenclaturas de Oracle

- **Algoritmo RU:** Elimina los bloques de memoria que son menos utilizados para generar espacio al Buffer Cache

- **Full Scan:** Recorrer toda la tabla

- **Data block:** Unidad minima de Oracle

- **Oracle Base:** Es la ruta padre donde se instala Oracle, solo existe un Oracle Base para todas las BDs

- **Oracle Home:** Es la ruta final donde estara un producto de Oracle

- **SCN (Sequence Change Number):** Es el ID de cualquier transaccion de la Base de datos

- **PFILE:** Es un archivo de texto, para que haga efecto los cambios del PFILE se tiene que reiniciar la instancia

- **SFILE:** Es un archivo binario, es prioridad porque es Online, ⅔ de los cambios del SFILE se puede hacer Online



## Demonios de la Base de datos (Background Process)

- **PMON:** Liberacion de recursos, registra dinamicamente el servicio de una BD al listener por defecto

- **SMON:** Sirve para el Instance Recovery; elimina las tablas temporales que no estan en uso, ademas, desecha transacciones no commiteadas

- **DBWR:** Baja la informacion del Buffer Cache y la manda al DataFile, desecha los bloques que no son utilizados

- **LGWR:** Sirve para recuperacion, cada 3 segundos baja la informacion del Redo Log Buffer, o cada vez que se commitea

- **CKPT (CHECKPOINT):** Poner la base de datos en modo consistente, sucede cuando ocurre un switch de redo log, tambian ocurre cada 1800 segundos, o cuando hay un shutdown immediate

- **MMAN:** Trabaja con la PGA, distribuye la cantidad de memoria en la instancia

- **MMON:** Captura una foto a nivel de instancia para ver la salud de la BD, lo guarda en un AWR, las fotos son tomadas cada 1 hora, las fotos se guardan por 8 dias, pero es modificable, recomendable que sea 31 dias

- **MMNL:** Toma la informacion del ASH que esta en el Shared Pool y lo baja al AWR, ocurre cuando el ASH esta por llenarse

- **RECO:** Permite la conexion de una tabla de otra BD, DBLINKS, permite que se pueda usar un DBLINK

- Las Bases de datos son inconsistentes a menos que ocurra una fase de CHECKPOINT


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

- desc **V$INSTANCE:** Informacion correspondiente a la instancia, se encuentra en Data Dictionary

- desc **V$SESSION:** Informacion detallada de cada sesion de la base de datos

- desc **V$DATABASE:** Informcion de la parte fisica

- desc **DBA_USERS:** Muestra usuarios creados

- desc **DBA_TABLESPACES:** Muestra todos los tablespace existentes

- desc **DBA_SEGMENTS:** Devuelve todos los segmentos creados en la Base de datos

- desc **DABA_DATA_FILES:** Informacion de los datafiles

- desc **DATABASE_PROPERTIES:** Muestra las propiedades de la base de datos

- desc **DBA_FREE_SPACE:** Muestra el espacio libre de los datafiles

- desc **V$TEMPSTAT:** Muestra metricas de los tempfile

- desc **V$TEMPFILE:** Devuelve la lista de archivos que componen los tempfile

- desc **V$DATAFILE:** 

- desc DBA_BLOCKERS: Muestra los usuarios que estan bloqueando a otros usuarios

- desc DBA_WAITERS: Muestra los usuarios que esperan porque otro usuario los estan bloqueando

- desc V$LOCK:

- desc V$LOCKED_OBJECT: Por cada bloqueo muestra que objeto se esta bloqueando

- desc V$SQL: Muestra todos los querys que estan en el library cache








## Comandos

- **Create undo tablespace MIUNDO datafile '/u02/oradata/PRD/miundo01.dbf' size 400M:** Creacion de UNDO

- **ALTER SYSTEM SET UNDO_TABLESPACE:'MIUNDO':** Activa el UNDO

- **DROP TABLESPACE UNDOTBS1 INCLUDING CONTENTS AND DATAFILES:** Borrar un tablespace

- **create user UL identified by limaperu:** Crear usuario UL con contraseña limaperu

- alter user ulima default tablespace LOGISTICA: Asignando tablespace logistica para el usuario ulima

- flasback table NODORMIR to timestamp sysdate-1/24/60x8: Regreso 8 minutos en el pasado y veo la data que habia en ese momento

- alter table NODORMIR disable row movement: Cerrar el candado con disable

- purge recyclebin: Elimina la papelera

- drop table NODORMIR purge: Eliminar la tabla sin la posibilidad de recuperarla

- alter system set UNDO_RETENTION = 3600: Cambiar el tiempo del undo_retention en una hora, es posible recuperar data de hace una hora

- create pfile from spfile: Se crea el pfile con datos del spfile

- alter system kill session '1,64264' immediate: Matar una sesion (SID, SERIAL#)

- create index online: Sirve para crear indices en caliente

- lock table EMP in exclusive mode: Bloquea la tabla a proposito, existen 5 tipos de bloqueo

- alter system set ddl



## Parametros

- SHOW PARAMETER UNDO: Configuracion del UNDO

- show patameter DDl 









