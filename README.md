# 🏟️ Proyecto: Integración de Microservicio de Reservas

Este documento detalla la implementación de la comunicación entre el microservicio de **Reservas** y el de **Canchas** para cumplir con los requisitos de la evaluación.

---

## 🛠️ 1. Cosas que Agregué (Nuevos Componentes)

Para habilitar la comunicación, se crearon los siguientes archivos desde cero:

1.  **`client/CanchaClient.java`**: 
    * **Función:** Interfaz que actúa como cliente Feign. Define el "contrato" para consumir el endpoint del microservicio de Canchas (`/api/canchas/{id}`).
    * **Tecnología:** `@FeignClient` de Spring Cloud.

2.  **`model/dto/CanchaDTO.java`**: 
    * **Función:** Objeto de transferencia de datos (DTO). Sirve para recibir y transformar la respuesta JSON que nos envía el servicio de Canchas en un objeto Java utilizable.
    * **Atributos:** `id`, `nombre`, `tipo`.

---

## 🔄 2. Cosas que Modifiqué (Cambios en Código Existente)

Se realizaron cambios en la estructura base para integrar la nueva funcionalidad:

1.  **`pom.xml`**:
    * Se agregó la dependencia `spring-cloud-starter-openfeign`.
    * Se incluyó la sección `dependencyManagement` para gestionar las versiones de Spring Cloud.

2.  **`ServicioreservasApplication.java`**:
    * Se agregó la anotación `@EnableFeignClients` sobre la clase principal para activar el escaneo de clientes Feign al arrancar la aplicación.

3.  **`ReservaServicesImpl.java`**:
    * **Inyección:** Se añadió `@Autowired private CanchaClient canchaClient`.
    * **Lógica de `crearReserva`:** Modifiqué este método para que, antes de hacer el `reservaRepository.save()`, realice una llamada al `canchaClient`. Si la cancha no existe, lanza una excepción y detiene la reserva.

---

## 📡 3a. Descripción de la Comunicación Sincrónica

Se implementó una **comunicación sincrónica** mediante **OpenFeign**. Se eligió este modelo porque la reserva depende obligatoriamente de la existencia de la cancha (regla de negocio crítica). Al ser sincrónica, el servicio de Reservas espera la respuesta del servicio de Canchas en tiempo real para validar la transacción antes de confirmar el guardado en la base de datos MySQL.

---

## ⚙️ 4. Requisitos de Ejecución
* **Base de Datos:** MySQL activa (XAMPP) con la DB `club_deportivo`.
* **Puertos:** * Servicio Canchas: `8081`
    * Servicio Reservas: `8082`

---
💡 *Tarea completada siguiendo
