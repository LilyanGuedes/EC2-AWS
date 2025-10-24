# 🔧 Troubleshooting - Problemas Comuns

## Não consigo conectar via SSH

### Connection timed out

**Causa:** Security Group não permite SSH

**Solução:**
```bash
aws ec2 authorize-security-group-ingress \
    --group-id sg-123456 \
    --protocol tcp \
    --port 22 \
    --cidr $(curl -s ifconfig.me)/32
```

### Permission denied

**Causa:** Permissões da chave incorretas

**Solução:**
```bash
chmod 400 minha-chave.pem
```

### Host key verification failed

**Causa:** Instância recriada

**Solução:**
```bash
ssh-keygen -R IP-PUBLICO
```

## CPU Alta

**Verificar:**
```bash
# Na instância
top

# CloudWatch
aws cloudwatch get-metric-statistics \
    --namespace AWS/EC2 \
    --metric-name CPUUtilization \
    --dimensions Name=InstanceId,Value=i-123456
```

**Solução:** Upgrade do tipo de instância

## Disco Cheio

**Verificar:**
```bash
df -h
```

**Solução:**
```bash
# Aumentar volume
aws ec2 modify-volume --volume-id vol-123456 --size 30

# Na instância
sudo growpart /dev/xvda 1
sudo resize2fs /dev/xvda1
```

## Custos Altos

**Verificar:**

1. Instâncias esquecidas rodando
2. Elastic IPs não associados ($0.005/h)
3. Volumes não anexados
4. Snapshots antigos

**Solução:**
```bash
# Listar recursos não usados
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"
aws ec2 describe-volumes --filters "Name=status,Values=available"
aws ec2 describe-addresses --query 'Addresses[?AssociationId==null]'
```

## Perdi minha chave SSH

**Solução:**
1. Criar snapshot do volume
2. Criar nova instância com novo par de chaves
3. Anexar volume antigo como secundário
4. Copiar dados necessários

## Instância não inicia

**Verificar:**
```bash
aws ec2 describe-instance-status --instance-ids i-123456
```

**Causas comuns:**
- Limite de instâncias atingido
- Problemas com User Data script
- Volume corrompido

**Solução:** Ver logs do sistema via console EC2
