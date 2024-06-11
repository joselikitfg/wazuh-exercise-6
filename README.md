# wazuh-exercise-6


### infra/deploy_cf.yml
Este stack de CloudFormation crea una configuración de red básica para Wazuh con dos subredes públicas. A continuación se detallan los recursos creados por este stack y su configuración.

1. **VPC (Virtual Private Cloud)**:
   - CIDR: `10.0.0.0/27`
   - Habilita soporte para DNS y nombres de host.

2. **Internet Gateway**:
   - Proporciona acceso a internet para la VPC.

3. **Subredes Públicas**:
   - **Subred Pública 1**:
     - CIDR: `10.0.0.0/28`
     - Zona de disponibilidad: `eu-west-1a`
   - **Subred Pública 2**:
     - CIDR: `10.0.0.16/28`
     - Zona de disponibilidad: `eu-west-1b`

4. **Tabla de Rutas**:
   - Tabla de rutas pública asociada con ambas subredes públicas.
   - Ruta predeterminada (`0.0.0.0/0`) hacia el Internet Gateway.

5. **KeyPair**:
   - KeyPair generado dinámicamente para acceder a las instancias EC2.

6. **Roles e Instancias**:
   - **Role para la instancia**: Permite a la instancia EC2 interactuar con Amazon SSM.
   - **Perfil de instancia**: Asociado con el role de la instancia.
   - **Security Group**: Permite acceso SSH (`puerto 22`) desde cualquier IP.
   - **Instancia EC2**:
     - Tipo de instancia: `t2.medium`
     - AMI: `ami-00138b07206d4ceaf`
     - KeyPair: Utiliza el KeyPair generado.
     - Se despliega en la Subred Pública 1.


#### Despliegue

Se puede hacer manual con:

```bash
aws cloudformation create-stack --stack-name Exercise6 --template-body file://infra/cf_template.yml --region eu-west-1 --capabilities CAPABILITY_NAMED_IAM
```

O con la pipeline de GitHub Action

> [!NOTE]  
> Asegúrate de que el KeyPair generado esté almacenado de manera segura, ya que se utilizará para acceder a las instancias EC2.
> Este stack está configurado para desplegarse en la región eu-west-1. Si deseas utilizar otra región, modifica el comando de despliegue en consecuencia.

> [!NOTE]  
> Este stack está configurado para desplegarse en la región eu-west-1. Si deseas utilizar otra región, modifica el comando de despliegue en consecuencia.