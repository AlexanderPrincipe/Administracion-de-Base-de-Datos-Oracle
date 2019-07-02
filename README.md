# Administracion-de-base-de-datos-Oracle


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

- desc **v$fixed_table:** Muestra vistas dinamicas

- **dictionary:** Muestra DBA y v$

- desc **ba_views:** Muestra vista del negocio

- desc **dba_tab_privs:** Devuelve los privilegios de objetos otorgados

- desc **sys.adu$:** Vista que guarda todo el track de auditoria

- desc **dba_synonyms:** Muestra todos los sinonimos creados

- desc **dba_roles:** Vista que contiene todos los roles

- desc **dba_tab_privs:** Muestra los objetos asignados a un rol

- desc **dba_priv_captures:** Muestra todas las politicas creadas

- desc **dba_used_sysprivs:** Privilegios de sistema que han sido utilizados en el analisis

- desc **dba_unused_sysprivs:** Privilegios de sistema que no han sido utilizados en el analisis

- desc **dba_used_objprivs:** Muestra los objetos que han sido utilizados

- desc **v$pwfile_users:** Contenido del password file

- desc **v$log:** Muestra los grupos de redo log creados


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

- **create user examen identified by jalado:** Crear usuario 'examen' con contraseña 'jalado'

- **alter user examen indentified by masjalado:** Cambiar contraseña

- **alter user examen account lock:** Bloquea al usuario examen, no se puede conectar

- **useradd -g oinstall franco:** Agregar un nuevo usuario

- **create user ops$franco identified externally:** Con esto el sistema operativo valida y al entrar al sqlplus no pide contraseña

- **drop user ivan cascade:** Borra todas las tablas del usuario ivan

## Autenticacion

- **Autenticacion por password:** Es la autenticacion con username y password

- **Autenticacion externa:** Te conectas con el usuario del sistema operativo, no se ingresa password

- **Autenticacion globarl:** Huella digital, retina   Oracle 18c


## Privelegios de sistema

- lista = [select, execute, insert, all, create ...]  

- for (i=0, i<lista.length,i++){
  grant <lista[i]> on ivan.cliente to franco
}

- Grant --> select, insert, delete, execute, update, all(DML)

- **grant dba to ops$franco:** Le asigna el rol dba al usuario franco

- **grant create table to Carlos:** Carlos podra crear tablas

- **grant create triggers to Carlos:** Carlos podra creat triggers

- **grant create any tables to Carlos:** Carlos podra crear tablas sobre cualquier esquema existente

- **grant delete any table to Carlos:** Puede borrar tablas de cualquier esquema

- **grant execute any procedure to Carlos:** Puede ejecutar cualquier procedure, hasta los de oracle

- **grant select on ivan.cliente to franco:** Le da permisos de select de la tabla cliente de ivan a franco

- **revoke select on ivan.cliente from franco:** Le quita permisos se select a franco de la tabla cliente de ivan

- **grant execute on ivan.sp_celular to franco:** Le da permisos de ejecutar en la tabla sp_celular de ivan a franco

## Privelegios de Sistemas Administrativos

- **grant create user to Carlos:** Carlos puede crear usuarios

- **grant drop user to Carlos:** Carlos puede borrar usuarios

- **grant alter user to Carlos:** Carlos puede modificar configuracion de usuarios

- **grant alter system to Carlos:** Permite cambiar parametros de la base de datos

- **grant create tablespace to Carlos:** Carlos puede crear tablespace

- **grant alter database tablespace to Carlos:**

- **revoke create user from Carlos:** Carlos no puede crear usuarios

- **grant create session to piero:** Permite que piero acceda a la BD

- **alter user piero quota 50M on USERS:** Permite escribir hasta 50M

- **alter user piero quota unlimited on USERS:** Permite escribir ilimitado hasta el tamaño del tablespace

- **select privilege from dba_sys_privs where GRANTEE='FRANCO':** Ver los permisos de franco

- **grant create user to piero with admin option:** Con el comando 'with admin option' permite que propagen el permiso

- **grant create user to piero with grant option:** Con el comando 'with grant option'

- **grant select any dictionary to ulima:** Permitir entrar al diccionario de datos (entrar a vistas v$)

 
## Roles

