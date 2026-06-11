# Ex No: 04 – Spring Boot with REST API and Hibernate Integration

### Name: Shaik Eesub
### Register Number: 2305002021
### Reg No: 2305002001

## AIM

To develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it through REST APIs.

---

## ALGORITHM

1. Create a Spring Boot project using Maven.
2. Add the required dependencies:

   * Spring Web
   * Spring Data JPA
   * H2 Database (or MySQL)
3. Configure `application.properties` with database connection and JPA settings.
4. Create a `Movie` entity with fields such as `id`, `title`, `genre`, `releaseYear`, and `rating`.
5. Create a `MovieRepository` interface by extending `JpaRepository`.
6. Create a `MovieController` class to define REST API endpoints.
7. Implement CRUD operations:

   * GET `/movies`
   * GET `/movies/{id}`
   * POST `/movies`
   * PUT `/movies/{id}`
   * DELETE `/movies/{id}`
8. Run the Spring Boot application.
9. Test the APIs using Postman or a web browser.
10. Verify the results.

---

## PROJECT STRUCTURE

```text
movie/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/movies/
│   │   │       ├── controller/
│   │   │       │   └── MovieController.java
│   │   │       ├── model/
│   │   │       │   └── Movie.java
│   │   │       ├── repository/
│   │   │       │   └── MovieRepository.java
│   │   │       └── MovieApplication.java
│   │   └── resources/
│   │       └── application.properties
├── pom.xml
```

---

## PROGRAM

### pom.xml

```xml
<dependencies>

    <!-- Spring Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Spring Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webmvc</artifactId>
    </dependency>

    <!-- H2 Database -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>

</dependencies>
```

### application.properties

```properties
spring.application.name=movie

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

### Movie.java

```java
package com.example.movies.model;

import jakarta.persistence.*;

@Entity
public class Movie {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String genre;

    @Column(name = "release_year")
    private int releaseYear;

    private double rating;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getGenre() {
        return genre;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public int getReleaseYear() {
        return releaseYear;
    }

    public void setReleaseYear(int releaseYear) {
        this.releaseYear = releaseYear;
    }

    public double getRating() {
        return rating;
    }

    public void setRating(double rating) {
        this.rating = rating;
    }
}
```

### MovieRepository.java

```java
package com.example.movies.repository;

import com.example.movies.model.Movie;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface MovieRepository extends JpaRepository<Movie, Long> {
}
```

### MovieController.java

```java
package com.example.movies.controller;

import com.example.movies.model.Movie;
import com.example.movies.repository.MovieRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/movies")
public class MovieController {

    @Autowired
    private MovieRepository repo;

    @GetMapping
    public List<Movie> getAllMovies() {
        return repo.findAll();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Movie> getMovieById(@PathVariable Long id) {
        return repo.findById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public Movie addMovie(@RequestBody Movie movie) {
        return repo.save(movie);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Movie> updateMovie(@PathVariable Long id,
                                             @RequestBody Movie movieDetails) {

        return repo.findById(id).map(movie -> {
            movie.setTitle(movieDetails.getTitle());
            movie.setGenre(movieDetails.getGenre());
            movie.setReleaseYear(movieDetails.getReleaseYear());
            movie.setRating(movieDetails.getRating());
            return ResponseEntity.ok(repo.save(movie));
        }).orElse(ResponseEntity.notFound().build());
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Object> deleteMovie(@PathVariable Long id) {
        return repo.findById(id).map(movie -> {
            repo.delete(movie);
            return ResponseEntity.ok().build();
        }).orElse(ResponseEntity.notFound().build());
    }
}
```

---

## OUTPUT

### POST /movies


<img width="706" height="451" alt="image" src="https://github.com/user-attachments/assets/f9bf6852-0754-44c2-ad2d-dfba302605fe" />

## GET /movies
<img width="702" height="446" alt="image" src="https://github.com/user-attachments/assets/aff7af3c-7366-4d00-9cae-2c4a8873f5b3" />

## PUT /movies/{id}
<img width="703" height="451" alt="image" src="https://github.com/user-attachments/assets/7510b07b-a3f2-4757-a243-4a9311cf4e57" />

## DELETE /movies/{id}
<img width="707" height="440" alt="image" src="https://github.com/user-attachments/assets/706c7904-eff6-441a-b317-df0ef6c5241f" />
---

## RESULT

Thus, a Spring Boot application to store and retrieve data from a Movies database using Hibernate ORM and REST APIs was developed and executed successfully.
