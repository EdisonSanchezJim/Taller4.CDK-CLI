#  AWS CDK Challenge – Simple Web Application with Lambda

##  Arquitectura

<img width="538" height="304" alt="Captura de Pantalla 2025-11-17 a la(s) 10 01 12 p m" src="https://github.com/user-attachments/assets/747ca9d1-2e7e-4d72-b347-ef355874982d" />


### Componentes principales

| Componente                  | Descripción                                                                 |
|----------------------------|------------------------------------------------------------------------------|
| **Usuario**                | Cliente que realiza una petición HTTP (navegador, curl, Postman, etc.)       |
| **Lambda Function URL**    | Endpoint HTTP generado automáticamente por AWS para invocar la función Lambda |
| **AWS Lambda**             | Función sin servidor que ejecuta el código y responde con un mensaje         |
| **IAM Role (LabRole)**     | Rol que otorga permisos a la función Lambda para ejecutarse y escribir logs  |
| **AWS CDK (Java)**         | Framework que define y despliega toda la infraestructura como código         |

---

###  Flujo de ejecución

1. **El usuario** accede al endpoint HTTP generado por CDK (`FunctionUrl`) desde su navegador o herramienta de prueba.
2. La petición llega directamente a la **Lambda Function URL**, sin pasar por API Gateway.
3. AWS enruta la solicitud a la **función Lambda**, que ejecuta el código definido en `Code.fromInline(...)`.
4. La función responde con un mensaje JSON: `"Hello World!"`.
6. El URL de la función se expone como salida (`CfnOutput`) en el despliegue de CDK.

---

###  Seguridad y permisos

- La función Lambda utiliza un rol IAM (`LabRole`) que debe tener permisos para:
  - Ejecutar funciones Lambda y el stack
- El `FunctionUrl` se configura con `authType: NONE`, lo que significa que **cualquier usuario puede invocar la función** 
---

###  Infraestructura del código

- Todo se define en una clase Java (`HelloCdkStack`) usando el framework **AWS CDK**.
- El código genera automáticamente los recursos en AWS mediante CloudFormation.

---

##  Pasos realizados

### 1. Instalación de CDK-CLI
- Instalé CDK siguiendo la guía.
- Verifiqué la instalación con:
  ```
  cdk --version
  ```
### 2. Configuracion del entorno

- Configure las credencias de aws con:
  ```
  aws config
  ```
-obtuve mi id de cuenta y la zona de trabajo,
con estos datos parametrice mi clase HelloCDKApp.java
- Verifiqué la instalación con:
  ```
  cdk --version
  ```

### 3. Desarrollo de la aplicación
- Creé un nuevo proyecto CDK con:
  ```
  cdk init app --language java
  ```
- Implementé una **Lambda Function** que responde con un mensaje simple.
- Obtenermos el endpoint mediante **Lambda Function URLs** para exponer la Lambda vía HTTP con el constructor CfnOutput
  y asi poder obtener el Endpoint HTTP directo para la función Lambda.

### 4. Despliegue en AWS
Para poder subir mi stack a AWS y debido a las limitantes de los permisos
de la cuenta educativa con respecto a crear roles  en AIM, descargamos un template localmente en el proyecto
que nos ayudara a desplegar el stack. En el template comentamos los recursos y lineas
de codigo que contuvieran la palabra rol.
para ello 
- Ejecuté:
  ```
  cdk bootstrap --show-template > bootstrap-template.yaml
  ```
  y para poderlo desplegar ejecute:

  ```
  cdk bootstrap --template bootstrap-template.yaml
  ```

-Luego, genere una plantilla de CloudFormation a partir de la pila CDK. stack creada: HelloCDStack con:

  ```
  cdk synth
  ```

  Luego para desplegar la pila cdk ejecute:

  ```
  cdk deploy -r arn:aws:iam::029678987753:role/LabRole
  ```

- Resultado: se generó un endpoint público accesible desde el navegador o curl.

### 5. Evidencias
- **Creacion de la funcion lambda**
<img width="569" height="378" alt="Captura de Pantalla 2025-11-17 a la(s) 9 08 09 p m" src="https://github.com/user-attachments/assets/23555d73-1a3f-4362-b23e-944645ff497f" />

![Nueva nota](https://github.com/user-attachments/assets/7375accc-04e7-4e98-8fbf-39545a4cf6ae)


- **Creacion del stack en CloudFormation**
<img width="572" height="369" alt="Captura de Pantalla 2025-11-17 a la(s) 9 10 01 p m" src="https://github.com/user-attachments/assets/382c0747-5e3b-4e55-b083-a24957366a03" />

<img width="1440" height="871" alt="Captura de Pantalla 2025-11-17 a la(s) 9 02 08 p m" src="https://github.com/user-attachments/assets/ad1511eb-c91d-46a8-bc95-f08eec298b04" />


- **Creacion del endpoint**
<img width="572" height="371" alt="Captura de Pantalla 2025-11-17 a la(s) 9 15 37 p m" src="https://github.com/user-attachments/assets/228d29e3-bd23-4a75-8b0e-605bd081093c" />


- **Prueba del endpoint**
  <img width="1440" height="349" alt="Captura de Pantalla 2025-11-17 a la(s) 9 17 13 p m" src="https://github.com/user-attachments/assets/4010af29-f116-441c-9f37-3080780fc1c9" />

---


## Conclusiones
- CDK permite definir infraestructura en **lenguajes de programación comunes** (TypeScript en este caso).
- La integración con **CloudFormation** es automática: CDK genera las plantillas y las despliega.
- El flujo completo (CLI → CDK → GitHub) facilita la reproducibilidad y el versionado de la infraestructura.
- Este ejercicio demuestra cómo pasar de código a infraestructura funcional en minutos.

