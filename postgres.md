list users /du
list database /l

CREATE DATABASE notifierdb OWNER notifier;
create user notifier with encrypted password 'qefsdc';
create schema ufs_routing_bf1;
CREATE SCHEMA if not exists cci;

ALTER USER notifier WITH SUPERUSER;
ALTER DATABASE cci OWNER TO cci_appl;
alter user ufs_routing_bf1 VALID UNTIL 'INFINITY';

GRANT ALL PRIVILEGES ON DATABASE notifierdb TO notifier;
grant select, insert, delete, update on cci.idx_string_table to "cci_appl";
grant usage on schema ufs_routing_bf1 to ufs_routing_bf1;
grant all on schema ufs_routing_bf1 to ufs_routing_bf1;
grant connect on database postgres to ufs_routing_bf1;