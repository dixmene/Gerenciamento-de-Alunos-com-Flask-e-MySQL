# **Apresentação do Projeto: Gerenciamento de Alunos com Flask e MySQL**

## **1. Introdução**

Olá a todos,

Hoje, vou apresentar um projeto de gerenciamento de alunos desenvolvido com Flask e MySQL. Este projeto visa criar uma aplicação web simples para visualizar, adicionar e gerenciar informações de alunos. Abaixo, vou detalhar o objetivo do projeto, o código utilizado, e fornecer um guia completo para configuração e execução.

## **2. Objetivo do Projeto**

O objetivo é desenvolver uma aplicação web que permita:

1. **Visualizar** a lista de alunos cadastrados no banco de dados.
2. **Adicionar** novos alunos com informações como nome, idade e curso.
3. **Gerenciar** os dados de maneira intuitiva e eficaz.

Utilizamos Flask para o desenvolvimento da aplicação web e MySQL como banco de dados para armazenar as informações dos alunos.

## **3. Estrutura do Projeto**

### **Código Python (`app.py`)**

Este é o código principal da aplicação Flask, responsável pela conexão com o banco de dados MySQL e definição das rotas da aplicação.

```python
from flask import Flask, render_template, request, redirect, url_for
import mysql.connector

app = Flask(__name__)

# Função para obter a conexão com o banco de dados
def get_db_connection():
    conn = mysql.connector.connect(
        host='localhost',
        user='root',
        password='123',
        database='escola'
    )
    return conn

# Rota para exibir a lista de alunos
@app.route('/')
def index():
    conn = get_db_connection()
    cursor = conn.cursor(dictionary=True)
    cursor.execute('SELECT * FROM alunos;')
    alunos = cursor.fetchall()
    cursor.close()
    conn.close()
    return render_template('index.html', alunos=alunos)

# Rota para adicionar um aluno
@app.route('/add', methods=['POST'])
def add_aluno():
    nome = request.form['nome']
    idade = request.form['idade']
    curso = request.form['curso']
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute('INSERT INTO alunos (nome, idade, curso) VALUES (%s, %s, %s)',
                   (nome, idade, curso))
    conn.commit()
    cursor.close()
    conn.close()
    return redirect(url_for('index'))

if __name__ == '__main__':
    app.run(debug=True)
```

#### **Explicação do Código:**

- **Conexão com o Banco de Dados:** A função `get_db_connection` estabelece a conexão com o banco de dados MySQL usando `mysql.connector`.
- **Rota Principal (`/`):** Recupera todos os alunos do banco de dados e os passa para o template HTML para exibição.
- **Rota de Adição (`/add`):** Processa os dados do formulário enviados via POST, insere um novo aluno no banco de dados e redireciona para a página principal.

### **Código HTML (`templates/index.html`)**

Este código define a interface da aplicação.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerenciamento de Alunos</title>
</head>
<body>
    <h1>Lista de Alunos</h1>
    <table border="1">
        <thead>
            <tr>
                <th>ID</th>
                <th>Nome</th>
                <th>Idade</th>
                <th>Curso</th>
            </tr>
        </thead>
        <tbody>
            {% for aluno in alunos %}
            <tr>
                <td>{{ aluno.id }}</td>
                <td>{{ aluno.nome }}</td>
                <td>{{ aluno.idade }}</td>
                <td>{{ aluno.curso }}</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>

    <h2>Adicionar Aluno</h2>
    <form action="{{ url_for('add_aluno') }}" method="post">
        <label for="nome">Nome:</label>
        <input type="text" id="nome" name="nome" required>
        <br>
        <label for="idade">Idade:</label>
        <input type="number" id="idade" name="idade" required>
        <br>
        <label for="curso">Curso:</label>
        <input type="text" id="curso" name="curso" required>
        <br>
        <input type="submit" value="Adicionar">
    </form>
</body>
</html>
```

#### **Explicação do Código HTML:**

- **Tabela de Alunos:** Exibe os dados dos alunos recuperados do banco de dados.
- **Formulário de Adição:** Permite adicionar um novo aluno ao banco de dados, enviando dados para a rota `/add`.

## **4. Configuração e Execução**

Aqui está um guia passo a passo para configurar e executar o projeto:

### **Passo 1: Instalação das Dependências**

Certifique-se de ter Flask e mysql-connector-python instalados. Use os seguintes comandos:

```cmd
pip install flask mysql-connector-python
```

### **Passo 2: Configuração do Banco de Dados**

1. **Crie o Banco de Dados e a Tabela**

   Abra o MySQL ou MariaDB e execute os seguintes comandos:

   ```sql
   CREATE DATABASE IF NOT EXISTS escola;
   USE escola;

   CREATE TABLE IF NOT EXISTS alunos (
       id INT AUTO_INCREMENT PRIMARY KEY,
       nome VARCHAR(100) NOT NULL,
       idade INT NOT NULL,
       curso VARCHAR(100) NOT NULL
   );
   ```

2. **Insira Dados Iniciais (Opcional)**

   Adicione alguns registros iniciais na tabela para teste:

   ```sql
   INSERT INTO alunos (nome, idade, curso) VALUES
       ('Ana Costa', 19, 'Arquitetura'),
       ('Carlos Pereira', 21, 'Biologia'),
       ('Juliana Santos', 22, 'Química'),
       ('Marcos Almeida', 20, 'Física'),
       ('Renata Oliveira', 23, 'História'),
       ('Fernanda Lima', 20, 'Geografia'),
       ('Paulo Souza', 21, 'Sociologia'),
       ('Beatriz Martins', 22, 'Educação Física'),
       ('Lucas Fernandes', 19, 'Psicologia'),
       ('Larissa Castro', 20, 'Medicina'),
       ('João Silva', 20, 'Engenharia'),
       ('Maria Oliveira', 22, 'Matemática');
   ```

### **Passo 3: Executar o Aplicativo Flask**

Navegue até o diretório do projeto e execute o comando:

```cmd
python app.py
```
A aplicação será iniciada e estará disponível em `http://127.0.0.1:5000`.

## **5. Conclusão**

Este projeto demonstra a criação de uma aplicação web básica com Flask e MySQL. Serve como uma introdução ao desenvolvimento web e à integração com bancos de dados, podendo ser expandido com mais funcionalidades, como edição e exclusão de registros, autenticação de usuários, e muito mais.
![image](https://github.com/user-attachments/assets/aab526a9-496b-4c29-af47-8c4f0571bbf6)
![image1](https://github.com/user-attachments/assets/d6db13a4-b4df-4cf8-9e01-23fb879d8e18)
