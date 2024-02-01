CREATE TABLE IF NOT EXISTS users
(
    id INT NOT NULL,
    email VARCHAR(255) COLLATE utf8mb4_general_ci,
    firstname VARCHAR(255) COLLATE utf8mb4_general_ci,
    lastname VARCHAR(255) COLLATE utf8mb4_general_ci,
    password VARCHAR(255) COLLATE utf8mb4_general_ci,
    role VARCHAR(255) COLLATE utf8mb4_general_ci,
    PRIMARY KEY (id)
);


and on User model make changes 

@Entity
@Table(name = "users")



Method : POST
URL : localhost:8080/api/v1/auth/register

body : 

{
    "firstname": "pradeep prajapati",
    "lastname": "pradeep prajapati",
    "email": "pradeeptest@gmail.com",
    "password": "Pass@1234"
}