- **create role ROL_ALUMNO:** Crear rol

- **grant create session to ROL_ALUMNO:** El rol tiene el derecho de poder conectarse

- **grant create table to ROL_ALUMNO:** 

- **grant ROL_ALUMNO to franco:** Se le asigna un rol al usuario franco

- **create any table to ROL_PROFILE:** Hereda el rol y se agrega mas opciones, 

- **drop role ROL_ALUMNO:** Borra un rol

- **grant RESOURCE to franco:** Se le asigna el rol RESOURCE que es un rol estandar para un developer

- **select GRANTED_ROLE from dba_role_privs where GRANTEE='Franco':** Muestra los roles que tiene asignado el usuario franco

- **select * from session_privs:** Ver los privilegios garantizados y activado en la sesion actual

- **create role ROL_TEST identified by miclave:** Agregar una clave para tener los permisos

- **set role ROL_TEST identified by miclave:** Asignar el permiso mediante la clave


## Profile

- alter user ULIMA profile PERFIL_ULIMA: Asignar un perfil al usuario ULIMA

- sysbackup: Es un privilegio para poder sacar backup

## Recuperacion

- control_files='/u02/oradata/PRD/control01.ctl','/u02/oradata/PRD/control02.ctl','/u03/oradata/PRD/control103.ctl' scope=spfile: Multiplexacion

- Grupos de Redo log: Inactivo --> Actual --> Activo

- ¿Cuando sucede un switch de redo log?: El redo log buffer baja al redo log file, sucede cada 3 segundos o por cada commit

