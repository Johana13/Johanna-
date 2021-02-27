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





AutoPublishAlias: Al añadir esta propiedad y especificar un nombre de alias, AWS SAM:

Detecta cuándo se está implementando nuevo código, en función de los cambios en el Lambda función Amazon S3 del URI.

Crea y publica una versión actualizada de esa función con el código más reciente.

Crea un alias con un nombre que proporcione (a menos que ya exista un alias) y apunta a la versión actualizada de la Lambda función. Las invocaciones de la función deben utilizar el cualificador de alias para poder usar esta característica. Si no está familiarizado con Lambda del control de versiones de la función y los alias, consulte AWS Lambda Control de versiones de funciones y alias .



Tipo de preferencia de implementación: En el ejemplo anterior, el 10 por ciento del tráfico del cliente se desvía inmediatamente a la nueva versión. Después de 10 minutos, todo el tráfico se desvía a la nueva versión. Sin embargo, si su gancho previo/gancho posterior las pruebas fallan, o si un CloudWatch la alarma se activa, CodeDeploy restaura la implementación. En la siguiente tabla se describen otras opciones de desplazamiento de tráfico que están disponibles más allá de las utilizadas anteriormente. Tenga en cuenta lo siguiente:

Canario: El tráfico se desvía en dos incrementos. Puede elegir entre opciones de valor controlado predefinidas. Las opciones especifican el porcentaje de tráfico que se desvía a su Lambda versión de la función en el primer incremento y el intervalo, en minutos, antes de que el tráfico restante se desvíe en el segundo incremento.

Lineal: El tráfico se desvía en incrementos iguales con el mismo número de minutos entre incrementos. Puede elegir entre opciones lineales predefinidas que especifiquen el porcentaje de tráfico desviado en cada incremento y el número de minutos entre cada incremento.

Todo a la vez: Todo el tráfico se desvía desde el original Lambda función a la función actualizada Lambda versión de la función a la vez.
