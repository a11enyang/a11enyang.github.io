---
title: springboot+vuejs+boostrap+axios
date: 2020-04-26 19:20:41
tags:
	- springboot
	- vuejs
---

![image-20200430094219940](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200430094219940.png)

<!-- more -->

参考链接：https://dev.to/brunodrugowick/complete-api-in-5-minutes-with-spring-data-rest-aqap-series-1ie8

## 完成一个spring data 的restful api

### 明确资源

```java
@Entity @Data
class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String firstName;
    private String lastName;

    @ManyToOne
    @JoinColumn(name = "role_id")
    private Role role;
}
```



```java
@Entity @Data
class Role {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String name;
}
```



### 创建和导出数据库

这个部分属于dao层，抽象了对于数据库的操作：

```java
@RepositoryRestResource(collectionResourceRel = "users", itemResourceRel = "user", path = "users")
interface PersonRepository extends JpaRepository<User, Long> {}

@RepositoryRestResource(collectionResourceRel = "roles", itemResourceRel = "role", path = "roles")
interface RoleRepository extends JpaRepository<Role, Long> {}
```







## Spring Boot, Vue.js, Axios and Thymeleaf with Bootstrap

### 1 基础依赖

```
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>bootstrap</artifactId>
    <version>4.4.1</version>
</dependency>
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>webjars-locator</artifactId>
    <version>0.38</version>
</dependency>
```



### 2 DAO和View的设置

DAO

```
@Entity(name = "role")
@Data
public class Role {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String name;
}
```

```
@Entity(name = "user")
@Data
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String firstName;
    private String lastName;

    @ManyToOne
    @JoinColumn(name = "role_id")
    private Role role;
}
```

```
@Repository
public interface RoleRepository extends JpaRepository<Role, Long> {
}
```

```
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```



Controller

```
@Controller
public class MainController {

    @RequestMapping("/")
    public String index() {
        return "index";
    }
}
```



模板设置

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <link href='/webjars/bootstrap/css/bootstrap.min.css' rel='stylesheet'>

    <meta charset="UTF-8">
    <title>Home</title>
</head>
<body>

<div th:replace="fragments/header :: header"></div>

<script src="/webjars/jquery/jquery.min.js"></script>
<script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
</body>
</html>
```



```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <link href='/webjars/bootstrap/css/bootstrap.min.css' rel='stylesheet'>

    <meta charset="UTF-8">
    <title>Restaurants</title>
</head>
<body>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark" th:fragment="header">
    <a class="navbar-brand" href="/">Home</a>
    <button aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation"
            class="navbar-toggler" data-target="#navbarSupportedContent" data-toggle="collapse" type="button">
        <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
            <li class="nav-item dropdown">
                <a aria-expanded="false" aria-haspopup="true" class="nav-link dropdown-toggle" data-toggle="dropdown"
                   href="#" id="navbarDropdown" role="button">
                    Entities
                </a>
                <div aria-labelledby="navbarDropdown" class="dropdown-menu">
                    <a class="dropdown-item" href="/users">Users</a>
                    <a class="dropdown-item" href="/roles">Roles</a>
                    <div class="dropdown-divider"></div>
                    <div class="dropdown-item-text p-4 text-muted" style="max-width: 200px;">
                        <p>
                            Administrative pages to list, edit, create and remove entities.
                        </p>
                    </div>
                </div>
            </li>
        </ul>
        <form class="form-inline my-2 my-lg-0">
            <input aria-label="Search" class="form-control mr-sm-2" placeholder="Search" type="search">
            <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
        </form>
    </div>
</nav>

<script src="/webjars/jquery/jquery.min.js"></script>
<script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
</body>
</html>
```



### 3 The HTML templates for the entities

```
@Controller
public class RoleController {

    @GetMapping("/roles")
    public String rolesPage() {
        return "roles";
    }
}
```



```
@Controller
public class UserController {

    @GetMapping("/users")
    public String users() {
        return "users";
    }
}
```



roles.html

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <link href='/webjars/bootstrap/css/bootstrap.min.css' rel='stylesheet'>

    <meta charset="UTF-8">
    <title>Roles</title>
</head>
<body>

<div th:replace="fragments/header :: header"></div>

<br><br>

<div class="container" id="main">

    <table class="table table-striped table-bordered">
        <thead>
        <tr>
            <th>Role ID</th>
            <th>Role Name</th>
            <th>Actions</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td></td>
            <td></td>
            <td>
                <a>Edit</a>
                <a>Delete</a>
            </td>
        </tr>
        </tbody>
    </table>
</div>

<script src="/webjars/jquery/jquery.min.js"></script>
<script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
</body>
</html>
```



users.html

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <link href='/webjars/bootstrap/css/bootstrap.min.css' rel='stylesheet'>

    <meta charset="UTF-8">
    <title>Users</title>
</head>
<body>

<div th:replace="fragments/header :: header"></div>

<br><br>

<div class="container" id="main">

    <table class="table table-striped table-bordered">
        <thead>
        <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Role</th>
            <th>Actions</th>
        </tr>
        </thead>
        <tbody>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td>
                <a>Edit</a>
                <a>Delete</a>
            </td>
        </tr>
        </tbody>
    </table>
</div>

<script src="/webjars/jquery/jquery.min.js"></script>
<script src="/webjars/bootstrap/js/bootstrap.min.js"></script>
</body>
</html>
```





# The Vue magic

添加vue依赖，其实我觉得可以直接利用cdn进行引用

```
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>vue</artifactId>
    <version>2.6.11</version>
