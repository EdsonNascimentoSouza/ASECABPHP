Código do cadastro do banco de dados
<?php
session_start();
if (!isset($_SESSION['username'])) {
    header("Location: login.php");
    exit;
}

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    include 'conexao.php';

    $username = $_POST['username'];
    $password = $_POST['password'];

    // Criptografar senha
    $hashed_password = password_hash($password, PASSWORD_DEFAULT);

    // Verificar se o usuário já existe
    $sql_check = "SELECT * FROM users WHERE username = '$username'";
    $result = $conn->query($sql_check);

    if ($result->num_rows > 0) {
        echo "<script>alert('Usuário já existe!'); window.location.href='cadastro.php';</script>";
    } else {
        // Inserir novo usuário
        $sql = "INSERT INTO users (username, password) VALUES ('$username', '$hashed_password')";
        if ($conn->query($sql) === TRUE) {
            echo "<script>alert('Usuário cadastrado com sucesso!'); window.location.href='index.php';</script>";
        } else {
            echo "Erro: " . $conn->error;
        }
    }

    $conn->close();
}
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cadastrar Usuário</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f5f5f5;
        }

        .container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 500px;
        }

        h2 {
            color: #003366;
            font-size: 28px;
            text-align: center;
            margin-bottom: 20px;
        }

        label {
            font-size: 16px;
            color: #333;
            display: block;
            margin-bottom: 8px;
        }

        input[type="text"], input[type="password"] {
            width: 100%;
            padding: 12px;
            font-size: 16px;
            margin-bottom: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        input[type="submit"], .btn-back {
            width: 100%;
            padding: 15px;
            font-size: 16px;
            background-color: #003366;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }

        input[type="submit"]:hover, .btn-back:hover {
            background-color: #0055cc;
        }

        .btn-back {
            background-color: #888;
            margin-top: 10px;
        }

        .btn-back:hover {
            background-color: #555;
        }

    </style>
</head>
<body>

    <div class="container">
        <h2>Cadastrar Usuário</h2>
        <form method="post" action="">
            <label for="username">Nome de Usuário:</label>
            <input type="text" name="username" id="username" required><br>

            <label for="password">Senha:</label>
            <input type="password" name="password" id="password" required><br>

            <input type="submit" value="Cadastrar">
            <a href="index.php">
                <input type="button" class="btn-back" value="Voltar para o Início">
            </a>
        </form>
    </div>

</body>
</html>
