# Anotações do Desafio

## Tema

Implementação da primeira stack com AWS CloudFormation.

## Conceitos Revisados

## Infrastructure as Code

Infrastructure as Code, ou IaC, é a prática de definir infraestrutura usando arquivos de configuração. Em vez de criar recursos manualmente no console, os recursos são descritos em um template e provisionados de forma automatizada.

## AWS CloudFormation

O AWS CloudFormation é um serviço da AWS que permite modelar, provisionar e gerenciar recursos em nuvem usando templates. Esses templates podem ser escritos em YAML ou JSON.

## Stack

Uma stack representa o conjunto de recursos criados a partir de um template CloudFormation. Ao criar, atualizar ou excluir uma stack, o CloudFormation gerencia os recursos associados a ela.

## Template

O template é o arquivo que descreve a infraestrutura desejada. Ele pode conter parâmetros, recursos, outputs, condições, mappings e outras seções.

## Parameters

Parâmetros permitem informar valores no momento da criação da stack. Eles tornam o template mais flexível e evitam valores fixos dentro do arquivo.

Exemplo:

```yaml
InstanceType:
  Type: String
  Default: t2.micro
```

## Resources

A seção `Resources` é obrigatória e define os recursos AWS que serão provisionados, como instâncias EC2, buckets S3, grupos de segurança e outros componentes.

## Outputs

Outputs retornam informações importantes após a criação da stack. No laboratório, o output principal foi a URL pública do servidor web.

## UserData

`UserData` permite executar comandos durante a inicialização de uma instância EC2. Neste desafio, ele foi usado para instalar e iniciar o Apache HTTP Server.

## Security Group

Security Groups funcionam como firewalls virtuais para controlar o tráfego de entrada e saída dos recursos. No template do desafio, foi liberado tráfego HTTP na porta 80.

Na versão com acesso remoto, também foi liberada a porta 22 para SSH, restrita ao IP informado no parâmetro `MyIP`.

## Key Pair

O par de chaves EC2 permite autenticação SSH em instâncias Linux. A chave pública fica associada à instância na AWS, enquanto a chave privada `.pem` deve ser guardada com segurança pelo usuário.

No laboratório, o nome definido para o par de chaves foi:

```text
ChavesLinux
```

## Pontos de Atenção

- O parâmetro `MyIP` deve ser configurado com cuidado.
- Usar `0.0.0.0/0` libera acesso público para qualquer origem.
- A porta 22 deve ser liberada apenas para o IP do usuário.
- A chave privada `.pem` não deve ser enviada para o GitHub.
- A stack deve ser excluída após os testes para evitar cobranças.
- Falhas na criação aparecem na aba de eventos da stack.
- A ausência de VPC padrão pode causar erro na criação da instância EC2.

## Checklist de Estudo

- [x] Entender a estrutura de um template YAML.
- [x] Identificar a função dos parâmetros.
- [x] Identificar os recursos criados pela stack.
- [x] Validar a importância dos outputs.
- [x] Compreender o ciclo de vida da stack.
- [x] Registrar aprendizados no repositório.
