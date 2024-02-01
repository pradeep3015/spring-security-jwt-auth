2024-02-01 10:55:47.385  WARN 25176 --- [  restartedMain] o.h.t.s.i.ExceptionHandlerLoggedImpl     : GenerationTarget encountered exception accepting command : Error executing DDL "create table _user (id integer not null auto_increment, email varchar(255), firstname varchar(255), lastname varchar(255), password varchar(255), role varchar(255), primary key (id)) type=MyISAM" via JDBC Statement

org.hibernate.tool.schema.spi.CommandAcceptanceException: Error executing DDL "create table _user (id integer not null auto_increment, email varchar(255), firstname varchar(255), lastname varchar(255), password varchar(255), role varchar(255), primary key (id)) type=MyISAM" via JDBC Statement
	at org.hibernate.tool.schema.internal.exec.GenerationTargetToDatabase.accept(GenerationTargetToDatabase.java:67) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.AbstractSchemaMigrator.applySqlString(AbstractSchemaMigrator.java:581) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.AbstractSchemaMigrator.applySqlStrings(AbstractSchemaMigrator.java:526) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.AbstractSchemaMigrator.createTable(AbstractSchemaMigrator.java:293) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.GroupedSchemaMigratorImpl.performTablesMigration(GroupedSchemaMigratorImpl.java:74) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]
	at org.hibernate.tool.schema.internal.AbstractSchemaMigrator.performMigration(AbstractSchemaMigrator.java:220) ~[hibernate-core-5.6.15.Final.jar:5.6.15.Final]


Table

package com.example.springjwt.user;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import javax.persistence.*;
import java.util.Collection;
import java.util.List;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@Entity
@Table(name = "_user")
public class User implements UserDetails {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Integer id;
    private String firstname;
    private String lastname;
    private String email;
    private String password;
    @Enumerated(EnumType.STRING)
    private Role role;

    public User() {
		super();
		// TODO Auto-generated constructor stub
	}

	public User(Integer id, String firstname, String lastname, String email, String password, Role role) {
		super();
		this.id = id;
		this.firstname = firstname;
		this.lastname = lastname;
		this.email = email;
		this.password = password;
		this.role = role;
	}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getFirstname() {
		return firstname;
	}

	public void setFirstname(String firstname) {
		this.firstname = firstname;
	}

	public String getLastname() {
		return lastname;
	}

	public void setLastname(String lastname) {
		this.lastname = lastname;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public Role getRole() {
		return role;
	}

	public void setRole(Role role) {
		this.role = role;
	}

	@Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return List.of(new SimpleGrantedAuthority(role.name()));
    }

    @Override
    public String getUsername() {
        return email;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
