CREATE TABLE IF NOT EXISTS users
(
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255),
    firstname VARCHAR(255) ,
    lastname VARCHAR(255) ,
    password VARCHAR(255),
    role VARCHAR(255) 
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


Method : POST
URL : localhost:8080/api/v1/auth/login
body : 
{
    "email": "pradeeptest@gmail.com",
    "password": "Pass@1234"
}
