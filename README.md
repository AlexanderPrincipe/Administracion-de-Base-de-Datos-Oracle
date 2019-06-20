# Administracion-de-base-de-datos


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

- **Big Tablespace:** Soporta 1 datafile

- **Small Tablespace:** Soporta varios datafile

- **Conexiones dedicadas:** Conexion por server process

- **Conexiones compartidas:** Comparten el dispatcher

- **Easy Connect:** sqlplus peru/lima@132.68.1.20:1521/PRD:share: Se conecta en modo compartido

- **Easy Connect:** sqlplus system/oracle@132.68.1.20:1521/PRD:share: Se conecta en modo compartido

- **Para conectarse en modo compartido se tiene que modificar el tnsnames.ora:** cd $ORACLE_HOME/network/admin/  vi tnsnames.ora
(SERVER = SHARED)

- Al crear la tabla Oracle la crea de manera diferida, recien cuando se hace el primer insert la tabla se crea, ya que Oracle no reserva espacio cuando no tiene datos la tabla 


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

- Las Bases de datos son inconsistentes a menos que ocurra una fase de **CHECKPOINT**


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

- desc **V$DATAFILE:** Informacion de los datafile

- desc **DBA_BLOCKERS:** Muestra los usuarios que estan bloqueando a otros usuarios

- desc **DBA_WAITERS:** Muestra los usuarios que esperan porque otro usuario los estan bloqueando

- desc **V$LOCK:**

- desc **V$LOCKED_OBJECT:** Por cada bloqueo muestra que objeto se esta bloqueando

- desc **V$SQL:** Muestra todos los querys que estan en el library cache

- desc **dba_resumable:** Muestra quien se ha quedado sin espacio cuando quisieron hacer una carga masiva

- desc **v$dispatchers:** Muestra informacion de los dispatchers

- desc **v$shared_server:**

- desc v$fixed_table: Muestra vistas dinamicas

- dictionary: Muestra DBA y v$

- desc dba_views: Muestra vista del negocio

- desc dba_tab_privs: Devuelve los privilegios de objetos otorgados

- desc sys.adu$: Vista que guarda todo el track de auditoria

- desc dba_synonyms: Muestra todos los sinonimos creados



## Comandos

- **Create undo tablespace MIUNDO datafile '/u02/oradata/PRD/miundo01.dbf' size 400M:** Creacion de UNDO

- **ALTER SYSTEM SET UNDO_TABLESPACE:'MIUNDO':** Activa el UNDO

- **DROP TABLESPACE UNDOTBS1 INCLUDING CONTENTS AND DATAFILES:** Borrar un tablespace

- **create user UL identified by limaperu:** Crear usuario UL con contraseña limaperu

- **alter user ulima default tablespace LOGISTICA:** Asignando tablespace logistica para el usuario ulima

- **flasback table NODORMIR to timestamp sysdate-1/24/60x8:** Regreso 8 minutos en el pasado y veo la data que habia en ese momento

- **alter table NODORMIR disable row movement:** Cerrar el candado con disable

- **purge recyclebin:** Elimina la papelera

- **drop table NODORMIR purge:** Eliminar la tabla sin la posibilidad de recuperarla

- **alter system set UNDO_RETENTION = 3600:** Cambiar el tiempo del undo_retention en una hora, es posible recuperar data de hace una hora

- **create pfile from spfile:** Se crea el pfile con datos del spfile

