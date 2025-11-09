
# 🐾 Proyecto PetClinic Integration Test - Resumen Completo

## 📘 Descripción General del Proyecto

Este proyecto es una implementación educativa basada en **Spring Boot (v3.5.6)** y **Java 17**, inspirada en el clásico ejemplo **Spring PetClinic**.
Su propósito principal es **demostrar cómo realizar pruebas unitarias e integradas** en un entorno profesional usando **Maven**, **Jenkins** y una base de datos **H2** o **MySQL**.

El proyecto incluye controladores, servicios, repositorios y pruebas para la entidad `Pet`, y se espera extenderlo para otras entidades: `Vet`, `Owner`, `Specialty`, `Visit`, `Types`, `VetSpecialty`.

---

## ⚙️ Tecnologías Clave

| Tecnología | Versión | Propósito |
|-------------|----------|------------|
| Java | 17 | Lenguaje principal |
| Spring Boot | 3.5.6 | Framework base |
| Maven | Wrapper incluido | Build & gestión de dependencias |
| H2 | Integración/Tests | Base de datos en memoria |
| MySQL | Producción | Base de datos persistente |
| Lombok | — | Eliminación de boilerplate |
| MapStruct | ${org.mapstruct.version} | Mapeo DTO ↔ Entity |
| Rest-Assured / MockMvc | — | Pruebas REST |
| JaCoCo | 0.8.13 | Reporte de cobertura de código |
| Jenkins | — | CI/CD pipeline |

---

## 🗃️ Estructura de Base de Datos

La base de datos se define en los scripts:

- `schema-mysql.sql`
- `data-mysql.sql`

### Tablas Principales

| Entidad | Propósito |
|----------|------------|
| `vets` | Veterinarios |
| `specialties` | Especialidades |
| `vet_specialties` | Relación N:M entre veterinarios y especialidades |
| `types` | Tipos de mascotas |
| `owners` | Dueños |
| `pets` | Mascotas |
| `visits` | Visitas veterinarias |

### Relaciones Importantes
- `owners (1) — (N) pets`
- `pets (N) — (1) types`
- `vets (N) — (N) specialties` (por `vet_specialties`)
- `pets (1) — (N) visits`

---

## 🔗 Perfiles y Configuración

- **Perfil activo:** `h2` (definido en `application.yml`)
- **Cambio a MySQL:** usando `-Dspring.profiles.active=mysql`
- **Ejecución de pruebas:**
  ```bash
  mvn clean test -Dspring.profiles.active=h2
  ```
- **Ejecución de aplicación:**
  ```bash
  mvn spring-boot:run -Dspring-boot.run.profiles=h2
  ```

---

## 🧩 Arquitectura por Capas

