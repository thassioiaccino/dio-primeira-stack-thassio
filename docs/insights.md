# Insights e Aprendizados

## 1. CloudFormation ajuda a padronizar ambientes

Criar recursos manualmente pelo console é útil para aprender, mas pode gerar diferenças entre ambientes. Com CloudFormation, a configuração fica registrada em um template e pode ser reaproveitada.

## 2. O template funciona como documentação viva

Além de criar recursos, o template descreve a arquitetura. Ao ler o arquivo YAML, é possível entender quais recursos serão criados e como eles se relacionam.

## 3. Parâmetros aumentam a reutilização

O uso de `Parameters` evita valores fixos no template. Assim, a mesma stack pode ser criada em diferentes cenários com pequenas alterações, como tipo de instância ou IP permitido.

## 4. Outputs facilitam a validação

Outputs tornam a experiência mais prática porque exibem informações importantes depois do provisionamento. Neste caso, a URL pública do servidor web permitiu testar rapidamente se a instância estava funcionando.

## 5. Eventos ajudam no troubleshooting

A aba de eventos da stack mostra o ciclo de criação dos recursos. Quando algo falha, o CloudFormation apresenta mensagens que ajudam a entender o problema.

## 6. Segurança precisa ser considerada desde o início

Mesmo em um laboratório, é importante controlar o acesso. O parâmetro `MyIP` permite restringir o tráfego HTTP e SSH a um intervalo específico, reduzindo exposição desnecessária.

Ao habilitar SSH, a chave privada `.pem` passa a ser um item sensível. Ela deve ser armazenada com cuidado e nunca versionada no GitHub.

## 7. Limpeza de recursos é parte do laboratório

Excluir a stack após os testes é tão importante quanto criá-la. Em ambientes de nuvem, recursos ativos podem gerar custos.

## Dificuldades Esperadas

- Entender a indentação do YAML.
- Diferenciar parâmetros, recursos e outputs.
- Compreender a relação entre Security Group e instância EC2.
- Saber onde procurar erros quando a stack falha.
- Lembrar de excluir os recursos depois da validação.

## Melhorias Futuras

- Criar uma versão do template com VPC e sub-rede parametrizadas.
- Adicionar HTTPS usando Load Balancer e certificado.
- Separar a infraestrutura em templates menores.
- Criar uma pipeline para validar templates automaticamente.
- Usar tags padronizadas para organização e controle de custos.

## Conclusão Pessoal

Este desafio reforçou a importância de tratar infraestrutura como código. A stack criada é simples, mas apresenta conceitos fundamentais que servem como base para projetos maiores: automação, padronização, versionamento e documentação.