![Captura de pantalla de 2019-06-29 19-26-48](https://user-images.githubusercontent.com/31213239/60390794-366df280-9aa4-11e9-9825-918fff970a63.png)

- alter system switch logfile: Forzar un switch

- select GROUP#, SEQUENCE#, BYTES, STATUS from v$log: Ver cual grupo esta activo, actual e inactivo

- alter database add logfile group 4 ('/u02/oradata/PRD/redo4a.log','/u02/oradata/PRD/redo4b.log') size 200M : Creacion de un nuevo miembro

- 



## Parametros


- **sec_case_sensitive_logon:** Alter system set sec_case_sensitive_logon = FALSE; : Para que al loguearse no importe si es mayuscula o minuscula

- **show parameter deferred_segment_creation:**

- spfile: Si este paramtero tiene un valor significa que la isntancia levanto con spfile y si no tiene valor (si sale vacio) inicio con pfile

- processes: Muestra la cantidad de conexiones concurrentes, los background process cuentan como conexiones concurrentes

- memory_target: En este parametro se configura la memoria de la instancia

- db_block_size: En este parametro se configura el tamaño del datablock, esta expresado en bytes

- audit_file_dest: En este parametro se modifica la ruta del archivo de auditoria

- undo_tablespace: En este parametro se coloca el nombre del tablespace undo activo (ejemplo: alter system set undo_tablespace='MIUNDO')

- undo_retention: En este parametro se configura el tipo de gestion que tendra el tablespace undo, puede ser manual o auto. Solo para tecnicas muy avanzadas de configuracion se utiliza el 'manual'

- temp_undo_enabled: A partir de la version 12c sus valores son TRUE o FALSE. Si su valor es TRUE no genera redolog y permite una mejora en el uso del tablespace UNDO

- ddl_lock_timeout: Se configura el tiempo de espera para una sesion en espera, manda error ORA-00054

- db_create_file_dest: En este parametro se configura la ruta especifica de los datafile para OMF

- db_32k_cache_size: Sirve para habilitar el buffer cache para block

- dispatchers:

- shared_servers: Especifica la cantidad de server process que desea crear cuando cuando se inicia una instancia. Se debe tener cuidado de no establecer SHARED_SERVERS demasiado alto al inicio del sistema

- os_authent_prefix: Permite visualizar el prefijo que se le debe colocar al usuario external. Su valor es "ops$"

- remote_os_authent: De la version 10g en adelante permite que un usuario external se conecte al sqlplus localmente, es decir, que debe estar en el S.O para poder conectarse y asi no puedan conectarse desde otros terminales. Debe estar en FALSE (valor por default)

- sec_case_sensitive_logon: A partir de la version 11g en adelante es TRUE por default, permite case sensitive en las contraseñas

- deferred_segment_creation: Su valor por default es TRUE, significa que al momento de crear la tabla, esta se crea pero no crea el segmento, es decir, Oracle crea las tablas de manera diferida, recien crea el segmento cuando se inserta el primer registro en la tabla

- resource_limit: Desde la version 11g su valor por default es TRUE, significa que las politicas de recursos se aplicaran

- control_files: Muestra el listado de los control file

- log_buffer: Muestra cuanto ocupa el redo log buffer

Son 5 canales para multiplexar los redolog, aca se configuran las rutas por default para los redolog en OMF

- db_create_online_log_dest_1

- db_create_online_log_dest_2

- db_create_online_log_dest_3

- db_create_online_log_dest_4

- db_create_online_log_dest_5

- fast_start_mttr_target: Parametro para configurar el tiempo de demora del instance recovery, sus valores van de 0 a 3600 segundos

- log_checkpoint_timeout: Tiempo (en segundos) o rango para establecer cada cuanto ocurre un checkpoint

- db_recovery_file_dest: En este parametro se configura la ruta especifica para el FRA en OMF

- db_recovery_file_dest_size: En este parametro se configura la ruta especifica para el FRA en OMF

- log_archive_dest: Ruta donde manda los archivelog, se puede tener gasta 31 copias de los archivelog, 1 para el FRA y 30 para 

- log_archive_format: 

- log_archive_max_processes: Cantidad de procesos archive, esto depende de la cantidad de grupos en el redo

- control_file_record_keep_time: Tiempo (en dias) que el control file va a mantener la informacion de los backups para que el RMAN los pueda ubicar, lo recomendable es que este en 30 dias

- pga_aggregate_target: A partir de la version 9i permite poder configurar la capacidad para el PGA y este gestione de manera automatica el tamaño de sus componentes

. sga_target: A partir de la version 10g permite poder configurar la capacidad para el SGA y este gestione de manera automatica el tamaño de sus componentes. No modifica los pool: redolog buffer, keep pool, recycle pool y buffer(N), aca interviene el MMAN

- memory_target: A partir de la version 11g permite controlar y gestionar el tamaño del PGA y el SGA. Este parametro no es compatible con Huge Pages

- db_file_multiblock_read_count: Permite traer n cantidad de datablocks en un barrido de lectura, es decir, si su valor es 89, trae 89 datablocks

- db_keep_cache_size: Permite establecer el tamaño del keep pool

- db_recycle_cache_size: Permite establecer el tamaño del recycle pool

- filesystemio_options: Tiene 3 valores, async, none, setall (async: por lo bajo hace la grabacion de un insert pesado a disco y muestra el insert realizado al usuario, esto mejora el performance) (setall: hace escritura asincrona y bypasea el cache del filesystem (buffer cache del S.O)). Este parametro no se puede modificar en caliente (scope=spfile)

- session_cached_cursors: Su valor por default es 50 a partir de la version 11g, significa que va a mantener hasta 50 cursores para que encuentre rapudo el plan de ejecucion, lo recomendable es tenerlo entre 200 a 500, para poner el parametro correcto se tiene que ver el porcentaje de utilizacion del parametro session_cached_cursors, si esta en 100% de utilizacion, se tiene que aumentar su valor

- cursor_sharing: Puede tener 3 valores, EXACT, SIMILAR Y FORCE (exact: significa que se hace un plan de ejecucion por cada sentencia asi solo cambie el valor un campo, por ejemplo si en el where se busca otro id, ya es una consulta distinta), (similar: omite los literal value) (force: Mantiene un plan de ejecucion de por vida, no hay mejoras ni por estadistica). Es recomendable que la aplicacion no tenga autocommit

- statistics_level: Puede tener 3 valores: BASIC, TYPICAL y ALL (basic: oracle no trabaja las estadisticas que ejecuta por lo bajo, no se podria ver el estado de la base de datos) (ALL: hace una captura excesiva a la base de datos para poder tener mayor informacion, se usa cuando no se logra encontrar un problema en el AWR y es necesario informacion mas especifica)