### 1. **Controller Layer**
- Ejemplo: `PetController.java`
- Anotaciones `@RestController`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`.
- Endpoints `/pets` manejan CRUD completo.

### 2. **Service Layer**
- Ejemplo: `PetService.java`
- Expone métodos como `findById`, `findByName`, `create`, `update`, `delete`.
- Maneja excepciones como `PetNotFoundException`.

### 3. **Repository Layer**
- Ejemplo: `PetRepository.java`
- Extiende `JpaRepository<Pet, Integer>`.
- Consultas personalizadas (`findByName`, `findByTypeId`, `findByOwnerId`).

### 4. **Mapper Layer**
- `PetMapper.java` convierte entre `Pet` y `PetDTO` usando MapStruct.

### 5. **DTO Layer**
- `PetDTO.java` representa la estructura JSON de la entidad para la API.

---

## 🧪 Pruebas Existentes

### 🧩 `PetServiceTest`
- Pruebas integradas sobre la base de datos H2 real.
- Verifica CRUD completo de `Pet`.

### 🧩 `PetServiceMockitoTest`
- Pruebas unitarias usando `@MockitoBean` para simular repositorios.

### 🧩 `PetControllerTest` y `PetControllerMockitoTest`
- Pruebas de endpoints REST (`/pets`) con `MockMvc`.
- Validan las respuestas HTTP, formato JSON y persistencia.

### 🧩 `VetServiceTest`, `OwnerServiceTest`
- Están vacíos (plantillas). Deben completarse.

---

## 🧱 Ejercicios Solicitados

### 6. Ejercicio 1: Pruebas de Integración para `Vet`
**Archivos esperados:**
- `VetControllerTest.java`
- `VetServiceTest.java` (actualizar si está vacío)
- Commit en Git con URL visible
- Captura de pantalla de GitHub (evidencias de push y tests)

**Debe incluir:**
- Endpoints `/vets`, `/vets/{id}`
- Casos: `findAll`, `findById`, `create`, `update`, `delete`

---

### 7. Ejercicio 2: Pruebas de Integración para `Owner`
**Archivos esperados:**
- `OwnerControllerTest.java`
- CRUD completo
- Validar integridad con base H2

---

### 8. Ejercicio 3: Pruebas de Integración para `Specialty`
**Archivos esperados:**
- `SpecialtyControllerTest.java`
- Casos `GET /specialties`, `POST /specialties`, `PUT /specialties/{id}`, `DELETE /specialties/{id}`

---

### 9. Ejercicio 4: Pruebas de Integración para `Visit`
**Archivos esperados:**
- `VisitControllerTest.java`
- Validar relación con `Pet` y `Vet`
- Casos: `findAll`, `findByPetId`, `create`, `delete`

---

### 10. Ejercicio 5: Pruebas de Integración para `Types`
**Archivos esperados:**
- `TypesControllerTest.java`
- CRUD básico
- Validar datos preexistentes en `types`

---

### 11. Ejercicio 6: Pruebas de Integración para `VetSpecialties`
**Archivos esperados:**
- `VetSpecialtyControllerTest.java`
- Validar relaciones N:M (`Vet` ↔ `Specialty`)
- Casos: `findByVetId`, `create`, `delete`

---

## 🚫 Qué **NO** debes hacer

- No modificar `pom.xml` ni eliminar dependencias existentes.  
- No alterar los scripts SQL originales (`schema.sql`, `data.sql`, `schema-mysql.sql`, `data-mysql.sql`).  
- No cambiar el paquete base `com.tecsup.petclinic`.  
- No hacer push sin pruebas locales exitosas (`mvn test`).  
- No subir screenshots falsas o sin commit en GitHub.

---

## ✅ Qué **SÍ** debes hacer

1. **Clonar o actualizar tu fork del repositorio.**
2. Crear una rama por ejercicio (ejemplo: `feature/vet-integration-tests`).
3. Implementar el controlador y los tests correspondientes.
4. Ejecutar `mvn clean test` y guardar la evidencia del resultado.
5. Subir tus cambios (`git push origin feature/vet-integration-tests`).
6. Crear PR y subir las capturas de pantalla a la entrega.

---

## 🧩 Ejemplo de Evidencia (VetControllerTest)

```java
@Test
public void testFindAllVets() throws Exception {
    this.mockMvc.perform(get("/vets"))
        .andExpect(status().isOk())
        .andExpect(content().contentType(MediaType.APPLICATION_JSON_VALUE))
        .andExpect(jsonPath("$[0].firstName", is("James")));
}
```

---

## 🧱 Jenkinsfile

El pipeline de CI ejecuta automáticamente:
1. **Checkout del código**
2. **Compilación (`mvn compile`)**
3. **Pruebas (`mvn test`)**
4. **Empaquetado (`mvn package -DskipTests`)**

---

## 📄 Conclusión

Este proyecto sirve como **base para prácticas de pruebas integradas**.  
Tu objetivo será **replicar la estructura de PetControllerTest** para cada una de las entidades restantes, asegurando la correcta interacción con la base H2 y cubriendo todo el CRUD.

