# 📚 Documentação - EC2 na AWS

## O que é EC2?

Amazon EC2 (Elastic Compute Cloud) é um serviço que fornece servidores virtuais na nuvem. Você paga apenas pelo que usar e pode escalar recursos conforme necessário.

## Tipos de Instâncias

| Tipo | Uso | Exemplo |
|------|-----|---------|
| **t3** | Uso geral, web servers | t3.micro |
| **m5** | Balanceado | m5.large |
| **c5** | CPU intensivo | c5.large |
| **r5** | Muita memória (databases) | r5.large |

## Criando uma Instância

1. **Console EC2** → Launch Instance
2. **Nome:** meu-servidor
3. **AMI:** Amazon Linux 2023
4. **Tipo:** t2.micro (Free Tier)
5. **Par de chaves:** Criar novo (.pem)
6. **Security Group:**
   - SSH (22) → Meu IP
   - HTTP (80) → 0.0.0.0/0
7. **Storage:** 8 GB gp3
8. Launch!

## Conectando via SSH
```bash
# Ajustar permissões
chmod 400 minha-chave.pem

# Conectar
ssh -i "minha-chave.pem" ec2-user@SEU-IP-PUBLICO
```

## Security Groups

Firewall virtual que controla o tráfego:
```
Regras comuns:
- SSH (22): Acesso administrativo
- HTTP (80): Tráfego web
- HTTPS (443): Tráfego web seguro
```

## Armazenamento (EBS)

**Tipos principais:**
- **gp3**: Uso geral (recomendado)
- **io2**: Alta performance
- **st1**: Big Data (HDD)

**Snapshots:** Backup dos volumes
```bash
# Criar snapshot
aws ec2 create-snapshot --volume-id vol-123456
```

## Monitoramento

**CloudWatch** monitora:
- CPU
- Rede
- Disco
- Status da instância

## Custos

| Modelo | Desconto | Quando usar |
|--------|----------|-------------|
| On-Demand | - | Testes, imprevisível |
| Reserved | 40-60% | Produção estável |
| Spot | Até 90% | Processamento batch |

## Boas Práticas

✅ **Fazer:**
- Usar IAM Roles
- Habilitar criptografia
- Fazer backups regulares
- Monitorar recursos
- Aplicar tags

❌ **Evitar:**
- SSH aberto para todos (0.0.0.0/0)
- Deixar instâncias paradas rodando
- Ignorar alarmes

## Recursos

- [Documentação oficial](https://docs.aws.amazon.com/ec2/)
- [Tipos de instâncias](https://aws.amazon.com/ec2/instance-types/)
- [Calculadora de custos](https://calculator.aws/)
