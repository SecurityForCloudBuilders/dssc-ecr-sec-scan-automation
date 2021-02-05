# Utilizando um aplicativo Serverless para automatizar Scan de Imagens no ECR

O aplicativo está disponível no AWS Serverless Application Repository

# Sobre a ferramenta de Scan de Imagens

O Smart Check realiza varreduras pré-tempo de execução de imagens Docker ™, permitindo que você corrija os problemas antes que cheguem ao ambiente de orquestração (por exemplo, Kubernetes®).

O Deep Security Smart Check oferece a capacidade de: 
  * Detectar vulnerabilidades no nível do sistema operacional e no nível do aplicativo
  * Detectar malware, detectar segredos e chaves incorporadas em seus aplicativos
  * Realizar consultas de varredura personalizadas para encontrar arquivos suspeitos ou indesejados, verificar o conteúdo da imagem em uma lista de verificação de conformidade que inclui itens do PCI -DSS, HIPAA e NIST 800-190.
  
Mais informações em https://github.com/deep-security/smartcheck-helm

Muitas equipes de devops trabalham em silos e muitas vezes essas equipes enviam imagens para AWS ECR sem pipeline de CI/CD. Isso leva a várias imagens não digitalizadas e cria uma lacuna de feedback entre a segurança e os desenvolvedores. Este aplicativo sem servidor ajudará o cliente a criar uma varredura baseada em eventos ECR da AWS.

Estamos usando o monitoramento de eventos AWS ECR Push usando eventos cloudwatch e o mesmo evento cloudwatch aciona a função lambda para iniciar a varredura. Essa automação de segurança fornece capacidade de reduzir a sobrecarga de varredura programada.

![alt text](https://github.com/tsheth/dssc-ecr-sec-scan-automation/blob/master/dssc_blob/DSSC_automation.png)

Este aplicativo pode ser implantado a partir do repositório de aplicativos sem servidor da AWS.

![alt text](https://github.com/tsheth/dssc-ecr-sec-scan-automation/blob/master/dssc_blob/AWS_SAR.png)

Após a implantação do aplicativo, você pode adicionar a verificação inteligente de segurança profunda e as credenciais ECR da AWS como variáveis de ambiente.
```
AWS_ECR_ACCESS: <AWS_ACCESS_KEY_FOR_ECR_ACCESS>
AWS_ECR_SECRET: <AWS_SECRET_KEY_FOR_ECR_ACCESS>
DSSC_PSW: <DSSC_USER_PASSWORD>
DSSC_URL: <DSSC_URL>
DSSC_USER: <DSSC_USER_FOR_ECR_AUTOMATION>
```

É altamente recomendável usar o AWS KMS para criptografar todas as variáveis de ambiente armazenadas em texto não criptografado. Lembre-se de usar o acesso de privilégios mínimos para acesso AWS e chaves secretas. Para obter mais detalhes sobre como proteger os segredos lambda da AWS. Mais informações em `https://docs.aws.amazon.com/lambda/latest/dg/env_variables.html`

Também recomendamos que você use o usuário separado no DSSC para essa automação. Para maior eficiência, encorajamos você a usar nosso aplicativo de notificação de folga para notificação de varredura se houver alguma vulnerabilidade detectada pela segurança profunda da Trend Micro.