- **alter system kill session '1,64264' immediate:** Matar una sesion (SID, SERIAL#)

- **create index online:** Sirve para crear indices en caliente

- **lock table EMP in exclusive mode:** Bloquea la tabla a proposito, existen 5 tipos de bloqueo

- **alter system set ddl_lock_tiemout=3600:** Se configura el tiempo de espera de un bloqueo DDL, 1 hora de espera

- **PCT_FREE:** El datablock deja el 10% libre para los updates, si queda menos del 10% Oracle hace un row migration, block chaining se da por insert

- **create bigfile tablespace INFO datafile '/u02/oradata/PRD/info01.dbf size 200M:** Creacion de tablespace

- **alter database set default bigfile tablespace:** 

- **alter database default tablespace INFO:** Se ha definido como default el tablespace INFO

- **alter table default temporary tablespace MITEMP:** Definir tablespace temporal por defecto

- **alter tablespace INFO read only:** Todo lo que esta dentro del tablespace INFO esta solo para lectura

- **alter tablespace INFO read write:** Todo lo que esta dentro del tablespace INFO es modificable

- **alter tablespace INFO offline:** No se podra hacer ni un Select

- **alter tablespace INFO online:** Tablespace activa

- **alter system set db_create_file_dest='/u02/oradata':** Definir ruta por default de los tablespace

- **create tablespace ULIMA:** Se crea un tablespace sin definir la ruta, se tomara la ruta por defecto

## Compresion

- **Compresion bacth:** Comprime hasta 4 veces el tamaño 

- **Compresion OLTP:** Comprime hasta 2 veces

- **create table CLASE (cod number, nombre varchar(20)) compress OLTP:** Comprimer por OLTP

- **alter table CLASE compress basic:** Comprimir por batch

## Desfragmentacion

- El HWM retrocede y elimina los espacios vacios de la tabla, ahora ya no recorre toda la tabla, mejora el performance

- los DDL realizan bloqueos exclusivos (bloqueos en la tabla), ademas, los indices en la tabla quedan dañados

- alter index IDX_TABLA rebuild: Reconstruir indices dañados

## Pasos de desfragmentacion

- **alter table PRODUCTO enable row movement:** Modificar el rowid de las filas

- **alter table PRODUCTO shrink space compact:** Mantener las filas de manera continua en la tabla, elimina espacios vacios

- **alter table PRODUCTO shrink space:** Mueve las filas y mueve el HWM

- **alter table PRODUCTO disable row movement:** Cerrar las filas



- **alter session enable resumable timeout 3600:** Si en una hora no se soluciona, da error, se quedaron sin espacio por la carga masiva

- **Snapshot to old ORA 1555:** La imagen es demasiado vieja y el Undo ya no tiene la informacion

- **alter system set undo_retention = 7200:** Aumentar el tiempo del undo

- **alter database nombre_datafile datafile:** Ampliar el tamaño del tablespace
  
- **alter system set dispatcher='(PRO=TCP)(SERVICE=PRD,PRDSIDXDB)(DIS=4)(TICK=1)(POOL=ON)' **

- **TICK = 1 :** 10 minutos de inactividad de una sesion, el dispatcher pierde conexion con el usuario

- **DIS = 4 :** Cantidad de dispatchers (mozos) 

- **select NAME, STATUS, BUSY/(IDLE+BUSY) from v$dispatcher:** Muestra el porcentaje de utilizacion

- **select NAME, BUSY/(IDLE+BUSY) from v$shared_server:** Muestra el porcentaje de utilizacion

## Seguridad

- create user examen identified by jalado: Crear usuario 'examen' con contraseña 'jalado'

- alter user examen indentified by masjalado: Cambiar contraseña

- alter user examen account lock: Bloquea al usuario examen, no se puede conectar

- useradd -g oinstall franco: Agregar un nuevo usuario

- create user ops$franco identified externally: Con esto el sistema operativo valida y al entrar al sqlplus no pide contraseña

- drop user ivan cascade: Borra todas las tablas del usuario ivan

## Autenticacion

- Autenticacion por password: Es la autenticacion con username y password

- Autenticacion externa: Te conectas con el usuario del sistema operativo, no se ingresa password

- Autenticacion globarl: Huella digital, retina   Oracle 18c


## Privelegios de sistema

- lista = [select, execute, insert, all, create ...]  

- for (i=0, i<lista.length,i++){
  grant <lista[0]> on ivan.cliente to franco
}

- Grant --> select, insert, delete, execute, update, all(DML)

- grant dba to ops$franco: Le asigna el rol dba al usuario franco

- grant create table to Carlos: Carlos podra crear tablas

- grant create triggers to Carlos: Carlos podra creat triggers

- grant create any tables to Carlos: Carlos podra crear tablas sobre cualquier esquema existente

- grant delete any table to Carlos: Puede borrar tablas de cualquier esquema

- grant execute any procedure to Carlos: Puede ejecutar cualquier procedure, hasta los de oracle

- grant select on ivan.cliente to franco: Le da permisos de select de la tabla cliente de ivan a franco

- revoke select on ivan.cliente from franco: Le quita permisos se select a franco de la tabla cliente de ivan

- grant execute on ivan.sp_celular to franco: Le da permisos de ejecutar en la tabla sp_celular de ivan a franco

## Privelegios de Sistemas Administrativos

- grant create user to Carlos: Carlos puede crear usuarios

- grant drop user to Carlos: Carlos puede borrar usuarios

- grant alter user to Carlos: Carlos puede modificar configuracion de usuarios

- grant alter system to Carlos: Permite cambiar parametros de la base de datos

- grant create tablespace to Carlos: Carlos puede crear tablespace

- grant alter database tablespace to Carlos: 

- revoke create user from Carlos: Carlos no puede crear usuarios

- grant create session to piero: Permite que piero acceda a la BD

- alter user piero quota 50M on USERS: Permite escribir hasta 50M

- alter user piero quota unlimited on USERS: Permite escribir ilimitado hasta el tamaño del tablespace

- select privilege from dba_sys_privs where GRANTEE='FRANCO': Ver los permisos de franco

- grant create user to piero with admin option: Con el comando 'with admin option' permite que propagen el permiso

- grant create user to piero with grant option: Con el comando 'with grant option'
 
## Roles

- create role ROL_ALUMNO: Crear rol

- grant create session to ROL_ALUMNO: El rol tiene el derecho de poder conectarse

- grant create table to ROL_ALUMNO: 




## Parametros

- **SHOW PARAMETER UNDO:** Configuracion del UNDO

- **show patameter ddl:**

- **show parameter shared_server:** 

- show parameter remote: remote_os_authent

- sec_case_sensitive_logon: Alter system set sec_case_sensitive_logon = FALSE; : Para que al loguearse no importe si es mayuscula o minuscula

- show parameter deferred_segment_creation: 











