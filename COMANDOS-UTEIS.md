# ⚡ Comandos Úteis - AWS EC2

## Instâncias
```bash
# Listar todas
aws ec2 describe-instances

# Listar rodando
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"

# Criar instância
aws ec2 run-instances \
    --image-id ami-0c55b159cbfafe1f0 \
    --instance-type t3.micro \
    --key-name minha-chave

# Iniciar
aws ec2 start-instances --instance-ids i-123456

# Parar
aws ec2 stop-instances --instance-ids i-123456

# Terminar (deletar)
aws ec2 terminate-instances --instance-ids i-123456
```

## Security Groups
```bash
# Criar
aws ec2 create-security-group \
    --group-name web-sg \
    --description "Web Server"

# Permitir SSH
aws ec2 authorize-security-group-ingress \
    --group-id sg-123456 \
    --protocol tcp \
    --port 22 \
    --cidr $(curl -s ifconfig.me)/32

# Permitir HTTP
aws ec2 authorize-security-group-ingress \
    --group-id sg-123456 \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0
```

## Volumes
```bash
# Criar volume
aws ec2 create-volume \
    --availability-zone us-east-1a \
    --size 20 \
    --volume-type gp3

# Criar snapshot
aws ec2 create-snapshot --volume-id vol-123456

# Listar volumes disponíveis
aws ec2 describe-volumes --filters "Name=status,Values=available"
```

## Conexão SSH
```bash
# Conectar
ssh -i minha-chave.pem ec2-user@IP-PUBLICO

# Ajustar permissões da chave
chmod 400 minha-chave.pem
```

## Aliases Úteis

Adicione ao `~/.bashrc`:
```bash
alias ec2-list='aws ec2 describe-instances --query "Reservations[].Instances[].[InstanceId,State.Name]" --output table'

alias ec2-running='aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"'

ec2-ssh() {
    IP=$(aws ec2 describe-instances --instance-ids $1 --query 'Reservations[0].Instances[0].PublicIpAddress' --output text)
    ssh -i ~/.ssh/minha-chave.pem ec2-user@$IP
}
```

## Referências

**Regiões:**
- us-east-1 (N. Virgínia)
- sa-east-1 (São Paulo)

**Portas:**
- 22: SSH
- 80: HTTP
- 443: HTTPS