</dependency>
<dependency>
    <groupId>org.webjars.npm</groupId>
    <artifactId>axios</artifactId>
    <version>0.19.0</version>
</dependency>
```



```
@RestController
@RequestMapping("/api/v1")
public class RolesController {

    private final RoleRepository roleRepository;

    public RolesController(RoleRepository roleRepository) {
        this.roleRepository = roleRepository;
    }

    @GetMapping("roles")
    public List<Role> list() {
        return roleRepository.findAll();
    }
}
```



```
@RestController
@RequestMapping("/api/v1")
public class UsersController {

    private final UserRepository userRepository;

    public UsersController(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @GetMapping("/users")
    public List<User> list() {
        return userRepository.findAll();
    }
}
```



roles.html

```
<!-- unrelated code omitted for brevity -->
<tr v-for="role in roles">
    <td>{ { role.id } }</td>
    <td>{ { role.name } }</td>
<!-- unrelated code omitted for brevity -->
```

```
<!-- Vue.js imports -->
<script src="webjars/vue/vue.min.js"></script>
<script src="webjars/axios/dist/axios.min.js"></script>
<!-- Actual Vue.js script -->
<script>
    var app = new Vue({
        el: '#main',
        data() {
            return {
                roles: null
            }
        },
        mounted(){
            axios
                .get("/api/v1/roles")
                .then(response => (this.roles = response.data))
        },
    })
</script>
```





users.html

```
<!-- unrelated code omitted for brevity -->
<tr v-for="user in users">
    <td>{ { user.firstName }}</td>
    <td>{ { user.lastName }}</td>
    <td>{ { user.role.name }}</td>
<!-- unrelated code omitted for brevity -->
```

```
<!-- Vue.js imports -->
<script src="webjars/vue/vue.min.js"></script>
<script src="webjars/axios/dist/axios.min.js"></script>
<!-- Actual Vue.js script -->
<script>
    var app = new Vue({
        el: '#main',
        data() {
            return {
                users: null
            }
        },
        mounted(){
            axios
                .get("/api/v1/users")
                .then(response => (this.users = response.data))
        },
    })
</script>
```



总结：request请求Controller返回一个thymeleaf模板，在这个模板中由vuejs，vuejs中执行axios向RestController发出请求（这个过程是异步的），得到json字符串后，再在vuejs中进行处理，然后将处理结果返回到html文件中。



## Complete CRUD with Spring Boot, Vue.js, Axios

### Adding REST operations

```
@PostMapping("roles")
public Role save(@RequestBody Role role) {
    return roleRepository.save(role);
}

@DeleteMapping("roles/{id}")
public void get(@PathVariable Long id) {
    roleRepository.deleteById(id);
}
```



### Role Form

```html
<form v-on:submit.prevent="postRole">
    <div class="card mb-auto">
        <div aria-controls="roleForm" aria-expanded="false" class="card-header" data-target="#roleForm"
             data-toggle="collapse" id="formHeader" style="cursor: pointer">
            <div class="float-left">New/Edit Role</div>
            <div class="float-right">+</div>
        </div>
        <div class="card card-body collapse" id="roleForm">
            <div class="form-group row">
                <label for="roleName" class="col-sm-4 col-form-label">Role Name</label>
                <input id="roleId" type="hidden" v-model="role_id">
                <input id="roleName" class="form-control col-sm-8" placeholder="Role Name" type="text"
                           v-model="role_name"/>
            </div>
            <div class="form-group row">
                <div class="col col-sm-4"></div>
                <input class="btn btn-primary col col-sm-8" type="submit" value="Save">
            </div>
        </div>
    </div>
</form>
```



### New Edit and Delete buttons

```
<td>
    <button class="btn btn-primary" v-on:click="editRole(role)">Edit</button>
    <button class="btn btn-danger" v-on:click="deleteRole(role)">Delete</button>
</td>
```



### vue.js

```
<script>
    var app = new Vue({
        el: '#main',
        data() {
            return {
                roles: null,
                role_id: '',
                role_name: '',
            }
        },
        mounted(){
            this.getRoles();
        },
        methods: {
            getRoles: function () {
                axios
                    .get("/api/v1/roles")
                    .then(response => (this.roles = response.data))
            },
            postRole: function (event) {
                // Creating
                if (this.role_id == '' || this.role_id == null) {
                    axios
                        .post("/api/v1/roles", {
                            "name": this.role_name,
                        })
                        .then(savedRole => {
                            this.roles.push(savedRole.data);
                            this.role_name = '';
                            this.role_id = '';
                        })
                } else { // Updating
                    axios
                        .post("/api/v1/roles", {
                            "id": this.role_id,
                            "name": this.role_name,
                        })
                        .then(savedRole => {
                            this.getRoles();
                            this.role_name = '';
                            this.role_id = '';
                        })
                }
            },
            editRole: function (role) {
                this.role_id = role.id;
                this.role_name = role.name;
                document.getElementById('roleForm').setAttribute("class", document.getElementById('roleForm').getAttribute("class") + " show");
            },
            deleteRole: async function (role) {
                await axios
                    .delete("/api/v1/roles/" + role.id);
                this.getRoles();
            }
        },
    })
</script>
```



## The most basic security for Spring Boot with Thymeleaf

https://dev.to/brunodrugowick/the-most-basic-security-for-spring-boot-with-thymeleaf-339h

防止跨站攻击