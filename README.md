# Johanna-
Una aplicación sin servidor es una combinación de funciones, orígenes de eventos y otros recursos de Lambda que se combinan para realizar tareas. Tenga en cuenta que una aplicación sin servidor .

Implemetación 
con  AWS Serverless Application Model (SAM)  - Cloud watch

La implementación de AWS Serverless Application Model (SAM) ahora está disponible bajo la licencia Apache 2.0. AWS SAM amplía AWS CloudFormation para ofrecer una forma simplificada de definir los recursos que necesita su aplicación sin servidor. La implementación de SAM es el código que traduce las plantillas SAM a pilas de AWS CloudFormation. Hasta ahora, podía enviar solicitudes de características a la especificación SAM y AWS tenía que hacer las actualizaciones correspondientes a la implementación de SAM. A partir de ahora, puede aportar nuevas características y mejoras a todo el SAM. Puede bifurcar el repositorio SAM y proponer cambios a la implementación creando una solicitud de extracción.


![image](https://user-images.githubusercontent.com/79769549/109401224-6d507780-791b-11eb-99ca-21c9973e4f6b.png)

Ejemplo 


Resources:
 MyLambdaFunction:
   Type: AWS::Serverless::Function
   Properties:
     Handler: index.handler
     Runtime: nodejs12.x
     CodeUri: s3://bucket/code.zip

     AutoPublishAlias: live

     DeploymentPreference:
       Type: Canary10Percent10Minutes 
       Alarms:
         # A list of alarms that you want to monitor
         - !Ref AliasErrorMetricGreaterThanZeroAlarm
         - !Ref LatestVersionErrorMetricGreaterThanZeroAlarm
       Hooks:
         # Validation Lambda functions that are run before & after traffic shifting
         PreTraffic: !Ref PreTrafficLambdaFunction
         PostTraffic: !Ref PostTrafficLambdaFunction
