2024-02-01 10:55:47.385  WARN 25176 --- [  restartedMain] o.h.t.s.i.ExceptionHandlerLoggedImpl     : GenerationTarget encountered exception accepting command : Error executing DDL "create table _user (id integer not null auto_increment, email varchar(255), firstname varchar(255), lastname varchar(255), password varchar(255), role varchar(255), primary key (id)) type=MyISAM" via JDBC Statement

org.hibernate.tool.schema.spi.CommandAcceptanceException: Error executing DDL "create table _user (id integer not null auto_increment, email varchar(255), firstname varchar(255), lastname varchar(255), password varchar(255), role varchar(255), primary key (id)) type=MyISAM" via JDBC Statement
	at org.hibernate.tool.schema.internal.exec.GenerationTargetToDatabase.accept(GenerationTargetToDatabase.java:67) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.AbstractSchemaMigrator.applySqlString(AbstractSchemaMigrator.java:581) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.AbstractSchemaMigrator.applySqlStrings(AbstractSchemaMigrator.java:526) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.AbstractSchemaMigrator.createTable(AbstractSchemaMigrator.java:293) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.GroupedSchemaMigratorImpl.performTablesMigration(GroupedSchemaMigratorImpl.java:74) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.AbstractSchemaMigrator.performMigration(AbstractSchemaMigrator.java:220) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
