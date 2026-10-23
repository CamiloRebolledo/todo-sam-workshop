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
<img width="589" height="331" alt="image" src="https://github.com/user-attachments/assets/a7654e19-4c84-4e0b-b7c3-64373eb8ff64" />
