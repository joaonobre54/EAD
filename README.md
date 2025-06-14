# EAD
from clickhouse_connect import get_client

client = get_client(host='localhost', port=8123, username='default', password='')

# Cria o banco de dados
client.command('CREATE DATABASE IF NOT EXISTS ShopBrasil')

# Usa o banco
client.command('USE ShopBrasil')  # Opcional, pois o banco pode ser especificado nas queries

# Cria as tabelas (adaptadas para ClickHouse)
client.command('''
CREATE TABLE IF NOT EXISTS ShopBrasil.clientes (
    id UInt32,
    nome String,
    email String,
    telefone String,
    endereco String,
    data_cadastro DateTime DEFAULT now()
) ENGINE = MergeTree()
ORDER BY id
''')

client.command('''
CREATE TABLE IF NOT EXISTS ShopBrasil.produtos (
    id UInt32,
    nome String,
    categoria String,
    preco Decimal(10, 2),
    estoque Int32,
    descricao String,
    data_criacao DateTime DEFAULT now()
) ENGINE = MergeTree()
ORDER BY id
''')

client.command('''
CREATE TABLE IF NOT EXISTS ShopBrasil.pedidos (
    id UInt32,
    cliente_id UInt32,
    data_pedido DateTime DEFAULT now(),
    total Decimal(10, 2),
    status String DEFAULT 'Pendente'
) ENGINE = MergeTree()
ORDER BY id
''')

client.command('''
CREATE TABLE IF NOT EXISTS ShopBrasil.itens_pedido (
    id UInt32,
    pedido_id UInt32,
    produto_id UInt32,
    quantidade Int32,
    preco_unitario Decimal(10, 2)
) ENGINE = MergeTree()
ORDER BY id
''')
