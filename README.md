## Aprendí
- Definir infraestructura como código en un solo template.yaml.
- Cómo SAM genera roles IAM, permisos y stage a partir de pocas líneas.
- Dar permisos mínimos con policy templates (Read vs Crud).
- El ciclo sam build → sam deploy → probar → logs → sam delete.
- Resolví un error del template: Cors en Globals.Api no funciona con una
  API explícita, así que lo moví dentro del recurso TodosApi.
Captura de los Outputs de sam deploy
 
<img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/e3e3115e-e4ad-4f94-85a6-706ed4e51e97" />
Capturas de las 5 operaciones CRUD respondiendo correctamente.
1.Captura CREATE

 <img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/e3e3115e-e4ad-4f94-85a6-706ed4e51e97" />



2.Captura  LIST

 <img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/fe9e7970-ed11-443b-919a-bb44c021f903" />

3.Captura GET

 <img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/a0699787-7971-472b-9741-c9631b8e04e1" />





4.Captura UPDATE

 <img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/e87c8cc2-2a6d-4016-b8a0-f5174a651a13" />


5.Captura DELETE

 <img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/45a88736-530c-4943-b2d4-7400992309fd" />




Captura Error 400, 404.

 <img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/aa903770-2bf0-4368-b6f1-2fc662c6e1f1" />


Captura LOGS

 <img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/3a520506-f071-451b-9ba5-2826062a7905" />

 Captura sam DELETE completo
 <img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/5a32d488-4d39-4094-945b-34699ef952b8" />





Captura sam DELETE completo
<img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/a7654e19-4c84-4e0b-b7c3-64373eb8ff64" />

# Parte 2: Microservicios con SNS y SQS
# Lo que aprendí
evolucionar una API de un modelo síncrono a una **arquitectura orientada a eventos (EDA) desacoplada y resiliente** mediante el patrón *fan-out* con AWS SNS y SQS. Comprobaste en la práctica cómo aislar fallos, filtrar eventos por atributos y absorber picos de tráfico con colas de buffer, al tiempo que dominaste técnicas críticas de producción como la **idempotencia** en el consumidor, el manejo de mensajes fallidos con **DLQ y redrive**, la resolución de errores en plantillas de **AWS SAM**, y la identificación de riesgos reales como la **doble escritura no atómica** y la necesidad del patrón *outbox*.
# Respuestas
1. **SNS vs SQS:** SNS publica un mensaje a varios suscriptores (fan-out); SQS es una cola que guarda mensajes para un consumidor. Se usan juntos para que un evento llegue a varios servicios, cada uno con su buffer, reintentos y DLQ.
2. **Servicio de estadísticas:** ningún archivo del CRUD; se agrega una cola suscrita al topic y una Lambda nueva, porque los productores no conocen a los consumidores.
3. **Lote con 1 fallo:** con `batchItemFailures` solo ese mensaje se reintenta; si el handler lanza una excepción, vuelve el lote completo, incluidos los que sí se procesaron.
4. **sk con eventId:** hace la clave determinista (idempotencia: reprocesar sobrescribe en vez de duplicar) y evita colisiones entre eventos con la misma hora.
5. **VisibilityTimeout > Timeout:** si la cola liberara el mensaje antes de que la Lambda termine, otro consumidor lo procesaría en paralelo.
6. **#fail:** demuestra el aislamiento de fallos: cada consumidor tiene su propia cola.
7. **Doble escritura:** si SNS falla tras escribir en DynamoDB, el TODO existe pero el evento se pierde. Se mitiga con el patrón outbox (DynamoDB Streams).

Logs Audit

<img width="589" height="182" alt="image" src="https://github.com/user-attachments/assets/834fdca4-9175-46b4-8c50-4c66e16dbe56" />


Logs Notify

<img width="589" height="146" alt="image" src="https://github.com/user-attachments/assets/09426607-8f28-42f3-bb43-41e460d530af" />

lista con 3 eventos (todo.created, todo.updated, todo.completed).

<img width="589" height="175" alt="image" src="https://github.com/user-attachments/assets/706a6c9f-2f49-4c02-aefd-cbedc41d8f89" />

Borrar y comprobar que el historial sobrevive.

<img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/8164281b-e648-4685-86f3-a250de0a2ed0" />

Mirar la cola

<img width="589" height="209" alt="image" src="https://github.com/user-attachments/assets/d184b56d-330b-4f4f-913a-26bdeb4fc81a" />

Fallos simulados

<img width="589" height="195" alt="image" src="https://github.com/user-attachments/assets/1f1dcc84-2535-4b55-bd54-9cc2cb922c9a" />

Arreglo de bug

<img width="589" height="289" alt="image" src="https://github.com/user-attachments/assets/2097cbcc-18fe-46bc-b4be-ecd78a6c239e" />

La salida de list-message-move-tasks con COMPLETED.

<img width="589" height="276" alt="image" src="https://github.com/user-attachments/assets/cb3023c1-87e7-4354-ae27-618c846e6ff3" />

El log [NOTIFICACION] en la pestaña de Notify.

<img width="589" height="186" alt="image" src="https://github.com/user-attachments/assets/36223e14-8b84-484b-b850-df62ff3ea576" />


La DLQ de vuelta en 0.

<img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/b8381f13-4f64-4716-befb-c2b0deb4b627" />



